# Cyso

```
flux install

flux create source git flux-system \
--url=https://gitlab.com/commonground/haven/havenplus/gitops-flux.git \
--branch=cyso

flux create kustomization bootstrap \
--source=GitRepository/flux-system \
--path=clusters/cyso/flux-system
```
