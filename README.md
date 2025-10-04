# What is Gitops
Gitops uses Git as a single source of truth to deliver Applications and Infrastructure or from Infrastructure to Infrastructure.


<img width="1018" height="505" alt="Image" src="https://github.com/user-attachments/assets/f7f3199b-5dc1-4342-8644-6793ec6e6d0c" />

example: In this case we are using Gitops tools (Argo CD or Flux) If you're updating the node configuration, probably adding something. Using Gitops way you Put yaml manifest in Git repository then there is a Devops who submit pull request to this repo once this has approved the yaml will change and there is a Gitops tools that picks this change and deploy to Kubernetes cluster.
so Gitops solving a problem where you don't have a track of your deployment, Gitops isn't just about ``Application delivery``
Gitops is also about your ``Infrastructure delivery`` as well. 

when you have hundred of Kubernetes clusters the Infrastructure management become more critical than management Application, when you have hundred Kubernetes cluster It becomes tousand of resources so you need proper Gitops way to Manage this entire Infrastructure. that why Gitops bring a value 

# Gitops principle
- **Declarative:**
let's say a yaml manifest that you are using for Kubernetes to moidify Pods, ns, etc. they are declarative manifest, which means whatever you see is what you have that means whatever you see in Git repository is the same configuration that you deploy on to kubernetes as well.

- **Versioned and Immutable:**
when you track resources in Git one of the main thing that you think about is your changes and version and your changes are immutable, for Gitops it's not mandatory it has to be Git only. example: you have S3 bucket so S3 can also be used because S3 is a Versioned it can store declarative manifest so it's not only Gitops but Gitops can also be used with different other solution as well.

- **Pulled Automatically:**whenever you make any change in Git, there is Gitops controller which is actively watching for the changes in kubernetes yaml manifest or Git and it is deploying to Kubernetes cluster this not only pull mechanism you can also use ``push`` mechanism like Github webhooks to trigger Gitops controller as well.

- **Continuous Reconciliation:**
When there is a user who try to change inside Kubernetes cluster is detected, Gitops will not allow the user change it. because Git is the single source of truth. that means If you wanna make the changes you have to do it in Git repository.


# Why use Gitops?

- **Version Control:** All changes are tracked, auditable, and reversible.
- **Collaboration:** Teams can collaborate using familiar Git workflows (pull requests, code reviews).
- **Automation:** Changes to infrastructure and applications are automatically applied using CI/CD pipelines.
- **Consistency:** Ensures environments are reproducible and consistent across deployments.
- **Security:** Reduces manual intervention, lowering the risk of human error, or when someone make some unwanted changes in Cluster, Gitops will prevent that.
- **Reconciliation:** When a change is detected, the automation tool applies the changes to the target environment to match the desired state.
- Auto healing behavior you can heal your Infrastructure when someone make an unwanted change.

# is gitops for kubernetes only?
By principle is No, But the popular Gitops tools like Argocd or Flux mostly target Kubernetes, because the modern Gitops tools tergeted Kubernetes only. Probably In future you might have different Gitops tools that solves different problem like docker swam, etc.
