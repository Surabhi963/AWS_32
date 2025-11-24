#  Terraform Infrastructure as Code - NGINX
---
##  Problem Statement 
[AWS Assignmnet 4](./problemStatement.txt)

### Key Requirements:

- Create an AWS infrastructure for your tool (NGINX).
- Write Terraform code following best practices.
- Use modules for VPC, EC2, Security Groups.
- Store Terraform state remotely (S3 + DynamoDB for locking).
- Maintain reusable, readable, and scalable Terraform structure.

Document and version-control the project.
---
##  Topics Covered

- Terraform Variables, Outputs, Modules
- VPC, Subnets, Route Tables
- NAT Gateways, IGW
- Security Groups, Key Pairs
- EC2 provisioning for NGINX
- Remote State Management (S3 Bucket + DynamoDB)
- Provider version locking
- Terraform commands and workflow

---
##  Objective
The objective of this assignment is to design and deploy a complete AWS Infrastructure for hosting an NGINX-based application using Terraform (IaC).
You will:

Design the Architecture Diagram
Implement modular Terraform code
Use Remote Backend (S3 + DynamoDB) for Terraform state
Ensure provider version consistency
Deploy core infrastructure components (VPC, subnets, route tables, EC2, SGs, etc.)

### 1. Design Phase

Architecture Components
- 1 VPC
- Public Subnets (for bastion / NGINX EC2)
- Private Subnets (for future DB or internal services)
- Internet Gateway
- Route Tables
- Security Groups
- EC2 Instance running NGINX
- Remote State (S3 + DynamoDB)
  
The VPC contains public and private subnets. The public subnet hosts your NGINX EC2 instance. Security Groups allow SSH only from your IP and HTTP from 0.0.0.0/0. Terraform saves state to S3 and uses DynamoDB for locking.

### 2. Implementation Phase

- Write static Terraform code based on the approved architecture
- Provision VPC with public and private subnets.
- Ensue terraform >=1.5 is installed
- Latest AWS CLI
- Latest VScode
- AWS IAM User -- Programmatic accesss(Access key + Secret Key)


### 3. State Management

- Store Terraform state file (tfstate) in an S3 bucket
- Enable remote state management for team collaboration
- Maintain consistent provider versioning

---
##  Key Features
### Infrastructure Components

### 1. VPC Setup

- CIDR Block: 10.0.0.0/16
- DNS Hostnames Enabled
- Deployed in ap-south-1 region


### 2. Network Architecture

- 1 Public Subnet (10.0.1.0/24)
- 2 Private Subnets (10.0.2.0/24)
- Internet Gateway for public internet access
- NAT Gateway for private subnet outbound connectivity


### 3. Security Implementation

- Security Groups for instance-level firewall
- Network ACLs for subnet-level access control



### 4. State Management

- Remote state stored in S3 bucket: terraform-state-surabhi
- State file path: terraform/statefile.tfstate
- Version-controlled provider configuration

---

## 📊 Architecture Diagram

<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/86ec9311-8a37-4a20-b8bb-7c2e3e72aa8a" />

---
## 🛠️ Prerequisites

Before starting the implementation, ensure you have:

### Required Tools
- Terraform (version ~> 1.5)
- AWS CLI (configured with credentials)
- SSH Client for accessing instances

### AWS Requirements
- AWS Account with appropriate IAM permissions
- S3 Bucket named terraform-state-surabhi (must be created beforehand)
- AWS Credentials configured 

---
##  Project Structure
<img width="715" height="616" alt="image" src="https://github.com/user-attachments/assets/9abb8802-3ae9-4469-b5dd-5cf1a2c404b1" />


---
##  Implementation Steps
### Step 1: Create S3 Bucket for Remote State
```
    Bucket name: terraform-surabhi-state
    Region: ap-south-1
    Block Public Access: Enabled
    Versioning: Enabled (recommended)

```
### Step 2: Create DynamoDB Table for Locking
AWS Console → DynamoDB → Create Table:
```
    Table Name: terraform-lock
    Primary Key: LockID (String)
```
### Step 3: Configure Remote Backend (backend.tf)
```
    terraform {
      backend "s3" {
        bucket         = "terraform-surabhi-state"
        key            = "assignment04/terraform.tfstate"
        region         = "ap-south-1"
        dynamodb_table = "terraform-lock"
      }
    }
```
### Step 4: Write Terraform Code (Modules + Root)
```
    VPC Module Creates:
    - VPC
    - Subnets
    - IGW
    - Route Tables

    Security Group Module Creates:
    - SSH SG
    - HTTP SG

    EC2 Module Creates:
    - Key Pair
    - EC2 instance
    - Installs NGINX using user data
```

### Step 5: Terraform Commands
```
    - terraform init: Initialize provider & download modules
    - terraform validate: Validate Syntax
    - terraform plan -out=tfplan: Plan the infrastructure
    - terraform apply tfplan: Apply the plan

```

### Step 6: State Management (Remote State)
```
Using S3 for state storage and DynamoDB for state locking

Benefits:
✔ No local corruption
✔ Team collaboration
✔ Prevents race conditions
✔ Secure state
✔ Versioned state rollback

```

### Step 7: Security Implementation
```
SSH allowed only from your IP
HTTP allowed from all (0.0.0.0/0)
EC2 runs in public subnet with public IP
State file stored securely in private S3 bucket

```
### Step 8: Infrastructure Components (Explained)
```
1. VPC Setup

10.0.0.0/16 CIDR
Public + Private subnets
Internet Gateway
NAT Gateway (if required later)

2. Network Architecture

Public route table → IGW
Private route table → NAT gateway (optional for later services)

3. EC2 Configuration

Amazon Linux 2
NGINX installed automatically
Key pair enabled for SSH
Security groups applied

4. State Management

Remote S3 backend
DynamoDB locking

```

### Step 9: Verification (AWS Console Checks)
```
After terraform apply, verify:
1. VPC
  AWS Console → VPC → VPCs
  Find: vpc-XXXXXXXX (10.0.0.0/16)

2. Subnets
 public-subnet-1
 public-subnet-2
 private-subnet-1
 private-subnet-2

3. Route Tables
 Public route table → IGW

4. Security Groups
 Check rules for SSH & HTTP

5. EC2
 Instance should show:
  nginx-server
  Running
  Public IPv4: x.x.x.x
  Hit browser: http://<EC2-Public-IP>

```

### Step 10: Conclusion
```
This assignment demonstrates:

- Complete Terraform-based AWS infrastructure
- Modular & scalable IaC design
- Proper state management
- Secure and production-ready architecture
- Automated deployment of an NGINX server
You now have a fully functional cloud infrastructure built using Terraform best practices.

```
