Implementing Blue-Green Deployments with Argo Rollouts

What is Argo Rollouts?
Argo Rollouts is a progressive delivery tool that uses the Kubernetes controller and custom resource definition (CRD) constructs to enhance the Kubernetes built-in deployment capabilities. This provides more control during the application deployment process and allows you to implement blue-green and canary deployments easily. Moreover, if necessary, you can use Argo Rollouts to roll back to a previous version, ensuring minimal downtime and improving the reliability of your application deployments.

Blue-green deployments use two identical environments: one hosts the stable version (blue) and the other the new version (green). This method is perfect for updates with no downtime as the new version matches the live environment, letting you switch all users to it at once.

Meanwhile, blue-green deployments with Argo Rollouts involves the following process:
1.	Argo directs all traffic to the stable version with activeService. If you have three replicas, all three serve the production version.
2.	Argo, upon updating the app, sets up a new (preview) environment with previewService, deploying three replicas of the new version. You typically pause here to check if the new version aligns well with production.
3.	You promote the rollout after verifying everything works as expected, which then leads Argo to switch activeService to the new version and ends the use of the old version’s replicas.

The progressive deployment unfolds in three stages: setting up the deployment, preparing the preview environment, and executing the update.

The structure of the rollout is very similar to that of the canary deployment, but instead of steps, there are three fields that define the process:
•	autoPromotionEnabled: false disables automatic promotion of the new ReplicaSet to the active service. In this example, the process is indefinitely paused, just as it was done with the canary deployment. However, you can use a combination of autoPromotionEnabled: true and autoPromotionSeconds to automatically promote the rollout after a stipulated time.
•	activeService specifies the active service for the blue-green deployment.
•	previewService sets the preview service for the blue-green deployment.


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

