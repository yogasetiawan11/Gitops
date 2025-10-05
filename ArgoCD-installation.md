# Installation
There are 3 ways to do it:
- Yaml Manifest
- Helm
- Operator

# Install with Yaml Manifest
```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

   ---  The output  ---

```bash
kubectl get pods -n argocd
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                     1/1     Running   0          79m
argocd-applicationset-controller-578697b885-kv9s4   1/1     Running   0          79m
argocd-dex-server-95477cdd-cfhkm                    1/1     Running   0          79m
argocd-notifications-controller-787447c77d-xxl6x    1/1     Running   0          79m
argocd-redis-5746c4c5fb-w8lp4                       1/1     Running   0          79m
argocd-repo-server-588c6f4648-llxhl                 1/1     Running   0          79m
argocd-server-656b9b6c6c-6zzj9                      1/1     Running   0          79m
yogaw@DESKTOP-MITEA40:~$
```

# Interact with ArgoCD
First list all service in ArgoCD
```bash
kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.98.4.10       <none>        7000/TCP,8080/TCP            106m
argocd-dex-server                         ClusterIP   10.108.226.180   <none>        5556/TCP,5557/TCP,5558/TCP   106m
argocd-metrics                            ClusterIP   10.104.180.210   <none>        8082/TCP                     106m
argocd-notifications-controller-metrics   ClusterIP   10.102.40.57     <none>        9001/TCP                     106m
argocd-redis                              ClusterIP   10.101.133.59    <none>        6379/TCP                     106m
argocd-repo-server                        ClusterIP   10.99.227.174    <none>        8081/TCP,8084/TCP            106m
argocd-server                             ClusterIP   10.104.139.173   <none>        80/TCP,443/TCP               106m
argocd-server-metrics                     ClusterIP   10.111.108.5     <none>        8083/TCP                     106m
```

Change service of ``argocd-server`` 

```bash
kubectl edit svc argocd-server -n argocd
```

Then change the ``ClusterIP`` to ``NodePort``

# Additional settings for Minikube
Because I'm using Minikube tunnel I have to create a ``tunnel`` just followed these commands

```bash
minikube service list -n argocd
```

The output

```bash
minikube service list -n argocd
|-----------|-----------------------------------------|--------------|-----|
| NAMESPACE |                  NAME                   | TARGET PORT  | URL |
|-----------|-----------------------------------------|--------------|-----|
| argocd    | argocd-applicationset-controller        | No node port |     |
| argocd    | argocd-dex-server                       | No node port |     |
| argocd    | argocd-metrics                          | No node port |     |
| argocd    | argocd-notifications-controller-metrics | No node port |     |
| argocd    | argocd-redis                            | No node port |     |
| argocd    | argocd-repo-server                      | No node port |     |
| argocd    | argocd-server                           | http/80      |     |
|           |                                         | https/443    |     |
| argocd    | argocd-server-metrics                   | No node port |     |
|-----------|-----------------------------------------|--------------|-----|
```

After thet get IP address of the ``argocd-server``
```bash
minikube service argocd-server -n argocd
```

Then you will get the output which contain IP adress for access ArgoCD

# Login to ArgoCD with Admin User
You can fill the user with ``admin`` To Get a password for ArgoCD you have to get the secret first

```bash
kubectl get secret -n argocd
```

The output

```bash
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      119m
argocd-notifications-secret   Opaque   0      123m
argocd-redis                  Opaque   1      119m
argocd-secret                 Opaque   5      123m
```

Then go to ``argocd-initial-admin-secret`` and edit.
```bash
kubectl edit secret argocd-initial-admin-secret -n argocd
```

You will get the secret that encrypted in base64 next is to decode the value inside this secret, example:
```bash
echo YnJNDA1ZmJMQXRXbtest== | base64 --decode
```

And Finally copy the Password