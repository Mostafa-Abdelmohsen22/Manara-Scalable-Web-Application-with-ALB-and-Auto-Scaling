# Manara-Scalable-Web-Application-with-ALB-and-Auto-Scaling
# ![Architecture Diagram]([manara project.drawio (1).png](https://github.com/Mostafa-Abdelmohsen22/Manara-Scalable-Web-Application-with-ALB-and-Auto-Scaling/blob/main/manara%20project.drawio%20(1).png))

# Next is the link of App Video
  #### https://drive.google.com/file/d/1jbKPJcN9bLs-HrJy6HTU0AUMe6_74cka/view?usp=sharing


# Setup Guide for AWS Multi-AZ Architecture with High Availability and Monitoring

## Introduction
This guide provides step-by-step instructions to set up a highly available and scalable architecture in AWS. The architecture includes public and private subnets, EC2 instances, a MySQL master-slave database setup, an Application Load Balancer (ALB), CloudWatch monitoring, and automated scaling.

## Prerequisites

1. **AWS Account**: Ensure you have access to an AWS account.
2. **Tools**:
   - AWS Management Console
   - AWS CLI installed and configured
   - (Optional) Infrastructure as Code tools like Terraform or CloudFormation.
3. **IAM Permissions**:
   - Administrator access or specific permissions for managing VPC, EC2, RDS, IAM, and S3.
4. **Key Pair**: Create an EC2 key pair for SSH access.

## Architecture Overview
- **VPC CIDR**: `10.0.0.0/16`
- **Public Subnets**:
  - `10.0.1.0/24` (AZ-a)
  - `10.0.2.0/26` (AZ-b)
- **Private Subnets** (Application Layer):
  - `10.0.10.0/24` (AZ-a)
  - `10.0.11.0/24` (AZ-b)
- **Private Subnets** (Database Layer):
  - `10.0.20.0/24` (AZ-a, Master MySQL)
  - `10.0.21.0/24` (AZ-b, Slave MySQL)
- **Components**:
  - Application Load Balancer (ALB)
  - EC2 instances for the application layer
  - MySQL master-slave replication setup
  - CloudWatch for monitoring
  - IAM roles for secure access to resources

## Steps

### 1. Create the VPC and Subnets
1. Log in to the AWS Management Console.
2. Navigate to the VPC service.
3. Create a new VPC:
   - Name: `MyVPC`
   - CIDR block: `10.0.0.0/16`
4. Create subnets:
   - Public Subnets:
     - `10.0.1.0/24` in AZ-a
     - `10.0.2.0/26` in AZ-b
   - Private Subnets:
     - Application Layer: `10.0.10.0/24` (AZ-a), `10.0.11.0/24` (AZ-b)
     - Database Layer: `10.0.20.0/24` (AZ-a), `10.0.21.0/24` (AZ-b)
5. Associate route tables:
   - Public subnets: Attach an internet gateway.
   - Private subnets: Create NAT gateway(s) for internet access if required.

### 2. Launch EC2 Instances
1. Navigate to EC2 and launch instances:
   - Use the AMI and instance type of your choice.
   - Place the instances in the private application subnets (`10.0.10.0/24` and `10.0.11.0/24`).
2. Assign IAM roles to the instances for access to S3 and CloudWatch.
3. Configure security groups:
   - Allow HTTP/HTTPS traffic from the ALB.
   - Restrict SSH access to trusted IPs.

### 3. Set Up MySQL Master-Slave Replication
1. Launch two EC2 instances in the private database subnets (`10.0.20.0/24` and `10.0.21.0/24`).
2. Install MySQL on both instances.
3. Configure the master database:
   - Edit MySQL configuration to enable binary logging.
   - Create a replication user.
4. Configure the slave database:
   - Point it to the master’s endpoint.
   - Start the replication process.

### 4. Deploy the Application Load Balancer (ALB)
1. Navigate to the EC2 Load Balancers section.
2. Create an Application Load Balancer:
   - Assign it to the public subnets.
   - Configure the target group to include the private EC2 instances (application layer).
3. Set up health checks for the target group.

### 5. Configure CloudWatch and Auto-Scaling
1. Create a CloudWatch alarm:
   - Metric: CPU utilization > 80%.
   - Action: Trigger an auto-scaling policy.
2. Set up an auto-scaling group:
   - Minimum instances: 1
   - Maximum instances: 2
   - Scaling policy: Launch additional instances when alarm is triggered.

### 6. IAM and Security Setup
1. Create an IAM role with the following permissions:
   - Access to S3 for storing AMIs or logs.
   - Access to CloudWatch for monitoring.
2. Attach the IAM role to the EC2 instances.

### 7. Connect and Test
1. Use the ALB DNS name to access the application.
2. Verify that requests are distributed across the EC2 instances.
3. Simulate high CPU usage to trigger auto-scaling.
4. Test MySQL master-slave replication by making updates to the master and verifying the slave.

## Best Practices
- Use Elastic IPs for NAT gateways.
- Enable logging for ALB and store logs in S3.
- Regularly back up the MySQL database.
- Use AWS Systems Manager Session Manager for secure access to instances.

## Conclusion
By following these steps, you can deploy a highly available and scalable architecture in AWS. Ensure continuous monitoring and follow AWS best practices for security and performance.

