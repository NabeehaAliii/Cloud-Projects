# Amazon EBS Snapshots

This document covers everything you need to know about Amazon EBS (Elastic Block Store) Snapshots, including what they are, their use cases, how to create them, and best practices for managing EBS Snapshots in your AWS environment.

---

## Table of Contents
- [What is an EBS Snapshot?](#what-is-an-ebs-snapshot)
- [Use Cases for Snapshots](#use-cases-for-snapshots)
- [How EBS Snapshots Work](#how-ebs-snapshots-work)
- [How to Create and Manage Snapshots](#how-to-create-and-manage-snapshots)
- [Best Practices](#best-practices)
- [Restoring a Volume from a Snapshot](#restoring-a-volume-from-a-snapshot)
- [Automating Snapshots](#automating-snapshots)
- [Screenshots](#screenshots)

---

## What is an EBS Snapshot?

An **EBS Snapshot** is a point-in-time backup of an Amazon EBS volume. Snapshots are stored in **Amazon S3** and can be used to create new EBS volumes or restore data at a specific point in time.

### Key Points:
- Snapshots capture the entire state of an EBS volume at the time they are created.
- They are stored in **Amazon S3**, but you don’t interact with S3 directly.
- Snapshots are **incremental** after the first one, meaning that only the changed blocks are saved, which reduces storage costs.

---

## Use Cases for Snapshots

- **Backup and Recovery**: Snapshots allow you to back up data on an EBS volume and restore it later in case of failure, data corruption, or human error.
- **Volume Migration**: You can take a snapshot of a volume in one Availability Zone and create a new volume in another AZ or even in a different AWS region.
- **Data Replication**: Snapshots can be shared across regions or AWS accounts for data replication or disaster recovery.
- **Volume Scaling**: You can create a snapshot, increase its size, and then create a larger volume from that snapshot.

---

## How EBS Snapshots Work

1. **First Snapshot**: When you create your first snapshot of an EBS volume, the entire volume is copied and stored.
2. **Subsequent Snapshots**: After the initial snapshot, EBS snapshots become **incremental**. This means that only the blocks of data that have changed since the last snapshot are copied, reducing storage costs.
3. **Snapshot Storage**: Snapshots are stored in **Amazon S3**. However, they are not stored as files in S3 buckets but are abstracted away so you don’t need to interact with them like standard S3 objects.

---

## How to Create and Manage Snapshots

### Step 1: Create a Snapshot

1. Log in to the **AWS Management Console**.
2. Navigate to **EC2 Dashboard** > **Elastic Block Store** > **Volumes**.
3. Select the EBS volume you want to back up.
4. Click **Actions** > **Create Snapshot**.
5. Provide a **Description** for the snapshot.
6. Click **Create Snapshot**.

### Step 2: View Snapshots

1. Go to the **Snapshots** section under **Elastic Block Store** in the EC2 Dashboard.
2. All snapshots associated with your account will be listed here.

### Step 3: Delete a Snapshot

1. In the **Snapshots** section, select the snapshot you want to delete.
2. Click **Actions** > **Delete Snapshot**.
3. Confirm the deletion.

---

## Best Practices

- **Automate Snapshots**: Use AWS Backup or Lambda to automate snapshot creation and deletion to ensure regular backups.
- **Use Tags**: Tag your snapshots with metadata like `Project`, `Environment`, or `Owner` to organize and manage them effectively.
- **Cross-Region Snapshots**: Copy snapshots to different regions for disaster recovery purposes. This ensures that you have data backed up even if an entire region goes down.
- **Manage Costs**: Since snapshots are incremental, they save costs, but old and unnecessary snapshots should be deleted to avoid extra storage costs.
- **Encryption**: Use encrypted EBS volumes to automatically create encrypted snapshots. You can also create encrypted volumes from unencrypted snapshots and vice versa.

---

## Restoring a Volume from a Snapshot

Snapshots can be used to create a new EBS volume with the same data as the original volume at the time the snapshot was taken.

### Step-by-Step:
1. Navigate to **Snapshots** in the EC2 Dashboard.
2. Select the snapshot you want to restore.
3. Click **Actions** > **Create Volume** from snapshot.
4. Choose the volume type, size, and Availability Zone.
5. Click **Create**.

The new volume will have all the data from the time the snapshot was created.

---
## Automating Snapshots

You can automate snapshot creation with **AWS Lifecycle Manager**, **AWS Backup**, or custom scripts with Lambda.

### Example: AWS Lifecycle Manager

AWS Lifecycle Manager can automatically create and delete snapshots based on a policy. Here’s how you can set it up:

1. Go to the **EC2 Dashboard**.
2. Under **Elastic Block Store**, click **Lifecycle Manager**.
3. Create a new policy and choose your EBS volumes.
4. Set the frequency and retention period for snapshots.
5. Save the policy.

This will ensure that your snapshots are automatically created and deleted based on your policy.

### Example: AWS Backup

1. Open **AWS Backup** from the AWS Console.
2. Create a backup plan that includes rules for how often to take snapshots and how long to retain them.
3. Assign resources like EBS volumes to the backup plan.

### Example: Lambda Automation (Custom Script)
You can use AWS Lambda with CloudWatch events to automate the creation and deletion of snapshots at specific intervals (e.g., daily or weekly).

---

## Screenshots
 
### 1. 
Ensuring that the new volume is a file system then mounting it and having some files.

<img width="655" alt="1" src="https://github.com/user-attachments/assets/5aa3d836-8a7d-4ea9-b0d6-3ec6efa5c073">


### 2. 
 Create a Snapshot and then add an xyz.txt file. This will show that it will not be present in the snapshot taken earlier.
 
<img width="655" alt="2" src="https://github.com/user-attachments/assets/db9bd6a2-a7eb-4006-94fe-cdcd6b211a14">



### 3. 
 When we made a new Volume through the snapshot taken earlier we checked if it was a file which it indeed is.
 We mounted it in data1 and then checked the files so it only had files added before the snapshot.
 
<img width="655" alt="3" src="https://github.com/user-attachments/assets/57777be9-d072-4bd3-9110-57015e266c23">

---

## Conclusion

Amazon EBS Snapshots are an essential tool for managing data backups, migrations, and disaster recovery for your EBS volumes. They allow you to back up data in a cost-effective, incremental manner and can be used to restore your EC2 instances or move data across regions and accounts. Following best practices like tagging, automating snapshots, and cross-region replication can help you manage your snapshots efficiently.

```
