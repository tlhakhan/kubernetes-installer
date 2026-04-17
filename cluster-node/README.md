# cluster-node
Bootstraps a Kubernetes control plane and joins worker nodes to the cluster.

## What it does

1. **Bootstrap node** — pulls control plane images, initializes the cluster with kubeadm, installs Cilium, and generates a worker join command.
2. **Worker nodes** — joins each worker to the cluster using the join command from the bootstrap node.

Cilium defaults to version `1.19.3` and is configured via `main.yaml`.
kubeadm is configured via `templates/kubeadm-init.yaml` (kubeproxy in `nftables` mode).

## Usage

Create an inventory file at `inventory/custom` (untracked by git):

```ini
[bootstrap_node]
kube-0.local

[worker_nodes]
node-1.local
node-2.local
node-3.local
```

Run the playbook:

```bash
./main.yaml
```

> **Note:** All nodes must have been prepared with `sysprep-node` before running this playbook.
