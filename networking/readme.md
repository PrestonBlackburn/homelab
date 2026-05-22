# Networking

Gateways will be behind MetalLB to expose on the local network   

internal ip (ex: 10.0.0.60) → MetalLB (lb) → Gateway API → Service  

```bash
microk8s enable metallb:10.0.0.60-10.0.0.70
microk8s kubectl get svc --all-namespaces
```

Put non-private things on the local network  

*add gateways for exposing different services on the local network*  

Install the gateway CRDs - 
```bash
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.5.0/standard-install.yaml
```

Get the Nginx Gateway Class:  
https://docs.nginx.com/nginx-gateway-fabric/install/helm/
```bash
helm install ngf oci://ghcr.io/nginx/charts/nginx-gateway-fabric --create-namespace -n nginx-gateway
```


Use the gateway api -  
```bash
microk8s enable gateway-api
```

```bash
kubectl apply -f gateway.yaml
kubectl apply -f route.yaml
```

Setup nginx gateway to use a nodeport so we can access the node's IP
```bash
microk8s kubectl get svc -n nginx-gateway
microk8s kubectl annotate svc ngf-nginx-gateway-fabric -n nginx-gateway metallb.universe.tf/loadBalancerIPs=10.0.0.60
microk8s kubectl get svc -n nginx-gateway
```


We also want gateways for ArgoCD

```bash
kubectl apply -f argocd_gateway.yaml
kubectl apply -f argocd_route.yaml
```