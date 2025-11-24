#  Load Balancer & Auto Scaling Group
---

##  Objective

The goal of this assignment is to design and implement a **highly available, scalable, and secure cloud infrastructure** for deploying a **Spring 3 Hibernate application**. The application will run on **private servers**, ensuring controlled access and security.

### Application Repository
[Spring 3 Hibernate Application](https://github.com/opstree/spring3hibernate.git)

---

##  Infrastructure Overview

This infrastructure is designed on AWS to ensure **high availability**, **scalability**, and **security**.  

### Key Components

1. **VPC (Virtual Private Cloud)**
   - Acts as the main network for all resources.
   - Divided into public and private subnets for better traffic management and isolation.

2. **Subnets**
   - **Public Subnets:** Contain the Load Balancer (LB) and NAT Gateway.
   - **Private Subnets:** Contain application servers (EC2 instances) managed by the Auto Scaling Group (ASG).

3. **Load Balancer**
   - An **Application Load Balancer (ALB)** distributes incoming HTTP/HTTPS traffic to EC2 instances in private subnets.
   - Ensures fault tolerance and even load distribution.

4. **Auto Scaling Group (ASG)**
   - Automatically adjusts the number of application servers based on incoming traffic or performance metrics.
   - Ensures optimal performance during peak loads and cost efficiency during low traffic periods.

5. **Security Groups**
   - Control inbound and outbound traffic.
   - The Load Balancer allows HTTP/HTTPS traffic from the internet.
   - The application servers allow traffic only from the Load Balancer.

6. **NAT Gateway**
   - Provides **outbound internet access** for private subnets to download dependencies or updates without exposing them to the public internet.

7. **EC2 Instances**
   - Host the Spring 3 Hibernate application.
   - Automatically scaled by the ASG and registered behind the ALB.

---

##  Infrastructure Architecture Diagram

Below is the logical flow of the infrastructure:
```
[User]
|
↓
[Application Load Balancer] (Public Subnet)
|
↓
[Auto Scaling Group - EC2 Instances] (Private Subnet)
|
↓
[Spring 3 Hibernate Application]

```


** Architecture Diagram Screenshot:**  
 

<img width="672" height="642" alt="image" src="https://github.com/user-attachments/assets/80900b3e-08b6-4cac-acd3-d74977d6e245" />




---

##  Implementation Steps

1. **Create a VPC**
   - Configure CIDR block (e.g., `10.0.0.0/16`).
   - Create public and private subnets across multiple Availability Zones.

2. **Create and Configure Security Groups**
   - **Load Balancer SG:** Allow inbound HTTP/HTTPS (80, 443) from the internet.
   - **App Server SG:** Allow inbound traffic only from the Load Balancer SG on port 8080.

3. **Deploy NAT Gateway**
   - Place it in a public subnet and associate it with the private route table.

4. **Launch Template / Launch Configuration**
   - Define instance type, AMI, security group, and user data script for the Spring application deployment.

5. **Create Auto Scaling Group (ASG)**
   - Associate it with the launch template.
   - Set minimum, maximum, and desired instance counts.
   - Attach the ASG to the Load Balancer target group.

6. **Configure Load Balancer**
   - Create an Application Load Balancer (ALB) in the public subnets.
   - Attach the target group that contains private EC2 instances managed by the ASG.

7. **Deploy Application**
   - Clone and deploy the Spring 3 Hibernate app on the EC2 instances.
   - Verify successful deployment through the Load Balancer DNS name.


---

##  Screenshots

_Add relevant screenshots here after implementation:_

### 1. VPC Configuration

<img width="762" height="202" alt="image" src="https://github.com/user-attachments/assets/8da46a49-7270-495a-a67a-e4c24a7332e2" />


### 2. Load Balancer setup
<img width="1238" height="511" alt="Screenshot 2025-11-04 at 21 31 47" src="https://github.com/user-attachments/assets/e61a8333-6d11-4435-8d80-f8e22def8f09" />

---
<img width="1244" height="392" alt="Screenshot 2025-11-04 at 21 33 00" src="https://github.com/user-attachments/assets/7db56abb-74e2-429a-a2fd-5553c66732e7" />

---
<img width="1449" height="553" alt="image" src="https://github.com/user-attachments/assets/97a2c74f-7104-463b-ab1e-8be5f6874bc1" />

### 3. Auto Scaling Group configuration
<img width="1233" height="667" alt="Screenshot 2025-11-04 at 21 34 46" src="https://github.com/user-attachments/assets/ad2b9368-69d7-4313-8391-a7490155005e" />

### 4. EC2 Instances dashboard
<img width="1261" height="170" alt="Screenshot 2025-11-04 at 21 36 14" src="https://github.com/user-attachments/assets/69e3b0cc-c1c0-468b-a354-d007b0217cdd" />


### 5. Deployment

  <img width="1520" height="782" alt="image" src="https://github.com/user-attachments/assets/7cddb698-a591-4d4e-9836-6274f9d4658b" />

---

##  Key Learnings

- Designing a secure and scalable AWS architecture using **VPC, ALB, ASG, and NAT Gateway**.  
- Implementing **private subnet isolation** for better security.  
- Understanding **dynamic scaling** and **load balancing** for performance optimization.  
- Deploying and managing a real-world **Spring 3 Hibernate application** in a cloud environment.

---

##  Deliverables

1. Infrastructure Architecture Diagram (with all components labeled).  
2. Implementation Code / Terraform or CloudFormation templates (if applicable).  
3. Screenshots of all AWS components and working application.  
4. This `README.md` file documenting your process.

---




