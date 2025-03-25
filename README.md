# n8n

```
minikube start --kubernetes-version='v1.30.9'
```
Install argocd

```
kubectl create ns argocd &&\
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
Get password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

```
minikube -n argocd service argocd-server --url
```

Create argo application
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: components
  namespace: argocd
spec:
  project: default
  source:
    repoURL: git@github.com:thanad/n8n.git
    path: manifest
    targetRevision: HEAD
    directory:
      recurse: true
      jsonnet: {}
  destination:
    name: in-cluster
    namespace: argocd
  syncPolicy:
    automated: {}
```

