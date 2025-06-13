# Manara-Scalable-Web-Application-with-ALB-and-Auto-Scaling
# Manara Project - High Availability AWS Architecture

This project outlines a scalable, secure, and highly available web application architecture deployed on AWS. It leverages auto-scaling, secure access via IAM and Systems Manager, and multi-AZ MySQL replication.

---

## 📐 Architecture Overview

### 🔹 VPC Configuration
- **Region**: `us-east-1`
- **CIDR Block**: `10.0.0.0/16`
- **Availability Zones**:
  - `AZ-2a`
  - `AZ-2b`

### 🔸 Subnets
| Type            | AZ-2a              | AZ-2b              |
|-----------------|--------------------|--------------------|
| Public Subnet   | 10.0.1.0/24        | 10.0.2.0/26        |
| App Subnet      | 10.0.10.0/24       | 10.0.11.0/24       |
| Data Subnet     | 10.0.20.0/24       | 10.0.21.0/24       |

---

## 🧱 Components

### 1. Load Balancer
- Deployed in public subnets
- Routes traffic to EC2 app instances across availability zones

### 2. EC2 Application Layer
- Instances launched in private app subnets
- Use **AMI + User Data** for bootstrap
- IAM role attached for:
  - S3 access
  - CloudWatch logging
  - Session Manager connectivity

### 3. Auto Scaling with CloudWatch
- If CPU > 80%, CloudWatch triggers new EC2 instance launch
- Instances created from prebuilt AMI stored in S3

### 4. MySQL Database Layer
- **Master** in `10.0.20.0/24` (AZ-2a)
- **Slave** in `10.0.21.0/24` (AZ-2b)
- Master-slave replication for redundancy and failover

---

## 🔐 IAM and Access Control

### IAM Role: `EC2InstanceRole`
Attached to all EC2 app servers.

#### Permissions:
- Access to:
  - **Amazon S3** for bootstrapping
  - **CloudWatch Logs** for observability
  - **Session Manager (SSM)** for secure shell access

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "logs:CreateLogStream",
    "logs:PutLogEvents",
    "ssm:StartSession",
    "ssm:DescribeSessions",
    "ssmmessages:*"
  ],
  "Resource": "*"
}

