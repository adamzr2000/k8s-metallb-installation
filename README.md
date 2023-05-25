# Active MetalLB in Kubernetes

This repo explains how to activate [MetalLB](https://metallb.universe.tf/)MetalLB in a Kubernetes cluster created with kubeadm.

1. Install MetalLB: Begin by installing MetalLB on your cluster. You can use *kubectl* to apply the MetalLB manifest from its GitHub repository
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/metallb.yaml
```
2. Configure MetalLB: After installing MetalLB, you need to configure it with the appropriate address pool. Create a *config.yaml* file using your preferred text editor:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - <YOUR_DESIRED_IP_RANGE>
```
Replace *<YOUR_DESIRED_IP_RANGE>* with the IP range you want MetalLB to assign services from. For example, you can use a range within your local network, such as *192.168.0.100-192.168.0.200*. Save the file.
3. Apply the configuration:
```bash
kubectl apply -f config.yaml
```
4. Verify installation: Check the installation and configuration by running the following command:
```bash
kubectl get pods -n metallb-system
```
