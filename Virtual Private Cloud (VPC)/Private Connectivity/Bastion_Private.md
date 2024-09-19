# Bastion Host & Jump Server

## Overview

A **Bastion Host** (also known as a Jump Server) is a special-purpose server designed to provide access to a private network from an external public network. It acts as an intermediary point for securing SSH (Secure Shell) or RDP (Remote Desktop Protocol) connections to servers in a private subnet of your Virtual Private Cloud (VPC).

This design ensures that you can manage access to sensitive servers without exposing them to the public internet directly, enhancing the overall security of your cloud infrastructure.

---

## Why Use a Bastion Host?

### 1. **Controlled Access to Private Subnets**
   - Bastion Hosts restrict access to the private subnets in your VPC by providing a single access point. Instead of giving public access to all instances in the private subnet, only the Bastion Host is accessible from the internet.

### 2. **Security**
   - Limits potential attack surface as you only expose a single hardened instance to the public internet. This instance uses strict security rules (e.g., key-based authentication, limited IP addresses, etc.).
   - The private instances remain inaccessible from the outside, enhancing security.

### 3. **Auditing & Monitoring**
   - All incoming/outgoing traffic to the private instances is routed through the Bastion Host, allowing for easier auditing and monitoring of user access.

---

## Working with Bastion Host

### Architecture Diagram

Below is a typical Bastion Host setup in AWS:

<img width="655" alt="Bastion host" src="https://github.com/user-attachments/assets/f2e07c3d-c728-4f6f-83b5-c07e5cdd5c76">

In this setup:
- **Public Subnet**: Contains the **Bastion Host**.
- **Private Subnet**: Contains the private server that you wish to protect.
- The user connects to the Bastion Host using SSH (via the PEM key) and then connects from the Bastion Host to the private server.

### Workflow Explained:
1. **End User**: The end user connects to the Bastion Host using SSH with the private key (PEM) in the public subnet.
2. **Bastion Host**: The Bastion Host acts as a gateway to the private subnet. Once inside, the user can SSH into the private instance without the need for another key.
3. **Private Server**: The private server is located in the private subnet and cannot be accessed directly from the public internet.

---

## Example: SSH via Bastion Host in AWS CloudShell

### Steps to SSH into a Private Server

1. **Upload Key to CloudShell**
   
   Screenshot:

<img width="655" alt="bastion-host-public" src="https://github.com/user-attachments/assets/88f5111d-63ce-4a30-9dd6-77f7dbc25f9e">

   To securely access the private instance, first, upload the **PEM key** for the private instance to CloudShell. Ensure the PEM file has restricted permissions using the `chmod` command.

   ```bash
   ls
   chmod 400 Practicekey.pem
   ```
  **SSH into Bastion Host**

   Use the uploaded PEM key to SSH into the **Bastion Host**, which is accessible via its public IP address.

   ```bash
   ssh -i private_server_key.pem ubuntu@<BastionPublicIP>
   ```

4. **SSH from Bastion Host to Private Instance**

   After successfully accessing the Bastion Host, you can SSH into the private instance from the Bastion Host using the internal private IP.

   ```bash
   vi private_server_key.pem   # now copy the Practice.pem file content paste it in private_server_key
   chmod 400 private_server_key.pem
   ssh -i private_server_key.pem ubuntu@<PrivateIP>
   ```

   Screenshot:
   
   <img width="655" alt="private attach" src="https://github.com/user-attachments/assets/dfe6206b-717d-4c65-8215-548b9e33a718">

6. **CloudShell Output**

   The following is an example output of successful SSH connections:

   ```bash
   Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-1012-aws x86_64)
   ```

## Bastion Host Security Best Practices

### 1. **Use Security Groups**
   - Assign Security Groups to the Bastion Host that only allow inbound SSH traffic from specific IP ranges (your own IP address). This limits access to only trusted users.

### 2. **Rotate Keys**
   - Regularly rotate SSH keys to prevent compromised credentials from being used over long periods.

### 3. **Enable Monitoring & Logging**
   - Use AWS services like **CloudTrail** and **CloudWatch Logs** to log and monitor all access to the Bastion Host.

### 4. **Limit Access**
   - Restrict access to the Bastion Host by using IAM roles and policies. Ensure only authorized personnel can SSH into the host.

---

## Alternatives to Bastion Host

### 1. **Session Manager (AWS Systems Manager)**
   - If you want to eliminate the need for SSH access altogether, you can use AWS **Session Manager**. It allows you to access instances without needing to open any ports or maintain bastion hosts. You can log into instances directly from the AWS console.

### 2. **VPN Setup**
   - A VPN can be used to securely access instances in private subnets. This would remove the need for a Bastion Host while maintaining secure connectivity.

---

## Conclusion

A **Bastion Host** provides a simple yet effective way to control access to instances in private subnets, enhancing security while ensuring easy management. It's especially useful when direct access to private instances is not an option.

By following best practices and using additional AWS tools like **Session Manager** or **CloudWatch Logs**, you can further strengthen your infrastructure security and reduce the risk of unauthorized access.
