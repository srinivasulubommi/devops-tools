Deployed using helm -
https://github.com/kubernetes/ingress-nginx


useful commands -

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx
kubectl apply -f configmap.yaml