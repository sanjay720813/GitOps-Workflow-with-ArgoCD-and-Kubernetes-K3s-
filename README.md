# 🚀 GitOps Workflow with ArgoCD and Kubernetes (K3s)

## 📌 Project Overview

This project demonstrates the implementation of a **GitOps pipeline** using **ArgoCD** on a lightweight Kubernetes distribution, **K3s**, hosted on an AWS EC2 instance. The deployment state of applications is managed and synced directly from a GitHub repository, enabling version-controlled, automated, and auditable deployments.

---

## 🛠️ Tools & Technologies

- **Kubernetes (K3s)** – Lightweight Kubernetes distribution
- **ArgoCD** – GitOps continuous delivery tool for Kubernetes
- **Docker** – Containerization platform
- **GitHub** – Source control for manifest files
- **AWS EC2** – Cloud compute instance for hosting K3s

---

## ✅ Project Objectives

- Set up K3s Kubernetes cluster on AWS EC2
- Deploy and configure ArgoCD for GitOps
- Store Kubernetes manifests in GitHub
- Use ArgoCD to auto-sync application deployments from GitHub
- Demonstrate GitOps workflow via Git-based updates

---

## 🧰 Setup Instructions

### 🔹 PART 1: K3s Installation (on AWS EC2)

```bash
# Install K3s
curl -sfL https://get.k3s.io | sh -

# Verify installation
k3s kubectl get nodes
```

### 🔹 PART 2: Install and Configure ArgoCD

```bash
# Create argocd namespace
kubectl create namespace argocd

# Apply ArgoCD installation manifest
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Verify pods are running
kubectl get pods -n argocd
```

➤ Install ArgoCD CLI

```bash
# Create argocd namespace
# Download ArgoCD CLI (Linux example)
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

# Make binary executable
sudo chmod +x /usr/local/bin/argocd
```

➤ Expose ArgoCD Server (Change to NodePort)

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

➤ Get ArgoCD Server Port & Access

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

➤ Get ArgoCD Server Port & Access

```bash
kubectl get svc -n argocd
Access ArgoCD: http://<EC2_PUBLIC_IP>:<NODE_PORT>
```

➤ Get Initial Admin Password

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d && echo
Username: admin
Change the password from the UI after first login.
```

### 🔹 PART 3: Prepare GitHub Repository

1. Create a public GitHub repository.
2. Add Kubernetes manifest files, e.g., deployment.yaml and service.yaml.
3. Commit and push.

### 🔹 PART 4: Configure ArgoCD Application

1. Login to ArgoCD UI
2. Click "New App" and fill in:
  Name: online-app,
  Project: default,
  Sync Policy	Manual or Auto (choose based on preference),
  Repo URL:	https://github.com/your-username/gitops-app,
  Path:	. ,
  Cluster:	https://kubernetes.default.svc,
  Namespace:	default
3. Click Create → Sync.






