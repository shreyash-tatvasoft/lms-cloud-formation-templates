# AWS 3-Tier Web Application Templates

This repository contains **AWS CloudFormation templates** for deploying a complete 3-Tier Web Application infrastructure. It includes separate templates for networking, database, backend, frontend, IAM, an infrastructure setup template for CI/CD, and a master stack to orchestrate deployment.

---

## 🏗️ Overview

The architecture follows a **3-Tier design**:

1. **Presentation Layer (Frontend)**  
   - Delivered through **CloudFront** for fast, secure, and global content distribution.  
   - Integrated with a CI/CD pipeline for automated deployments.

2. **Application Layer (Backend)**  
   - Hosted on **EC2 Auto Scaling Groups (ASG)** behind an **Application Load Balancer (ALB)**.  
   - Automated deployments handled via CI/CD pipeline.

3. **Data Layer (Database)**  
   - **Amazon RDS** instance for reliable, managed relational database storage.

4. **Networking Layer**  
   - Configured **VPC, subnets, route tables, security groups**, and other networking essentials.

5. **IAM Layer**  
   - Centralized **IAM Roles and Policies** for EC2, CodePipeline, CodeBuild, and other services.

6. **Infrastructure Orchestration**  
   - Automated execution of the **Master Stack** using CI/CD to fetch templates directly from Git.

---

## 📂 Templates

The repository contains the following **CloudFormation templates**:

### 1. Infra Setup
- **File:** `Infra-Setup.yml`  
- **Purpose:** Bootstraps the infrastructure by:  
  - Fetching CloudFormation templates from the Git repository  
  - Using **CodePipeline + CodeBuild** for CI/CD  
  - Automatically deploying the `Master-template.yml`  
- **Usage:** Deploy this stack first to enable automated infrastructure provisioning via Git.

### 2. Master Stack
- **File:** `Master-template.yml`  
- **Purpose:** Orchestrates the deployment of all other stacks in sequence.  
- **Usage:** Triggered by the CI/CD pipeline (via `Infra-Setup.yml`) or deployed manually.

### 3. IAM Configuration
- **File:** `IAM-Setup.yml`  
- **Purpose:** Creates required **IAM Roles and Policies** for the project.  
- **Features:**
  - Roles for:
    - **EC2 Instances** (S3, CloudWatch Logs, SSM access)  
    - **CodePipeline & CodeBuild** (GitHub/CodeCommit, S3, ECR, CloudFormation, Deploy)  
    - **Frontend & Backend Deployments** (S3, CloudFront invalidation, AutoScaling, ALB registration, etc.)  
  - Managed and custom inline policies  
  - IAM Instance Profiles for EC2  

### 4. VPC Configuration
- **File:** `VPC-Setup.yml`  
- **Purpose:** Sets up the networking layer:
  - VPC with public and private subnets  
  - Internet Gateway & NAT Gateway  
  - Route Tables and associated routes  
  - Security Groups for BE and FE  

### 5. RDS Stack
- **File:** `RDS-Setup.yml`  
- **Purpose:** Creates a managed **RDS instance** in private subnets.  
- **Supports:** MySQL, PostgreSQL, or other engines.  
- **Features:**
  - Multi-AZ support (optional)  
  - Parameter Groups and Security Groups for secure DB access  

### 6. Backend Setup (BE)
- **File:** `BE-Setup.yml`  
- **Purpose:** Deploys backend services with CI/CD pipeline.  
- **Components:**
  - **Auto Scaling Group (ASG)** for backend EC2 instances  
  - **Application Load Balancer (ALB)** with proper listeners and target groups  
  - **CodePipeline** integration for CI/CD from GitHub repository to deployment  
  - CloudWatch monitoring & logging  

### 7. Frontend Setup (FE)
- **File:** `FE-Setup.yml`  
- **Purpose:** Deploys frontend with CI/CD pipeline using **CloudFront**.  
- **Components:**
  - S3 bucket to host static files  
  - CloudFront distribution for global caching and HTTPS  
  - CI/CD pipeline for automated deployment from GitHub to CloudFront  
  - Optional Route53 integration for custom domains  

---

## ⚡ Deployment Order

1. Deploy `Infra-Setup.yml` to configure CI/CD and enable automatic deployment of the Master Stack from Git.  
2. (If deploying manually) start with `IAM-Setup.yml` to create roles and policies.  
3. Deploy `VPC-Setup.yml` for networking resources.  
4. Deploy `RDS-Setup.yml` for the database.  
5. Deploy `BE-Setup.yml` for backend services and CI/CD pipeline.  
6. Deploy `FE-Setup.yml` for frontend services and CI/CD pipeline.  
7. Or, simply let `Infra-Setup.yml` handle the automation of `Master-template.yml`, which orchestrates all stacks sequentially.  

---

## 🔧 Prerequisites

- AWS CLI configured with proper credentials  
- CloudFormation permissions for VPCs, EC2, RDS, S3, CloudFront, IAM, CodePipeline, and related services  
- GitHub repositories for backend and frontend code (integrated into CI/CD pipelines)  

---

## 📦 Features

- Fully **modular architecture**: each stack can be deployed independently  
- Automated **CI/CD pipelines** for FE and BE deployments  
- **Scalable backend** with ASG and ALB  
- **Global frontend delivery** with CloudFront  
- **Secure networking** using VPC, subnets, and security groups  
- **Centralized IAM** roles and policies for consistent security  
- **Infra-Setup** for Git-based CI/CD automation  
- Master stack for **sequential orchestration**  

---
