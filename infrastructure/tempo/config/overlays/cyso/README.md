# Object Storage

Tempo needs 1 Object Storage container. The given Tempo config relies on a container named `tempo`. However, you can rename it according to your needs and change the reference to the bucket when creating the required secret (see steps below). 
  
For guidance on creating this, please refer to the instructions provided by [Cyso]((https://cyso.cloud/docs/object-storage/getting-started/)) here.

## S3 Credentials Secret
Look-up the EC2 / S3 credentials in the Cyso dashboard and use these to create a `secret.yaml`:

```
apiVersion: v1
kind: Secret
metadata:
  name: s3-credentials
stringData:
  endpoint: core.fuga.cloud:8080
  bucket: tempo
  access_key_id: <access-key-id>
  access_key_secret: <secret-access-key>
type: Opaque
```

Next, use `kubeseal` to generate a sealed-secret based on the secret above:

```
kubeseal -f secret.yaml -w sealed.yaml  --controller-namespace sealed-secrets -n tempo
```

An entry for `sealed.yaml` has already been preconfigured in `kustomization.yaml`, you just need to uncomment it. Furthermore, ensure that `sealed.yaml` is placed in the same folder as the `kustomization.yaml`. 

> Don't forget to remove the secret.yaml and definately do NOT commit the actual secret.yaml to Git! 
The [TempoStack custom resource](./tempo.yaml#L11) refers to the secret.

# Data Retention
By default Tempo is configured to retain data for 3 days. You can adjust this to your organisation's needs [here](./tempo.yaml#L8)
