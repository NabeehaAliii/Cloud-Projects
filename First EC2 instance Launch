# First EC2 Instance Launch

## Introduction to EC2
**Objective:** Explain what Amazon EC2 is and why it’s important.

Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable compute capacity in the cloud. It allows users to run applications on a virtual server in AWS. EC2 instances can be scaled up or down based on demand, providing flexibility and cost efficiency.

## Prerequisites
**Objective:** List the requirements needed before starting the project.

- **AWS Account:** Ensure you have an active AWS account. If you don’t have one, sign up at [AWS](https://aws.amazon.com/).
- **Basic Knowledge:** Familiarity with basic cloud computing concepts and comfort navigating the AWS Management Console.
- **SSH Client:** Ensure you have an SSH client installed (e.g., Git Bash on Windows, Terminal on macOS/Linux) to connect to the EC2 instance.

## Step 1: Navigate to the EC2 Dashboard
**Objective:** Access the EC2 service within the AWS Management Console.

- **Instructions:** Log into your AWS Management Console. From the services menu, select **EC2** under the "Compute" category to navigate to the EC2 Dashboard.

## Step 2: Select Your Region
**Objective:** Choose the AWS region where you want to launch your EC2 instance.

- **Instructions:**
  1. In the top right corner of the AWS Management Console, you will see the current region (e.g., "US East (N. Virginia)").
  2. Click on the region name to open a drop-down menu and select your preferred region.
  3. **Considerations:** Choose a region that is closest to your user base or where you expect the lowest latency. Some services and AMIs may also be region-specific.

## Step 3: Choose an Amazon Machine Image (AMI)
**Objective:** Select the operating system for your EC2 instance.

- **Instructions:**
  1. Click on **Launch Instance**.
  2. Under the **Choose an Amazon Machine Image (AMI)** section, select the desired AMI. For beginners, you can choose **Amazon Linux 2 AMI (Free tier eligible)** or **Ubuntu Server**.

## Step 4: Choose an Instance Type
**Objective:** Select the appropriate instance type based on your requirements.

- **Instructions:**
  1. On the **Choose an Instance Type** page, select **t2.micro** (Free tier eligible) if you’re just starting.
  2. Click **Next: Configure Instance Details**.

## Step 5: Configure Instance Details
**Objective:** Customize the instance settings.

- **Instructions:**
  1. **Number of Instances:** Leave as 1.
  2. **Network:** Choose the default VPC (or configure a custom VPC if required).
  3. **Subnet:** Select the default or create a new one.
  4. **Auto-assign Public IP:** Ensure this is enabled so you can access your instance.
  5. Other settings can be left at default. Click **Next: Add Storage**.

## Step 6: Add Storage
**Objective:** Configure the storage attached to your instance.

- **Instructions:**
  1. The default storage of **8 GB** is usually sufficient for simple tasks. Adjust this if needed.
  2. Leave the volume type as **General Purpose SSD (gp2)**.
  3. Click **Next: Add Tags**.

## Step 7: Add Tags
**Objective:** Organize your resources by adding tags.

- **Instructions:**
  1. Click **Add Tag**.
  2. **Key:** Enter "Name".
  3. **Value:** Enter a descriptive name for your instance (e.g., "MyFirstEC2Instance").
  4. Click **Next: Configure Security Group**.

## Step 8: Configure Security Group
**Objective:** Set up firewall rules to control traffic to your instance.

- **Instructions:**
  1. Create a new security group.
  2. **Security Group Name:** Enter a name (e.g., "MyFirstEC2SecurityGroup").
  3. **Description:** Add a brief description.
  4. **Rules:**
     - **Type:** SSH
     - **Protocol:** TCP
     - **Port Range:** 22
     - **Source:** Select **My IP** to restrict SSH access to your IP address.
  5. Click **Review and Launch**.

## Step 9: Review and Launch
**Objective:** Review your configurations and launch the instance.

- **Instructions:**
  1. Review the instance details to ensure everything is correct.
  2. Click **Launch**.
  3. **Select an existing key pair** or create a new one. Download the PEM file if creating a new key pair.
  4. Confirm and click **Launch Instances**.

## Step 10: Connecting to Your EC2 Instance
**Objective:** Access the EC2 instance using SSH.

- **Instructions:**
  1. Go back to the EC2 Dashboard and find your instance under **Instances**.
  2. Copy the **Public IP** or **Public DNS**.
  3. Open your terminal or SSH client.
  4. Use the following command to connect:
     ```bash
     ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
     ```
  5. Replace `"your-key.pem"` with the path to your key pair and `your-ec2-public-ip` with the instance's IP address.
  6. Once connected, you can run commands on your instance.

## Step 11: Terminate the Instance (Optional)
**Objective:** Safely terminate your EC2 instance when you’re done.

- **Instructions:**
  1. In the EC2 Dashboard, select your instance.
  2. Click on **Actions > Instance State > Terminate**.
  3. Confirm termination.

## Conclusion
**Objective:** Summarize the project outcomes.

- **Summary:** In this project, you successfully launched an EC2 instance, configured its settings, and connected to it using SSH. This project serves as a foundational exercise in cloud computing, providing the basic skills needed to explore more advanced AWS services.

## Next Steps
**Objective:** Encourage further exploration and learning.

- **Suggestions:** Experiment with deploying a web server on your EC2 instance, setting up Elastic Load Balancing (ELB), or using AWS CLI for automation.
