# sysprep-node
Prepares a clean Ubuntu node for Kubernetes installation.

## What it does

| Task | Description |
|---|---|
| `00_sysprep` | Disables swap, loads kernel modules (`overlay`, `br_netfilter`), sets sysctl parameters |
| `01_runc` | Installs runc |
| `02_containerd` | Installs containerd, configures it with systemd cgroup driver, installs crictl |
| `03_cni_plugins` | Installs CNI plugins |
| `04_kubernetes` | Adds the Kubernetes apt repo, installs and holds kubeadm, kubelet, kubectl |
| `05_cilium` | Installs the Cilium CLI |
| `06_etcd_tools` | Installs etcdctl and etcdutl |
| `07_helm` | Installs the Helm CLI |
| `08_tailscale` | Installs and optionally enables Tailscale |

## Versions

Defined in `inventory.yaml`:

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

## Usage

Edit `inventory.yaml` with your target hosts:

```yaml
  children:
    kubernetes_nodes:
      hosts:
        kube-1.local: {}
        kube-2.local: {}
```

Optionally create `overrides.yaml` (untracked by git) to set Tailscale and Docker Hub credentials:

```yaml
tailscale_auth_key: "tskey-auth-..."   # enables and authenticates Tailscale
docker_hub_user: "myuser"              # avoids Docker Hub pull rate limits
docker_hub_token: "mytoken"
```

Run the playbook:

```bash
./main.yaml
```
