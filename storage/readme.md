# NFS Setup
We want to expose storage from one node to other nodes.  
[Setup Docs](https://canonical.com/microk8s/docs/how-to-nfs)   

Setup a dedicated mount directory
```bash
sudo mkdir -p /mnt/storage/k8s-nfs
# Give the cluster nodes full read/write access
sudo chown -R nobody:nogroup /mnt/storage/k8s-nfs
sudo chmod 777 /mnt/storage/k8s-nfs
```

Install NFS
https://ubuntu.com/server/docs/how-to/networking/install-nfs/
```bash
sudo apt update
sudo apt install nfs-kernel-server
```

Configure the directories to be exported 
```bash
sudo vim /etc/exports
```


```txt
/mnt/storage/k8s-nfs  192.168.1.0/24(rw,sync,no_subtree_check,no_root_squash)
```

Apply changes:
```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

Enable NFS support on a client system  
```bash
sudo apt install nfs-common
```

Add the NFS CSI to microk8s
```bash
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts
helm repo update
helm install csi-driver-nfs csi-driver-nfs/csi-driver-nfs \
    --namespace kube-system \
    --set kubeletDir=/var/snap/microk8s/common/var/lib/kubelet
```

Wait for new pods to come up
```bash
kubectl --namespace=kube-system get pods --selector="app.kubernetes.io/instance=csi-driver-nfs" --watch
```

View CSI drivers
```bash
kubectl get csidrivers
```

Deploy storage class:
```bash
kubectl apply -f sc_nfs.yaml
```

Test with a new PVC
```bash
kubectl apply -f pvc_nfs.yaml
kubectl describe pvc test-nfs-claim
```


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