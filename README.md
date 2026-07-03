argo rollout


```shell
kubectl create namespace argo-rollouts
kubectl get ns argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
kubectl get all -n argo-rollouts
```

```shell
curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64
chmod +x ./kubectl-argo-rollouts-linux-amd64
sudo mv ./kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts
kubectl argo rollouts version
```

```shell
kubectl argo rollouts dashboard
```
execute - 

create files:
vim rollout.yaml and service.yaml
kubectl apply -f rollout.yaml -f service.yaml
kubectl get all, kubectl get svc, kubectl get pods

now, check in browser and argorollout dashboard - 
http://localhost:30080/ (blue app)


```shell
kubectl argo rollouts get rollout rollout-bluegreen
kubectl argo rollouts set image rollout-bluegreen rollouts-demo=argoproj/rollouts-demo:green
http://localhost:30081/ (green app)
kubectl argo rollouts get rollout rollout-bluegreen
kubectl argo rollouts promote rollout-bluegreen
kubectl argo rollouts get rollout rollout-bluegreen

```

You can click on 'Promote' or 'Rollback' from the dashboard.
http://localhost:3100/rollouts/rollout/argocd/rollout-bluegreen

---------------------------
Meanwhile, blue-green deployments with Argo Rollouts involves the following process:

Argo directs all traffic to the stable version with activeService. If you have three replicas, all three serve the production version.
Argo, upon updating the app, sets up a new (preview) environment with previewService, deploying three replicas of the new version. You typically pause here to check if the new version aligns well with production.
You promote the rollout after verifying everything works as expected, which then leads Argo to switch activeService to the new version and ends the use of the old version’s replicas.

There are three fields that define the process:

autoPromotionEnabled: false disables automatic promotion of the new ReplicaSet to the active service. In this example, the process is indefinitely paused, just as it was done with the canary deployment. However, you can use a combination of autoPromotionEnabled: true and autoPromotionSeconds to automatically promote the rollout after a stipulated time.
activeService specifies the active service for the blue-green deployment.
previewService sets the preview service for the blue-green deployment.

---------------------------------

FAQs
What is the difference between Argo CD and Argo rollout?
Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes that manages application deployment using Git repositories as the source of truth. Argo Rollouts, on the other hand, is a Kubernetes controller for managing advanced deployment strategies such as blue-green, canary, and progressive delivery. While Argo CD focuses on syncing app states from Git to the cluster, Argo Rollouts enhances how updates are delivered to ensure minimal disruption.

What is a major limitation of Argo rollouts?
A major limitation of Argo Rollouts is its limited support with Helm-based applications and lack of deep integration with custom resource definitions (CRDs), which can make it complex to adopt in environments heavily reliant on Helm or other templating tools.
