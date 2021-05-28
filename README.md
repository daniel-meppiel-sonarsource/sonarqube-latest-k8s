# sonarqube-latest-k8s
Runs latest SonarQube behind Nginx Ingress on Kubernetes. Launch it with one command thanks to Skaffold.
The instructions below explain how to access this deployment Ngrok secure HTTPS tunnel.

## Requirements
Install Docker, Minikube, Ngrok and Skaffold. 
## Setup

Edit `/k8s/ingress/sq-ingress.yaml` and set `host` key to match your (Ngrok) custom domain.

From the project root folder, run the following commands:

``` bash
# Start Minikube - set vm=true for MacOS
minikube start --vm=true --disk-size 30GB
# Enable ingress nginx
minikube addons enable ingress
# Edit your etc/hosts file to point DeFROST hosts to Minikube
minikube ip
# Add host 'my.sonarqube.io' to above IP
sudo nano /etc/hosts 
# launch SonarQube deployment with skaffold
skaffold run
# Open ngrok tunnel with your ngrok domain
ngrok http http://{minikube_ip} -region=eu -hostname={your_ngrok_host}
```

Wait for `ce-lts-sonarqube-0` pod to be in Ready status. Check it by running:

``` bash
kubectl get pods
```

Access SonarQube after the Pod is in ready status under `your_ngrok_host`.