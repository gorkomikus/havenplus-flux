# pinniped with ingress-nginx

Pinniped pods run on port 443 which is why an ingress needs to do TLS passthrough or terminate TLS and set up a subsequent
TLS connection to the service using for instance a `ServersTransport` in Traefik or a `BackendTLSPolicy` in Gateway API.
However, ingress-nginx can only be used with TLS passthrough.
