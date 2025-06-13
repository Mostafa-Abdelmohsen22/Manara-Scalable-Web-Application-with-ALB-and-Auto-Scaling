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
- Routes traffic to EC2 app instan
