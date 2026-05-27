# Deployment

Deployment Tools (CI/CD):  
- ArgoCD  
- Github  

[ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/getting_started/) 

Install ArgoCD on the cluster:
```bash
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```