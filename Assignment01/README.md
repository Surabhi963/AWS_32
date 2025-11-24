# 🌩️ AWS Assignment-01 | Load Balancer & Auto Scaling Group

> 🎯 **Objective:**  
> Design and implement a **highly available, scalable, and secure** cloud infrastructure for deploying the  
> [Spring 3 Hibernate Application](https://github.com/opstree/spring3hibernate.git) on AWS.



## 🧠 Project Overview

This assignment focuses on designing and deploying a cloud-native architecture that ensures:

- 🔹 **High Availability** via Load Balancer and Multi-AZ setup  
- 🔹 **Scalability** using Auto Scaling Groups (ASG)  
- 🔹 **Security** by isolating application servers in private subnets  
- 🔹 **Performance Optimization** using NAT and routing controls  



## 🏗️ Infrastructure Components

| 🧩 Component | ⚙️ Description |
|--------------|----------------|
| **VPC** | Acts as a secure network boundary for all AWS resources |
| **Public Subnets** | Hosts Load Balancer (ALB) for internet-facing traffic |
| **Private Subnets** | Hosts EC2 instances running Spring 3 Hibernate app |
| **Application Load Balancer (ALB)** | Routes client requests to backend EC2s in private subnets |
| **Auto Scaling Group (ASG)** | Automatically adjusts instance count based on traffic load |
| **NAT Gateway** | Enables private instances to access the internet securely |
| **Security Groups** | Controls inbound/outbound traffic rules |
| **Route Tables** | Ensures proper routing between subnets and gateways |


## 🗺️ Infrastructure Diagram

<img width="657" height="582" alt="image" src="https://github.com/user-attachments/assets/6baede54-1324-41d1-8aed-8edaface0caa" />


🧩 Step-by-Step Implementation Plan
1️⃣ VPC Setup

Create a VPC (CIDR: 10.0.0.0/16)

Add 2 Public Subnets and 2 Private Subnets (in different AZs)

2️⃣ Internet Gateway & NAT Gateway

Attach Internet Gateway for public internet access

Deploy NAT Gateway in a public subnet for private subnet connectivity

3️⃣ Security Groups

LB SG → Allow HTTP/HTTPS from the internet

App SG → Allow traffic only from LB SG

Database SG → Restrict access to App SG only

4️⃣ Application Load Balancer

Create an ALB in public subnets

Target Group → Points to app servers in private subnets

5️⃣ Auto Scaling Group

Configure Launch Template with Spring 3 Hibernate deployment script

Attach to Target Group

Configure scaling policy (e.g., CPU > 70%)

6️⃣ Spring 3 Hibernate App Deployment

Clone repo:

git clone https://github.com/opstree/spring3hibernate.git


Deploy app on EC2 private servers (via user data or manual SSH)


✅ Traffic restricted strictly between defined layers.

🧭 Design Highlights

🌐 Public-Private Subnet Segregation

🔁 Auto-healing with ASG

🧱 Defense-in-depth Security Model

⚙️ Load-balanced, fault-tolerant architecture

📈 Optimized for cost and performance

🧰 Tools & Services Used
Category	AWS Service
Networking	VPC, Subnets, Route Tables, IGW, NAT
Compute	EC2, Auto Scaling Group
Load Balancing	Application Load Balancer (ALB)
Security	Security Groups, IAM Roles
Monitoring	CloudWatch
Application	Spring 3 Hibernate
🏁 Outcome

<img width="1452" height="756" alt="image" src="https://github.com/user-attachments/assets/04c5fbba-c773-4265-bfaa-1c3fefc9fe64" />


<img width="1442" height="387" alt="image" src="https://github.com/user-attachments/assets/44b16985-42cc-4a09-8874-d00272f276ab" />


<img width="1447" height="722" alt="image" src="https://github.com/user-attachments/assets/bff4c6ee-e910-49ce-bd91-de751b38c620" />


<img width="1450" height="757" alt="image" src="https://github.com/user-attachments/assets/c0840bbc-752c-4e78-9ba7-9990833f506d" />


<img width="1448" height="772" alt="image" src="https://github.com/user-attachments/assets/aed63a0b-22f9-45e3-87d8-37762024f7dd" />




✅ Successfully designed and implemented a highly available, scalable, and secure AWS infrastructure for the Spring 3 Hibernate application — using best practices of cloud architecture and DevOps deployment flow.

⭐ Author: Neha 
📚 AWS Ninja Batch-32

