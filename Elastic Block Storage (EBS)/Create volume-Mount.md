# Tutorial: Working with EBS Volumes on EC2

This tutorial covers creating and attaching an Amazon EBS (Elastic Block Store) volume to an EC2 instance and working with basic Linux commands such as `lsblk`, `mount`, and `umount`. By the end of this tutorial, you will understand how to manage storage volumes, mount and unmount filesystems, and use these volumes within your EC2 instance.

## Prerequisites

- AWS account with EC2 and EBS access.
- An existing running EC2 instance (Linux-based).
- Basic knowledge of Linux terminal commands.

---

## Table of Contents

- [Step 1: Create and Attach an EBS Volume](#step-1-create-and-attach-an-ebs-volume)
- [Step 2: Working with the EBS Volume in the EC2 Instance](#step-2-working-with-the-ebs-volume-in-the-ec2-instance)
  - [Listing Block Devices (`lsblk`)](#listing-block-devices-lsblk)
  - [Mounting the Volume (`mount`)](#mounting-the-volume-mount)
  - [Creating Files and Directories](#creating-files-and-directories)
  - [Unmounting the Volume (`umount`)](#unmounting-the-volume-umount)
- [Why These Steps Are Important](#why-these-steps-are-important)
- [Conclusion](#conclusion)

---

## Step 1: Create and Attach an EBS Volume

### 1.1. **Create an EBS Volume:**

1. Log in to the AWS Management Console.
2. Go to the **EC2 Dashboard** and navigate to **Elastic Block Store** > **Volumes**.
3. Click **Create Volume** and configure the following:
   - **Size**: Set the volume size (e.g., 8 GiB).
   - **Availability Zone**: Ensure this matches the Availability Zone of your EC2 instance.
   - **Volume Type**: Choose General Purpose SSD (gp2).
4. Click **Create Volume**.

### 1.2. **Attach the EBS Volume to the EC2 Instance:**

1. Once the volume is created, select it from the list in the **Volumes** section.
2. Click **Actions** and select **Attach Volume**.
3. Choose the running EC2 instance from the dropdown.
4. Click **Attach**.

---

## Step 2: Working with the EBS Volume in the EC2 Instance

Once the EBS volume is attached to the EC2 instance, you need to work with it in your instance via the terminal.

### 2.1. **Listing Block Devices (`lsblk`):**

The `lsblk` command is used to list all block devices attached to the instance, including disks and partitions.

```bash
lsblk
```

Output will show the block devices like:
```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0    8G  0 disk
├─xvda1 202:1    0    7G  0 part /
xvdf    202:80   0    8G  0 disk
```

- `xvda`: This is the root volume of your instance.
- `xvdf`: This is the newly attached EBS volume (in this example).

### 2.2. **Mounting the Volume (`mount`):**

Mount the new EBS volume to a directory (e.g., `/test`).

1. Create a directory for the mount point:
   ```bash
   sudo mkdir /test
   ```

2. Mount the volume:
   ```bash
   sudo mount /dev/xvdf /test
   ```

This command mounts the block device (`/dev/xvdf`) to the directory `/test`.

### 2.3. **Creating Files and Directories:**

Now that the volume is mounted, you can interact with it like any other directory.

1. Create files using `touch`:
   ```bash
   cd /test
   touch file1 file2 file3
   ```

2. Write to a file using `echo`:
   ```bash
   echo "Hello, EBS Volume!" > demo.txt
   ```

3. List the files to verify:
   ```bash
   ls
   ```

### 2.4. **Unmounting the Volume (`umount`):**

Once you are done working with the volume, you need to unmount it properly.

1. Before unmounting, ensure no processes are using the volume. Attempt to unmount:
   ```bash
   sudo umount /test
   ```

2. If you get a "target is busy" error, close any open shells or processes using the volume.

---

## Why These Steps Are Important

### **1. lsblk Command:**
- **Purpose:** `lsblk` lists all the attached block devices (disks) to your instance. This helps in identifying the new EBS volume and its corresponding device name (e.g., `/dev/xvdf`).

### **2. mount Command:**
- **Purpose:** `mount` attaches the EBS volume to a directory so that you can store data and interact with the filesystem. Without mounting, the system doesn't know how to access the storage on the volume.

### **3. Creating Files:**
- **Purpose:** Creating files and writing data demonstrates how you can use the EBS volume like a regular filesystem. Data written to this directory is stored on the EBS volume.

### **4. umount Command:**
- **Purpose:** Unmounting the volume is necessary before detaching it from the EC2 instance. If the volume is still mounted and you detach it, you can encounter filesystem corruption.

---

## Conclusion

In this tutorial, you learned how to:
- Create and attach an EBS volume to an EC2 instance.
- Use basic Linux commands (`lsblk`, `mount`, `umount`) to interact with the volume.
- Create files and directories on the mounted volume.
- Safely unmount the volume before detaching it.

These steps are fundamental for working with persistent storage on AWS EC2 instances and managing EBS volumes.

### **Explanation:**
- This `README.md` file is structured to provide a step-by-step guide for creating, attaching, and working with an EBS volume.
- It explains how and why each step was taken, from creating the volume to using key Linux commands.
- The document is written to be a helpful resource for someone learning about EBS and EC2 interaction for the first time.

```
