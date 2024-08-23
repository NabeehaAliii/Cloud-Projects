# Nginx Installation and Configuration on EC2 Instance

This guide will walk you through the steps to install and configure Nginx on an EC2 instance running Ubuntu.

## Prerequisites
- You must have an EC2 instance running with SSH access.
- Ensure that your security group allows traffic on port 22 (for SSH) and port 80 (for HTTP).

## Step 1: Connect to Your EC2 Instance

First, connect to your running EC2 instance via SSH.

```bash
ssh -i "your-key.pem" ubuntu@your-ec2-public-ip
```

or you can directly connect through web browser.

## Step 2: Switch to the Root User

Once connected, switch to the root user to ensure you have the necessary permissions for installation.

```bash
sudo -i
```

## Step 3: Update Package List

Update the package list to ensure you have the latest information on the newest versions of packages and their dependencies.

```bash
apt-get update
```

## Step 4: Install Nginx

Install Nginx using the following command:

```bash
apt-get install nginx -y
```

## Step 5: Test Nginx Configuration

After the installation is complete, check the Nginx configuration to ensure it is correct. This command will validate the syntax of the configuration files.

```bash
nginx -t
```

If the configuration is correct, you will see a message like:

```plaintext
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## Step 6: Check the Status of Nginx

To verify that Nginx is running, check its status using the following command:

```bash
service nginx status
```

If Nginx is active and running, you will see:

```plaintext
Active: active (running)
```

## Step 7: Access Nginx on Localhost

You can now test if Nginx is serving content on localhost. Use the following command:

```bash
curl localhost
```

This command should return the HTML content of the default Nginx page.

## Step 8: Navigate to the Nginx Web Directory

To manage the web content served by Nginx, navigate to its web directory:

```bash
cd /var/www/html/
```

## Step 9: List the Files in the Web Directory

List the contents of the web directory to see the default files provided by Nginx:

```bash
ls
```

You should see a file named `index.nginx-debian.html`.

## Step 10: View the Default Nginx HTML File

You can view the contents of the default Nginx HTML file using the following command:

```bash
cat index.nginx-debian.html
```

## Step 11: Create a New HTML File

Now, let's create a new HTML file to serve as the main page. Use the following command to create a file named `index.html`:

```bash
echo "Hi! I am Nabeeha Ali" > index.html
```
>: The greater-than sign redirects the output from the command on its left to a file specified on its right.


## Step 12: Test the New HTML File

Finally, verify that your new file is being served by Nginx by running the following command:

```bash
curl localhost
```

You should see the message:

```plaintext
Hi! I am Nabeeha Ali
```

This confirms that Nginx is correctly serving your custom HTML file.

## Conclusion

You have successfully installed and configured Nginx on your EC2 instance, created a custom HTML file, and verified that it is being served correctly. You can now proceed to further customize your Nginx setup as needed.
```

This README.md format will guide users through the process of setting up and verifying Nginx on an EC2 instance.
