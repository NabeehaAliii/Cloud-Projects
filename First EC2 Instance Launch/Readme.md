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

<img width="960" alt="EC2 Mangement Console" src="https://github.com/user-attachments/assets/9c18ac39-4ff8-4d88-89a4-703290450fff">

## Step 2: Select Your Region
**Objective:** Choose the AWS region where you want to launch your EC2 instance.

- **Instructions:**
  1. In the top right corner of the AWS Management Console, you will see the current region (e.g., "US East (N. Virginia)").
  2. Click on the region name to open a drop-down menu and select your preferred region.
     
<img width="655" alt="Select Region" src="https://github.com/user-attachments/assets/b6449008-8a8d-4081-8695-7d75746efc3f">

  4. **Considerations:** Choose a region that is closest to your user base or where you expect the lowest latency. Some services and AMIs may also be region-specific.

## Step 3: Add Tags
**Objective:** Organize your resources by adding tags.

- **Instructions:**
  1. Click **Add Tag**.
  2. **Key:** Enter "Name".
  3. **Value:** Enter a descriptive name for your instance (e.g., "MyFirstEC2Instance").

<img width="655" alt="Name and tag" src="https://github.com/user-attachments/assets/eaf24c45-81e1-4bb5-8a01-e68f3c493134">

## Step 4: Choose an Amazon Machine Image (AMI)
**Objective:** Select the operating system for your EC2 instance.

- **Instructions:**
  1. Under the **Choose an Amazon Machine Image (AMI)** section, select the desired AMI. For beginners, you can choose **Amazon Linux 2 AMI (Free tier eligible)** or **Ubuntu Server**.

     <img width="849" alt="Choose an AMI" src="https://github.com/user-attachments/assets/6975e038-33ea-4f32-948a-5fd3ff686432">


## Step 5: Choose an Instance Type
**Objective:** Select the appropriate instance type based on your requirements.

- **Instructions:**
  1. On the **Choose an Instance Type** page, select **t2.micro** (Free tier eligible) if you’re just starting.
  2. Click **Next: Configure Instance Details**.

<img width="655" alt="Instance type" src="https://github.com/user-attachments/assets/637934ef-5924-4bb3-86a3-2a9f0a7d558a">


## Step 6: Configure Instance Details
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
  
<img width="890" alt="Configure Storage" src="https://github.com/user-attachments/assets/5c69c656-c423-4aed-bfe2-99467ec23f93">

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

<img width="655" alt="Network Setting for Traffic" src="https://github.com/user-attachments/assets/4fc68146-b06c-401e-9666-3e6bde188c47">


## Step 9: Review and Launch
**Objective:** Review your configurations and launch the instance.

- **Instructions:**
  1. Review the instance details to ensure everything is correct.
  2. Click **Launch**.
  3. **Select an existing key pair** or create a new one. Download the PEM file if creating a new key pair.
  4. Confirm and click **Launch Instances**.
     
<img width="890" alt="Successfully Launch" src="https://github.com/user-attachments/assets/438cd80c-bbce-4d4b-86c9-41365f3d1546">


## Step 10: Connecting to Your EC2 Instance
**Objective:** Access the EC2 instance using SSH.

<img width="960" alt="Instance is Launched" src="https://github.com/user-attachments/assets/3c3266e3-b46c-4253-9aab-655707e573db">

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
