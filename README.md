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

