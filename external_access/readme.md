# Cloudflare tunnels for external access
[Cloudflare Tunnel K8s Docs](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/deployment-guides/kubernetes/)  
[Code Examples](https://github.com/cloudflare/argo-tunnel-examples/tree/master/named-tunnel-k8s)  

## Create a tunnel

Namespace for tunnel:
```bash
kubectl create namespace cloudflared
```

Create Secret example:
```bash
kubectl create secret generic tunnel-credentials --from-file=credentials.json=./credentials.json -n cloudflared
```
Copy over cert as well
```bash
kubectl create secret generic cloudflared-cert --from-file=cert.pem=./cert.pem -n cloudflared
```

Deploy the tunnel
```bash
kubectl apply -f cloudflared-configmap.yaml
kubectl apply -f tunnel.yaml
```

### Update a tunnel
Modify the config - `cloudflared-configmap.yaml` and redeploy -
```bash
kubectl apply -f cloudflared-configmap.yaml
kubectl rollout restart deployment cloudflared-deployment -n cloudflared
```

Update cloudflare DNS
```bash
cloudflared tunnel login
cloudflared tunnel route dns preston-personal-site chirp.prestonblackburn.com
``` 

Verify that new DNS record shows up in Cloudflare UI