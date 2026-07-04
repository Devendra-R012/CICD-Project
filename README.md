# End-to-End CI/CD Pipeline using Jenkins, Docker, Amazon ECR, Amazon EKS, Prometheus & Grafana

## Project Overview

This project demonstrates an end-to-end CI/CD pipeline for deploying a Node.js application on Amazon EKS using Jenkins. The pipeline automates the entire software delivery process, from source code management to deployment and monitoring.

The pipeline performs the following tasks:

- Pulls source code from GitHub
- Builds a Docker image
- Pushes the Docker image to Amazon ECR
- Deploys the application to Amazon EKS
- Monitors the Kubernetes cluster using Prometheus and Grafana

---

## Live Demo

**Application URL:**

http://a3b63f0f9ea0c4a21b6868ab80f7772a-1137535609.ap-south-1.elb.amazonaws.com

The application is currently deployed on Amazon EKS and is publicly accessible for evaluation.

---

## Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
Jenkins Pipeline
    │
    ▼
Docker Build
    │
    ▼
Amazon Elastic Container Registry (ECR)
    │
    ▼
Amazon Elastic Kubernetes Service (EKS)
    │
    ▼
Kubernetes Deployment & Service
    │
    ▼
Application
    │
    ▼
Prometheus + Grafana Monitoring
```

---

## Tech Stack

- Git
- GitHub
- Jenkins
- Docker
- Amazon ECR
- Amazon EKS
- Kubernetes
- Helm
- Prometheus
- Grafana
- AWS CLI
- kubectl
- eksctl
- Node.js

---

## Prerequisites

Before starting, ensure the following tools and services are available:

- AWS Account
- Amazon Linux EC2 Instance
- IAM Role with EKS and ECR permissions
- GitHub Repository
- Jenkins
- Docker
- AWS CLI
- kubectl
- eksctl
- Helm

---

## Project Structure

```text
CICD-Project/
│
├── app.js
├── package.json
├── Dockerfile
├── Jenkinsfile
├── README.md
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

---

## Jenkins Pipeline Stages

The Jenkins pipeline consists of the following stages:

1. Checkout SCM
2. Clone Repository
3. Build Docker Image
4. Login to Amazon ECR
5. Tag Docker Image
6. Push Docker Image
7. Deploy to Amazon EKS

---

## Deployment Steps

### Clone the Repository

```bash
git clone https://github.com/Devendra-R012/CICD-Project.git
cd CICD-Project
```

### Build Docker Image

```bash
docker build -t cicd-project .
```

### Authenticate Docker with Amazon ECR

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 911167911799.dkr.ecr.ap-south-1.amazonaws.com
```

### Tag and Push Docker Image

```bash
docker tag cicd-project:latest 911167911799.dkr.ecr.ap-south-1.amazonaws.com/cicd-project:latest

docker push 911167911799.dkr.ecr.ap-south-1.amazonaws.com/cicd-project:latest
```

### Deploy to Kubernetes

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

---

## Verify Deployment

Check the Kubernetes resources:

```bash
kubectl get nodes

kubectl get deployments

kubectl get pods

kubectl get svc
```

---

## Monitoring Setup

Install Prometheus and Grafana using Helm.

### Add Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update
```

### Create Monitoring Namespace

```bash
kubectl create namespace monitoring
```

### Install Prometheus & Grafana

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
--namespace monitoring
```

### Verify Monitoring Components

```bash
kubectl get pods -n monitoring

kubectl get svc -n monitoring
```

---

## Access the Application

The application is publicly accessible at:

**http://a3b63f0f9ea0c4a21b6868ab80f7772a-1137535609.ap-south-1.elb.amazonaws.com**

You can also verify the Kubernetes service:

```bash
kubectl get svc
```

Example Output:

```text
NAME                   TYPE           EXTERNAL-IP
cicd-project-service   LoadBalancer   a3b63f0f9ea0c4a21b6868ab80f7772a-1137535609.ap-south-1.elb.amazonaws.com
```

---

## Access Grafana

Retrieve the Grafana admin password:

```bash
kubectl get secret monitoring-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 -d
```

Forward the Grafana service:

```bash
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
```

Open Grafana in your browser:

```text
http://localhost:3000
```

Default Username:

```text
admin
```

Password:

```text
(Output from the previous command)
```

---

## CI/CD Workflow

```text
Developer
    │
    ▼
Push Code to GitHub
    │
    ▼
Jenkins Pipeline Triggered
    │
    ▼
Build Docker Image
    │
    ▼
Push Image to Amazon ECR
    │
    ▼
Deploy Application to Amazon EKS
    │
    ▼
Application Updated
    │
    ▼
Prometheus Collects Metrics
    │
    ▼
Grafana Displays Dashboards
```

---

## Notes

- The application is currently deployed on Amazon EKS and is publicly accessible using the URL above.
- The deployment environment will remain available for evaluation.
- The repository includes the Dockerfile, Jenkins pipeline, and Kubernetes deployment configuration required to reproduce the deployment.

---


GitHub: https://github.com/Devendra-R012/CICD-Project.git
