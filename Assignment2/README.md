# Deployment Strategies with Amazon S3 Integration
---
##  Problem Statement
[AWS Assignment 2](./problemStatement.txt)

### Key Requirements:
- Implement Recreate Deployment using EC2 instances with AMI snapshots
- Implement Rolling Deployment using Auto Scaling Groups
- Integrate Amazon S3 for static asset hosting and deployment artifact storage
- Document the deployment process with clear implementation steps
- Understand trade-offs between different deployment strategies
---
## Topics Covered
### Deployment Strategies:

- Recreate Deployment
- Rolling Deployment
- Blue-Green Deployment (Conceptual)
- A/B Deployment (Conceptual)
- Canary Deployment (Conceptual)


### AWS Services:

- Amazon EC2 (Elastic Compute Cloud)
- Amazon S3 (Simple Storage Service)
- Auto Scaling Groups (ASG)
- Amazon Machine Images (AMI)
- Launch Configurations/Launch Templates


### DevOps Concepts:

- Infrastructure as Code principles
- Zero-downtime deployments
- Asset management and CDN integration
- Version control and rollback strategies
---
## Objective
The primary objective of this assignment is to gain hands-on experience with various deployment strategies and understand their practical applications. You will:

- Learn to implement different deployment patterns for production environments
- Integrate Amazon S3 for efficient static asset delivery and artifact storage
- Configure Auto Scaling Groups for automated rolling deployments
- Understand the advantages and limitations of each deployment strategy
- Develop skills in managing infrastructure and application lifecycle on AWS

---
##  Key Features
### Recreate Deployment Implementation

- Manual EC2 instance deployment with complete environment setup
- AMI snapshot creation for rapid instance provisioning
- S3 integration for static asset hosting (images, CSS, JavaScript)
- Application configuration to fetch assets from S3 buckets

### Rolling Deployment Implementation

- Auto Scaling Group configuration with multiple instances
- Zero-downtime deployment using gradual instance replacement
- Launch configuration management for version updates
- S3-based deployment artifact storage and retrieval
- Health check integration for deployment validation

### Additional Features

- Comparison documentation of all deployment strategies
- Best practices for each deployment approach
- Rollback procedures and disaster recovery planning
- Cost optimization considerations

---
##  Prerequisites
### Knowledge Requirements

- Basic understanding of Linux/Unix command line
- Familiarity with web application architecture
- Understanding of AWS core services (EC2, S3)
- Basic networking concepts (VPC, Security Groups, Load Balancers)

### AWS Account Setup

- Active AWS account with appropriate permissions
- IAM user with EC2, S3, and Auto Scaling access
- AWS CLI installed and configured (optional but recommended)
- Understanding of AWS Free Tier limitations

### Software Requirements

- SSH client for EC2 instance access
- Web browser for AWS Console access
- Text editor for configuration files
- Basic web application for deployment (Node.js, Python, or any simple app)

### Technical Prerequisites

- Sample application ready for deployment
- Static assets (images, CSS, JS files) prepared
- Understanding of application server setup (Apache, Nginx, or similar)

---

##  Task Breakdown

###   Research and Understand Deployment Strategies

| Strategy | Description |
|-----------|--------------|
| **Recreate Deployment** | Stops the old version completely before deploying the new one. Simple but causes downtime. |
| **Rolling Deployment** | Gradually replaces old instances with new ones, ensuring zero downtime. |
| **Blue-Green Deployment** | Two environments exist — Blue (current) and Green (new). Traffic is switched to Green once verified. |
| **A/B Deployment** | Two versions (A and B) run simultaneously to test user experience or performance. |
| **Canary Deployment** | New version is released to a small subset of users first, then rolled out fully if stable. |

---



## Implementation Steps
### Part 1: Recreate Deployment
#### Step 1: Prepare Your Application

- Create or choose a simple web application
- Separate static assets (images, CSS, JavaScript) from application code
- Prepare application configuration files

#### Step 2: Set Up Amazon S3 Bucket

 **1.** Create an S3 bucket for static assets
  - Navigate to S3 in AWS Console
  - Create bucket with appropriate naming convention
  - Configure bucket for public access (for static assets)


**2.** Upload static assets to S3
  - Organize assets in folders (css/, js/, images/)
  - Set appropriate permissions for public read access


**3.** Configure S3 bucket policy for public access

<img width="783" height="255" alt="image" src="https://github.com/user-attachments/assets/0513cedd-75c2-434f-b874-c383798f4648" />



**4.** Note the S3 bucket URL for assets

#### Step 3: Launch EC2 Instance

**1.** Launch an EC2 instance (Amazon Linux 2 or Ubuntu recommended)

   - Choose appropriate instance type (t2.micro for testing)
   - Configure security group (allow HTTP/HTTPS and SSH)
   - Create or select key pair for SSH access
 

**2.** Connect to the instance via SSH

**3.** Install necessary software:
```
   # Update system packages
   sudo yum update -y  # For Amazon Linux
   # or
   sudo apt update && sudo apt upgrade -y  # For Ubuntu
   
   # Install web server and dependencies
   # (Commands will vary based on your application)
```

<img width="696" height="165" alt="image" src="https://github.com/user-attachments/assets/a31565e4-3530-47c8-8e85-c69b897671c3" />


#### Step 4: Deploy Application

 - Transfer application code to EC2 instance
 - Configure application to use S3 URLs for static assets
 - Set up web server configuration
 - Start the application and test functionality

#### Step 5: Create AMI Snapshot

**1.** Stop the EC2 instance (optional, but recommended for consistency)

**2.** Create AMI from the EC2 Console

 - Select your instance → Actions → Image and templates → Create image
 - Provide meaningful name and description
 - Wait for AMI creation to complete


**3.** Document the AMI ID for future deployments

<img width="651" height="160" alt="image" src="https://github.com/user-attachments/assets/c1427b44-ae55-42e8-9951-efbe30d27b60" />


#### Step 6: Test Recreate Deployment

 - Terminate the original instance
 - Launch new instance from AMI
 - Verify application functionality
 - Test asset loading from S3

<img width="2038" height="442" alt="image" src="https://github.com/user-attachments/assets/76004abd-ad59-4ee2-9c92-7f5f9acc486f" />

<img width="1370" height="287" alt="image" src="https://github.com/user-attachments/assets/2ff83b25-663e-437e-a99c-a74c28d88bff" />


<img width="2064" height="382" alt="image" src="https://github.com/user-attachments/assets/7f160b4d-b7f3-4b78-9f55-31c3c4bab32a" />

---
### Part 2: Rolling Deployment
#### Step 1: Prepare Updated Application Version

- Make changes to your application (v2)
- Update static assets in S3 if needed
- Test the new version locally

#### Step 2: Create S3 Bucket for Deployment Artifacts

**1.** Create new S3 bucket for deployment artifacts
**2.** Upload application deployment package

 - Create zip/tar file of application code
 - Upload to S3 with version naming (app-v1.zip, app-v2.zip)


**3.** Set appropriate bucket permissions

<img width="647" height="226" alt="image" src="https://github.com/user-attachments/assets/f1c2de90-3caa-4339-8b5b-f43de4d90750" />



#### Step 3: Create Launch Template (or Configuration)

**1.** Navigate to EC2 → Launch Templates
**2.** Create new launch template with:

 - AMI ID (from Part 1 or base AMI)
 - Instance type
 - Security groups
 - User data script for automated setup:

```
   #!/bin/bash
   # Install dependencies
   # Download application from S3
   aws s3 cp s3://your-bucket/app-v1.zip /home/ec2-user/
   # Extract and configure application
   # Start application
```

**3.** Save launch template

<img width="651" height="147" alt="image" src="https://github.com/user-attachments/assets/8242e7c2-b5f3-409b-a812-f51b763cba6d" />



#### Step 4: Create Auto Scaling Group

**1.**  Navigate to EC2 → Auto Scaling Groups → Create

**2.**  Configure ASG:

 - Select launch template
 - Set minimum instances: 2
 - Set desired capacity: 2
 - Set maximum instances: 4
 - Choose VPC and subnets (multi-AZ recommended)


**3.**  Configure health checks

**4.** Create and monitor ASG

<img width="655" height="341" alt="image" src="https://github.com/user-attachments/assets/6ec3afa0-5dee-43ee-8548-7a81b0602a20" />


#### Step 5: Perform Rolling Deployment

**1.** Create new launch template version with updated application:

 - Update user data to download app-v2.zip
 - Increment version number

<img width="651" height="151" alt="image" src="https://github.com/user-attachments/assets/ea231bc4-7f4d-4bcb-9897-3ba42fd8b6f5" />


**2.** Update Auto Scaling Group:

 - Navigate to ASG → Edit
 - Select new launch template version
 - Configure instance refresh settings:

   - Minimum healthy percentage: 50%
   - Maximum replacement percentage: 100%

**3.** Start instance refresh

**4.** Monitor deployment progress:

 - Watch instances being replaced gradually
 - Verify health checks
 - Test application during deployment


**5.**  Verify all instances running new version

#### Step 6: Documentation and Testing

 - Document deployment time and process
 - Test rollback procedure by reverting to previous version
 - Monitor application performance and logs
 - Create deployment runbook

<img width="2078" height="412" alt="image" src="https://github.com/user-attachments/assets/60a8d393-4080-4b03-a3c2-a9952c9a7b80" />

<img width="2074" height="396" alt="image" src="https://github.com/user-attachments/assets/5541c4af-2976-4ee6-9711-01617f76d5bb" />


### Part 3: Research and Documentation

- Compare Deployment Strategies
- Create documentation covering:

### Blue-Green Deployment:

 - Concept and architecture
 - Use cases and benefits
 - Implementation approach on AWS

<img width="680" height="385" alt="image" src="https://github.com/user-attachments/assets/06f2de84-406f-4b33-b219-8e3c59863e63" />

### Canary Deployment:

 - Progressive rollout strategy
 - Monitoring and metrics requirements
 - AWS implementation options

<img width="583" height="284" alt="image" src="https://github.com/user-attachments/assets/7e68fdb7-c2ab-4451-aac2-3e511d500537" />

---
## 📝 Deliverables

### Documentation:

- Detailed implementation guide with screenshots
- Comparison of deployment strategies
- Architecture diagrams
- Lessons learned and best practices


### Configuration Files:

- Launch template configurations
- User data scripts
- Application configuration files
- S3 bucket policies


### Testing Results:

- Deployment logs
- Performance metrics
- Rollback test results

---
## 🎓 Learning Outcomes
By completing this assignment, you will:

- Understand practical differences between deployment strategies
- Gain hands-on experience with AWS core services
- Learn to implement zero-downtime deployments
- Develop skills in infrastructure automation
- Understand asset management and CDN concepts
- Build confidence in production deployment scenarios

---
## 📚 Additional Resources

- AWS Auto Scaling Documentation
- Amazon S3 Static Website Hosting
- AWS Best Practices for Deployments
- Deployment Strategy Patterns



















