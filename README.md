# sonarqube-latest-k8s
Runs latest SonarQube behind Nginx Ingress and self-signed certs using k8s and Skaffold.

minikube start --vm=true --disk-size 30GB
sudo nano /etc/hosts
minikube addons enable ingress