# OpenEBS Setup
[Source Docs](https://canonical.com/microk8s/docs/addon-openebs)

*Note: 3 node minimum, so will revisit once I add another node*  

```bash
microk8s enable openebs
```
(may also need to enable microk8s community)

View CSI drivers
```bash
kubectl get csidrivers
```