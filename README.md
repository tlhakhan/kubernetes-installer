# kubernetes-installer
Ansible playbooks to prepare and install a Kubernetes cluster on Ubuntu over Tailscale.

## Overview

Run the playbooks in order:

1. **`sysprep-node`** — prepares a clean Ubuntu node: installs runc, containerd, CNI plugins, crictl, kubeadm, kubelet, kubectl, Helm, Cilium CLI, and etcd tools. Optionally enables Tailscale.
2. **`cluster-node`** — bootstraps the control plane with kubeadm, installs Cilium, joins worker nodes, and installs cluster add-ons (Tailscale Operator, ingress-nginx, metrics-server).

All cluster networking is over Tailscale — nodes must be on the same tailnet before running `cluster-node`.

## Component Versions

### sysprep-node (`sysprep-node/inventory.yaml`)

| Component | Version |
|---|---|
| Kubernetes | 1.34 |
| containerd | 2.3.2 |
| runc | 1.5.0 |
| CNI plugins | 1.9.1 |
| crictl | 1.36.0 |
| Helm | 4.2.2 |
| Cilium CLI | 0.19.5 |
| etcd | 3.6.12 |

### cluster-node (`cluster-node/inventory.yaml`)

| Component | Version |
|---|---|
| Kubernetes | 1.34 |
| Cilium | 1.19.3 |
| Tailscale Operator | 1.84.0 |
| ingress-nginx | 4.12.3 |
| metrics-server | 3.12.2 |

## Usage

### 1. sysprep-node

Edit `sysprep-node/inventory.yaml` with your target hosts:

```yaml
  children:
    kubernetes_nodes:
      hosts:
        kube-1.local: {}
        kube-2.local: {}
```

Optionally create `sysprep-node/overrides.yaml` (untracked) to set Tailscale and Docker Hub credentials:

```yaml
tailscale_auth_key: "tskey-auth-..."
docker_hub_user: "myuser"
docker_hub_token: "mytoken"
```

Run the playbook:

```bash
cd sysprep-node
./main.yaml
```

### 2. cluster-node

Edit `cluster-node/inventory.yaml` with your bootstrap and worker nodes:

```yaml
  children:
    bootstrap_node:
      hosts:
        kube-1.local:
          controlplane_schedulable: true
    worker_nodes:
      hosts:
        kube-2.local: {}
```

Create `cluster-node/overrides.yaml` (untracked) with your Tailscale OAuth credentials:

```yaml
tailscale_oauth_client_id: "..."
tailscale_oauth_client_secret: "..."
```

The OAuth client requires **write** scope for General/Services, Devices/Core, and Keys/Auth Keys — all tagged `tag:k8s-operator`.

Run the playbook:

```bash
cd cluster-node
./main.yaml
```
