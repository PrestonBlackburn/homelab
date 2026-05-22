# Cluster Monitoring

Monitoring Tools:  
- Headlamp  
- Prometheus  
- Graphana  
- OpenWRT Dashboard  


## kube-prometheus-stack (Prometheus/Graphana)

[kube-prometheus-stack chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)  


I just want this monitoring stack on my second node  
```bash
microk8s kubectl label nodes <second-node-name> role=monitoring  
```

Just managing the gateway and secrets seperately for now
```bash
kubectl apply -f secrets.yaml
```

```bash
kubectl apply -f gateway.yaml
kubectl apply -f route.yaml
```

## Headlamp
helm install: https://helm.sh/docs/intro/install/
```bash
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

config helm:

# Create the directory if it doesn't exist
mkdir -p ~/.kube

# Export the config to the default location
microk8s config > ~/.kube/config

# Secure the file (highly recommended)
chmod 600 ~/.kube/config


helm repo add headlamp https://kubernetes-sigs.github.io/headlamp/
helm install my-headlamp headlamp/headlamp --namespace kube-system

1. Get the application URL by running these commands:
  export POD_NAME=$(microk8s kubectl get pods --namespace kube-system -l "app.kubernetes.io/name=headlamp,app.kubernetes.io/instance=my-headlamp" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(microk8s kubectl get pod --namespace kube-system $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace kube-system port-forward $POD_NAME 8080:$CONTAINER_PORT
2. Get the token using
  microk8s kubectl create token my-headlamp --namespace kube-system

ssh -L 8080:localhost:8080 prestonb@10.0.0.7 "microk8s kubectl port-forward -n kube-system svc/my-headlamp 8080:80"




## OpenWRT

