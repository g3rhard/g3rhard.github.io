---
layout:     post
title:      "Kubernetes: Исследование с помощью Minikube - часть 1"
date:       2021-07-05 19:00:00 +0800
categories: k8s kubernetes minikube
---

В этой (и надеюсь последующих) статьях будут описаны личные заметки по погружению в мир Kubernetes. Данная заметка не претендует на академическую точность и была сделана как пример, с которого можно начинать работу с Kubernetes, не арендуя большие вычислительные мощности.

## Часть 0. Подготовка

Сначала подготовим все, что понадобится для развертывания окружения. Подразумевается, что у нас уже есть ноутбук/компьютер/арендованный инстанс где можно запустить minikube. По моим личным наблюдениям, комфортная работа начинается на машинах c 8+Gb RAM, хотя показатели потребления памяти внутри панелей Kubernetes достаточно скромны. Для себя я выбрал [Linode](https://www.linode.com), так как там демократичные цены и бонус в $100 при регистрации. Инструкции ниже подразумевают, что у вас есть root доступ на сервера, уже установлен Docker, а для работы minikube был создан отдельный пользователь, пример:

  ```sh
  useradd -m minikube
  usermod -aG docker minikube
  usermod -aG sudo minikube
  ```

И для удобства можно добавить alias для пользователя, от которого будем работать:

  ```sh
  alias kubectl="minikube kubectl --"
  # Примерно так можно зафиксировать alias
  # echo 'alias kubectl="minikube kubectl --"' >> $HOME/.bashrc
  ```

* Устанавливаем зависимости:
  * [minikube](https://minikube.sigs.k8s.io/docs/start/)

  ```sh
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```

  * [ngrok](https://ngrok.com/download)

  ```sh
  wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
  unzip /path/to/ngrok.zip
  ./ngrok authtoken <your_auth_token>
  ```

* Запускаем minikube (от имени пользователя, которого мы создали раньше, не от root):

  ```sh
  minikube start && minikube status
  ```

* Сразу устанавливаем нужные дополнения для minikube:

  ```sh
  minikube addons enable ingress
  minikube addons enable dashboard
  minikube addons enable metrics-server
  ```

## Часть 1. Запуск минимального приложения PHP

* Добавляем Namespace (или отдельное окружение) для нашего проекта:

  ```sh
  cat <<EOF | kubectl apply -f -
  apiVersion: v1
  kind: Namespace
  metadata:
    name: developer
    labels:
      name: developer
  EOF
  ```

* Описываем Deployment (что и где мы запускаем), указывая созданный Namespace "developer":

  ```sh
  cat <<EOF | kubectl apply -n developer -f -
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: php-sample-app
    labels:
      app: php-sample-app
  spec:
    replicas: 3
    minReadySeconds: 15
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxUnavailable: 1
        maxSurge: 1
    selector:
      matchLabels:
        app: php-sample-app
    template:
      metadata:
        labels:
          app: php-sample-app
      spec:
        containers:
        - name: php-sample-app
          image: g3rhard/php-sample-app:467794
          ports:
          - containerPort: 80
  EOF
  ```

* Создаем простой сервис чтобы достучаться до нашего приложения:
  * Вариант 1 - Service NodePort:

    ```sh
    cat <<EOF | kubectl apply -n developer -f -
    apiVersion: v1
    kind: Service
    metadata:
      name: php-sample-app
      labels:
        name: php-sample-app
    spec:
      type: NodePort
      ports:
        - port: 80
          name: http
      selector:
        app: php-sample-app
    EOF
    ```

  * Вариант 2 - создаем Service LoadBalancer:

    ```sh
    cat <<EOF | kubectl apply -n developer -f -
    apiVersion: v1
    kind: Service
    metadata:
      name: php-sample-app-lb
    spec:
      type: LoadBalancer
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        app: php-sample-app
    EOF
    ```

* Теперь нужно получить доступ к сервису, который находится внутри minikube:
  * Вариант 1:

    ```sh
    minikube service --url php-sample-app -n developer
    ./ngrok http $(minikube ip):SERVICE_PORT
    ```

  * Вариант 2 - с помощью Service LoadBalancer:

    ```sh
    minikube service php-sample-app-lb -n developer
    ./ngrok http $(minikube ip):SERVICE_PORT
    ```

* Также можно немного усложнить схему и добавить тип объекта Ingress (по аналогии - как Nginx):

  ```sh
  cat <<EOF | kubectl apply -n developer -f -
  ---
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: php-sample-app-ingress
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /$1
  spec:
    rules:
      - host: php-sample-app.com
        http:
          paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                  name: php-sample-app # or name: php-sample-app-lb
                  port:
                    number: 80
  EOF
  ```

* Аналогично получаем доступ через Ingress, только теперь нам не нужно использовать рандомный порт:
  * Вариант 1 - Через ngrok (тут также потребуется подменить DNS запись для доступа к нашему сайту)

    ```sh
    kubectl get ingress -n developer
    ./ngrok http $(minikube ip):80
    ```

  * Вариант 2: Bash

    ```sh
    minikube tunnel # for Ingress
    curl -H "Host: php-sample-app.com" http://127.0.0.1
    ```

  * Вариант 3: Powershell

    ```PowerShell
    minikube tunnel # for Ingress
    $(Invoke-WebRequest "http://127.0.0.1" -Headers @{'Host'= 'php-sample-app.com'}).Content
    ```

На этом с первой частью мы закончили. Вне заметки осталась сборка docker image в GitHub Actions, публикация image в hub.docker.com и ручное обновление Deployment. Как это было реализовано в моем случае, можно посмотреть в репозитории [g3rhard/php-sample-app](https://github.com/g3rhard/php-sample-app).

That's all.

## Additional links

1. [minikube - start](https://minikube.sigs.k8s.io/docs/start/)
2. [ngrok](https://ngrok.com/download)
3. [linode](https://www.linode.com)
