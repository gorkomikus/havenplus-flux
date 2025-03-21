# Tenants

## Creating a new tenant

```bash
export TENANT_NAME=dev-team
export CLUSTER_NAME=example

mkdir "./${CLUSTER_NAME}/${TENANT_NAME}"
cd "./${CLUSTER_NAME}/${TENANT_NAME}"

flux create tenant "${TENANT_NAME}" \
  --with-namespace="${TENANT_NAME}" \
  --export > rbac.yaml

flux create kustomization "${TENANT_NAME}" \
    --source="GitRepository/${TENANT_NAME}" \
    --path="./overlays/${CLUSTER_NAME}" \
    --prune=true \
    --interval=10m \
    --wait=true \
    --export > sync.yaml

kustomize create \
  --namespace "${TENANT_NAME}" \
  --resources rbac.yaml,sync.yaml
```
