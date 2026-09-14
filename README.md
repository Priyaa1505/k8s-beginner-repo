# Project Overview
This project is a beginner-level hands-on exercise to understand the fundamentals of k8s by deploying and managing an Nginx application on an AWS EC2 instances. 

It covers k8s cluster, deployment.yaml, service.yaml, pod.yaml, configMaps.yaml and rolling updates. 

The Kubernetes cluster was created using Minikube with Docker as the container runtime/driver.

# technologies Used 
AWS EC2
Docker 
kubernetes 
minikube
kubectl 
Nginx
Git 
GitHub 

# Phase 1 - Kubernetes Environment Setup  

1. launch ec2 instance (t3.small)

2. ssh to cmd : ssh -i <path to pem key> <ec2-user@publicIP>

3. Installation: 

* install docker: yum install docker -y 
adding ec2-user to docker grp: sudo usermod -aG docker ec2-user
verify docker version 

* kubectl: It is the command-line tool used to communicate k8s 

Download the current stable linux binary: curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
Install kubectl: sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
check version

* minikube: minikube gives us k8s cluster locally on out ec2 machine 
download minikube: curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
install minikube: sudo install minikube-linux-amd64 /usr/local/bin/minikube
check version 

* Create k8s cluster : minikube start --driver=docker

# Phase 2 - Docker Image 

Created a simple Nginx application 
* Build the docker image:
  docker build -t <docker-image-name:version> .
* Create a second application version for rolling update
  <docker-image-name:version2> .
* Load it into Minikube
  minikube image load <docker-image-name:version>

# Phase 3 - Pod 

Created a pod using pod.yaml 
* Apply the pod:
  kubectl apply -f manifest/pod.yaml
* Check the pod:
kubectl get pods 

# Phase 4 - Deployment 

The Deployment manages multiple pods and it makes sure the desired number of pods are running.
* Apply the Deployment:
  kubectl apply -f manifest/deployment.yaml
* Check the Deployment:
  kubectl get deployment
* check Pods:
  kubectl get pods
* check ReplicaSet:
  kubectl get replicasets

# Phase 5 - Service 

Created a NodePort Service to provide network access to the Nginx Pods.
*Apply the Service 
kubectl apply -f manifest/service.yaml
*Check the service:
kubectl get svc / service 
*Check endpoints (endpoints shows the backend pod ip address and ports associated with a k8s service)
kubectl get endpoints
* Access the service through Minikube:
  minikube service nginx-service --url

  # Phase 6 - ConfigMap

  Created ConfigMap to store non-sensitive application configuration
  *Apply the ConfigMap
  kubectl apply -f manifest/ConfigMap.yaml
  *Check
  kubectl get ConfigMap

  # Phase 7 - Secret

  Create a k8s Secret for demonstration purposes
  The secret contains dummy credentials such as:
  username & password
  * apply the Secret
    kubectl apply -f manifests/secret.yaml 
  * Check:
    kubectl get secrets

  # Phase 8 - Scaling

  The Deployment was configured with multiple replicas
  example: replicas: 3
  *Scale the Deployment:
  kubectl scale deployment nginx-deployment --replicas=3
  *Verify:
   kubectl get deployment
   kubectl get pods

  # Phase 9 - Rolling Update

  A Second Version of the application was created:
  kubernetes-nginx-app:2.0
  *The Deployment image was changed form: version 1 to version 2
  kubernetes-nginx-app:1.0
  to:
  kubernetes-nginx-app:2.0
  *Apply the Updated Deployment:
  kubectl apply -f manifest/deployment.yaml
  *Monitor the rollout:
  kubectl rollout status deployment/nginx-deployment
  *Check Pods:
  kubectl get pods
  *Check rollout History:
  kubectl rollout history deployment/nginx-deployment

  # Phase 10 - Rollback

  The Deployment was rolled back to the previous working version
  *Rollback command
  kubectl rollout undo deployment/nginx-deployment
  *Verify the rollback
  kubectl rollout status deployment/nginx-deployment
  *Check pods
  kubectl get pods

  # Phase 11 - ImagePullBackOff Troubleshooting

  Intentionally change the image to something else. so that k8s could not find or successfully obtain the requested image.
  *Check pods:
  kubectl get pods
  A failed pod may show: ImagePullBackOff or ErrImagePull
  * Troubleshoot it by finding the correct image name/tag and apply again
    kubectl apply -f manifest/deployment.yaml

# PROJECT STRUCTURE 

k8s-beginner-repo/ 
│ 
├── README.md 
│ 
├── app/ 
│    ├── Dockerfile 
│    └── index.html 
│ 
├── manifests/ 
│   ├── pod.yaml 
│   ├── deployment.yaml 
│   ├── service.yaml 
│   ├── configmap.yaml 
│   └── secret.yaml 
│  
└── screenshots/ 
    ├── 01-ec2-instance.png 
    ├── 02-docker-installed.png 
    ├── 03-minikube-cluster.png 
    ├── 04-pod.png 
    ├── 05-deployment.png
    ├── 06-service.png
    ├── 07-configmap-secret.png 
    ├── 08-docker-image.png 
    ├── 09-scaling.png 
    ├── 10-rolling-update.png 
   
    














  
  
