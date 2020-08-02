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

## Create Cluster Object

- Export your gcloud service account to adc.json.

```
ABSOLUTE_PATH=`pwd`
export GOOGLE_APPLICATION_CREDENTIALS="${ABSOLUTE_PATH}/gcp/adc.json"
```

- Creates the Cluster object and InstanceGroup object you'll be working with in kops

```
PROJECT=`gcloud config get-value project`
export KOPS_FEATURE_FLAGS=AlphaAllowGCE
kops create cluster simple.k8s.local --zones asia-southeast1-a --state ${KOPS_STATE_STORE}/ --project=${PROJECT}
```

- Get detail of your cluster

```
kops get cluster --state ${KOPS_STATE_STORE}/ simple.k8s.local -oyaml
```

- Get Instance Group

```
kops get instancegroup --state ${KOPS_STATE_STORE}/ --name simple.k8s.local
```

## Create Cluster

- Created the Cluster object & InstanceGroup object in our state store, but didn't actually create any instances or other cloud objects in GCE. To do that, we'll use

```
kops update cluster simple.k8s.local
```

## Validate Cluster

- Test the cluster nodes to see if all are ready

```
kops validate cluster simple.k8s.local
```

- List node

```
kubectl get nodes --show-labels
```

## Deleting the cluster

```
kops delete cluster simple.k8s.local --yes
```