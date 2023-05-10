# Install argocd

https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd

```
helm repo add argo https://argoproj.github.io/argo-helm
helm template argocd argo/argo-cd -f values.yaml  > toto.yaml
helm install argocd argo/argo-cd -f values.yaml
```
