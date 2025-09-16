# OVHcloud

```
flux install

flux create source git flux-system \
--url=https://gitlab.com/commonground/haven/havenplus/gitops-flux.git \
--branch=main

flux create kustomization bootstrap \
--source=GitRepository/flux-system \
--path=clusters/ovh-cloud/flux-system
```
