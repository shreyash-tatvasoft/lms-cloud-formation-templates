# AWS 3-Tier Web Application Templates

This repository contains **AWS CloudFormation templates** for deploying a complete 3-Tier Web Application infrastructure. It includes separate templates for networking, database, backend, frontend, and a master stack to orchestrate deployment.

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

---

## 📂 Templates

The repository contains the following **CloudFormation templates**:

### 1. Master Stack
- **File:** `Master-template.yml`  
- **Purpose:** Orchestrates the deployment of all other stacks in a sequential manner.  
- **Usage:** Deploy this stack first to automatically trigger all dependent stacks.

### 2. VPC Configuration
- **File:** `VPC-Setup.yml`  
- **Purpose:** Sets up the networking layer:
  - VPC with public and private subnets  
  - Internet Gateway & NAT Gateway  
  - Route Tables and associated routes  
  - Security Groups for BE and FE

### 3. RDS Stack
- **File:** `RDS-Setup.yml`  
- **Purpose:** Creates a managed **RDS instance** in private subnets.  
- **Supports:** MySQL, PostgreSQL, or other engines.  
- **Features:**
  - Multi-AZ support (optional)  
  - Parameter Groups and Security Groups for secure DB access

### 4. Backend Setup (BE)
- **File:** `BE-Setup.yml`  
- **Purpose:** Deploys backend services with CI/CD pipeline.  
- **Components:**
  - **Auto Scaling Group (ASG)** for backend EC2 instances  
  - **Application Load Balancer (ALB)** with proper listeners and target groups  
  - **CodePipeline** integration for CI/CD from source repository to deployment  
  - CloudWatch monitoring & logging

### 5. Frontend Setup (FE)
- **File:** `frontend-stack.yaml`  
- **Purpose:** Deploys frontend with CI/CD pipeline using **CloudFront**.  
- **Components:**
  - S3 bucket to host static files  
  - CloudFront distribution for global caching and HTTPS  
  - CI/CD pipeline for automated deployment from source to CloudFront  
  - Optional Route53 integration for custom domains

---

## ⚡ Deployment Order

1. Deploy `VPC-Setup.yml` to create networking resources.  
2. Deploy `RDS-Setup.yml` to create the database.  
3. Deploy `BE-Setup.yml` to launch backend services and CI/CD pipeline.  
4. Deploy `FE-Setup.yml` to launch frontend services and CI/CD pipeline.  
5. Alternatively, deploy `Master-template.yml` to deploy all stacks sequentially.

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
- Master stack for **sequential orchestration**  

