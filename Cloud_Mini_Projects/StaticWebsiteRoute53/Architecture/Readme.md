# Hosting a Static Website on AWS with CloudFront, S3, Route 53, and SSL

## Overview

This project describes how to host a static website using AWS services: S3, Route 53, CloudFront, and ACM (AWS Certificate Manager) to enable secure HTTPS access. The domain used is `practiceaws.click` along with the subdomain `www.practiceaws.click`. 

The steps outlined below explain how a request from the user reaches the hosted website and how AWS services work together to deliver content efficiently and securely.

## Architecture Workflow

This architecture is designed to deliver content to users from the closest geographical edge location using CloudFront CDN, secured with HTTPS via ACM. The website content is stored in S3, and the domain is managed with Route 53.

## Visual Architecture Flow:

![Hosting Static Website on S3 Bucket and DNS Resolution](https://github.com/user-attachments/assets/32822c7b-8c5c-4f76-91e0-c5375b1bd583)


---

## Tools Used:
1. **Amazon S3** – For static website hosting.
2. **Amazon CloudFront** – For content delivery with caching and HTTPS.
3. **Amazon Route 53** – For DNS resolution and domain management.
4. **AWS Certificate Manager (ACM)** – For SSL certificates to enable secure communication.
5. **A registered domain name** – `practiceaws.click` and `www.practiceaws.click`.

---

## Detailed Steps

### 1. **User Enters Domain in Browser**
   
   - **Scenario 1**: The user enters `practiceaws.click` in their browser.
   - **Scenario 2**: The user enters `www.practiceaws.click` in their browser.

   The browser sends an HTTPS request for the specified domain.

---

### 2. **DNS Resolution via Route 53**

   - Route 53 is responsible for resolving the domain name (`practiceaws.click` or `www.practiceaws.click`) to an IP address.
   - Route 53 checks its Hosted Zone configuration for DNS records associated with these domains.
   
   - **For both domains**, Route 53 points to the CloudFront distribution via an **Alias Record**. This Alias record directs traffic to the CloudFront distribution's URL.

---

### 3. **CloudFront Distribution Handles the Request**

   - Once the DNS lookup is complete, the browser receives the **IP address of the CloudFront Distribution** and sends the HTTPS request.
   - **CloudFront** handles the request by checking its cache to see if the content for the requested domain is already stored in an edge location.

   **Cached Content:**
   - If the requested content (website files such as `index.html`, CSS, images) is cached, CloudFront immediately delivers it from the nearest edge location, reducing latency and speeding up delivery.

   **Non-Cached Content:**
   - If the content is **not cached**, CloudFront forwards the request to the **Origin**, which in this case is the **S3 bucket** hosting the static website.

---

### 4. **Content is Delivered from S3 (Origin) if Not Cached**

   - If CloudFront does not have the content cached, it fetches the website files from the **Amazon S3 bucket** that is configured as the Origin for the CloudFront distribution.
   - The S3 bucket contains the static website files (HTML, CSS, JS) and serves them as a static website.

---

### 5. **SSL/TLS Encryption via ACM (Secure HTTPS)**

   - **CloudFront** is configured with an SSL certificate issued by **AWS Certificate Manager (ACM)**. 
   - The SSL certificate is tied to both the root domain (`practiceaws.click`) and the subdomain (`www.practiceaws.click`).
   - This ensures that all communication between the user's browser and CloudFront happens over a secure **HTTPS** connection.

---

### 6. **Response is Sent Back to the User**

   - After retrieving the website content (either from CloudFront's cache or directly from S3), CloudFront delivers the website files to the user's browser over **HTTPS**.
   - The user can now view the website in their browser, and CloudFront caches the content for future requests, improving performance for other users in the same geographical region.

---

## Domain and Caching Details

- **practiceaws.click** and **www.practiceaws.click** both resolve to the same CloudFront distribution.
- The **Alias record** in Route 53 points to the CloudFront distribution URL.
- CloudFront uses **Global Edge Locations** to cache content closer to the users, resulting in faster load times.
- **SSL** ensures the communication is encrypted, providing security and trust to users accessing the website.

---

## Conclusion

This setup ensures that your static website is highly available, scalable, and secure with global content distribution and HTTPS enabled. The use of Route 53 for DNS, CloudFront for caching and performance optimization, S3 for static website hosting, and ACM for SSL certification makes this architecture efficient and secure.

---



### Next Steps:
- Test the website by visiting `https://practiceaws.click` and `https://www.practiceaws.click`.
- Ensure that both domains respond with the correct content over HTTPS.
- Check the performance improvements via CloudFront's caching.

---
### Visual Architecture Flow:

*Diagram created using [Lucidchart](https://www.lucidchart.com).*
