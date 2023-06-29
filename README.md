# K8S-CICD-Lab

This is a custom CICD project for automated build and deployment of a simple nginx web app on kubernetes.

## Prerequisites and dependencies:

 - A host (Linux/Windows) with pre-installed Jenkins, Docker and any CD application (in our case Ubuntu EC2 instance + ArgoCD) + Helm and kubeseal installed
 - A kubernetes cluster with deployed ingress controller, metrics-server
 - A custom-built image with docker and git to use as a Jenkins Agent
##    
* All of the following steps are assuming and Ubuntu EC2 instance as a host for the CICD
* Sources:
  - [Jenkins](https://www.jenkins.io/doc/book/installing/linux/)
  - [Docker](https://docs.docker.com/desktop/install/ubuntu/#install-docker-desktop)
  - [ArgoCD](https://operatorhub.io/operator/argocd-operator)
  - [SealedSecrets](https://github.com/bitnami-labs/sealed-secrets)

```bash

# install java and verify installation

sudo apt update
sudo apt install openjdk-11-jre
java -version

# install Jenkins

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

# Acquire the admin pass for Jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Install the "Docker Pipeline" Jenkins plugin
# Install the "Git Plugin" Jenkins plugin

# Install Docker

sudo apt update
sudo apt install docker.io

# Grant Jenkins user and Ubuntu user permission to Docker.

sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
systemctl restart docker

# Deploy The ArgoCD Operator on the kubernetes cluster

curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.25.0/install.sh | bash -s v0.25.0
kubectl create -f https://operatorhub.io/install/argocd-operator.yaml

# Deploy the SealedSecrets helm chart

helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets
```
