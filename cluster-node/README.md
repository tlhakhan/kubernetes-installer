# cluster-node
Bootstraps a Kubernetes control plane and joins worker nodes to the cluster.

## What it does

1. **Pre-flight** — discovers Tailscale IPs on all nodes and asserts tailnet connectivity.
2. **Bootstrap node** — pulls control plane images, initializes the cluster with kubeadm, approves kubelet serving CSRs, installs Cilium, and generates a worker join command.
3. **Worker nodes** — joins each worker to the cluster.
4. **Verification** — asserts all node internal IPs are Tailscale addresses.
5. **Add-ons** — installs Tailscale Operator, ingress-nginx (with `LoadBalancer` service type), and metrics-server via Helm.

Cilium replaces kube-proxy (`kubeProxyReplacement=true`) and uses VXLAN tunnel mode. All API server traffic is bound to the Tailscale interface.

## Configuration

Versions and defaults are in `inventory.yaml`. Override any variable by creating `overrides.yaml` (untracked by git) in this directory.

Required overrides for the Tailscale Operator:

```yaml
tailscale_oauth_client_id: "..."
tailscale_oauth_client_secret: "..."
```

The OAuth client requires **write** scope for General/Services, Devices/Core, and Keys/Auth Keys — all tagged `tag:k8s-operator`.

Optional overrides:

```yaml
cluster_name: "my-cluster"   # defaults to "kubernetes"
```

## Usage

Edit `inventory.yaml` with your bootstrap and worker nodes:

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

Set `controlplane_schedulable: true` on a bootstrap node to allow workload pods to run on it (removes the NoSchedule taint).

Run the playbook:

```bash
./main.yaml
```

> **Note:** All nodes must have been prepared with `sysprep-node` and must be on the same Tailscale tailnet before running this playbook.
