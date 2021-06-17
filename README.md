# sonarqube-latest-k8s
Runs latest SonarQube on Kubernetes. Launch it with one command thanks to Skaffold.
The instructions below explain how to access this deployment Ngrok secure HTTPS tunnel.
This branch of the project uses an ngrok container pod to provide direct tunneled access to your setup:

* no need for an ingress service (removed)
* no local access, SQ connectivity is exclusively through ngrok
* no need for additional ngrok commands or /etc/hosts edits on your laptop or desktop
* the docker driver can be used: your minikube cluster can run in a dedicated docker (even on MacOS) 

## Requirements
Install [Docker](https://www.docker.com), [Minikube](https://minikube.sigs.k8s.io), [Ngrok](https://ngrok.com) and [Skaffold](https://skaffold.dev). 
## Setup

Edit `/k8s/ngrok/sq-ngrok.yaml` and set your `authtoken` and ngrok address to allow the ngrok container to set your domain directly.

From the project root folder, run the following commands:

``` bash
# Start Minikube - set vm=true for MacOS
# minikube start --vm=true --disk-size 30GB
# the above command can be used, but also:
minikube start --driver=docker --disk-size 30GB
# launch SonarQube deployment with skaffold
skaffold run
```

Wait for `ce-lts-sonarqube-0` pod stateful set to be up. Check it in the dashboard with:

``` bash
minikube dashboard
```

## Troubleshooting
In order to check whether the SonarQube service is answering and reachable from the ngrok container, you may apply thoses commands:

``` bash
√ sonarqube-latest-k8s %  kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
ce-lts-postgresql-0      1/1     Running   0          24m
ce-lts-sonarqube-0       1/1     Running   0          24m
ngrok-5f8f988f5d-trjcx   1/1     Running   0          24m
√ sonarqube-latest-k8s % kubectl exec ngrok-5f8f988f5d-trjcx -- wget -O- ce-lts-sonarqube:9000
Connecting to ce-lts-sonarqube:9000 (10.109.148.49:9000)
writing to stdout
.
.
.
```
