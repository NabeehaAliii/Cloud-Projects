# AWS Identity and Access Management (IAM)

## Introduction

**AWS Identity and Access Management (IAM)** is a web service that enables secure control of access to AWS services and resources. IAM allows you to manage users, groups, roles, and permissions to securely control who can access what in your AWS environment.

## Key Concepts

### 1. **Users**
   - An IAM **User** represents a person or an application interacting with AWS resources.
   - Each user has a unique name within the AWS account and can have its own set of permissions.
   - Users can be assigned **Access Keys** and **Passwords** for programmatic and console access.

### 2. **Groups**
   - IAM **Groups** allow you to organize multiple users under a common set of permissions.
   - A group has policies attached that define what actions the group members are allowed or denied.
   - Groups make permission management easier by attaching policies once to a group instead of to each individual user.

### 3. **Policies**
   - **Policies** are JSON documents that define the permissions for users, groups, and roles.
   - Policies specify what actions are allowed or denied, and on which resources.
   - AWS provides **managed policies**, or you can create your own **custom policies**.

### 4. **Roles**
   - An **IAM Role** is an entity that can be assumed by anyone or any service that needs access to resources, without long-term credentials.
   - Roles are used to delegate permissions, grant temporary access, or allow AWS services (like EC2 or Lambda) to access other AWS services on your behalf.

### 5. **Permissions**
   - **Permissions** are the backbone of AWS IAM. Permissions specify who is authorized to access specific AWS resources and what actions they can perform.
   - Permissions are granted via policies attached to users, groups, or roles.
   - Permissions can be **Allow** or **Deny**.

### 6. **Authentication Methods**
   - **Programmatic Access**: Uses **Access Keys** (Access Key ID and Secret Access Key) to make API requests or use AWS CLI.
   - **Console Access**: Uses a **Username and Password** to log in to the AWS Management Console.

### 7. **Multifactor Authentication (MFA)**
   - MFA adds an extra layer of security by requiring users to enter a code from a hardware or virtual MFA device in addition to their username and password.
   - MFA can be enforced per user or for specific actions.

### 8. **Temporary Security Credentials**
   - **Temporary credentials** can be generated via AWS Security Token Service (STS) and allow users or applications to assume an IAM role for a specified period.

### 9. **Deny Overrides Rule**
   - **Deny Overrides** is an IAM policy evaluation rule that takes precedence when multiple permissions are in place.
   - If there is a conflict between an **Allow** and a **Deny** permission, the **Deny** will always override the **Allow**. This ensures that if any action or resource access is explicitly denied, that denial will take effect, even if other policies allow it.
   - This rule ensures stronger security since it prevents accidental overrides of critical Deny permissions that are set in place to protect sensitive resources.

---

## Getting Started

### 1. **Create an IAM User**
   - Go to the IAM console.
   - Select **Users** and click **Add User**.
   - Assign **Programmatic access** or **AWS Management Console access**.
   - Attach a **policy** or add the user to a **group** to grant necessary permissions.

### 2. **Create an IAM Group**
   - Go to the IAM console.
   - Select **Groups** and click **Create New Group**.
   - Attach policies to define what permissions the group has.

### 3. **Create and Attach a Policy**
   - Go to the IAM console.
   - Select **Policies** and click **Create policy**.
   - Choose to write your policy in JSON or use the visual editor.
   - Attach the policy to a user, group, or role.

### 4. **Create an IAM Role**
   - Go to the IAM console.
   - Select **Roles** and click **Create Role**.
   - Choose a trusted entity (AWS service, another AWS account, or web identity provider).
   - Attach policies that define what actions the role can perform.

---

## IAM Best Practices

### 1. **Enable MFA for All Users**
   - Add extra security by enabling **Multi-Factor Authentication (MFA)** for the root account and IAM users.

### 2. **Follow the Principle of Least Privilege**
   - Grant only the permissions necessary for users to accomplish their tasks.
   - Regularly audit permissions and remove any unnecessary ones.

### 3. **Use Groups to Assign Permissions**
   - Use **Groups** instead of directly attaching policies to individual users to simplify permission management.

### 4. **Use IAM Roles for Applications**
   - Use **IAM Roles** to delegate permissions to services like EC2, Lambda, or ECS. Avoid embedding long-term access keys in your application code.

### 5. **Rotate Credentials Regularly**
   - Regularly rotate IAM access keys and delete unused ones to minimize the security risk.

### 6. **Use AWS CloudTrail for Auditing**
   - Enable **AWS CloudTrail** to record all AWS API calls, including IAM actions.

---

## Example Policy

Here is an example policy that grants read-only access to all S3 buckets:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion"
      ],
      "Resource": "arn:aws:s3:::*/*"
    }
  ]
}
```

---

## IAM Features

### 1. **IAM Access Analyzer**
   - Provides insights into who has access to your AWS resources. It uses logic to analyze your IAM policies and identify resources shared with external entities.

### 2. **Identity Federation**
   - IAM supports **Federated Access**, which allows users to access AWS resources without needing a dedicated IAM user. This is typically used with corporate directories like Active Directory or SAML-based authentication.

### 3. **Service Control Policies (SCP)**
   - Part of **AWS Organizations**, SCPs allow you to define permission boundaries across your AWS accounts within an organization. SCPs override any permissions set in IAM.

### 4. **Permissions Boundaries**
   - Permissions boundaries allow you to set the maximum permissions that IAM entities (users or roles) can have.

---

## Summary

AWS IAM is a powerful tool to control access to your AWS resources securely. Following IAM best practices, such as enforcing least privilege and enabling MFA, ensures that your AWS environment is secure. Use IAM roles, groups, and policies effectively to manage access control and monitor access using CloudTrail and Access Analyzer.
