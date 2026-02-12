🚀 Docker & Kubernetes Beginner Notes
🐳 What is Docker?

Docker is a platform that:

Packages application + dependencies

Runs them in isolated environments called containers

A container is a lightweight package of:

Application

Dependencies

System tools

Kubernetes uses a container runtime (like Docker or containerd) to run containers.

🛠 Install Docker (Ubuntu)
Step 1 — Update system
sudo apt update
sudo apt upgrade -y

Step 2 — Install required packages
sudo apt install -y ca-certificates curl gnupg lsb-release

Step 3 — Add Docker Official GPG Key
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

Step 4 — Add Docker Repository
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Step 5 — Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Step 6 — Start Docker
sudo systemctl start docker
sudo systemctl enable docker
docker --version

☸️ Kubernetes Tools
Tool	What It Does
kubectl	Command tool to talk to Kubernetes
kubeadm	Creates production cluster
minikube	Local single-node cluster for learning
📦 Install kubectl
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl

chmod +x kubectl

sudo mv kubectl /usr/local/bin/


Check version:

kubectl version --client

📦 Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube


Start Kubernetes Cluster:

minikube start --driver=docker


Why --driver=docker?

Minikube will create the Kubernetes node using Docker containers.

Check cluster:

kubectl get nodes

🧠 Core Concepts
Node

Machine where Pods run (Minikube is one node)

Pod

Smallest unit in Kubernetes
Pod runs one or more containers

Deployment

Manages Pods

Ensures desired number of replicas are always running

Provides self-healing and scaling

Service

Exposes Pods to the network

🛠 Commands You Must Know
Check Cluster
kubectl cluster-info
kubectl get nodes

Create Application Deployment
kubectl create deployment khushbunginx --image=nginx

See Running Pods
kubectl get pods

See System Pods
kubectl get pods -n kube-system

Scale Application
kubectl scale deployment khushbunginx --replicas=3

Describe a Pod (Detailed Info)
kubectl describe pod etcd-minikube -n kube-system

Enter Inside a Pod

First check pod name:

kubectl get pods


Then:

kubectl exec -it <pod-name> -- /bin/bash


You are now inside the container.
From here, you cannot use kubectl.

Exit:

exit

Delete Deployment
kubectl delete deployment khushbunginx

📄 YAML (Declarative Way)

Instead of commands, we use YAML files.

Example: khushbunginx-deployment.yaml

Apply it:

kubectl apply -f khushbunginx-deployment.yaml
