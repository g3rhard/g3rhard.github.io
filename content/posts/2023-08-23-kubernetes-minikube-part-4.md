---
layout:     post
title:      "Kubernetes: Исследование с помощью Minikube - часть 4"
date:       2023-08-23 08:00:00 +0200
categories: k8s kubernetes minikube
---

Это продолжение [третьей части](/2021/07/kubernetes-minikube-part-3.html).

## ArgoCD

* Let's install argocd cli tool first

  ```sh
  brew install argocd
  ```

* Create new namespace for ArgoCD

  ```sh
  kubectl create namespace argocd
  ```

* Deploy ArgoCD manifest

  ```sh
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```

* Get admin password from pod

  ```sh
  kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
  ```

* Authorize argocd cli app

  ```sh
  ARGO_PWD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
  argocd login localhost:8080 --username admin --password $ARGO_PWD --insecure
  ```

* Get access to UI via kubectl port-forward

  ```sh
  kubectl port-forward svc/argocd-server -n argocd 8080:443
  ```

* ArgoCD should be available on https://localhost:8080/

## Weave

* Adding Weave to visualize network:

  ```sh
  kubectl apply -f https://github.com/weaveworks/scope/releases/download/v1.13.2/k8s-scope.yaml
  ```

* Make sure, that everything is working in namespace "weave":

  ```sh
  kubectl get all -n weave
  ```

* Receiving access via Web UI:

  ```sh
  kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
  ```

That's all.

### Additional links

1. [ArgoCD - Installation](https://argo-cd.readthedocs.io/en/stable/operator-manual/core/)
2. [Weave - Installation](https://www.weave.works/docs/scope/latest/installing/#k8s)
