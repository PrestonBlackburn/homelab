# Cloudflare tunnels for external access
https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/deployment-guides/kubernetes/
https://github.com/cloudflare/argo-tunnel-examples/tree/master/named-tunnel-k8s  


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
