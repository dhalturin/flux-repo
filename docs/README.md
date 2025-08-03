# flux-repo Taskfile

This repository uses [Taskfile](https://taskfile.dev) to automate cluster management and secret handling for k0s and Flux.

## Environment Variables

- **KEY_NAME** — GPG key name for SOPS (default: `cluster.local`)
- **KEY_COMMENT** — GPG key comment (default: `flux secrets`)
- **KUBECONFIG** — Path to kubeconfig (default: `~/.kube/test-stand.yaml`)
- **KUBENODES** — List of cluster nodes in the format: `name;ip;[taints]`

## Tasks

### 1. `cluster-init`
Initializes a k0s cluster:
- Generates a k0sctl config from `KUBENODES`
- Applies the cluster configuration
- Retrieves kubeconfig
- Applies node taints if specified

### 2. `cluster-cleanup`
Cleans up the k0s cluster:
- Resets the cluster using k0sctl
- Stops and resets k0s on each node, removes related files, and reboots nodes

### 3. `bootstrap`
Bootstraps Flux on GitHub:
- Initializes Flux with the specified owner, repository, branch, and path

### 4. `sops-generate`
Generates a GPG key for SOPS:
- Creates a new GPG key (no password) for secret encryption

### 5. `sops-export`
Exports the SOPS GPG key:
- Creates a `.sops.yaml` config for encryption rules
- Exports the public key to `.sops.pub.asc`
- Exports the private key and creates a Kubernetes secret in the `flux-system` namespace

### 6. `secret-test`
Tests SOPS secret encryption:
- Creates a test Kubernetes secret (`basic-auth`)
- Encrypts it with SOPS and saves it to `infra/base/env/secret.yaml`

## Usage

Run any task with:
```sh
task <task-name>
```
Example:
```sh
task cluster-init
task sops-generate
task bootstrap
task sops-export
task secret-test

```

## Requirements

- [Task](https://taskfile.dev)
- [k0sctl](https://github.com/k0sproject/k0sctl)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [flux](https://fluxcd.io/)
- [gpg](https://gnupg.org/)
- [sops](https://github.com/mozilla/sops)

---

See Taskfile.yaml for full details.
