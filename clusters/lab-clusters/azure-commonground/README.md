# Azure

## Installation

```
flux install

flux create source git flux-system \
--url=https://gitlab.com/commonground/haven/havenplus/gitops-flux.git \
--branch=main

flux create kustomization flux-system \
--source=GitRepository/flux-system \
--path=clusters/lab-clusters/azure-commonground/flux-system \
--prune=true
```

## URLs

* SealedSecrets: https://public-key.havenplus-azure.haven.vng.cloud/v1/cert.pem
