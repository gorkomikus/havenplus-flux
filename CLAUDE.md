# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitOps-based Kubernetes platform using FluxCD, implementing the HavenPlus reference architecture. It manages multiple Kubernetes clusters (local, Azure, Cyso, OVH Cloud, ODC-Noord, and lab clusters) using a declarative GitOps approach.

## Architecture

### Directory Structure

The repository follows a standard FluxCD GitOps layout:

- **`clusters/`**: Cluster-specific configurations for each environment (local, azure, cyso, ovh-cloud, odc-noord, lab-clusters)
  - Each cluster directory contains `flux-system/` and `infrastructure/` subdirectories
  - Cluster-specific Kustomizations reference infrastructure overlays via `path: ./infrastructure/<component>/overlays/<cluster-name>`

- **`infrastructure/`**: Platform infrastructure components, organized by service
  - Each component follows the base/overlays pattern:
    - `controller/base/`: Base HelmRelease, HelmRepository, Namespace, and Kustomization
    - `controller/overlays/<cluster>/`: Cluster-specific patches and configurations
    - `config/`, `instances/`, `policies/`: Additional component-specific structures
  - Components include: cert-manager, cloudnative-pg, kyverno, ingress-nginx, istio, keycloak, grafana, loki, mimir, tempo, alloy, sealed-secrets, external-secrets, velero, trivy-operator, and more

- **`tenants/`**: Multi-tenant configurations
  - Each tenant has: kustomization.yaml, git-repository.yaml, sync.yaml, rbac.yaml
  - Tenants manage their own workloads via separate GitRepositories

- **`scripts/`**: Validation and utility scripts
  - `validate.sh`: Validates Flux resources and kustomize overlays using kubeconform

### Kustomize Overlay Pattern

Infrastructure components use a consistent pattern:
- **Base**: Defines the HelmRelease, HelmRepository, Namespace
- **Overlays**: Per-cluster customizations using Kustomize patches
- Cluster Kustomizations reference: `./infrastructure/<component>/overlays/<cluster-name>`

### FluxCD Resources

Key Flux resource types used:
- **GitRepository**: Source definitions for Git repositories
- **Kustomization**: Defines what to deploy from a GitRepository path
- **HelmRepository**: Defines Helm chart repositories
- **HelmRelease**: Defines Helm chart deployments with values

## Common Development Commands

### Local Development Setup

```bash
# Install required tools (requires asdf)
awk '{ system("asdf plugin add " $1) }' < .tool-versions
asdf install

# Create .env file from example
cp .env.example .env
# Edit .env to set GITLAB_REPO_URL to your fork

# Run local Kind cluster with FluxCD
task run-local

# Access Headlamp UI
task port-forward-headlamp  # Access at http://localhost:4466/
task get-headlamp-token     # Get login token

# Uninstall local cluster
task uninstall
```

### Working with FluxCD

```bash
# Force reconciliation of Git source
flux reconcile source git flux-system

# Check all Flux resources
flux get all

# Watch specific resource types
flux get kustomizations
flux get helmreleases

# Check specific kustomization status
flux get kustomization <name>
```

### Validation and Testing

```bash
# Validate kustomize overlays (CI pipeline)
find ./infrastructure/**/overlays -name "kustomization.yaml" | while read -r kustomization; do
  overlay_dir=$(dirname "$kustomization")
  echo "Validating $overlay_dir"
  kubectl kustomize "$overlay_dir" > /dev/null
done

# Full validation with kubeconform
./scripts/validate.sh
```

### Kubernetes Operations

```bash
# Get LoadBalancer IP (local cluster with cloud-provider-kind)
kubectl get svc ingress-nginx-controller -n ingress-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}'

# Run cloud-provider-kind to expose LoadBalancer services locally
sudo cloud-provider-kind
```

## Important Conventions

### Commit Messages

Follow Conventional Commits specification. Types:
- `feat`: New feature
- `fix`: Bug fix
- `chore`: Maintenance (e.g., dependency updates)
- `docs`: Documentation changes
- `ci`: CI/CD changes
- `refactor`: Code refactoring
- `test`: Test additions or corrections
- `build`: Build system changes
- `perf`: Performance improvements
- `revert`: Revert previous changes
- `style`: Code style changes

Commits are validated via commitlint in CI pipeline.

### Resource Organization

When adding new infrastructure components:
1. Create directory under `infrastructure/<component-name>/`
2. Create `controller/base/` with: namespace.yaml, helm-repository.yaml, helm-release.yaml, kustomization.yaml
3. Create `controller/overlays/<cluster-name>/` for each target cluster
4. Reference in cluster's `infrastructure/kustomization.yaml`
5. Create cluster-specific Kustomization in `clusters/<cluster-name>/infrastructure/<component-name>.yaml`

### Local Development Resource Management

For local development, comment out unused services in `clusters/local/infrastructure/kustomization.yaml` to reduce resource consumption. The local cluster deploys all services by default.

### Multi-Tenancy

Multi-tenancy is available but requires configuration:
- Flux controllers support `--no-cross-namespace-refs` flag (currently disabled)
- Tenants are isolated via RBAC and separate GitRepository sources
- Tenant configurations are applied via `clusters/<cluster>/tenants.yaml`

## Key Technologies

- **FluxCD v2**: GitOps continuous delivery
- **Kustomize**: Kubernetes manifest customization
- **Helm**: Package management for Kubernetes
- **Kind**: Local Kubernetes cluster (development)
- **Kyverno**: Kubernetes policy engine
- **Sealed Secrets**: Encrypted secrets management
- **External Secrets**: External secret store integration
- **CloudNative-PG**: PostgreSQL operator
- **Observability Stack**: Grafana, Loki, Mimir, Tempo, Alloy
- **Security**: Trivy Operator, Falco
- **Service Mesh**: Istio (optional)
- **Storage**: Garage (S3-compatible), NFS provisioner, Velero (backups)
