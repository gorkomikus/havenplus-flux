# Azure

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
