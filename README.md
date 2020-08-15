# ArgoCD Tutorial

This tutorial helps you working ArgoCD with Kubernetes

## Prerequisites

This tutorial will require:

- Installed Docker
- Installed Minikube
- Installed kubectl CLI

## TL;DR

### Minikube

- Ref: https://kubernetes.io/docs/tasks/tools/install-minikube
- Quickstart with MacOS by `brew install minikube && brew link minikube`

- Starting Minikube: `minikube start`
- Setting kubectl context config with Minikube `kubectl config set-context minikube`
- Verifying by: `kubectl get namespaces`
- Installing and accessing to the Kubernetes dashboard by `minikube dashboard`

### ArgoCD

#### Installing ArgoCD on Kubernetes

- Accessing your Kubernetes cluster and create a namespace  `argocd`:
  
  ```shell
  kubectl create namespace argocd
  ```
  
- Installing the stable version of ArgoCD:
  
  ```shell
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```

#### Installing ArgoCD CLI

- On macOS uses with Homebrew:

  ```shell
  brew update && brew install argoproj/tap/argocd
  ```

- Installing with cURL:

  ```shell
  VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')
  curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-darwin-amd64
  chmod +x /usr/local/bin/argocd
  ```

#### Login to ArgoCD:

- Set default namespace:

```shell
kubectl config set-context --current --namespace=argocd
```

- Tunneling ArgoCD's server:

```shell
kubectl port-forward svc/argocd-server 8080:443
```

- You need to receive the password for the first time deploying ArgoCD by:

```shell
export ARGO_PASSWORD=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2)
```

- Login with CLI by default username `admin`:

```shell
argocd login localhost:8080 --username admin --password $ARGO_PASSWORD --insecure
```

- You also could login with UI at http://localhost:8080

- Change password by using:

```shell
argocd account update-password
```