# AWS Network Load Balancer (NLB)

This document provides an in-depth overview of the **AWS Network Load Balancer (NLB)**, covering its purpose, architecture, key features, setup, and best practices. The document also explains the differences between NLB and other types of AWS load balancers, along with use cases, cost considerations, and common configurations.

---

## Table of Contents

1. [What is a Network Load Balancer (NLB)?](#what-is-a-network-load-balancer-nlb)
2. [Key Features of NLB](#key-features-of-nlb)
3. [How NLB Works](#how-nlb-works)
4. [Use Cases for NLB](#use-cases-for-nlb)
5. [Setting Up a Network Load Balancer](#setting-up-a-network-load-balancer)
6. [Target Groups in NLB](#target-groups-in-nlb)
7. [Health Checks in NLB](#health-checks-in-nlb)
8. [NLB vs ALB vs Classic Load Balancer](#nlb-vs-alb-vs-classic-load-balancer)
9. [Best Practices for NLB](#best-practices-for-nlb)
10. [Monitoring and Logging](#monitoring-and-logging)
11. [Security Considerations](#security-considerations)
12. [Q&A Section](#qna-section)
13. [Conclusion](#conclusion)

---

## What is a Network Load Balancer (NLB)?

An **AWS Network Load Balancer (NLB)** is designed to handle extreme performance and manage large-scale traffic at the **transport layer (Layer 4)** of the OSI model. It can handle millions of requests per second while maintaining ultra-low latencies, making it ideal for real-time applications and large workloads.

### Key Points:
- **Layer 4 Load Balancer**: Operates at the transport layer and routes traffic based on IP address and port.
- **Connection-based Routing**: NLB establishes connections to the backends and forwards traffic without modifying it.
- **Designed for Extreme Performance**: It can scale automatically to handle massive traffic spikes.
- **Preserves Client IP Address**: This is useful for applications that need the source IP for security or logging.

---

## Key Features of NLB

1. **Extreme Performance**:
   - NLB can handle millions of requests per second, ensuring the best performance for high-throughput applications.
  
2. **Low Latency**:
   - Provides ultra-low latencies, with performance optimized for real-time use cases like gaming, video streaming, and financial services.
  
3. **Static IP Support**:
   - NLB supports **Elastic IPs (EIPs)**, allowing you to assign a static IP to the load balancer, which simplifies integration with other systems.

4. **Preserve Client IP**:
   - NLB preserves the source IP of the client, allowing backend instances to see the actual client IP.

5. **Cross-Zone Load Balancing**:
   - Distributes traffic evenly across backend instances in multiple availability zones, ensuring resilience.

6. **TLS Termination**:
   - NLB supports **TLS (Transport Layer Security) termination**, offloading the decryption work from the backend servers.

7. **TCP/UDP Load Balancing**:
   - Supports both **TCP** and **UDP** protocols, making it highly flexible for a wide range of applications.
  
8. **Target Groups**:
   - You can register EC2 instances, IP addresses, or Lambda functions as targets in NLB target groups.

---

## How NLB Works

1. **Client Connection**:
   - A client connects to the NLB via a **static IP** or **DNS name**. The NLB listens for requests on a specific port (e.g., TCP:80).

2. **Forwarding Requests**:
   - NLB routes the request to one of the backend targets (EC2 instance, IP address, or Lambda function) in the target group, based on the listener configuration.

3. **Connection Handling**:
   - The NLB forwards the traffic **without modifying** the content, maintaining ultra-low latency and high throughput.
   
4. **Health Checks**:
   - NLB uses health checks to ensure that only healthy targets receive traffic.

---

## Use Cases for NLB

1. **Real-time Applications**: Low-latency workloads like online gaming, video streaming, and real-time communications benefit from the performance of NLB.
  
2. **Microservices with Multiple Protocols**: Applications that use both TCP and UDP traffic (e.g., DNS servers, VoIP) can leverage NLB’s multi-protocol support.
  
3. **Financial Services and Trading Platforms**: High-performance trading platforms with low-latency requirements.
  
4. **IoT and High Throughput Systems**: Applications requiring large-scale traffic handling with minimal latency and high reliability.

5. **TLS Termination at Load Balancer**: Applications needing encrypted communication without burdening backend services.

---

## Setting Up a Network Load Balancer

### Step 1: Create an NLB

1. Go to **EC2 Dashboard** > **Load Balancers**.
2. Choose **Create Load Balancer**.
3. Select **Network Load Balancer**.
4. Provide a **name**, **scheme** (Internet-facing or Internal), and **IP address type** (IPv4 or Dualstack).

### Step 2: Configure Listener

1. Add a **listener** on a specific port (e.g., TCP:80).
2. Specify the protocol (TCP/UDP/TLS).

### Step 3: Configure Availability Zones

1. Select the **VPC** and **subnets** in the **Availability Zones** where the NLB will operate.
2. Choose to assign **Elastic IPs** (optional) for static IP addressing.

### Step 4: Create Target Group

1. Define a **target group** and select the type of target (e.g., EC2 instance, IP addresses).
2. Set **Health Check** options (path, port, protocol).

### Step 5: Register Targets

1. Register your **EC2 instances** or **IP addresses** as targets in the target group.
2. Complete and **create the load balancer**.

---

## Target Groups in NLB

A **Target Group** in NLB defines the backend resources that will serve traffic routed by the NLB. Each target group can consist of **EC2 instances**, **IP addresses**, or **Lambda functions**.

- **Target Type**: Can be EC2 instances, IP addresses, or Lambda functions.
- **Protocol Support**: NLB supports **TCP**, **UDP**, and **TLS** protocols.
- **Health Checks**: NLB regularly checks the health of the targets to ensure traffic is only routed to healthy instances.

---

## Health Checks in NLB

NLB performs **health checks** on targets to ensure that traffic is only routed to healthy instances. If an instance becomes unhealthy, NLB automatically stops sending traffic to that instance until it passes the health checks again.

- **Protocol**: The protocol for health checks (TCP or HTTP).
- **Path**: For HTTP health checks, specify a path (e.g., `/health`).
- **Interval**: The time interval between health checks.
- **Threshold**: The number of failed checks required to mark a target as unhealthy.

---

## NLB vs ALB vs Classic Load Balancer

| Feature                         | **Network Load Balancer (NLB)**                | **Application Load Balancer (ALB)**           | **Classic Load Balancer**                   |
|----------------------------------|------------------------------------------------|-----------------------------------------------|--------------------------------------------|
| **OSI Layer**                    | Layer 4 (Transport Layer)                      | Layer 7 (Application Layer)                   | Layer 4 and Layer 7                        |
| **Protocols**                    | TCP, UDP, TLS                                  | HTTP, HTTPS, WebSockets                       | HTTP, HTTPS, TCP                           |
| **Latency**                      | Ultra-low latency                              | Higher latency due to application-layer processing | Moderate                                  |
| **Content-based Routing**        | No                                             | Yes (host/path-based routing)                 | No                                         |
| **Preserve Client IP**           | Yes                                            | No                                            | Yes                                        |
| **SSL Termination**              | Yes                                            | Yes                                            | Yes                                        |
| **Target Types**                 | Instances, IP addresses, Lambda                | Instances, IP addresses, Lambda               | Instances only                             |
| **Static IP Support**            | Yes (Elastic IPs)                              | No                                            | No                                         |

---

## Best Practices for NLB

1. **Use Elastic IPs**: Assign **Elastic IPs (EIPs)** to ensure a static IP address for your NLB. This helps in whitelisting IPs and integrating with external services.
  
2. **Enable Cross-Zone Load Balancing**: Ensures even distribution of traffic across multiple availability zones, providing high availability and fault tolerance.
  
3. **Optimize Health Checks**: Set appropriate **intervals** and **thresholds** for health checks to ensure that only healthy instances receive traffic.

4. **Preserve Client IP Address**: If your backend application requires the client’s original IP address for logging or security, ensure that **preserve client IP** is enabled.

5. **Use TLS Termination**: Offload TLS decryption to the load balancer to reduce the CPU load on backend instances.

---

## Monitoring and Logging

### CloudWatch Metrics:
- **Request Count**: Number of requests the NLB receives.
- **Active Flow Count**: Number of active TCP/UDP flows.
- **HealthyHostCount**

: The number of healthy targets in each target group.
  
### Access Logs:
- Enable **access logs** for detailed information about the requests handled by the NLB. You can store logs in **S3** for further analysis.

---

## Security Considerations

1. **Security Groups**: NLB does not directly support security groups, but you must apply security groups to your backend EC2 instances.
   
2. **TLS/SSL Certificates**: Use AWS **Certificate Manager (ACM)** to manage SSL/TLS certificates for encrypted traffic.

3. **Preserving Client IP**: Ensure that the backend EC2 instances or services are properly configured to handle the client's original IP.

4. **Network ACLs**: Apply **Network Access Control Lists (ACLs)** to VPC subnets for an additional layer of security.

---

## Q&A Section

### Q: What is the main difference between NLB and ALB?
   - **Answer**: NLB operates at Layer 4 (transport layer), forwarding traffic based on IP address and port without inspecting the content, making it ideal for high-performance, low-latency workloads. ALB, on the other hand, operates at Layer 7 (application layer) and routes traffic based on the content of the HTTP request, such as URL paths or headers.

### Q: Can I assign a static IP to NLB?
   - **Answer**: Yes, NLB supports assigning **Elastic IPs** to provide a static IP for the load balancer, which is useful for whitelisting and integration with external systems.

### Q: Can I use NLB for both TCP and UDP traffic?
   - **Answer**: Yes, NLB supports both **TCP** and **UDP** protocols, making it versatile for different kinds of applications like VoIP, DNS, and real-time communications.

### Q: What are the benefits of preserving the client IP address?
   - **Answer**: Preserving the client IP address allows your backend instances or services to see the original IP address of the client, which is useful for logging, auditing, and security purposes.

### Q: Does NLB support WebSocket connections?
   - **Answer**: No, WebSockets are supported only by the **Application Load Balancer (ALB)**.

---

## Conclusion

The **AWS Network Load Balancer (NLB)** is an ideal solution for handling high-performance, low-latency traffic at the transport layer (Layer 4). It supports millions of requests per second and preserves the client IP, which is important for applications requiring source IP visibility. Whether you're managing a high-throughput application or a real-time service, NLB provides the reliability, performance, and scalability needed to handle the most demanding workloads.

By following the best practices and leveraging NLB’s advanced features like **TLS termination**, **health checks**, and **static IP support**, you can optimize your application’s performance and reliability.
