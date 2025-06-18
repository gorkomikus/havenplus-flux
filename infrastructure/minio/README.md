# MinIO
> MinIO is a Kubernetes-native high performance object store with an S3-compatible API. 

Within this reference implementation, MinIO is primarily used for the `local` overlay, where it provides S3 compatible object-storage for e.g. Velero, Loki, etc. On actual IaaS-providers, you should probably use S3 compatible Object Storage of the provider itself. 

## MinIO Operator
The [MinIO-Operator](https://min.io/docs/minio/kubernetes/upstream/operations/installation.html) is used to provision one or more tenants within the cluster. Each MinIO tenant gets it's own segregated MinIO installation. 

### MinIO in the local overlay
For the [local overlay](./config/overlays/local/patches/helm-release.yaml), we only create one generic tenant which can be used for all components/services which need S3 object storage; the required buckets are created through the tenant resource itself. In case you need additional buckets, you can add these in the local overlay patch.