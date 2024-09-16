# Retrieving Client IP Address in AWS Application Load Balancer (ALB)

In an AWS Application Load Balancer (ALB) environment, one of the common needs for backend systems, especially for logging and security purposes, is to know the **client’s original IP address**. By default, ALBs forward traffic to backend instances, but the IP address seen by the backend is usually the load balancer's IP rather than the original client IP. This guide explains how to retrieve the client's IP address, why it is important, how to implement it, and how to edit load balancer attributes.

---

## Table of Contents

- [Why is Retrieving the Client IP Address Important?](#why-is-retrieving-the-client-ip-address-important)
- [How to Retrieve the Client IP Address](#how-to-retrieve-the-client-ip-address)
- [Steps to Implement Client IP Logging in NGINX](#steps-to-implement-client-ip-logging-in-nginx)
- [When Should You Use It?](#when-should-you-use-it)
- [Edit Load Balancer Attributes](#edit-load-balancer-attributes)
- [Screenshots](#screenshots)

---

## Why is Retrieving the Client IP Address Important?

When an Application Load Balancer (ALB) forwards traffic to your backend services, it masks the original client's IP address because the traffic is sent through the load balancer itself. For various reasons, including:

- **Security Logging**: Logging the real client IP helps track malicious or unauthorized activity.
- **Rate Limiting**: Some applications implement request rate limiting based on the IP address of the client. Without the client’s real IP, rate limiting would apply to the ALB’s IP, which doesn’t represent the individual client.
- **Geo-location Tracking**: Knowing the client’s IP is useful for geo-location services that provide personalized content or restrict access based on location.

ALB forwards the client’s original IP address in a special **HTTP header** called `X-Forwarded-For`. The backend services need to be configured to extract this information for logging and processing purposes.

---

## How to Retrieve the Client IP Address

The client IP address is forwarded by the ALB in the `X-Forwarded-For` HTTP header. This header contains a list of IP addresses, and the first one is the **client’s original IP address**. 

For example:

```
X-Forwarded-For: 203.0.113.195, 70.41.3.18
```

Here:
- `203.0.113.195` is the original client’s IP.
- `70.41.3.18` is an intermediary IP (like a proxy).

The backend application or web server needs to capture this header to log the client’s IP.

### Steps:

1. **Enable Logging for X-Forwarded-For**: You need to configure your backend services, such as NGINX or Apache, to log the client’s IP from the `X-Forwarded-For` header.
2. **Modify Web Server Configurations**: Extract the client IP from the header and ensure it's used correctly in logs.

---

## Steps to Implement Client IP Logging in NGINX & also Checking Nginx Access Logs for ALB Health Checks

Access Nginx's access logs:

1. Navigate to the **Nginx logs** directory:
   ```bash
   cd /var/log/nginx/
   ```

2. Run the `tail` command to monitor the access logs in real-time:
   ```bash
   tail -f *.log
   ```

3. You should see logs like the following, indicating successful health checks from the ALB:
   ```
   172.31.18.150 - - [16/Sep/2024:06:49:10 +0000] "GET / HTTP/1.1" 200 409 "-" "ELB-HealthChecker/2.0"
   172.31.4.67 - - [16/Sep/2024:06:49:26 +0000] "GET / HTTP/1.1" 200 409 "-" "ELB-HealthChecker/2.0"
   ```

4. The IP addresses represent the ALB's health checkers, and the `200` status indicates that the instance is responding correctly.
5. 
If you're using **NGINX** as the web server behind your Application Load Balancer, here’s how you can configure it to log the client’s IP address using the `X-Forwarded-For` header:

1. **Open your NGINX configuration file**.
   ```bash
   sudo su
   vi nginx.conf
   ```
   - i for Insert Mode
   - write your desired log
   - Esc - :wq and press Enter to exit the configuration.
     
2. Add a custom log format that includes the `X-Forwarded-For` header. For example:

   ```nginx
   log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                     '$status $body_bytes_sent "$http_referer" '
                     '"$http_user_agent" "$http_x_forwarded_for"';
   
   access_log /var/log/nginx/access.log main;
   ```

   Here:
   - `$remote_addr` is the IP address of the load balancer.
   - `$http_x_forwarded_for` is the original client’s IP address.

3. **Reload NGINX** to apply the changes:

   ```bash
   servive nginx reload
   ```

Now, the client IP will appear in your access logs.

---

## When Should You Use It?

1. **Security Auditing**: When you need to track requests by the original IP to monitor suspicious or malicious activity.
2. **Web Analytics**: When you need to track visitors' geographic locations or IP-based statistics.
3. **Rate Limiting**: When your application limits the number of requests per IP address, knowing the client’s original IP is essential.
4. **Compliance**: For compliance with regulations that require logging the source of requests, you need to record the real client IP.
5. **Debugging**: When troubleshooting an issue with your application, knowing the client’s IP can help trace specific users' requests.

---
Here’s an updated section of the **`README.md`** file that includes details about the **Edit Load Balancer Attributes** screen you’ve shared:

---

## Edit Load Balancer Attributes

<img width="655" alt="editLB" src="https://github.com/user-attachments/assets/5ddd47e7-1451-4825-aec8-dfb1bfdc1575">

The **Load Balancer Attributes** in AWS allow you to configure specific settings to control the behavior of your Application Load Balancer (ALB). These settings help optimize performance, security, and logging.

### Important Load Balancer Attributes:

1. **Deletion Protection**: 
   - When enabled, this prevents the ALB from being accidentally deleted. Useful for production environments to avoid unintended deletion.

2. **Idle Timeout**: 
   - Specifies the maximum time (in seconds) that a connection is allowed to be idle (no data transfer). 
   - Example: A 60-second idle timeout means if no data is transmitted for 60 seconds, the connection will be closed.

3. **Cross-Zone Load Balancing**: 
   - When enabled, this distributes incoming traffic evenly across all registered targets across different Availability Zones.
   - This improves resilience and ensures that instances in all AZs share the load.

4. **HTTP/2**: 
   - Enable this to support HTTP/2 connections. HTTP/2 provides improved performance with multiplexing, which allows multiple requests to be sent on the same connection without blocking.

5. **Desync Mitigation Mode**: 
   - Provides protection against HTTP desynchronization attacks.
   - Modes:
     - **Defensive**: The default setting for general protection.
     - **Strictest**: Provides maximum protection by mitigating edge cases, but could impact legitimate traffic.
     - **Monitor**: Logs potential desync attacks but doesn’t block traffic.

6. **Preserve Host Header**: 
   - This preserves the original `Host` header sent by the client instead of replacing it with the load balancer's DNS name.

7. **X-Forwarded-For Header**: 
   - Allows you to handle client IP addresses. You can:
     - **Append**: Add the client’s IP to the existing `X-Forwarded-For` header.
     - **Preserve**: Maintain the current `X-Forwarded-For` header sent by the client.
     - **Remove**: Delete the `X-Forwarded-For` header before sending the request to your backend.

8. **Drop Invalid Header Fields**: 
   - When enabled, the ALB drops requests with malformed or invalid headers, improving security by preventing malicious requests from reaching your backend.

9. **Access Logs**: 
   - Enable this to capture detailed access logs for requests sent to your ALB. These logs are stored in an Amazon S3 bucket for further analysis and troubleshooting.

10. **WAF Fail Open**: 
    - In case AWS Web Application Firewall (WAF) fails to evaluate requests, enabling this option allows the ALB to continue routing requests without WAF protection. Useful when you prioritize availability over WAF filtering.

11. **TLS Version and Cipher Headers**: 
    - Configure which versions of TLS and associated ciphers are supported for secure communication. This allows for enhanced security by disabling weak or outdated TLS versions.

12. **Client Port Preservation**: 
    - When enabled, the source port used by the client to connect to the ALB is preserved when forwarding requests to the target. Otherwise, the ALB uses its own port when communicating with the backend.

### Example Use Cases:

- **Cross-Zone Load Balancing**: Essential for ensuring your backend services are distributed evenly across all availability zones, which helps improve redundancy and availability.
  
- **Preserve Host Header**: Useful when you need to maintain the client’s original `Host` header (for example, in multi-tenant applications where routing is based on the `Host` header).

- **X-Forwarded-For**: This is crucial for logging client IPs, as the client’s original IP can get lost in the traffic routed through the load balancer.

---

### Conclusion:

The file provides critical configurations that help optimize the performance and security of your Application Load Balancer. By understanding each attribute and how it affects your load balancer's behavior, you can fine-tune the ALB to meet the specific needs of your application, ensuring that it operates securely, efficiently, and reliably.
