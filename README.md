# ToDo App Deployment on AWS EKS Using GitOps and Argo CD

A DevOps capstone project demonstrating how to containerize a ToDo application with Docker, store its image in Amazon ECR, deploy it on Amazon EKS, and automate Kubernetes deployments using GitHub and Argo CD.

## Project Overview

This project implements a GitOps-based deployment workflow for a Dockerized ToDo application. Kubernetes manifests are stored in GitHub, and Argo CD continuously monitors the repository and synchronizes the desired application state with the EKS cluster.

### Objectives

- Containerize the application using Docker.
- Build and publish the Docker image to Amazon Elastic Container Registry (ECR).
- Deploy the application on Amazon Elastic Kubernetes Service (EKS).
- Use Kubernetes Deployments and Services to manage application availability and networking.
- Implement GitOps-based continuous delivery using Argo CD.
- Demonstrate automated synchronization and self-healing from Git changes.

## Architecture

```text
Developer
   |
   v
GitHub Repository
   |
   | Kubernetes manifests
   v
Argo CD
   |
   | Automated synchronization
   v
Amazon EKS Cluster
   |
   +-- Kubernetes Deployment
   |      |
   |      +-- ToDo Pod 1
   |      +-- ToDo Pod 2
   |
   +-- ClusterIP Service
          |
          v
      ToDo Application
```

**Container image flow:**

```text
Application Source
       |
       v
Docker Build
       |
       v
Amazon ECR
       |
       v
Amazon EKS Worker Nodes
```

## Technology Stack

| Technology | Purpose |
|---|---|
| Docker | Containerization |
| Amazon ECR | Container image registry |
| Amazon EKS | Managed Kubernetes |
| Kubernetes | Deployment, pods and service management |
| Argo CD | GitOps continuous delivery |
| Git and GitHub | Source control and configuration management |
| AWS CLI | AWS command-line operations |
| kubectl | Kubernetes cluster management |

## Repository Structure

```text
.
├── Appcode/
│   └── Appcode/
│       ├── public/
│       ├── src/
│       ├── Dockerfile
│       └── package.json
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── argocd-app.yaml
├── .gitignore
└── README.md
```

## AWS Resources

The project was deployed with the following configuration:

- **AWS Region:** `ap-south-1` (Mumbai)
- **EKS Cluster:** `todo-eks-cluster`
- **Managed Node Group:** `todo-workers`
- **Worker Nodes:** 2 × `t3.small`
- **ECR Repository:** `todo-app`
- **Container Image Tag:** `1.0`
- **Kubernetes Deployment:** `todo-app`
- **Desired Replicas:** 2
- **Kubernetes Service:** `ClusterIP`

## Deployment Workflow

### 1. Build the Docker image

From the application directory:

```bash
cd Appcode/Appcode
docker build -t todo-app:1.0 .
```

### 2. Authenticate with Amazon ECR

Run this command in a terminal configured with AWS CLI credentials and the required ECR permissions:

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 782208973324.dkr.ecr.ap-south-1.amazonaws.com
```

### 3. Tag and push the image

```bash
docker tag todo-app:1.0 782208973324.dkr.ecr.ap-south-1.amazonaws.com/todo-app:1.0

docker push 782208