# Kubernetes in Docker
Prerequisite: install docker


install kind
```
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
Install kubectl binary with curl on Linux
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```
KinD commands
```
kind version
kind create cluster --name single-node-k8s
kind get clusters
kubectl config current-context
kubectl config view
kubectl config use-context kind-single-node-cluster
kubectl get nodes
kubectl get sa
kubectl get sa -n kube-system
kubectl create deployment nginx --image nginx
kubectl get pods -w
vim multi-node-k8s-cluster.yaml
kind create cluster --name 9-node-k8s --config=multi-node-k8s-cluster.yaml
docker ps | grep -i 9-node-k8s

Expose the application: kubectl port-forward
kubectl create deployment nginx --image nginx
kubectl get pods -w
kubectl port-forward pod/pod-name  8989:80
kubectl port-forward svc/argocd-server  9000:80 -n argocd [local pc]
kubectl port-forward svc/argocd-server  9000:80 -n argocd --address 0.0.0.0 [cloud VM]
access from browser
localhost:8989
```
Run `kubectl get pods` to verify the Pods are ready and running.

Run `kubectl port-forward deployment/frontend 8080:8080` to forward a port to the frontend service.

Navigate to `localhost:8080` to access the web frontend.

You can create a multi node cluster with the following config:
```
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

```
Reference:
1. <https://kind.sigs.k8s.io/>
2. <https://kind.sigs.k8s.io/docs/user/quick-start/#installation>
