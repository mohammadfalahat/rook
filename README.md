# rook

Prerequisites:
https://rook.io/docs/rook/v1.12/Getting-Started/Prerequisites/prerequisites

## Install Prerequisites:

```
# Master Node:
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.11.1/cert-manager.yaml
```
```
# All Nodes:
sudo apt-get install -y lvm2
```

# Quick Start:
https://rook.io/docs/rook/latest/Getting-Started/quickstart

## simple rook cluster with kubectl
```
git clone --single-branch --branch master https://github.com/rook/rook.git
cd rook/deploy/examples
kubectl create -f crds.yaml -f common.yaml -f operator.yaml

# To run cluster.yaml we need at least 3 worker node.
# cluster-test.yaml cluster settings for a test environment such as minikube
# cluster-on-pvc.yaml cluster settings for a production cluster running in a dynamic cloud environment.
kubectl create -f cluster.yaml
```

# Ceph Toolbox
https://rook.io/docs/rook/latest/Troubleshooting/ceph-toolbox

```
kubectl create -f toolbox.yaml
kubectl -n rook-ceph rollout status deploy/rook-ceph-tools

# Once the rook-ceph-tools pod is running, you can connect to it with:
#Example:
  # ceph status
  # ceph osd status
  # ceph df
  # rados df
kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash
```

# Ceph Dashboard
https://rook.io/docs/rook/latest/Storage-Configuration/Monitoring/ceph-dashboard

1. deploy ingress nginx controller:
https://kubernetes.github.io/ingress-nginx/deploy
```
# always latest ingress-nginx from main branch:
https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```


2. Create a secret to keep domain certificate inside kubernetes:

```
kubectl create secret tls my-tls-secret --cert=path/to/certificate.crt --key=path/to/private-key.key
```

3. Customize dashboard-ingress-https.yaml and correct hosts and secretname and comment `nginx.ingress.kubernetes.io/server-snippet` configs. then apply.

4. Get Login password from secretfile:

```
kubectl -n rook-ceph get secret rook-ceph-dashboard-password -o jsonpath="{['data']['password']}" | base64 --decode && echo
```

# Add a disk to a worker

```
# example: add a disk for worker3
# we have to delete same worker crashcollector pod
# this make rook ceph to reload the pod and rescan the node.
kubectl delete pod --force -n rook-ceph rook-ceph-crashcollector-worker3-5844474bfb-5qzpt
```

# RADOS Gateway (Object Storage S3)
https://rook.io/docs/rook/v1.12/Storage-Configuration/Object-Storage-RGW/object-storage

# Cleanup

Follow steps here: https://rook.io/docs/rook/v1.12/Getting-Started/ceph-teardown

If your bucket crds stuck on terminating, edit crd's with this command:
```
kubectl edit crd <crd-name>
```
Find the finalizer section and delete it.

Then go to rook deploy examples directory and run this command to check everything is deleted successfully.
if this code stuck it means that something was wrong while your cleanup.
```
# Find all .yaml files in the current directory
cd ./rook/deploy/examples
for FILE in *.yaml; do
  if [ -f "$FILE" ]; then
    # Run kubectl delete on each .yaml file
    kubectl delete --force -f "$FILE" --ignore-not-found
  fi
done
```
