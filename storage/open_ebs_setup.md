# OpenEBS Setup
[Source Docs](https://canonical.com/microk8s/docs/addon-openebs)


```bash
microk8s enable openebs
```
(may also need to enable microk8s community)

View CSI drivers
```bash
kubectl get csidrivers
```