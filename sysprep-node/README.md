# sysprep-node
Prepares a clean Ubuntu node for Kubernetes installation.

## What it does

Runs the following tasks in order:

| Task | Description |
|---|---|
| `00_sysprep` | Disables swap, loads kernel modules (`overlay`, `br_netfilter`), sets sysctl parameters |
| `01_runc` | Installs runc |
| `02_containerd` | Installs containerd, configures it with systemd cgroup driver, installs crictl |
| `03_cni_plugins` | Installs CNI plugins |
| `04_kubernetes` | Adds the Kubernetes apt repo, installs and holds kubeadm, kubelet, kubectl |
| `05_cilium` | Installs the Cilium CLI |

## Versions

Defined in `vars.yaml`:

| Component | Version |
|---|---|
| Kubernetes | 1.35 |
| containerd | 2.2.3 |
| runc | 1.4.2 |
| CNI plugins | 1.9.1 |
| crictl | 1.35.0 |
| Cilium CLI | 0.19.2 |

## Usage

Create an inventory file at `inventory/custom` (untracked by git):

```ini
[kubernetes_nodes]
node-0.local
node-1.local
node-2.local
```

Optionally create `overrides.yaml` (untracked by git) to set Docker Hub credentials and avoid pull rate limits:

```yaml
docker_hub_user: "myuser"
docker_hub_token: "mytoken"
```

Run the playbook:

```bash
./main.yaml
```
