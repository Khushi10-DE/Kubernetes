# 🚀 Docker & Kubernetes Beginner Guide

---

## 🐳 What is Docker?

Docker is a platform that:

- Packages applications with their dependencies  
- Runs them in isolated environments called **containers**  
- A container includes:
  - Application
  - Dependencies
  - System tools  

Kubernetes uses a **container runtime** (like Docker or containers) to run containers.

---

# 🛠 Docker Installation (Ubuntu)

## Step 1 — Update System

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2 — Install Required Packages

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

## Step 3 — Add Docker Official GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## Step 4 — Add Docker Repository

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Step 5 — Install Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step 6 — Start Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
docker --version
```

---

# ☸️ Kubernetes Tools

| Tool      | Purpose |
|-----------|----------|
| kubectl   | Command-line tool to control Kubernetes |
| kubeadm   | Tool to create production clusters |
| minikube  | Local single-node cluster for learning |

---

# 📦 Install kubectl

```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl

chmod +x kubectl

sudo mv kubectl /usr/local/bin/
```

Check version:

```bash
kubectl version --client
```

---

# 📦 Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Start the cluster:

```bash
minikube start --driver=docker
```

Why `--driver=docker`?

Minikube creates the Kubernetes node using Docker containers.

Check cluster:

```bash
kubectl get nodes
```

---

# 🧠 Core Kubernetes Concepts

## 🔹 Node
A machine where Pods run.  
(Minikube creates one node for learning.)

## 🔹 Pod
- Smallest unit in Kubernetes  
- Runs one or more containers  

## 🔹 Deployment
- Manages Pods  
- Maintains desired number of replicas  
- Provides self-healing  
- Enables scaling  

## 🔹 Service
Exposes Pods to the network and provides load balancing.

---

# 🛠 Essential Kubernetes Commands

## 🔹 Check Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

## 🔹 Create Application Deployment

```bash
kubectl create deployment khushbunginx --image=nginx
```

## 🔹 View Running Pods

```bash
kubectl get pods
```

## 🔹 View System Pods

```bash
kubectl get pods -n kube-system
```

## 🔹 Scale Application

```bash
kubectl scale deployment khushbunginx --replicas=3
```

## 🔹 Describe a Pod (Detailed Info)

```bash
kubectl describe pod etcd-minikube -n kube-system
```

## 🔹 Enter Inside a Pod

First check pod name:

```bash
kubectl get pods
```

Then:

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

You are now inside the container.  
You cannot use `kubectl` inside the container unless it is installed there.

Exit:

```bash
exit
```

## 🔹 Delete Deployment

```bash
kubectl delete deployment khushbunginx
```

---

# 📄 YAML (Declarative Approach)

Instead of using commands, we can define resources using YAML files.

Apply a YAML file:

```bash
kubectl apply -f khushbunginx-deployment.yaml
```

---

# ⚙️ What Happens Internally?

When you apply a Deployment:

1. `kubectl` sends request to API Server  
2. API Server stores desired state in **etcd**  
3. Scheduler selects a node  
4. Kubelet creates the Pod  
5. Container runtime pulls the image  
6. Container starts running  

---

# 🎯 Summary

- **Docker** → Builds and runs containers  
- **Kubernetes** → Manages containers at scale  
- **Minikube** → Local Kubernetes cluster for learning  
- **kubectl** → Tool to control Kubernetes  
