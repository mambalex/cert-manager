```bash
# get cert-manager
curl -LO https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.yaml
# install cert-manager
kubectl apply --validate=false -f cert-manager.yaml

kubectl -n cert-manager get all
```

## Self-signed certificate
```bash
cd self-signed
kubectl create ns cert-manager-test

kubectl apply -f issuer.yaml
kubectl apply -f certificate.yaml

kubectl describe certificate -n cert-manager-test
kubectl get secrets -n cert-manager-test

kubectl delete ns cert-manager-test
```

## Ingress Controller
```
kubectl create ns ingress-nginx

kubectl -n ingress-nginx apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.41.2/deploy/static/provider/cloud/deploy.yaml

kubectl -n ingress-nginx get pods

kubectl -n ingress-nginx --address 0.0.0.0 port-forward svc/ingress-nginx-controller 80
kubectl -n ingress-nginx --address 0.0.0.0 port-forward svc/ingress-nginx-controller 443

```

## Set up DNS
1. Point the domain name to the public ip
2. Configure port forwarding on the router

## Deploy a sample app
```bash
kubectl apply -f example-app/deployment.yaml
kubectl apply -f example/service.yaml
```

## Create Let's Encrypt Issuer for the cluster
```bash
# create the issuer
kubectl apply -f ingress-nginx/certificate.yaml
# check the issuer
kubectl describe clusterissuer letsencrypt-cluster-issuer
```
## Issue Certificate (Optional)
```bash
# Don't need to create a certifacte object manually if we configure the ingress annotation properly
kubectl apply -f ingress-nginx/certificate.yaml
```

## Create an ingress route and issue a certificate
```bash
kubectl apply -f example-app/ingress.yaml

# check the cert has been issued 
kubectl describe certificate example-app
```
