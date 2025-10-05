# History of ArgoCD
- Created by engineers at applatix
- Open Source
- Actively Contributed by Black rock, red hat, intuit
- Graduate CNCF Project
- Gain Popularity in Github with more than 10k star



# Architecture of Gitops (ArgoCD)

<img width="988" height="400" alt="Image" src="https://github.com/user-attachments/assets/e2353bab-4981-49c4-98b9-b10136e4c9d1" />

If you are trying build a Gitops tools the first thing it has to keep a state of Git, so for that It has something called as **``Repo server``**. This Repo will try to connect to Git and get the state of Git this is one Microservice inside Gitops application that is talking to git, then there has to be another Microservice that should talk to Kubernetes and get the state. this is called as **``application controller``** this application controller is talking to Kubernetes and get the state of Kubernetes.

### Why do you need repo and app contoller
you have to mantain state between Git and Kubernetes, that why there is one Microservice which is getting the state of Git and there is another state which is getting from Kubernetes and afterwards ``application controller`` will get the state from ``Git`` and try to compare so what you get from ``git`` and what do you have from Kubernetes is it same or not.

If it is right everything is right, and If it is not then ``application controller`` will try to sync according to the state from ``repo server`` has got on to Kubernetes cluster. 

There is another component that is **``API server``** which is used by the users to Interract with Argo CD. so you can Use UI or CLI to communicate with Argo CD

Then there is something called as **``Dex``** when you Install Argo CD by default comes up with ``Dex``. Dex is a lightweight OIDC you can consider It as  Provider Proxy server which can connect to any your existing provider, les's say you have Gmail authentication or you have Facebook authentication. you can integrate them with ``Dex`` 

And finnaly you have **``Redis``** for caching. let's say for some reason this application controller goes down once it comes up, It need to have Information till now on the cluster. that why this application is stateful set and this redis is used for caching the Information.