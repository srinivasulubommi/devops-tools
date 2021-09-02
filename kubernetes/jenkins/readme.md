run below commands
    helm repo add jenkinsci https://charts.jenkins.io
    helm repo update
validation
    helm search repo jenkinsci

setup ingress
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.47.0/deploy/static/provider/aws/deploy.yaml

helm install jenkins -n jenkins -f jenkins-values-override.yaml jenkinsci/jenkins

to get the secrete
    jsonpath="{.data.jenkins-admin-password}"
    secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
    echo $(echo $secret | base64 --decode)


helm uninstall jenkins -n jenkins 
