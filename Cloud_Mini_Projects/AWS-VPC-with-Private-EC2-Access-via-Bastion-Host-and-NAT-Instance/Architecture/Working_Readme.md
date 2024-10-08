# VPC Setup and SSH Connection with Bastion Host

## 1. IAM User Setup

We created an IAM user vpc_ec2 with restricted permissions to manage EC2 and VPC services.

### Image: IAM User Created

<img width="960" alt="IAM_USER_Created" src="https://github.com/user-attachments/assets/d3709897-8225-4a2d-8eda-72a9f7a86ec5">

---

## 2. VPC Configuration Overview

We started by creating a custom VPC named vpc_ec2 with two subnets:
- **Public Subnet** in ap-south-1b for internet access.
- **Private Subnet** in ap-south-1a for internal communication without public internet access.

### Steps:
- Create a VPC with CIDR: 10.0.0.0/16
- Create two subnets:
  - **Public Subnet**: CIDR 10.0.1.0/24
  - **Private Subnet**: CIDR 10.0.2.0/24

### Image: VPC Setup

<img width="960" alt="vpc_setup" src="https://github.com/user-attachments/assets/cefba7f5-49e9-4258-b33d-7d6eb62ba1a3">


- **Internet Gateway (IGW)** attached to the VPC.
- Route tables for subnets configured for proper routing:
  - Public subnet uses the IGW for internet access.
  - Private subnet has local-only routes for internal communication.

### Image: Route Table Setup for Public Subnet

<img width="960" alt="set_public_subnet_rt" src="https://github.com/user-attachments/assets/e8cd51cd-79eb-4864-9e09-82e596675dfe">

---

## 3. SSH into Public Instance (Bastion Host)

Made two instances Private and Public, IP Address is enable in public.

### Problem Statement:
We needed a way to securely connect to the **Private Instance**, which cannot be accessed directly from the internet. To solve this, we introduced a **Bastion Host**. The Bastion Host, set up in the public subnet, acts as a secure intermediary that allows access to the Private Instance.

In this learning scenario, the **Public EC2 Instance** is used as the Bastion Host to help us connect to our Private Instance.

### Steps to SSH into Bastion Host:
1. Upload your PEM key to CloudShell.
2. Set permissions using chmod 400 Practicekey.pem.
3. SSH into the Bastion Host with its public IP.

bash
ssh -i Practicekey.pem ubuntu@13.127.93.2


### Image: SSH into Public Instance

<img width="960" alt="1" src="https://github.com/user-attachments/assets/62e65666-383a-424d-9277-5d90c3a9a9a3">

---

## 4. SSH from Bastion Host to Private Instance

Once you’re connected to the **Bastion Host**, you need to SSH into the **Private Instance** using its private IP address.

### Problem Statement:
Although we can access the Bastion Host, the **Private Instance** doesn't have a public IP. This is why we use the **Bastion Host** to connect to it. Additionally, we need to transfer the PEM key to the Bastion Host to authenticate.

### Steps to SSH into Private Instance:
1. Use vi to create the PEM file on the Bastion Host.
2. Paste the contents of your PEM key and set permissions using chmod 400.
3. SSH into the Private Instance.

bash
vi private_instance_key.pem   # Copy PEM key content
chmod 400 private_instance_key.pem
ssh -i private_instance_key.pem ubuntu@10.0.2.41


### Image: SSH into Private Instance

<img width="960" alt="2" src="https://github.com/user-attachments/assets/f91b28ee-6ea2-4513-af5d-aede3b259665">


---

## 5. Setting Up NAT Instance for Internet Access

### Problem Statement:
Now that we have access to the **Private Instance** through the **Bastion Host**, we realize that the private instance **cannot access the internet**. Without internet access, we cannot download any software or update packages in the Private Instance.

However, we can still **communicate between the instances**, such as transferring files or executing internal commands. For example:
- **Ping** the private instance from the Bastion Host to confirm the internal connection.
  
To solve the internet access issue, we need to introduce a **NAT Instance** (for learning purposes, as NAT Gateways incur charges).

### Why Use NAT Instance:
The **NAT Gateway** is a managed service in AWS, but it comes with an extra cost. To avoid these costs, we’ll set up a **NAT Instance**, which is a free option that performs the same function, allowing the private instance to access the internet without exposing it directly.

### NAT Instance Setup

1. **Create the NAT Instance**:
    - Use the NAT AMI: `amzn-ami-vpc-nat-2018.03.0.20220118.0-x86_64-ebs`.
    - Launch the instance in the **Public Subnet**.

2. **Security Group for NAT Instance**:
    - Allow **All Traffic** inbound from the **Private Subnet** CIDR (`10.0.2.0/24`).
    - Allow **ICMP** for testing pings.

3. **Route Table Configuration**:
    - Modify the route table of the **Private Subnet** to route internet-bound traffic (0.0.0.0/0) to the NAT Instance.
  
   ### Image: Route Table Modification for NAT Instance

  <img width="960" alt="change_Private_rt_for_NAT" src="https://github.com/user-attachments/assets/c101b93e-50d6-4e75-98b3-d7587364e4b6">


4. **Disable Source/Destination Check**:
    - Disable the **source/destination check** for the NAT instance to allow it to forward traffic.
  
   ### Image: Disable Source/Destination Check

  <img width="960" alt="StopSource_DestinationCheck" src="https://github.com/user-attachments/assets/555745c0-5b88-45ff-aba5-3728cf716e8d">

6. **Test Connectivity**:
    - SSH into the **Private Instance** and try to ping an external IP (e.g., 8.8.8.8).
    - You should now have internet access on your private instance through the NAT instance.
      
---

Let’s continue from **Adding an IAM Role for EC2 Instances and CloudWatch**:

---

## 6. Adding an IAM Role for EC2 Instances and CloudWatch

### Problem Statement:
We wanted to set up CloudWatch to monitor our EC2 instances and capture metrics for CPU usage, memory, disk, and network activity. This requires adding an IAM role with the necessary permissions for EC2 instances to interact with CloudWatch.

### Steps to Add the IAM Role:
1. Go to the **IAM Management Console** and create a new role.
2. Select **AWS Service** and choose **EC2**.
3. Attach the following policies:
   - `CloudWatchAgentServerPolicy` (allows the instance to use the CloudWatch Agent).
   - `CloudWatchLogsFullAccess` (grants full access to CloudWatch logs).
   - `IAMFullAccess` (optional but useful for managing IAM roles).

### Image: IAM Role Creation

<img width="960" alt="IAM_Role_for_cloudWatch" src="https://github.com/user-attachments/assets/10a9c081-4a21-4077-a1a1-e4dd8947577f">


4. Attach the role to your EC2 instances.

---

## 7. Installing and Configuring CloudWatch Agent

### Problem Statement:
To send detailed logs and metrics (like memory usage) to CloudWatch, we needed to install and configure the CloudWatch Agent on both the **Public** and **Private** EC2 instances.

### Steps to Install the CloudWatch Agent:
1. **Download the CloudWatch Agent** on the EC2 instances.

   ```bash
   wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
   sudo dpkg -i amazon-cloudwatch-agent.deb
   ```

### Image: CloudWatch Agent Installation

<img width="960" alt="CloudWatchInstallationANDConfig" src="https://github.com/user-attachments/assets/fae7efe4-d2dc-4ab5-93de-bed733134695">

2. **Configure the CloudWatch Agent**:

   - Use the CloudWatch agent wizard to set up metrics.

   ```bash
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
   ```

   - Select the appropriate options during the configuration process (e.g., select EC2, choose CPU, memory, and disk metrics).
   
   - You will be prompted to select dimensions and metrics to monitor, such as **CPU usage per core**, **EC2 instance dimensions**, etc.

### Image: Configuring the CloudWatch Agent

<img width="960" alt="CloudWatchInstallationANDConfig2" src="https://github.com/user-attachments/assets/6f23b2c2-47b7-454e-aae3-72819462ad18">


3. **Start the CloudWatch Agent**:

   ```bash
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a start
   ```

---

## 8. Testing CloudWatch and Viewing Metrics

### Problem Statement:
Once the CloudWatch agent is installed and running, we need to ensure that metrics are being sent to CloudWatch successfully.

### Steps:
1. **Go to the CloudWatch Dashboard**:
   - Navigate to **Metrics > All Metrics > CWAgent** (Custom Namespace).
   - You should see the metrics being reported by your EC2 instances.

### Image: CloudWatch Metrics

<img width="960" alt="CWAgent" src="https://github.com/user-attachments/assets/5648e3bf-7727-4bac-807c-85302f0d92c0">


2. **Create Alarms**:
   - Set up alarms for specific metrics (like high CPU utilization) to trigger actions or notifications via SNS (Simple Notification Service).

---

## 9. Learning Outcomes

This task helped us understand:
1. **VPC Setup**: How to configure a custom VPC with public and private subnets, and use a NAT instance for internet access from a private subnet.
2. **Bastion Host**: How to securely SSH into a private instance via a public instance (Bastion Host).
3. **IAM Roles**: How to create and attach an IAM role to EC2 instances to allow them to interact with AWS services like CloudWatch.
4. **CloudWatch Agent**: Installing and configuring the CloudWatch agent to monitor system-level metrics like CPU, memory, and disk usage, and sending logs/metrics to CloudWatch.
5. **Monitoring and Alarms**: Viewing EC2 metrics in CloudWatch and setting up alarms for notifications on high resource utilization.

---
