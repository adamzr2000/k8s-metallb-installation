# Enable MetalLB in Kubernetes

This guide shows how to install and configure [MetalLB](https://metallb.universe.tf/), a load-balancer implementation for bare metal Kubernetes clusters.

## What is MetalLB?

MetalLB provides external IPs for services in environments without a cloud provider. It assigns IPs from a specified range and handles traffic using standard protocols (Layer 2 or BGP).

## Installation Steps

1. **Install MetalLB components:**
   Apply the official MetalLB manifests to create the required namespace and controller components.
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/namespace.yaml
   kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/metallb.yaml
   ```

2. **Create MetalLB ConfigMap:**
   Define the IP address pool MetalLB will use. Replace the example range with one from your local network.
   ```yaml
   # config.yaml
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
         - 192.168.0.100-192.168.0.200
   ```

3. **Apply the configuration:**
   ```bash
   kubectl apply -f config.yaml
   ```

4. **Verify the installation:**
   Ensure the MetalLB components are running correctly.
   ```bash
   kubectl get pods -n metallb-system
   ```

Once configured, any `LoadBalancer`-type service will be assigned an external IP from the defined range.
