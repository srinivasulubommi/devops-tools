helm chart url -
https://github.com/SonarSource/helm-chart-sonarqube

helm upgrade --install -f values.yaml -f /home/ec2-user/snehal/kubernetes/sonarqube-helm-chart/override-values.yaml -n sonarqube sonarqube ./