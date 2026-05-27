# NVIDIA GPU Setup

Make sure drivers are installed:  
[Ubuntu Docs](https://ubuntu.com/server/docs/how-to/graphics/install-nvidia-drivers/)  

Also need to install nvidia-smi utils
```bash
sudo apt install nvidia-utils-595-server
```

Check driver is working with:
```bash
nvidia-smi
```

Create the namespace for the nvidia gpu operator
```bash
kubectl create namespace gpu-operator
```

```bash
sudo microk8s enable gpu-integration
```

Label the nodes for the time-slice config
```bash
kubectl apply -f time-slice-config.yaml
kubectl label node <node-name> nvidia.com/device-plugin.config=rtx-2070
```

Deploy the operator (make sure that values.yaml exists in repo first)
```bash
kubectl apply -f nvidia-gpu-operator.yaml
```

Verify Install
```bash
kubectl describe node <node-name>
```
