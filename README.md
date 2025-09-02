# AWS 3-Tier Web Application Templates

This repository contains **AWS CloudFormation templates** for deploying a complete 3-Tier Web Application infrastructure. It includes separate templates for networking, database, backend, frontend, IAM, and a master stack to orchestrate deployment.

---

## 🏗️ Overview

The architecture follows a **3-Tier design**:

1. **Presentation Layer (Frontend)**  
   - Served via **CloudFront** for fast and global content delivery.  
   - Integrated with a CI/CD pipeline for automated deployment.

2. **Application Layer (Backend)**  
   - Hosted on **EC2 Auto Scaling Groups (ASG)** behind an **Application Load Balancer (ALB)**.  
   - CI/CD pipeline automates deployment.

3. **Data Layer (Database)**  
   - **RDS** instance for reliable, managed relational database storage.

4. **Networking Layer**  
   - Configured **VPC, subnets, route tables, security groups**, and other networking essentials.

5. **IAM Layer**  
   - Centralized **IAM Roles and Policies** for EC2, CodePipeline, CodeBuild, and other services.

---

## 📂 Templates

The repository contains the following **CloudFormation templates**:

### 1. Master Stack
- **File:** `Master-template.yml`  
- **Purpose:** Orchestrates the deployment of all other stacks in a sequential manner.  
- **Usage:** Deploy this stack first to automatically trigger all dependent stacks.

### 2. IAM Configuration
- **File:** `IAM-Setup.yml`  
- **Purpose:** Creates required **IAM Roles and Policies** for the project.  
- **Features:**
  - IAM Roles for:
    - **EC2 Instances** (access to S3, CloudWatch logs, SSM)  
    - **CodePipeline & CodeBuild** (access to GitHub/CodeCommit, S3, ECR, CloudFormation, Deploy)  
    - **Frontend & Backend Deployments** (permissions for S3, CloudFront invalidation, AutoScaling, ALB registration, etc.)  
  - Managed & custom inline policies as needed  
  - IAM Instance Profiles for EC2  

### 3. VPC Configuration
- **File:** `VPC-Setup.yml`  
- **Purpose:** Sets up the networking layer:
  - VPC with public and private subnets  
  - Internet Gateway & NAT Gateway  
  - Route Tables and associated routes  
  - Security Groups for BE and FE

### 4. RDS Stack
- **File:** `RDS-Setup.yml`  
- **Purpose:** Creates a managed **RDS instance** in private subnets.  
- **Supports:** MySQL, PostgreSQL, or other engines.  
- **Features:**
  - Multi-AZ support (optional)  
  - Parameter Groups and Security Groups for secure DB access

### 5. Backend Setup (BE)
- **File:** `BE-Setup.yml`  
- **Purpose:** Deploys backend services with CI/CD pipeline.  
- **Components:**
  - **Auto Scaling Group (ASG)** for backend EC2 instances  
  - **Application Load Balancer (ALB)** with proper listeners and target groups  
  - **CodePipeline** integration for CI/CD from source repository to deployment  
  - CloudWatch monitoring & logging

### 6. Frontend Setup (FE)
- **File:** `FE-Setup.yml`  
- **Purpose:** Deploys frontend with CI/CD pipeline using **CloudFront**.  
- **Components:**
  - S3 bucket to host static files  
  - CloudFront distribution for global caching and HTTPS  
  - CI/CD pipeline for automated deployment from source to CloudFront  
  - Optional Route53 integration for custom domains

---

## ⚡ Deployment Order

1. Deploy `IAM-Setup.yml` to manage roles and policies.  
2. Deploy `VPC-Setup.yml` to manage networking resources.  
3. Deploy `RDS-Setup.yml` to manage the database.  
4. Deploy `BE-Setup.yml` to launch backend services and CI/CD pipeline.  
5. Deploy `FE-Setup.yml` to launch frontend services and CI/CD pipeline.  
6. Alternatively, deploy `Master-template.yml` to deploy all stacks sequentially.

---

## 🔧 Prerequisites

- AWS CLI configured with proper credentials  
- CloudFormation permissions to create VPCs, EC2, RDS, S3, CloudFront, IAM, and CodePipeline resources  
- Git repository for backend/frontend source code (for CI/CD pipelines)  

---

## 📦 Features

- Fully **modular architecture**: each stack can be deployed independently  
- Automated **CI/CD pipelines** for BE and FE  
- **Scalable backend** with ASG and ALB  
- **Global frontend delivery** with CloudFront  
- **Secure network design** using VPC, private/public subnets, and security groups  
- **Centralized IAM** roles and policies for consistent security  
- Master stack for **sequential orchestration**  

---
