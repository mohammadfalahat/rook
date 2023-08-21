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


3. Create a secret to keep domain certificate inside kubernetes:

```
kubectl create secret tls my-tls-secret --cert=path/to/certificate.crt --key=path/to/private-key.key
```

3. Customize dashboard-ingress-https.yaml and correct hosts and secretname
4. 
