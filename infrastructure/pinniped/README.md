# pinniped

Reference implementation of [Pinniped](https://pinniped.dev/).

This reference implementation deploys [Pinniped Concierge](https://pinniped.dev/docs/howto/install-concierge/) and uses [JWT Authentication](https://pinniped.dev/docs/howto/concierge/configure-concierge-jwt/) using an existing (not included) OIDC provider (without Pinniped Supervisor). 
For a better understanding of the architecture, see https://pinniped.dev/docs/background/architecture/.

Pinniped is configured to use the Impersonation Proxy with a `ClusterIP` to prevent a new `LoadBalancer` to be provisioned, see https://pinniped.dev/docs/reference/supported-clusters/.
This does require the ingress controller or Gateway API controller to passthrough SSL which is configured in this implementation.

## Dependencies
* An OIDC provider like [Dex](https://github.com/dexidp/dex), [authentik](https://goauthentik.io/) or [Pinniped Supervisor](https://pinniped.dev/docs/howto/install-supervisor/) 
* [Kustomize](https://kustomize.io/)
* a [Gateway API controller](https://gateway-api.sigs.k8s.io/implementations/) like:
  * [Istio Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/)
* [Flux CD](https://fluxcd.io/)

## Configuration

The fully qualified domain name of your Pinniped instance needs to be configured in the `HelmRelease` and `Ingress` by patching the manifests in `/overlays/local/kustomization.yaml`.
The configuration of your OIDC provider needs to be configured in the `JWTAuthenticator` in `infrastructure/pinniped/config/overlays/local/patches/jwt-authenticator.yaml`.

## Installation

Using for example Flux CD you can install the Pinniped implementation using the following `Kustomization`:

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: pinniped
  namespace: flux-system
spec:
  interval: 1h
  sourceRef:
    kind: GitRepository
    name: flux-system
  path: ./infrastructure/pinniped/controller/overlays/local
  prune: true
  wait: true
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: pinniped-config
  namespace: flux-system
spec:
  interval: 1h
  sourceRef:
    kind: GitRepository
    name: flux-system
  path: ./infrastructure/pinniped/config/overlays/local
  prune: true
  dependsOn:
    - name: pinniped
```
