# AWS AMI (Amazon Machine Image)

This document provides an in-depth overview of Amazon Machine Images (AMI) in AWS, explaining their purpose, how they work, their use cases, and how to manage them effectively.

---

## Table of Contents
- [What is an AMI?](#what-is-an-ami)
- [Types of AMIs](#types-of-amis)
- [How AMIs Work](#how-amis-work)
- [Creating and Managing AMIs](#creating-and-managing-amis)
- [AMIs vs Snapshots](#amis-vs-snapshots)
- [Sharing and Copying AMIs](#sharing-and-copying-amis)
- [Best Practices](#best-practices)
- [Costs of Using AMIs](#costs-of-using-amis)
- 
---

## What is an AMI?

An **Amazon Machine Image (AMI)** is a pre-configured virtual machine image that contains the information needed to launch an Amazon EC2 instance. This includes the operating system, application software, and any necessary configurations.

### Key Points:
- AMIs provide the basic building blocks for deploying EC2 instances.
- An AMI contains the following components:
  - **Root volume** with an operating system.
  - **Launch permissions** to control who can use the AMI.
  - **Block device mappings** to specify storage volumes attached to the instance.

---

## Types of AMIs

AWS provides three main types of AMIs:

1. **Amazon-supplied AMIs**:
   - These are provided and maintained by AWS and include commonly used operating systems like Amazon Linux, Ubuntu, and Windows Server.
   
2. **Marketplace AMIs**:
   - These are AMIs provided by third-party vendors on the AWS Marketplace. They may include pre-configured software packages and are typically used for specific solutions like database applications or development environments.
   
3. **Custom AMIs**:
   - These are AMIs you create yourself based on your own custom configuration. For example, you can install software, configure settings, and save the instance state as a custom AMI.

---

## How AMIs Work

An AMI includes everything needed to launch an EC2 instance. When you launch an instance, you select an AMI, which serves as a template. The following key components are associated with an AMI:

### Components:
- **Root Volume**: This contains the operating system and software packages.
- **Launch Permissions**: You can define which AWS accounts or users have access to launch instances from the AMI.
- **Block Device Mappings**: These define the volumes (e.g., EBS volumes) to attach to the instance upon launch.

### AMI Lifecycle:
1. **Create an AMI** from an existing EC2 instance or select an existing AMI from the AWS Marketplace or public AMIs.
2. **Launch an EC2 instance** using the AMI.
3. **Customize the EC2 instance** (installing software, updating configurations).
4. **Create a new AMI** based on this customized instance, if needed.

---

## Creating and Managing AMIs

You can create your own custom AMIs or use an existing AMI to launch an EC2 instance.

### Step 1: Create an AMI from an Existing EC2 Instance

1. **Stop the EC2 instance** that you want to create an AMI from.
2. Navigate to the **EC2 Dashboard**.
3. Select the instance, click **Actions**, and choose **Create Image**.
4. Provide a **Name** and **Description** for the AMI.
5. Review and confirm the block device mappings.
6. Click **Create Image**.

### Step 2: Launching an EC2 Instance from an AMI

1. In the **EC2 Dashboard**, go to the **AMIs** section under **Images**.
2. Select the AMI you want to use.
3. Click **Launch**.
4. Configure the instance type, networking, storage, etc.
5. Click **Launch**.

### Step 3: Deregistering an AMI

1. Navigate to **AMIs** under **Images** in the **EC2 Dashboard**.
2. Select the AMI you want to deregister.
3. Click **Actions** > **Deregister**.

Note: Deregistering an AMI prevents further instances from being launched from that AMI but does not affect existing instances created from it.

---

## AMIs vs Snapshots

While AMIs and snapshots are related, they serve different purposes:

- **AMI**:
  - An AMI is a full image used to launch EC2 instances.
  - It contains the operating system, configurations, and software packages.
  
- **Snapshot**:
  - A snapshot is a backup of an EBS volume, which could be a root volume or data volume.
  - Snapshots are used to back up the storage and can be used to create new volumes.

### How They Work Together:
- AMIs rely on **snapshots** for the root volume of an EC2 instance. When you create an AMI, the root volume is saved as a snapshot in Amazon S3.

---

## Sharing and Copying AMIs

### Sharing AMIs

You can share your AMIs with other AWS accounts, making it easier for others to launch instances using your AMI:

1. Navigate to the **AMIs** section.
2. Select the AMI you want to share.
3. Click **Actions** > **Modify Image Permissions**.
4. Add the **AWS account IDs** that you want to share the AMI with.

Note: Shared AMIs can only be used in the same region unless you copy them to other regions.

### Copying AMIs

You can copy AMIs to other regions for disaster recovery or migration purposes:

1. Navigate to the **AMIs** section.
2. Select the AMI you want to copy.
3. Click **Actions** > **Copy AMI**.
4. Choose the destination region.

---

## Best Practices

1. **Keep AMIs Updated**: Regularly update your AMIs to include security patches and software updates.
2. **Tag Your AMIs**: Use tags to keep track of AMIs based on projects, environments, or versions.
3. **Automate AMI Creation**: Use AWS Lambda or automation scripts to regularly create AMIs, especially for production workloads.
4. **Delete Unused AMIs**: Deregister AMIs and delete associated snapshots that are no longer needed to save on storage costs.

---

## Costs of Using AMIs

AMIs themselves do not incur charges, but the **snapshots** associated with them are stored in Amazon S3, and you are charged for the storage:

- **Snapshot storage**: You are billed based on the amount of data stored in Amazon S3 for EBS snapshots.
- **Data Transfer**: There may be charges for transferring data when copying AMIs across regions.

To optimize costs:
- Delete unused AMIs and snapshots.
- Review storage costs in the AWS Cost Explorer.

---

## Conclusion

Amazon Machine Images (AMIs) are a fundamental building block for launching and managing EC2 instances. Whether you're using AWS-provided AMIs, Marketplace AMIs, or creating your own custom AMIs, understanding how to manage and maintain them is critical for optimizing performance, security, and costs in your AWS environment. Regularly update, tag, and automate the creation of your AMIs to ensure smooth operations.


### Additional Steps:
1. **Start by creating an Ubuntu instance**:
   - Select the Ubuntu AMI from the AMI section.
   - Launch the instance with the desired configuration.

2. **Install Nginx on Ubuntu**:
   - Connect to your instance via SSH.
   - Run
   - ```bash
     `sudo apt update` and `sudo apt install nginx`
     ```
   - Ensure Nginx is running by accessing your instance's public DNS in a web browser.

3. **Modify the default webpage**:
   - Navigate to the `/var/www/html` directory.
   - Edit the `index.html` file using `sudo vi index.html`.
   - Enter insert mode by pressing `i` and type "Welcome Nabeeha".
   - Save and exit by pressing `Esc`, then type `:wq` and hit Enter.

4. **Create an AMI from this setup**:
   - Follow the steps above to create an image from your configured instance.

This sequence will ensure that you have a custom AMI that includes an Ubuntu instance with Nginx installed and a personalized welcome message. This AMI can be used to launch new instances with the same setup.
