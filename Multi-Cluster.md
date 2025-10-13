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

# 1. Create a Kind Cluster
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

# 2. Install ArgoCD on the Cluster
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

## "Note"
This demo I demostrate in dev environment only (http), If you want use Argocd for production environment or in organization you should change to ``https`` where you have to sign up for ``certificate authority``, you have to ``self signed certificate`` and you have to use that ``self signed certificate`` with Argocd API server then you can run Argocd in secure way.

## Change your Argocd to insecure
1. List all Config Map
```bash
kubectl get cm -n argocd
```
2. edit ```argocd-cmd-params-cm```
```bash
kubectl edit configmap argocd-cmd-params-cm -n argocd
```
3. then attach ``particular data entry`` just to run argocd in insecure mode
```bash
  creationTimestamp: "2025-10-12T14:54:17Z"
  labels:
    app.kubernetes.io/name: argocd-cmd-params-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-cmd-params-cm
  namespace: argocd
  resourceVersion: "62981"
  uid: 75898fb5-4ec3-4273-8b51-65384bc0748b
data:         # here you put the data entry
  server.insecure: "true"
```
you can refers to documentation to information like this [Here](https://github.com/argoproj/argo-cd/blob/master/docs/operator-manual/argocd-cmd-params-cm.yaml#L7)

4. Verify if the configmap has configured
```bash
kubectl describe deploy/argocd-server -n argocd
```
```bash
kubectl edit deploy/argo-server -n argocd
```

5. Change the ClusterIP to Nodeport
```bash
kubectl edit svc argocd-server -n argocd
```

# 3. Log in to ArgoCD
1. Expose argocd from localhost. In this case Im using KIND, this method can vary of different cluster 
```bash
kubectl port-forward svc/argocd-server -n argocd 8081:443
```

2. To log in to the ArgoCD UI, you'll need the default admin password. You can retrieve it from the Kubernetes cluster.
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```
Use the admin username and the retrieved password to log in.

[refers this documentation to use argocd further more](https://argo-cd.readthedocs.io/en/latest/getting_started/)

# 4. Add Cluster
1. Go to setting 
2. Go to Clusters

<img width="1142" height="319" alt="Image" src="https://github.com/user-attachments/assets/db998f01-340b-4713-a9da-4c4440d5d299" />

In Clusters you can see what are the clusters that argocd can interact with

3. Install Argocd and CLI 
By default argocd doesn't support adding new resources through UI but it only support to delete that cluster. to doing that you have to go with CLI.
- install including CLI on linux  (you can refers to this docs to install CLI)[https://argo-cd.readthedocs.io/en/stable/cli_installation/]
```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

3. Add your Cluster to Argocd
This is the command to create the cluster
```bash
argocd cluster add 
```
Same command with addition parameter.
```bash
argocd cluster add <cluster-name>
```
this command I pass the context in the cubeconfig of the cluster that you want to add.

``To check the context use this command``
```bash
kubectl config get-contexts
```

you will see cluster, then you can pick up one cluster.
```bash
argocd cluster add kind-test-argocd-cluster --server <ip-address-ui/port>
```
server means you tell your cluster should connect to particular argocd by using IP address followed by port

4. Login trough CLI
after you execute that command maybe you'll get an error. If so you have to login to argocd using CLI 
- Login to argocd with CLI
```bash
argocd login <argocd-IP/port>
```

then you have to fill username ``(admin)`` and password from ``decode base64``