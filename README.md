# ArgoCD Example to deploy Helm Charts via Declaritive Setup

### Runbook

1. Install & Setup ArgoCD: [Deploying Argo CD in Kubernetes with Helm Chart](https://gravitycloud.ai/blog/deploying-argo-cd-in-kubernetes-with-helm-chart)

2. Download the `argo-nginx-app.yaml`. If you want to change the container image, then clone the repo and make the changes in the yaml file also.
```
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

> ArgoCD UI


