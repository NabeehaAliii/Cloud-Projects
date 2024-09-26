# **AWS VPC with Private EC2 Access via Bastion Host and NAT Instance**

This project showcases the setup of a secure VPC in AWS, demonstrating private and public EC2 access with a **Bastion Host** and **NAT Instance**. The configuration allows SSH access to private EC2 instances while routing internet access through a NAT instance for enhanced security.

---

## **Table of Contents**

- [Project Overview](#project-overview)
- [Architecture Diagram](#architecture-diagram)
- [Components](#components)
- [Pre-Requisites](#pre-requisites)
- [Step-by-Step Setup](#step-by-step-setup)
  - [1. IAM User and Role Setup](#1-iam-user-and-role-setup)
  - [2. Create a Custom VPC](#2-create-a-custom-vpc)
  - [3. EC2 Instances](#3-ec2-instances)
  - [4. Configure Bastion Host](#4-configure-bastion-host)
  - [5. NAT Instance Setup](#5-nat-instance-setup)
  - [6. Security Groups](#6-security-groups)
  - [7. Route Tables](#7-route-tables)
  - [8. IAM Role for CloudWatch](#8-iam-role-for-cloudwatch)
- [Validation](#validation)
- [Conclusion](#conclusion)

---

## **Project Overview**

This project creates a secure AWS network setup by:
- Deploying a **Custom VPC** with public and private subnets.
- Launching a **Bastion Host** for secure SSH access to private EC2 instances.
- Configuring a **NAT Instance** to provide internet access for the private instances.
- Setting up proper **Security Groups** and **Route Tables** to manage traffic flow and security.

---

## **Architecture Diagram**

![Architecture](https://github.com/user-attachments/assets/ec94184d-5699-410c-9ed3-dcb032f1316d)

---

## **Components**

1. **IAM User**:
   - IAM user with full access to EC2 and VPC for managing resources.
2. **Custom VPC**:
   - A VPC with CIDR block `10.0.0.0/16`.
   - Two subnets: 
     - **Private Subnet** (`10.0.2.0/26`) in `ap-south-1a`.
     - **Public Subnet** (`10.0.1.0/26`) in `ap-south-1b`.
3. **EC2 Instances**:
   - **Public EC2 Instance**: For Bastion Host and NAT Instance in public subnet.
   - **Private EC2 Instance**: Resides in private subnet.
4. **Bastion Host**:
   - SSH access point to private instances.
5. **NAT Instance**:
   - Provides internet access to private EC2 instances.
6. **Security Groups**:
   - Three Security Groups to manage inbound/outbound traffic.
7. **Route Tables**:
   - Route tables to control traffic routing in the VPC.
8. **IAM Role for CloudWatch**:
   - Monitors EC2 instances and collects logs via CloudWatch.

---

## **Pre-Requisites**

- AWS Account
- IAM user with VPC and EC2 permissions.
- Basic understanding of networking and AWS components.

---

## **Step-by-Step Setup**

### **1. IAM User and Role Setup**

1. **Create an IAM User**:
   - Name: `ec2-vpc-user`
   - Attach policies:
     - `AmazonEC2FullAccess`
     - `AmazonVPCFullAccess`

2. **IAM Role for EC2 CloudWatch Access**:
   - Attach the `CloudWatchAgentServerPolicy` policy to the role to enable logging and monitoring for instances.
   - Assign the role to the private EC2 instance.

---

### **2. Create a Custom VPC**

1. Navigate to the **VPC console**.
2. **Create a new VPC**:
   - CIDR: `10.0.0.0/16`.
   - Name: `MyCustomVPC`.
3. **Create two subnets**:
   - Public Subnet (`10.0.1.0/26`)
   - Private Subnet (`10.0.2.0/26`)
4. **Create and Attach an Internet Gateway**.

---

### **3. EC2 Instances**

1. **Public EC2 Instance**:
   - Launch an Amazon Linux 2 instance in the public subnet.
   - Assign a public IP address.
   - Use this instance as the **Bastion Host**.
   
2. **Private EC2 Instance**:
   - Launch an Amazon Linux 2 instance in the private subnet without a public IP.
   - This instance will be accessed via the Bastion Host.

---

### **4. Configure Bastion Host**

1. **Security Group for Bastion Host**:
   - Inbound: Allow SSH from your IP.
   - Outbound: Allow SSH to the private instance’s security group.
   
2. **SSH into Bastion Host**:
   - Use SSH with the key pair and the Bastion Host’s public IP to connect.

---

### **5. NAT Instance Setup**

1. **Launch the NAT Instance**:
   - Use Amazon Linux 2 AMI.
   - Disable Source/Destination checks.
2. **Security Group for NAT Instance**:
   - Inbound: Allow HTTP/HTTPS traffic.
   - Outbound: Allow all traffic to the internet.

---

### **6. Security Groups**

1. **SG_Private**:
   - Inbound: Allow SSH from SG_Bastion.
   - Outbound: Allow all traffic to the NAT instance for internet access.

2. **SG_Bastion**:
   - Inbound: Allow SSH from your IP.
   - Outbound: Allow SSH to SG_Private.

3. **SG_NAT**:
   - Inbound: Allow HTTP/HTTPS from SG_Private.
   - Outbound: Allow all traffic to the internet.

---

### **7. Route Tables**

1. **Public Route Table**:
   - Destination: `0.0.0.0/0` → **IGW** (Internet Gateway).

2. **Private Route Table**:
   - Destination: `0.0.0.0/0` → **NAT Instance Private IP**.

---

### **8. IAM Role for CloudWatch**

1. Attach the `CloudWatchAgentServerPolicy` to an IAM role.
2. Attach the role to the EC2 instance in the private subnet to monitor and log metrics.

---

## **Validation**

1. **SSH Access**:
   - Connect to the private instance via the Bastion Host.
2. **Internet Access**:
   - Verify that the private instance can access the internet via the NAT instance.
3. **CloudWatch Monitoring**:
   - Check CloudWatch logs for EC2 metrics and monitoring.

---

## **Conclusion**

This project demonstrates a secure and scalable network setup using AWS VPC, EC2, Bastion Host, NAT Instance, and CloudWatch. It showcases best practices for accessing private instances securely and configuring outbound internet access through a NAT instance.
