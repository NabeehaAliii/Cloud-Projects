# AWS Classic Elastic Load Balancer (ELB)

## Overview
An **Elastic Load Balancer (ELB)** is a service that automatically distributes incoming application traffic across multiple Amazon EC2 instances. ELB helps improve the fault tolerance of your applications by providing high availability and automatic scaling.

---

## Table of Contents
- [What is an Elastic Load Balancer?](#what-is-an-elastic-load-balancer)
- [How Does ELB Work?](#how-does-elb-work)
- [Types of Load Balancers](#types-of-load-balancers)
- [How to Create and Use a Classic ELB](#how-to-create-and-use-a-classic-elb)
- [Health Checks in ELB](#health-checks-in-elb)
- [Security Groups and ELB](#security-groups-and-elb)
- [Benefits of Using ELB](#benefits-of-using-elb)
- [FAQ: Classic ELB Usage](#faq-classic-elb-usage)

---

## What is an Elastic Load Balancer?

Elastic Load Balancer (ELB) is a fully managed load balancing service that automatically distributes incoming application traffic across multiple EC2 instances to maximize performance and ensure high availability.

It helps in:
- **Distributing traffic** across multiple instances.
- **Detecting unhealthy instances** and rerouting traffic to healthy instances.
- **Scaling** your application as traffic increases.

---

## How Does ELB Work?

### Key Components:
- **Listeners**: A listener checks for connection requests from clients using the protocol and port configured (e.g., HTTP, HTTPS). It forwards requests to the EC2 instances behind the ELB.
- **Load Balancing Algorithm**: ELB uses round-robin or sticky sessions to distribute requests to EC2 instances.
- **Health Checks**: ELB performs health checks on the EC2 instances to ensure they are healthy and can handle traffic.

Here’s a high-level workflow:
1. **User Request**: A user makes a request to your website (e.g., www.yoursite.com).
2. **Route to Load Balancer**: The domain (DNS) points to the DNS name of the Load Balancer.
3. **Distribute Traffic**: The Load Balancer forwards the request to one of the healthy EC2 instances based on its routing logic.
4. **Return Response**: The EC2 instance processes the request and returns the result via the Load Balancer.

---

## Types of Load Balancers

AWS provides three types of Load Balancers:
1. **Classic Load Balancer (CLB)**: The legacy option used primarily for simple use cases.
2. **Application Load Balancer (ALB)**: Best for Layer 7 (HTTP/HTTPS) traffic and when dealing with modern web apps or microservices.
3. **Network Load Balancer (NLB)**: Best for Layer 4 traffic, offering extremely low latency and TCP traffic management.

This document focuses on **Classic Load Balancer (CLB)**.

---

## How to Create and Use a Classic ELB

1. **Step 1: Create EC2 Instances**
   - First, launch multiple **EC2 instances** with your website deployed. These instances will handle the traffic coming through the Load Balancer.

2. **Step 2: Create a Classic Load Balancer**
   - Go to **EC2 Dashboard** > **Load Balancers** > **Create Load Balancer**.
   - Choose the **Classic Load Balancer**.
   - Set the **Listener** (protocol and port; e.g., HTTP:80).

3. **Step 3: Configure Health Checks**
   - Set up **Health Checks** to ensure traffic is routed to only healthy instances.
   - You can configure the path, protocol (HTTP), and response thresholds.
   
   Example from your screenshot:
   ```yaml
   Ping Protocol: HTTP
   Ping Port: 80
   Ping Path: /index.html
   Timeout: 2 seconds
   Interval: 5 seconds
   Unhealthy Threshold: 2 failures
   Healthy Threshold: 3 successes
   ```

4. **Step 4: Add Instances**
   - Add your EC2 instances to the Load Balancer. The ELB will distribute traffic between these instances.

5. **Step 5: Assign Security Groups**
   - Assign the **Load Balancer's security group** to allow traffic through HTTP/HTTPS.

---

## Health Checks in ELB

Health checks ensure that the ELB only forwards requests to **healthy EC2 instances**. Health checks ping your instances on a specific path and port (e.g., `/index.html` on port 80) and determine if the instance is **healthy** based on its response.

In your case, the **Health Check Settings** specify:
- **Ping Target**: The URL path (`/index.html`) where the health check will be made.
- **Timeout**: How long the ELB should wait for a response.
- **Healthy/Unhealthy Threshold**: The number of consecutive successful (or failed) checks to mark an instance as healthy or unhealthy.

### Screenshot Explanation:
The image shows how the ELB is configured to ping the `/index.html` file on port 80 using HTTP, ensuring that the instance is responsive and healthy before sending user traffic to it.

---

## Security Groups and ELB

When working with EC2 instances behind a Load Balancer, **Security Groups** are critical for controlling which traffic can reach your instances.

### Key Learning from Our Security Group Experiment:

1. **Initially**, the EC2 instances had a security group allowing traffic from **anywhere** (`0.0.0.0/0`) on port 80. This allowed anyone to access the instances directly.
   
2. **We removed the public access** rule (`0.0.0.0/0`) from the instances, which made them inaccessible directly. As a result, the Load Balancer couldn't forward traffic to them.

3. **We added a rule** that only allowed traffic from the **Load Balancer's Security Group**. This way, the instances could only be accessed via the Load Balancer, enhancing security.

### What We Learned:
- By removing `0.0.0.0/0`, we prevent **direct public access** to the instances. This means only the Load Balancer can route traffic to them, increasing security.
- The Load Balancer acts as a **gateway**, distributing traffic, while the EC2 instances are kept secure behind it.

---

## Benefits of Using ELB

1. **High Availability**: Automatically distributes incoming traffic across multiple instances in multiple Availability Zones.
2. **Fault Tolerance**: Detects unhealthy instances and routes traffic to healthy ones.
3. **Scaling**: ELB can handle increases in traffic by adding more instances behind it.
4. **Security**: Control access with security groups and use HTTPS for encrypted communication.
5. **Session Stickiness**: Keep users connected to the same instance if needed (session affinity).

---

## FAQ: Classic ELB Usage

### 1. **Why didn't my website work when I removed the HTTP rule (0.0.0.0/0) from the instances?**
   - When you removed the rule, the instances could no longer be accessed directly by the public or the Load Balancer. Adding the **Load Balancer's security group** to the EC2 instances’ inbound rules fixed this by allowing traffic only from the Load Balancer.

### 2. **What is the purpose of using a Load Balancer for my website?**
   - A Load Balancer distributes traffic across multiple EC2 instances to ensure high availability, fault tolerance, and scalability. It ensures that users get routed to healthy instances and prevents one instance from becoming a single point of failure.

### 3. **Where is my website exactly hosted?**
   - Your website is hosted on **EC2 instances**. The code (HTML/CSS/JS) and backend logic run on these instances. The **Load Balancer** distributes traffic to these instances based on availability and health.

### 4. **How do instances get created for my website?**
   - Instances are created manually or via automation tools. You develop your website, launch EC2 instances, deploy the code on those instances, and configure the Load Balancer to route traffic to them.

---

## Example Use Case: Website Hosted by AWS with ELB

### Scenario:
You’ve developed a website that needs to be **accessible globally**. To ensure high availability and performance, you decide to use an **Elastic Load Balancer (ELB)**.

### Steps:
1. **Launch EC2 Instances**: Deploy your website on multiple EC2 instances running web servers (e.g., Apache, Nginx).
2. **Configure ELB**: Set up an ELB to distribute traffic between these instances. 
3. **DNS Routing**: Use Route 53 to point your domain (e.g., www.mysite.com) to the ELB’s DNS name.
4. **User Access**: When users visit your website, their requests are routed to the Load Balancer, which forwards the traffic to one of your healthy EC2 instances.

---

## Conclusion

Classic Elastic Load Balancer (ELB) ensures that your website or application is highly available, scalable, and fault-tolerant by distributing traffic across multiple EC2 instances. By integrating ELB with **security groups**, **health checks**, and multiple instances, you can create a secure, performant, and reliable infrastructure for your website.
