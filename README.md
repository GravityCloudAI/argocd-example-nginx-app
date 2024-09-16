# ArgoCD Example to deploy Helm Charts via Declaritive Setup

### Runbook

1. Install & Setup ArgoCD: [Deploying Argo CD in Kubernetes with Helm Chart](https://gravitycloud.ai/blog/deploying-argo-cd-in-kubernetes-with-helm-chart)
2. Install `Argo Rollout` for a Blue-Green deployment strategy: `kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml`

3. Download the `argo-nginx-app.yaml`. If you want to change the container image, then clone the repo and make the changes in the yaml file also.
```yaml
source:
    repoURL: 'https://github.com/code-crusher/test-nginx'
    targetRevision: HEAD
    path: './chart'
```

3. Apply the ArgoCD Application file: `kubectl apply -f argo-nginx-app.yaml`

4. Check the Application in ArgoCD UI or in CLI:

> CLI
**Input**
```
kubectl get pods -A
```

**Expected Output**
```
NAMESPACE              NAME                                                READY   STATUS      RESTARTS       AGE
nginxer                nginx-gateway-deployment-55cc7cd658-4xp8l           1/1     Running     0              17s
nginxer                nginx-gateway-deployment-55cc7cd658-rsmmw           1/1     Running     0              17s
```
**Argo Rollout Watcher**
```
kubectl argo rollouts get rollout nginx-gateway-rollout -n nginxer --watch


Name:            nginx-gateway-rollout
Namespace:       nginxer
Status:          ✔ Healthy
Strategy:        Canary
  Step:          8/8
  SetWeight:     100
  ActualWeight:  100
Images:          nginx:latest (stable)
Replicas:
  Desired:       5
  Current:       5
  Updated:       5
  Ready:         5
  Available:     5

NAME                                               KIND        STATUS     AGE  INFO
⟳ nginx-gateway-rollout                            Rollout     ✔ Healthy  19m
└──# revision:1
   └──⧉ nginx-gateway-rollout-66cdc56f79           ReplicaSet  ✔ Healthy  19m  stable
      ├──□ nginx-gateway-rollout-66cdc56f79-5llb2  Pod         ✔ Running  19m  ready:1/1,restarts:1
      ├──□ nginx-gateway-rollout-66cdc56f79-9sx9r  Pod         ✔ Running  19m  ready:1/1,restarts:1
      ├──□ nginx-gateway-rollout-66cdc56f79-bzdtt  Pod         ✔ Running  19m  ready:1/1,restarts:1
      ├──□ nginx-gateway-rollout-66cdc56f79-hk548  Pod         ✔ Running  19m  ready:1/1,restarts:1
      └──□ nginx-gateway-rollout-66cdc56f79-lmzx8  Pod         ✔ Running  19m  ready:1/1,restarts:1
```
> ArgoCD UI

Applications Page

![argocd-applications](https://res.cloudinary.com/dor5uewzz/image/upload/v1720765484/readme-assets/argocd-applications_sn5rx1.png)

Application Details

![argocd-application-details](https://res.cloudinary.com/dor5uewzz/image/upload/v1720765484/readme-assets/argocd-app-details_qbppwz.png)

5. To make changes to the deployment or replicas via Sync. Clone the repo and make changes to the `Values.yaml` file
