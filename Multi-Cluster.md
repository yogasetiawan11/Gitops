# Multi Cluster


<img width="835" height="832" alt="Image" src="https://github.com/user-attachments/assets/19a96132-7ace-40ce-b23c-bf22cc06fec2" />
Update Linux before installation
```bash
sudo apt update
```

## essensial command for kubectl
```bash
# List all cluster on vm
kubectl config get-contexts

# Print current cluster
kubectl config current-context

# Login to specific cluster
kubectl config use-context <name-cluster>
```

## Create a Kind Cluster
Once Kind is installed, create a new Kubernetes cluster with:
```bash
kind create cluster --name argocd-cluster
```

## Set Up kubectl to Use the Kind Cluster
After creating the cluster, set kubectl to use your new kind cluster:
```bash
kubectl cluster-info --context kind-argocd-cluster
```
This command verifies that kubectl is pointed to the right cluster.

## Install ArgoCD on the Cluster
You can now install Argo CD on your kind cluster. First, apply the Argo CD manifest to create the necessary resources:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Expose ArgoCD API Server
By default, Argo CD's API server is not exposed outside the cluster. You need to expose it to access the UI locally. For development purposes, you can use Kubectl 'port-forward'.
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
This will forward port 8080 on your local machine to the ArgoCD API server’s port 443 inside the Kubernetes cluster.

" If you have some error when use port 8080 just change with another port "

## Access ArgoCD UI
Now, you can open your browser and navigate to http://localhost:8080 to access the ArgoCD UI.

## Note
This demo I demostrate in dev environment only (http), If you want use Argocd for production environment or in organization you should change to ``https`` where you have to sign up for ``certificate authority``, you have to ``self signed certificate`` and you have to use that ``self signed certificate`` with Argocd API server then you can run Argocd in secure way.

## Log in to ArgoCD
To log in to the ArgoCD UI, you'll need the default admin password. You can retrieve it from the Kubernetes cluster.
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```
Use the admin username and the retrieved password to log in.

```bash

```
```bash

```

[refers this documentation to use argocd further more](https://argo-cd.readthedocs.io/en/latest/getting_started/)