# rancher deployment/setup
kubectl create ns rancher
    helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
    helm repo update
    kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.2/cert-manager.crds.yaml
    kubectl create ns cert-manager
    helm repo add jetstack https://charts.jetstack.io
    helm repo update
    helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.13.2 
    kubectl get pods -n cert-manager
    helm install rancher rancher-stable/rancher -n cattle-system --set hostname=rancher.smartuniversaldevops.com
    kubectl -n cattle-system rollout status deploy/rancher
    kubectl get ingress -n cattle-system
# setting up ingress
like we usually do it by creating ingress rules, here we do  not because we already did when we run: 
helm install rancher rancher-stable/rancher -n cattle-system --set hostname=rancher.smartuniversaldevops.com
 for this to work fine, we need to run kubectl get ingress (folllowed by name space)
 then kubectl edit the ingress.
 under spec:, and above rules, we add ingressClassName: nginx
 # to get the password:
 run: kubectl get secret --namespace cattle-system bootstrap-secret -o go-template='{{.data.bootstrapPassword|base64decode}}{{ "\n" }}'
 if this doesnt work, try:  kubectl -n cattle-system exec $(kubectl -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password

 # the following are what helped me get it done: 
 1. pay attention to the version v1.13.2
 2. check out the following links:
 3. https://cert-manager.io/docs/installation/helm/
 4. https://artifacthub.io/packages/helm/rancher-stable/rancher
 5. https://github.com/cert-manager/cert-manager/releases
 6. https://www.youtube.com/watch?v=APsZJbnluXg 
