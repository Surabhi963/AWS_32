# AWS Nginx High Availability, Auto Scaling, and CDN Integration Project


##  Overview
The client’s application has recently faced heavy load and performance issues. To overcome this, we will set up **NGINX as a reverse proxy** middleware with **High Availability (HA)** and **Disaster Recovery (DR)** on **AWS Cloud**.  
This project will be executed **day-wise**, implementing features like **AMI versioning**, **Auto Scaling**, **Load Balancing**, **S3 hosting**, **IAM roles**, and **CloudFront CDN**.

---

##  Day 1 — NGINX Setup, AMI Versioning & Auto Scaling

###  Objective
Set up NGINX as a middleware reverse proxy with versioning, HA, and DR capabilities.

###  Tasks
1. **Launch EC2 instance** and install **NGINX**.
2. Create **AMI-1** of the instance.
3. Launch new instance from AMI-1 → **Version V1**.
4. Make configuration changes → create **AMI-2 (Version V2)**.
5. Launch new instance from AMI-2 → **Version V2**.
6. Maintain both AMIs (V1 and V2) for rollback and upgrade testing.

<img width="1853" height="775" alt="image" src="https://github.com/user-attachments/assets/8a1363bb-d2d3-40c0-9eaf-349571089dae" />

### Auto Scaling Setup
- Create **Launch Template** using NGINX AMIs.
- Attach template to an **Auto Scaling Group (ASG)**.
- Attach ASG to an **Application Load Balancer (ALB)**.

<img width="903" height="407" alt="image" src="https://github.com/user-attachments/assets/12e1c4f5-8f1e-4aa7-aab0-91d64ff2fac8" />



###  Load Testing
Apply policies and analyze scaling behavior:
- **Avg CPU Utilization Policy**
- **Network In/Out Policy**
- **ALB Request Count per Target Policy**

<img width="1858" height="854" alt="image" src="https://github.com/user-attachments/assets/f266c202-1ebd-4bb7-9475-ac99931c96a9" />

<img width="2786" height="1077" alt="image" src="https://github.com/user-attachments/assets/586b7d59-a946-4c95-a71b-8eae417b14fa" />


Use stress testing tools (like `stress-ng`) to trigger ASG scaling.

<img width="1847" height="980" alt="image" src="https://github.com/user-attachments/assets/fb71d4da-c24a-41cd-b844-39a3d035cd17" />

<img width="2897" height="1475" alt="image" src="https://github.com/user-attachments/assets/c5c000b1-f76f-4eb6-bd19-73f7d0853242" />

<img width="2627" height="1411" alt="image" src="https://github.com/user-attachments/assets/90b06c42-9269-42a8-972b-85640826c149" />



###  Version Rollback
If **V2** is not compatible:
- Update Launch Template to use **AMI-1 (V1)**.
- Perform **Rolling Deployment** to revert to the previous version.
delete the version 2 instance 

<img width="2897" height="1475" alt="image" src="https://github.com/user-attachments/assets/c5c000b1-f76f-4eb6-bd19-73f7d0853242" />
---

##  Day 2 — NGINX as Web Host + S3 Integration

###  Objective
Use NGINX to host a webpage that fetches static assets (images) from **S3**, deployed via **EC2** only.

###  Tasks
1. Clone image content from **Git repository (VCS)**.
2. Use **AWS CLI** on EC2 to:
   - Upload images to **S3 bucket** (without using access/secret keys — use IAM roles).
3. Modify NGINX configuration to host a **frontend webpage** that directly loads images from the **S3 bucket**.

Step 1: IAM Role Setup (no keys needed)

 Goal: Allow your EC2 to access S3 securely (no access/secret keys).

1️⃣ Create IAM Role

Go to AWS Console → IAM → Roles → Create role

Choose Trusted Entity: AWS service

Choose Use case: EC2

Click Next

2️⃣ Attach Policy

Choose: AmazonS3FullAccess

(or you can create a custom policy later with limited access)

3️⃣ Name Role

Example: EC2-S3Access-Role

4️⃣ Attach Role to EC2

Go to your EC2 instance → Actions → Security → Modify IAM Role → Select EC2-S3Access-Role → Apply.


<img width="2705" height="892" alt="image" src="https://github.com/user-attachments/assets/6c13f565-8782-4967-9c64-68e240fcda84" />

<img width="1294" height="1163" alt="image" src="https://github.com/user-attachments/assets/55a187b1-527a-4210-aabd-8d14bdcf66af" />

<img width="2809" height="1334" alt="image" src="https://github.com/user-attachments/assets/f48d16bc-5694-456d-8a3e-2fe4e4e6765c" />


<img width="1245" height="486" alt="image" src="https://github.com/user-attachments/assets/099d424c-5e84-4574-88bd-cef39ee37edb" />

## Day 3 — Auto Scaling Group Health Check Test
 Objective

Verify ASG self-healing and desired state maintenance.

## Tasks

Log in to an EC2 instance under ASG.

Modify system settings or stop NGINX service to simulate an unhealthy state.

Observe ASG:

Detects unhealthy instance.

Terminates and replaces it with a new instance automatically.

<img width="2343" height="1283" alt="image" src="https://github.com/user-attachments/assets/5e8307db-da82-4200-802c-1d501409fe7a" />


<img width="1278" height="621" alt="image" src="https://github.com/user-attachments/assets/8a71e7a1-e606-4b65-9653-d17567b91714" />


## Day 4 — Path-Based Routing with ALB

 Objective

Host multiple NGINX webpages behind one ALB using path-based routing.

## Setup Architecture

1 Bastion Host in Public Subnet.

2 NGINX EC2 Instances in Private Subnets.

## Security Rules

Bastion host SSH (port 22): only from your public IP.

EC2 (port 22): only from Bastion host.

NGINX port 80: accessible only from ALB.

ALB port 80: accessible only from your public IP.

## Configuration
Path	Server	Content
/ninja1	EC2-1	Displays Image-1
/ninja2	EC2-2	Displays Image-2
 Steps

Create Target Groups for both NGINX servers.

Create Application Load Balancer (ALB).

Add Listener Rules:

/ninja1 → Target Group 1

/ninja2 → Target Group 2

Upload images for both pages to S3.

Fetch and display them on respective NGINX pages.

## Day 5 — S3 Bucket, IAM Role & Access Restriction
 Objective

Secure S3 access using IAM policies and roles.

## Tasks

Create S3 Bucket in US-East-1 region.

Create two folders inside:
```
s3://my-website-bucket/
├── prod/
└── nonprod/
````

Upload different images in both folders.

Create an IAM Role with S3 Full Access (for EC2 and NGINX).

Create an IAM User with the following restrictions:

Can access only nonprod folder.

Cannot access prod folder.

## IAM & Bucket Policy Highlights

Bucket access only for:

Root user

IAM user

NGINX EC2 instances

Restrict IAM user from prod/*

Allow access to nonprod/*

<img width="2597" height="597" alt="image" src="https://github.com/user-attachments/assets/2fb79816-d011-4413-8c64-b552f9e18b07" />

```
s3://my-nginx-images-bucket-786/
├── prod/
│ ├── image1.jpg
│ └── image2.png
└── noprod/
├── test1.jpg
└── test2.png
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListingOnlyNoprodFolder",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::my-nginx-images-bucket-786",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "noprod/*"
                }
            }
        },
        {
            "Sid": "AllowAccessToNoprodObjects",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::my-nginx-images-bucket-786/noprod",
                "arn:aws:s3:::my-nginx-images-bucket-786/noprod/*"
            ]
        },
        {
            "Sid": "DenyProdFolderAccess",
            "Effect": "Deny",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::my-nginx-images-bucket-786/prod",
                "arn:aws:s3:::my-nginx-images-bucket-786/prod/*"
            ]
        }
    ]
}
```
## Day 6 — CloudFront CDN Integration
 Objective

Reduce latency by fetching S3-hosted images via CloudFront CDN.

## Tasks

Validate trust relationship in IAM (required for CDN integration).

Create CloudFront Distribution for the existing S3 bucket.

Use the same IAM Role used by EC2 (no policy modification).

Configure CloudFront URL in NGINX for image access:

<img width="2541" height="1477" alt="image" src="https://github.com/user-attachments/assets/8cade50d-736f-45b2-a53e-03c0ba62ec50" />

<img width="2828" height="1498" alt="image" src="https://github.com/user-attachments/assets/ce016ee0-1ebf-42d0-92a8-c9290112b2ff" />

  
 
 
 







