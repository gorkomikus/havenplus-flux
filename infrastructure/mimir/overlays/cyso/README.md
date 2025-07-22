# Object Storage

Mimir needs 3 Object Storage containers:
- mimir-alertmanager
- mimir-blocks
- mimir-ruler

For guidance on creating these, please refer to the instructions provided by [Cyso]((https://cyso.cloud/docs/object-storage/getting-started/)) here.

## S3 Credentials Secret
Look-up the EC2 / S3 credentials in the Cyso dashboard and use these to create a `secret.yaml`:

```
apiVersion: v1
kind: Secret
metadata:
  name: s3-credentials
type: Opaque
data:
  S3_SECRET_ACCESS_KEY: <base64-encoded-secret-access-key>
  S3_ACCESS_KEY_ID: <base64-encoded-access-key-id>
```

Next, use `kubeseal` to generate a sealed-secret based on the secret above:

```
kubeseal -f secret.yaml -w sealed.yaml  --controller-namespace sealed-secrets -n mimir
```

An entry for `sealed.yaml` has already been preconfigured in `kustomization.yaml`, you just need to uncomment it. Furthermore, ensure that `sealed.yaml` is placed in the same folder as the `kustomization.yaml`. 

> Don't forget to remove the secret.yaml and definately do NOT commit the actual secret.yaml to Git! 

The [helmrelease patch](infrastructure/mimir/overlays/cyso/patches/helmrelease.yaml#L13) refers to the secret.

# Data Retention
By default Mimir is configured to retain data for 7 days. You can adjust this to your organisation's needs [here](infrastructure/mimir/overlays/cyso/patches/helmrelease.yaml#L18)