# Tutorial: Working with EBS Volumes on EC2

This tutorial covers creating and attaching an Amazon EBS (Elastic Block Store) volume to an EC2 instance and working with basic Linux commands such as `lsblk`, `mkfs`, `mount`, and `umount`. By the end of this tutorial, you will understand how to manage storage volumes, format them (if required), mount and unmount filesystems, and use these volumes within your EC2 instance.

## Prerequisites

- AWS account with EC2 and EBS access.
- An existing running EC2 instance (Linux-based).
- Basic knowledge of Linux terminal commands.

---

## Table of Contents

- [Step 1: Create and Attach an EBS Volume](#step-1-create-and-attach-an-ebs-volume)
- [Step 2: Formatting and Mounting the EBS Volume](#step-2-formatting-and-mounting-the-ebs-volume)
  - [Listing Block Devices (`lsblk`)](#listing-block-devices-lsblk)
  - [Formatting the Volume (`mkfs`)](#formatting-the-volume-mkfs)
  - [Mounting the Volume (`mount`)](#mounting-the-volume-mount)
  - [Creating Files and Directories](#creating-files-and-directories)
  - [Unmounting the Volume (`umount`)](#unmounting-the-volume-umount)
- [Step 3: Reattaching the Volume to Another Instance](#step-3-reattaching-the-volume-to-another-instance)
  - [Mounting the Existing Volume and Accessing Data](#mounting-the-existing-volume-and-accessing-data)
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

## Step 2: Formatting and Mounting the EBS Volume

Once the EBS volume is attached to the EC2 instance, you may need to format it before using it. After formatting, you will mount the volume to a directory for use.

### 2.1. **Listing Block Devices (`lsblk`):**

The `lsblk` command lists all block devices attached to the instance, including disks and partitions.

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

### 2.2. **Formatting the Volume (`mkfs`):**

If the EBS volume is new (i.e., it doesn’t contain a file system), you will need to format it before you can use it.

1. Check if the volume needs formatting:
   ```bash
   sudo file -s /dev/xvdf
   ```
   - If the output shows `data`, it means the volume is unformatted and needs to be formatted.

2. Format the volume with the ext4 file system:
   ```bash
   sudo mkfs -t ext4 /dev/xvdf
   ```
   - This command creates a new ext4 file system on the volume, making it ready for use.

### 2.3. **Mounting the Volume (`mount`):**

Mount the formatted EBS volume to a directory on the EC2 instance (e.g., `/test`).

1. Create a mount point:
   ```bash
   sudo mkdir /test
   ```

2. Mount the volume:
   ```bash
   sudo mount /dev/xvdf /test
   ```

This mounts the volume to the `/test` directory, allowing you to store and access files on it.

### 2.4. **Creating Files and Directories:**

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

### 2.5. **Unmounting the Volume (`umount`):**

Once you are done working with the volume, you need to unmount it properly.

1. Before unmounting, ensure no processes are using the volume. Attempt to unmount:
   ```bash
   sudo umount /test
   ```

2. If you get a "target is busy" error, close any open shells or processes using the volume.

---

## Step 3: Reattaching the Volume to Another Instance

If you terminate the instance or need to use the volume on another EC2 instance, you can detach the volume and reattach it to a new instance.

### 3.1. **Detach the EBS Volume:**

1. In the AWS Management Console, navigate to **Volumes**.
2. Select the volume you want to detach.
3. Click **Actions** and select **Detach Volume**.
4. Once detached, you can attach the volume to another EC2 instance.

### 3.2. **Attach the Volume to the New EC2 Instance:**

1. After creating the new EC2 instance, go to the **Volumes** section.
2. Select the volume and choose **Actions > Attach Volume**.
3. Select the new instance from the dropdown and click **Attach**.

### 3.3. **Mounting the Existing Volume and Accessing Data:**

When you reattach the EBS volume to the new instance, there’s no need to format it again because the data is already present. Follow these steps to mount it and verify your data:

1. **List the block devices:**
   ```bash
   lsblk
   ```

2. **Mount the volume:**
   ```bash
   sudo mount /dev/xvdf /data
   ```

3. **Verify the contents:**
   ```bash
   cd /data
   ls
   cat demo.txt
   ```
   - You should see the files and data you had created on the previous instance.

---

## Why These Steps Are Important

### **1. lsblk Command:**
- **Purpose:** `lsblk` lists all the attached block devices (disks) to your instance. This helps in identifying the new or existing EBS volume and its corresponding device name (e.g., `/dev/xvdf`).

### **2. mkfs Command:**
- **Purpose:** Formatting the EBS volume (with `mkfs`) prepares it for storing data. This step is essential for a new volume but should not be repeated if the volume already contains data.

### **3. mount Command:**
- **Purpose:** `mount` attaches the EBS volume to a directory so that you can store data and interact with the filesystem. Without mounting, the system cannot access the storage on the volume.

### **4. Creating Files:**
- **Purpose:** Creating files and writing data demonstrates how you can use the EBS volume like a regular filesystem. Data written to this directory is stored on the EBS volume.

### **5. umount Command:**
- **Purpose:** Unmounting the volume is necessary before detaching it from the EC2 instance. If the volume is still mounted and you detach it, you can encounter filesystem corruption.

---

## Conclusion

In this tutorial, you learned how to:
- Create and attach an EBS volume to an EC2 instance.
- Format the volume using `mkfs` (if required).
- Use basic Linux commands (`lsblk`, `mount`, `umount`) to interact with the volume.
- Reattach the volume to a new EC2 instance and verify that the stored data is still accessible.

These steps are essential for managing persistent storage in AWS and safely interacting with your data in an EC2 instance.

---
