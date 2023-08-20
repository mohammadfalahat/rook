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

Quick Start:
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

