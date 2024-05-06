# kube-argocd-go
## Create cluster
```bash
minikube start --driver=hyperkit
```
## Install argocd
```bash
helm repo add argo-cd https://argoproj.github.io/argo-helm

helm dep update charts/argo-cd/

helm install argo-cd charts/argo-cd/ --namespace argocd --create-namespace

#before access argocd server
minikube tunnel

kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

#Sync apps
helm template charts/root-app/ | kubectl apply -f -

#Delete argocd owner helm, let argocd manage itself
kubectl delete secret -l owner=helm,name=argo-cd
```
