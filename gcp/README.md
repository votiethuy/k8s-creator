# Started with kops on GCE

## Dependency

- kops
- kubectl
- gcloud tool

- gsutil

```
curl https://sdk.cloud.google.com | bash
```

## Creating a state store

- Authen gcloud

```
gcloud init
```

- Create an empty bucket

```
gsutil mb -l asia gs://tel4vn-kubernetes-clusters/
```

- export the KOPS_STATE_STORE environment variable

```
export KOPS_STATE_STORE=gs://tel4vn-kubernetes-clusters/
```

## Create Cluster

- Creates the Cluster object and InstanceGroup object you'll be working with in kops

```
PROJECT=`gcloud config get-value project`
export KOPS_FEATURE_FLAGS=AlphaAllowGCE
kops create cluster simple.k8s.local --zones asia-southeast1-a --state ${KOPS_STATE_STORE}/ --project=${PROJECT}
```