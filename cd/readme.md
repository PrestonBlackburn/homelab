# ArgoCD

[ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/getting_started/)  

Installing:  
```bash
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```