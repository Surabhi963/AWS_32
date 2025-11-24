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

![alt text](./images/image.png)

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

![alt text](./images/image-1.png)

![alt text](./images/image-2.png)

![alt text](./images/image-3.png)

![alt text](./images/image-4.png)

![alt text](./images/image-5.png)



✅ Successfully designed and implemented a highly available, scalable, and secure AWS infrastructure for the Spring 3 Hibernate application — using best practices of cloud architecture and DevOps deployment flow.

⭐ Author: Neha 
📚 AWS Ninja Batch-32

