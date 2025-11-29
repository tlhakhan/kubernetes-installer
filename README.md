# setup-kubernetes
A repository to help setup a Kubernetes cluster.

The `sysprep-node` should be run first, it prepares a clean Ubuntu OS for a Kubernetes installation.
The `cluster-node` then configures and installs a simple Kubernetes cluster.

## example cluster
```
 % k get nodes -o wide
NAME    STATUS   ROLES           AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
k8s-0   Ready    control-plane   6m16s   v1.34.2   192.168.5.189   <none>        Ubuntu 24.04.3 LTS   6.8.0-88-generic   containerd://2.2.0
k8s-1   Ready    <none>          5m37s   v1.34.2   192.168.5.190   <none>        Ubuntu 24.04.3 LTS   6.8.0-88-generic   containerd://2.2.0
k8s-2   Ready    <none>          5m37s   v1.34.2   192.168.5.188   <none>        Ubuntu 24.04.3 LTS   6.8.0-88-generic   containerd://2.2.0
```
