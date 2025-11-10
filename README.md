# ArgoCD - GitOps-based Continuous Delivery

ArgoCD is a declarative GitOps-based Continuous Delivery tool for Kubernetes.  
It continuously monitors the desired application state defined in a Git repository and automatically syncs it to a Kubernetes cluster, ensuring the live state always matches the Git-defined desired state.

## Why ArgoCD?
- GitOps approach for Kubernetes deployment
- Automatic sync from Git to Cluster
- Easy Rollbacks & Version History
- Real-time application health status
- Supports Helm, Kustomize, Kubernetes Manifests, Jsonnet, etc.
- UI Dashboard for deployment visibility

---

## Installation (Quick Setup)

> Prerequisites: Kubernetes cluster + kubectl configured
```bash
# Download the latest version
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64


# Create ArgoCD namespace
kubectl create namespace argocd

# Install ArgoCD components
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Default Username and password
kubectl port-forward svc/argocd-server -n argocd 8080:443


#Port Forwarding
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
The API server can then be accessed using https://localhost:8080

Now, create a new application and fill in the credentials like GitHub repo URL, Namespace, sync policy, Branch name (default is HEAD), etc, and click on "Create".



