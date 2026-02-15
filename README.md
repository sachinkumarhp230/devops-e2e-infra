# DevOps E2E Infrastructure (Terraform + AWS EKS)

This repository provisions AWS infrastructure for the DevOps End-to-End GitOps Platform using Terraform.

It creates a fully reproducible Kubernetes platform on AWS EKS, following industry best practices for Infrastructure as Code (IaC).

---

## 🏗 Infrastructure Overview

This repository provisions:

- VPC
- Public Subnets
- Private Subnets
- NAT Gateway
- Internet Gateway
- AWS EKS Cluster
- Managed Node Group (2 × t3.small)
- Public Kubernetes API endpoint (development setup)

Region:

eu-north-1

---

## 🎯 Responsibilities

This repository owns:

- Cloud infrastructure provisioning
- Networking setup
- Kubernetes cluster creation
- Node group configuration

This repository does NOT:

- Build Docker images
- Run CI pipelines
- Deploy Kubernetes workloads
- Manage application manifests

Infrastructure and application layers are strictly separated.

---

## 📂 Repository Structure

```
devops-e2e-infra/
└── eks/
    ├── main.tf
    ├── variables.tf
    ├── versions.tf
    └── terraform.tfvars
```

### File Descriptions

- `main.tf` – VPC and EKS module definitions
- `variables.tf` – Input variables
- `versions.tf` – Provider and Terraform version constraints
- `terraform.tfvars` – Environment-specific values

---

## 🚀 Provision Infrastructure

Navigate to the `eks` directory:

```bash
cd eks
terraform init
terraform plan
terraform apply
```

Confirm with:

```bash
aws eks update-kubeconfig --region eu-north-1 --name devops-e2e-eks
kubectl get nodes
```

---

## 🧹 Destroy Infrastructure

To prevent unnecessary AWS charges:

```bash
terraform destroy
```

Important:
EKS control plane and NAT Gateway incur hourly costs. Always destroy when not actively working.

---

## ⚙️ Cluster Configuration

- 2 × t3.small managed worker nodes
- Private worker subnets
- NAT Gateway for outbound access
- Public cluster endpoint enabled (development phase)
- Terraform AWS modules used for stability and best practices

---

## 🔐 Design Principles

- Infrastructure as Code only (no manual console configuration)
- Fully reproducible environment
- Clean separation of infrastructure, CI, and CD layers
- Safe cluster rebuild capability
- Destroy and recreate without manual cleanup

---

## 🔁 Platform Architecture Context

Layer 1 — CI  
Handled in `devops-e2e-app`

Layer 2 — Infrastructure  
Handled in this repository

Layer 3 — GitOps CD  
Handled in `devops-e2e-gitops`

Each layer is independently managed.

---

## 🔮 Future Enhancements

- Remote Terraform backend (S3 + DynamoDB)
- Private cluster endpoint with bastion access
- AWS Load Balancer Controller
- Ingress configuration
- TLS via cert-manager
- IAM Roles for Service Accounts (IRSA)
- Network Policies
- Multi-environment (dev / staging / prod)
- Autoscaling policies (HPA / Cluster Autoscaler)

---

## 🧠 Key Learning Objectives

- Managing AWS infrastructure using Terraform modules
- EKS cluster lifecycle management
- Networking design for Kubernetes
- Cost-awareness during development
- Clean separation between platform and workload layers

---

## 📜 Project Purpose

This infrastructure layer is part of a production-style DevOps platform built to demonstrate:

- Cloud-native architecture
- GitOps-based deployment
- CI/CD separation
- Infrastructure reproducibility
- Industry-aligned engineering practices
