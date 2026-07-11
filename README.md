# KubeFusion: Cloud-Native GitOps CI/CD Deployment Platform

> An end-to-end GitOps-based CI/CD platform that automates containerized application deployment using GitHub Webhooks, Jenkins, Docker, Docker Hub, ArgoCD, and Kubernetes on AWS.

---

## 📖 Overview

KubeFusion is an enterprise-inspired GitOps CI/CD platform that demonstrates modern cloud-native deployment practices.

The project automates the complete software delivery lifecycle—from source code commit to production deployment—using a fully automated GitOps workflow.

Whenever application code is pushed to GitHub, Jenkins automatically builds a Docker image, pushes it to Docker Hub, updates the Kubernetes deployment manifest, and commits the updated image version back to GitHub. ArgoCD continuously monitors the repository, detects the manifest changes, and synchronizes the Kubernetes cluster automatically.

This project demonstrates how Continuous Integration, Continuous Delivery, Infrastructure Automation, and GitOps work together in a real-world deployment pipeline.

---

# 🏗 Project Architecture

Developer
        │
        ▼
GitHub Repository
        │
        ▼
GitHub Webhook
        │
        ▼
Jenkins Pipeline
        │
        ▼
Docker Image Build
        │
        ▼
Docker Hub
        │
        ▼
Update deployment.yaml
        │
        ▼
GitHub Repository
        │
        ▼
ArgoCD
        │
        ▼
Kubernetes Cluster
        │
        ▼
Live Application

---

# ✨ Features

- Fully Automated CI/CD Pipeline
- GitOps-Based Continuous Deployment
- GitHub Webhook Integration
- Jenkins Declarative Pipeline
- Docker Image Versioning
- Docker Hub Integration
- Automatic Kubernetes Manifest Updates
- ArgoCD Auto Sync
- Kubernetes Deployment Automation
- Kubernetes Rollback Support
- Automatic Deployment after Git Push
- Docker Image Cleanup
- Enterprise Project Dashboard
- Production-Oriented Project Structure

---

# ⚙ Technologies Used

## Cloud

- AWS EC2

## Version Control

- Git
- GitHub

## CI/CD

- Jenkins

## Containerization

- Docker
- Docker Hub

## GitOps

- ArgoCD

## Container Orchestration

- Kubernetes

## Application

- Flask
- HTML
- Tailwind CSS
- JavaScript

---

# 📂 Project Structure

```
KubeFusion/
│
├── app.py
├── Dockerfile
├── Jenkinsfile
├── requirements.txt
├── README.md
│
├── templates/
│   └── index.html
│
├── static/
│   ├── css/
│   ├── js/
│   └── images/
│
└── k8s/
    ├── namespace.yaml
    ├── deployment.yaml
    └── service.yaml
```

---

# 🚀 CI/CD Workflow

### Step 1

Developer pushes source code to GitHub.

↓

### Step 2

GitHub Webhook automatically triggers Jenkins.

↓

### Step 3

Jenkins Pipeline

- Clones Repository
- Builds Docker Image
- Tags Image
- Pushes Image to Docker Hub

↓

### Step 4

Jenkins updates

```
deployment.yaml
```

with the latest Docker image tag.

↓

### Step 5

Jenkins commits the updated manifest to GitHub.

↓

### Step 6

ArgoCD detects repository changes.

↓

### Step 7

ArgoCD synchronizes Kubernetes automatically.

↓

### Step 8

Application is deployed successfully.

---

# 🔄 GitOps Workflow

```
Git Push

↓

Jenkins

↓

Docker Build

↓

Docker Hub

↓

Manifest Update

↓

GitHub

↓

ArgoCD

↓

Kubernetes

↓

Deployment
```

---

# 📦 Kubernetes Resources

- Namespace
- Deployment
- ReplicaSet
- Service
- Pods

---

# 📈 Pipeline Capabilities

- Continuous Integration
- Continuous Deployment
- Docker Image Versioning
- Automated Manifest Update
- Auto Sync
- Self Healing
- Rollback Support
- Declarative Infrastructure

---

# 🖥 Application Dashboard

The project includes a lightweight Flask-based dashboard that provides:

- Project Overview
- Deployment Status
- GitOps Workflow
- Technology Stack
- Deployment Architecture

---

# 🔐 Jenkins Credentials

The Jenkins Pipeline securely stores credentials using Jenkins Credentials Manager.

- Docker Hub Credentials
- GitHub Personal Access Token

No credentials are hardcoded inside the project.

---



# 🌟 Future Improvements

- Helm Chart Deployment
- Prometheus Monitoring
- Grafana Dashboards
- Slack Notifications
- SonarQube Integration
- Trivy Image Scanning
- Kubernetes HPA
- Ingress Controller
- SSL/TLS
- Multi-Environment Deployments
- GitHub Actions Integration

---

# 👨‍💻 Author

**Aditya M Khiroji**

Electronics & Communication Engineer

Cloud | DevOps | Kubernetes | GitOps | AWS

GitHub:
https://github.com/adityamk123

---

# ⭐ If you found this project useful

Please consider giving this repository a ⭐ on GitHub.
