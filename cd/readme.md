# ArgoCD

[ArgoCD Docs](https://argo-cd.readthedocs.io/en/stable/getting_started/)  

Installing:  
```bash
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```


# Sealed Secrets

```bash
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm repo update
```
Create namespace
```bash
kubectl create namespace sealed-secrets
```

```bash
helm install sealed-secrets sealed-secrets/sealed-secrets --namespace sealed-secrets --wait
```

The kubeseal cli is also needed
```bash
go install github.com/bitnami-labs/sealed-secrets/cmd/kubeseal@main
```

```bash
kubeseal --version
```

Test a secret
```bash
# Seal the secret
kubeseal --format yaml < example-secret.yaml > example-sealed-secret.yaml --controller-name=sealed-secrets --controller-namespace=sealed-secrets

# Apply the sealed secret
kubectl apply -f example-sealed-secret.yaml

# Verify the secret was created
kubectl get secret my-secret -n default
```