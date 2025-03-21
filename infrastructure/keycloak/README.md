# keycloak

Reference implementation of [Keycloak](https://keycloak.org/).

This reference implementation deploys [Keycloak](https://www.keycloak.org/getting-started/getting-started-kube). 

## Dependencies
* [Kustomize](https://kustomize.io/)
* an [ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) or a [Gateway API controller](https://gateway-api.sigs.k8s.io/implementations/) like:
  * [ingress-nginx](https://kubernetes.github.io/ingress-nginx/) ([enable-ssl-passthrough](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough) must be enabled)
* A GitOps CD tool like [Argo CD](https://argo-cd.readthedocs.io/en/stable/) or [Flux CD](https://fluxcd.io/)

## Configuration

The fully qualified domain name of your Keycloak instance needs to be configured in the `HelmRelease` and `Ingress` by patching the manifests in `/overlays/local/kustomization.yaml`.

## Installation

Using for example Flux CD you can install the Keycloak implementation using the following `Kustomization`:

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: keycloak
  namespace: flux-system
spec:
  interval: 1h
  sourceRef:
    kind: GitRepository
    name: flux-system
  path: ./apps/keycloak/overlays/local
  prune: true
  wait: true
```