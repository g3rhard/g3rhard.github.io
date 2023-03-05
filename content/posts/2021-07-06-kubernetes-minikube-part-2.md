---
layout:     post
title:      "Kubernetes: Исследование с помощью Minikube - часть 2"
date:       2021-07-06 19:00:00 +0800
categories: k8s kubernetes minikube
---

Это продолжение [первой части](/2021/07/kubernetes-minikube-part-1.html), которое надеюсь, служит логичным ее продолжением - мы имеем кластер Kubernetes, работаем в отдельном Namespace, умеем запускать небольшое stateless приложение на PHP и знаем, как получить к нему доступ. Что дальше можно сделать? Вероятнее всего, нужно упрощать обновление приложения - так как тяжело каждый раз вручную копировать Deployment в консоль с лишь одной обновленной строкой (в нашем случае это ID коммита), пример из первой части:

```yaml
image: g3rhard/php-sample-app:467794
```

## Небольшая часть с лайфхаками

* Для удобного просмотра состояния объектов в кластере можно использовать [k9s](https://github.com/derailed/k9s):

  ```sh
  minikube kubectl -- config view --flatten --minify > cluster-config.txt
  # kubectl config view --flatten --minify > cluster-config.txt
  k9s --kubeconfig cluster-config.txt
  ```

* Проверить, как будет выглядеть наш общий шаблон helm chart (об этом будет чуть ниже):

```sh
helm template ./helm-php-sample-app
```

Итак, приступим.

## Часть 2. Упрощаем deploy приложения

Выбор инструмента по упрощению deploy пал на helm, как на достаточно подробно описанный инструмент.

* Создаем пустой репо на GitHub, клонируем его себе и создаем пустой шаблон пакета helm:

  ```sh
  git clone git@github.com:g3rhard/helm-php-sample-app.git
  helm create helm-php-sample-app
  cd helm-php-sample-app
  ```

* Структура каталога выглядит так:

  ```sh
  ❯ tree -L 3 -a -I ".git" helm-php-sample-app
  helm-php-sample-app
  ├── .helmignore
  ├── Chart.yaml
  ├── README.md
  ├── charts
  ├── templates
  │   ├── NOTES.txt
  │   ├── _helpers.tpl
  │   ├── deployment.yaml
  │   ├── hpa.yaml
  │   ├── ingress.yaml
  │   ├── service.yaml
  │   ├── serviceaccount.yaml
  │   └── tests
  │       └── test-connection.yaml
  └── values.yaml

  3 directories, 12 files
  ```

* И отправляем изменения в репо:

  ```sh
  git add .
  git commit -a -m 'first commit'
  git push origin main
  ```

Теперь займемся последовательным переносом тех конфигураций, что описаны в первой части этой заметки:

* Для отладки клонируем helm chart на машину с minikube:

  ```sh
  git clone https://github.com/g3rhard/helm-php-sample-app
  ```

* На этой же машине проверить шаблон helm chart можно так:

  ```sh
  helm install --dry-run --debug ./helm-php-sample-app --set service.type=NodePort --generate-name
  ```

Пробежавшись по конфигурациям, можно заметить, что helm сгенерировал совсем не то, что нам бы хотелось, поэтому обновляем файл values.yaml и правим в нем переменные:

```yaml
replicaCount: 1 -> replicaCount: 3
repository: nginx -> repository: "g3rhard/php-sample-app"
tag: "" -> tag: "ad2dfb"
serviceAccount::create: false
type: ClusterIP -> type: NodePort
```

После этого можно проверить, как будет выглядеть наш общий шаблон helm chart:

```sh
helm template ./helm-php-sample-app
```

На вид все ок, а значит отправляем изменения в репо и загружаем данные на наш сервер и пробуем применить:

```sh
helm install -n developer php-sample-app ./helm-php-sample-app
Error: rendered manifests contain a resource that already exists. Unable to continue with install: Service "php-sample-app" in namespace "developer" exists and cannot be imported into the current release: invalid ownership metadata; label validation error: missing key "app.kubernetes.io/managed-by": must be set to "Helm"; annotation validation error: missing key "meta.helm.sh/release-name": must be set to "php-sample-app"; annotation validation error: missing key "meta.helm.sh/release-namespace": must be set to "developer"
```

К сожалению, в шаблоне было много новых и непонятных (пока что) label, поэтому сходу применить изменения не получилось. Но так как это и не было нашей целью, то мы просто удаляем старые отдельные ресурсы, созданные в первой части:

```sh
kubectl -n developer delete service php-sample-app
kubectl -n developer delete deployment php-sample-app
kubectl -n developer delete ingress php-sample-app-ingress
```

И снова попробуем установить наш helm chart:

```sh
helm install -n developer php-sample-app ./helm-php-sample-app
```

В результате видим сообщение:

```sh
NAME: php-sample-app
LAST DEPLOYED: Wed Jul  7 19:05:44 2021
NAMESPACE: developer
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
export NODE_PORT=$(kubectl get --namespace developer -o jsonpath="{.spec.ports[0].nodePort}" services php-sample-app)
export NODE_IP=$(kubectl get nodes --namespace developer -o jsonpath="{.items[0].status.addresses[0].address}")
echo http://$NODE_IP:$NODE_PORT
```

Проверяем установку:

* Helm:

  ```sh
  helm ls --all -n developer
  NAME          	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART               	APP VERSION
  php-sample-app	developer	1       	2021-07-07 19:05:44.009257183 +0000 UTC	deployed	php-sample-app-0.1.0	1.16.0
  ```

* Pods:

  ```sh
  kubectl -n developer get pods
  NAME                              READY   STATUS    RESTARTS   AGE
  php-sample-app-6555dd88f5-6ttkq   1/1     Running   0          16m
  php-sample-app-6555dd88f5-6zkx8   1/1     Running   0          16m
  php-sample-app-6555dd88f5-xjlk4   1/1     Running   0          16m
  ```

* Deployments:

  ```sh
  kubectl -n developer get deployments
  NAME             READY   UP-TO-DATE   AVAILABLE   AGE
  php-sample-app   3/3     3            3           16m
  ```

* Service:

  ```sh
  kubectl -n developer get service
  NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
  php-sample-app      NodePort       10.103.150.105   <none>        80:32200/TCP   16m
  ```

И напоследок попробуем обновить наше приложение, все еще вручную, но уже более автоматизированно: Добавляем некоторые изменения в index.php, собираем новый образ и тэг нового образа сохраняем в helm chart, а после обновляем на сервере:

```sh
cd helm-php-sample-app
git pull
cd ..
helm upgrade -n developer php-sample-app ./helm-php-sample-app
```

И видим вывод:

```sh
Release "php-sample-app" has been upgraded. Happy Helming!
NAME: php-sample-app
LAST DEPLOYED: Wed Jul  7 19:31:09 2021
NAMESPACE: developer
STATUS: deployed
REVISION: 2
```

Действительно, есть с чем поздравить. На этом с этой частью можно заканчивать.

После завершения, еще можно было проверить две операции - rollback и delete:

* Rollback:
  * Возвращаемся на первую версию нашего приложения (helm chart), где 1 - это номер из параметра "REVISION: 1", который можно найти в листингах выше.

  ```sh
  helm rollback -n developer php-sample-app 1
  ```

  * И возвращаемся на вторую версию нашего приложения (REVISION: 2)

  ```sh
  helm rollback -n developer php-sample-app 2
  ```

* И простой момент - удаление:

```sh
helm delete -n developer php-sample-app
release "php-sample-app" uninstalled
```

That's all.

## Additional links

1. [baeldung - Using Helm and Kubernetes](https://www.baeldung.com/ops/kubernetes-helm)
2. [bitnami - Create Your First Helm Chart](https://docs.bitnami.com/tutorials/create-your-first-helm-chart)
