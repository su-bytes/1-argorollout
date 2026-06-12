argo rollout

# argoroll-bg


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
