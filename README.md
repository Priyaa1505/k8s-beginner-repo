# kubernetes beginner hands-on 
#objective : Learn k8s fundamentals using minikube and docker 

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
