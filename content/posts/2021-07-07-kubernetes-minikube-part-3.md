---
layout:     post
title:      "Kubernetes: Исследование с помощью Minikube - часть 3"
date:       2021-07-07 19:00:00 +0800
categories: k8s kubernetes minikube
---

Это продолжение [второй части](/2021/07/kubernetes-minikube-part-2.html). На данный момент мы уже знаем некоторые объекты в Kubernetes и умеем пользоваться Helm. Что можно сделать дальше? Конечно, добавить больше автоматизации, секретов, хранилищ - много чего. Целью сегодняшней части будет являтся автоматизация и упрощение процесса упаковки описания приложения в helm chart.

## Часть 3. Еще больше упрощаем deploy приложения

* Попробуем упаковать наш helm chart стандартными средствами:

  ```sh
  helm package helm-php-sample-app
  Successfully packaged chart and saved it to: .../php-sample-app-0.1.0.tgz
  ```

Теперь можно не пользоваться git для загрузки helm chart, а сохранять каждую версию в виде артефакта сборки (например, добавив эту сборку в GitHub Actions). Это действие мы и будем автоматизировать.

Сохраняем исходный код нашего helm chart в тот же репозиторий, что и код нашего тестового приложения PHP, чтобы получилась такая структура:

```text
❯ tree -L 3 -a -I ".git"
.
├── .github
│   └── workflows
│       ├── helm-charts.yml
│       ├── main.yml
│       └── snyk-container-analysis.yml
├── README.md
├── charts
│   └── php-sample-app
│       ├── .helmignore
│       ├── Chart.yaml
│       ├── README.md
│       ├── templates
│       └── values.yaml
└── php-sample-app
    ├── Dockerfile
    └── src
        ├── css
        └── index.php

8 directories, 10 files
```

Возможно это и немного неправильно (для helm charts вероятнее лучше всего создать отдельный репозиторий), но для изучения этого будет достаточно.

А теперь попробуем настроить репозиторий для helm chart с помощью GitHub (GitHub Pages) в том же репозитории, где хранится наше приложение:

* Создаем пустую ветку под названием "gh-pages" и удаляем из нее все файлы:

  ```sh
  git checkout --orphan gh-pages
  git rm --cached -r .
  ```

* Добавляем в ветку "gh-pages" файл README.md и по желанию - "_config.yml" в котором будет хранится конфигурация Jekyll для GitHub Pages:

  ```yaml
  theme: jekyll-theme-cayman
  ```

* Отправляем изменения в ветке в репозиторий:

  ```sh
  git commit -a -m "add branch for github pages"
  git push origin gh-pages
  ```

* И переключаемся на основную ветку (в моем случае main):

  ```sh
  git checkout main
  ```

* В основной ветке (main) создаем файл GitHub Action (.github/workflows/release.yml) следующего содержания:

  {% gist 2d6f6d85bef57d9a4c1a5601127505f1 %}

Если все сделали правильно, то после выполнения GitHub Actions мы сможем увидеть свежий релиз в разделе "Releases" на главной странице нашего репозитория.

Следующий шаг - проверить работу репозитория на кластере Kubernetes. Заходим на наш сервер с установленным Minikube и выполняем следующие команды:

* Добавляем репозиторий с помощью Helm (NameSpace указывать не нужно):

  ```sh
  # helm repo add YOUR_REPO_NAME https://GITHUB_USER.github.io/YOUR_REPO_NAME
  helm repo add php-sample-app https://g3rhard.github.io/php-sample-app
  helm repo update
  ```

* Проверяем, что другие инсталляции были удалены:

  ```sh
  helm uninstall -n developer php-sample-app ./helm-php-sample-app
  kubectl -n developer get pods
  ```

* И устанавливаем наше приложение:

  ```sh
  # helm install -n developer YOUR_APP_NAME REPO_NAME/APP_NAME
  helm install -n developer php-sample-app php-sample-app/php-sample-app
  ```

* Проверяем развертывание приложения:

  ```sh
  kubectl -n developer get pods
  kubectl -n developer get services
  NODE_PORT=$(kubectl get --namespace developer -o jsonpath="{.spec.ports[0].nodePort}" services php-sample-app)
  ./ngrok http $(minikube ip):$NODE_PORT
  ```

Теперь сборка новых версий у нас более-менее автоматизирована и пришло время проверить это.

* Повышаем версию в файле charts/php-sample-app/Chart.yaml и отправляем изменения в репо (и дожидаемся выполнения GitHub Action):

  ```diff
  -version: 0.3.0
  +version: 0.3.1
  ```

* Обновляем репозиторий на сервере с Minikube и проверяем получение новой версии:

  ```sh
  helm repo update
  helm search repo php-sample-app
  NAME                         	CHART VERSION	APP VERSION	DESCRIPTION
  php-sample-app/php-sample-app	0.3.1        	0.3.0      	A Helm chart for Kubernetes
  ```

* Обновляем наше приложение:

  ```sh
  helm upgrade -n developer php-sample-app php-sample-app/php-sample-app

  Release "php-sample-app" has been upgraded. Happy Helming!
  NAME: php-sample-app
  LAST DEPLOYED: Sun Jul 11 17:10:17 2021
  NAMESPACE: developer
  STATUS: deployed
  REVISION: 2
  ```

That's all.

## Additional links

1. [helm.sh - Chart Releaser Action to Automate GitHub Page Charts](https://helm.sh/docs/howto/chart_releaser_action/#github-actions-workflow)
2. [medium - Create a public Helm chart repository with GitHub Pages](https://medium.com/@mattiaperi/create-a-public-helm-chart-repository-with-github-pages-49b180dbb417)
3. [harness.io - Turning a GitHub Repo Into a Helm Chart Repo](https://harness.io/blog/devops/helm-chart-repo)
4. [baeldung - Using Helm and Kubernetes](https://www.baeldung.com/ops/kubernetes-helm)
