# Velero

> Velero (formerly Heptio Ark) gives you tools to back up and restore your Kubernetes cluster resources and persistent volumes. 

## Velero on the local environment 
The Velero controller automatically connects with a pre-created bucket in the local MinIO installation. A backup [Schedule](./config/base/schedules/tenant-demo.yaml) is deployed as well. 

### Validate the backup / restore process
Follow the [basic example](https://velero.io/docs/v1.16/examples/#basic-example-without-persistentvolumes) from the Velero docs to validate the backup / restore process. Be aware that the second example (csi snapshot) does not work on the local Kind environment. 
  
To verify the actual backup data, open a port-forward to the MinIO console:
```
kubectl -n minio-generic-tenant port-forward svc/minio-generic-tenant-console 9090:9090
```

Browse to http://localhost:9090 and login with `minio/minio123`. You should see a `velero` bucket with data in it. 