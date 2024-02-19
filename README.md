# Ingress-nginx-on-prem
A Kubernetes Ingress Nginx modified template for on-prem environment.
This modified template can be used to deploy an ingress nginx controller to an on-prem environment as per the recommendation described here: https://kubernetes.github.io/ingress-nginx/deploy/baremetal/.
The design chosen is the `Via the host network` one (i.e. with no LoadBalancer, no NodePort: controller deployment directly binded to the hosts). 

## How the template was generated: 
```
helm template ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace > ingress-nginx.yml
```
Then, the following modifications have been made: 
* Deployment pod `ingress-nginx-controller` with hostNetwork=true
* Service ingress-nginx commented
* `ingress-nginx-controller` type changed from Deployment to DaemonSet
* In `ingress-nginx-controller` DaemonSet commented replicas field
* In `ingress-nginx-controller` DaemonSet commented --publish-service argument.

## Utilization: 
```
kubectl apply -f ingress-nginx.yml
```
Verification: 
```
kubectl get all -n ingress-nginx
```

## Demo ingress
After the controller is deployed, we can then create ingress rules to publish services. An example is given in `demo-ingress.yml`: 
```
kubectl apply -f demo-ingress.yml
```
Verification: 
```
kubectl get ingress
```
Assuming a service is deployed, the service should now be available.   