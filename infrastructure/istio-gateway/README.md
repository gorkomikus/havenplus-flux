# istio-gateway

Reference implementation of [Istio Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/) using the [Kubernetes Gateway API](https://istio.io/latest/docs/tasks/traffic-management/ingress/gateway-api/).

## Dependencies
* `istiod` and `instio-base` (Example implementation provided by this implementation) 
* [Gateway API CRDs](https://gateway-api.sigs.k8s.io/guides/#installing-gateway-api) (Experimental Channel is installed with this implementation to support TLSRoute)
* [Kustomize](https://kustomize.io/)
* [Flux CD](https://fluxcd.io/)

## Configuration

Create wildcard certificates and store the certificate and key in the `tls-certs.yaml` and apply some form of secrets encryption.

If you want to use an existing IP address for the Azure loadbalancer, configure the labels `service.beta.kubernetes.io/azure-load-balancer-resource-group` and `service.beta.kubernetes.io/azure-pip-name` in `public-gateway.yaml`. See https://learn.microsoft.com/en-us/azure/aks/static-ip#create-a-service-using-the-static-ip-address for more information.

## Installation

Using for example Flux CD you can install the Istio Gateway implementation using the following `Kustomization`:

```yaml
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: istio
  namespace: flux-system
spec:
  interval: 1h
  sourceRef:
    kind: GitRepository
    name: flux-system
  path: ./infrastructure/istio/overlays/local
  prune: true
  wait: true
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: istio-gateway
  namespace: flux-system
spec:
  interval: 1h
  sourceRef:
    kind: GitRepository
    name: flux-system
  path: ./infrastructure/istio-gateway/overlays/local
  prune: true
  wait: true
```
