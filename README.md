# kubernetes-installer
Ansible playbooks to prepare and install a Kubernetes cluster on Ubuntu.

## Overview

Run the playbooks in order:

1. **`sysprep-node`** — prepares a clean Ubuntu node: installs runc, containerd, CNI plugins, crictl, kubeadm, kubelet, kubectl, and the Cilium CLI.
2. **`cluster-node`** — bootstraps the control plane with kubeadm, installs Cilium, and joins worker nodes to the cluster.

## Component Versions (sysprep-node/vars.yaml)

| Component | Version |
|---|---|
| Kubernetes | 1.35 |
| containerd | 2.2.3 |
| runc | 1.4.2 |
| CNI plugins | 1.9.1 |
| crictl | 1.35.0 |
| Cilium CLI | 0.19.2 |

Cilium itself is installed by `cluster-node` and defaults to version `1.19.3`.

## Usage

### 1. sysprep-node

Edit `sysprep-node/inventory/custom` (untracked) with your target hosts:

```ini
[kubernetes_nodes]
node-0.local
node-1.local
node-2.local
```

Optionally set Docker Hub credentials in `sysprep-node/overrides.yaml` to avoid pull rate limits:

```yaml
docker_hub_user: "myuser"
docker_hub_token: "mytoken"
```

Run the playbook:

```bash
cd sysprep-node
./main.yaml
```

### 2. cluster-node

Edit `cluster-node/inventory/custom` (untracked) with your bootstrap and worker nodes:

```ini
[bootstrap_node]
kube-0.local

[worker_nodes]
node-1.local
node-2.local
```

Run the playbook:

```bash
cd cluster-node
./main.yaml
```
