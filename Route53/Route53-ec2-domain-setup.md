# AWS Route 53 - Creating a Name Record for EC2 Instance

## Objective

This document walks through the process of setting up a name record for an Amazon EC2 instance using **Amazon Route 53**, and configuring a domain to point to the public IP address of the EC2 instance. We will also show how the domain can be accessed via the configured name in the browser.

## Prerequisites

1. **Amazon Web Services (AWS) Account**.
2. **Domain**: You should either have a domain registered through AWS or any other domain registrar.
3. **Amazon EC2 Instance**: A running EC2 instance with a public IP.
4. **Route 53 Hosted Zone**: The domain must be set up in Amazon Route 53.

---

## Steps

### Step 1: Launch an EC2 Instance

1. **Launch an EC2 instance** in your AWS account.
   - Ensure it has a **public IP**.
   - Make sure **security groups** allow HTTP (port 80) and SSH (port 22) access from the internet.

2. Once the EC2 instance is launched, install a web server (e.g., NGINX) to serve web content.
   
   Example User Data script to install NGINX:
   ```bash
   #!/bin/bash
   sudo apt update
   sudo apt install -y nginx
   echo "HI! I am here to learn AWS" > /var/www/html/index.html
   sudo systemctl restart nginx
   ```

3. Note down the **Public IP Address** of the EC2 instance.

<img width="960" alt="ec2 insstance" src="https://github.com/user-attachments/assets/60f05795-9a0b-4e19-a9e5-529f295535c6">

### Step 2: Configure a Hosted Zone in Route 53

1. Open **Route 53** in the AWS Management Console.
   
2. In the navigation pane, choose **Hosted Zones**.

3. If you do not already have a hosted zone for your domain:
   - Create a new **Hosted Zone**.
   - Add the domain name for which you'd like to create records (e.g., `practiceaws.click`).
   
4. You should see your hosted zone in the list.

### Step 3: Create an "A" Record for Your EC2 Instance

1. In the **Hosted Zone** for your domain, choose **Create Record**.

2. Configure the following:
   - **Record Type**: `A` (routes traffic to an IPv4 address)
   - **Record Name**: (Leave blank to create a root domain record, e.g., `practiceaws.click`)
   - **Value**: Enter the **public IPv4 address** of your EC2 instance.
   - **TTL (seconds)**: Set to `60` seconds or the default value.
   - **Routing Policy**: Simple routing.

3. Click **Create records**.
   
   <img width="960" alt="Hosted-Zone-A Name" src="https://github.com/user-attachments/assets/4548b0c7-11d3-4744-abd8-7105846bd5fe">

### Step 4: Wait for DNS Propagation

DNS changes may take a few minutes to propagate. Once the changes are propagated, you should be able to access your EC2 instance using the domain name.

For example:
```bash
http://practiceaws.click
```

This should load the NGINX page or any content hosted on your EC2 instance.

### Step 5: Verify Domain Resolution

To verify that your domain is pointing to your EC2 instance:
1. Open your browser and enter the domain name in the address bar (e.g., `practiceaws.click`).
2. If configured correctly, you should see the webpage hosted on your EC2 instance (e.g., "HI! I am here to learn AWS").

<img width="960" alt="ec2_instance_is-Accessed_by_a_Domain" src="https://github.com/user-attachments/assets/a9798ea0-5a70-45d4-94eb-de54eeed626a">

---

## Example Output

If successful, your browser should display the following content when you visit your domain:

```html
HI! I am here to learn AWS
```

## Troubleshooting

1. **DNS Not Resolving**: Wait for DNS propagation, which can take a few minutes.
2. **Public IP Inaccessible**: Ensure that the security group of your EC2 instance allows traffic on port 80 (HTTP).
3. **Domain Name Mismatch**: Double-check the domain name configuration in Route 53.
4. **Incorrect Web Server Configuration**: Ensure the web server (e.g., NGINX) is correctly set up and running on the EC2 instance.

---

## Conclusion

This guide provides a step-by-step method for associating a domain name to an EC2 instance using Route 53. This method is useful for creating a memorable, easy-to-access name for websites hosted on EC2 instances.

---
