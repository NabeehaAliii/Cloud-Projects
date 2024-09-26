# **DNS Resolution and Static Website Hosting on AWS**

## **Table of Contents**

1. [Project Overview](#project-overview)
2. [Architecture Diagram](#architecture-diagram)
3. [Key Components](#key-components)
4. [Step-by-Step Flow Explanation](#step-by-step-flow-explanation)
   - [User Access via Browser](#1-user-access-via-browser)
   - [DNS Resolver Queries Root and TLD Servers](#2-dns-resolver-queries-root-and-tld-servers)
   - [TLD Name Server Directs to Route 53 Name Server](#3-tld-name-server-directs-to-route-53-name-server)
   - [Route 53 Responds with CNAME Record](#4-route-53-responds-with-cname-record)
   - [Browser Connects to S3 Static Website](#5-browser-connects-to-s3-static-website)
   - [AWS Cloud Setup](#6-aws-cloud-setup)
5. [Technical Setup Summary](#technical-setup-summary)
6. [Phases of DNS Resolution and Static Website Hosting Process](#phases-of-dns-resolution-and-static-website-hosting-process)
   - [Phase 1: DNS Resolution Process (Blue Lines)](#phase-1-dns-resolution-process-blue-lines)
   - [Phase 2: Connection to S3 Website (Black Lines)](#phase-2-connection-to-s3-website-black-lines)
7. [Summary](#summary)
8. [Additional Notes for the README](#additional-notes-for-the-readme)

---

## **Project Overview**

This project demonstrates the process of hosting a static website on AWS using **S3** and **Route 53**. It provides details about how DNS resolution is handled when users access a domain (e.g., `www.practiceaws.click`), and how the domain is mapped to a static website hosted in an S3 bucket.

---

## **Architecture Diagram**

The following architecture diagram illustrates the flow of DNS resolution and the process of hosting a static website using AWS S3 and Route 53.

![Hosting Static Website on S3 Bucket and DNS Resolution](https://github.com/user-attachments/assets/2b36a0ed-60bb-4015-8002-8108351e5a52)

---

## **Key Components**

1. **S3 Bucket**: The storage service used to host the static website.
   - The S3 bucket is configured with **Static Website Hosting Enabled** to serve the website files (HTML, CSS, JS).

2. **Route 53**: AWS's scalable DNS service that resolves domain names to IP addresses or other endpoints (in this case, the S3 static website URL).

3. **Custom Domain**: Users access the static website through a custom domain (e.g., `www.practiceaws.click`).

---

## **Step-by-Step Flow Explanation**

### **1. User Access via Browser**
- **Users** type the URL `www.practiceaws.click` in their browser to access the website.
- The browser initiates a DNS query to resolve the domain name to an IP address.

### **2. DNS Resolver Queries Root and TLD Servers**
- The **DNS Resolver** (e.g., from the user's ISP or a public DNS resolver like Google's 8.8.8.8) receives the query and begins the resolution process:
  - The DNS resolver first queries the **DNS Root Name Server** to find the authoritative server for the Top-Level Domain (TLD) `.click`.
  - The **Root Name Server** responds by directing the DNS resolver to the **TLD Name Server** for `.click`.

### **3. TLD Name Server Directs to Route 53 Name Server**
- The **TLD Name Server** for `.click` receives the query and responds with the location of the authoritative **Name Server**, in this case, AWS **Route 53**, which is managing the DNS records for the domain `www.practiceaws.click`.

### **4. Route 53 Responds with CNAME Record**
- The DNS resolver forwards the query to the **Route 53 Name Server**.
- **Route 53** is configured with a **CNAME record** that maps the custom domain `www.practiceaws.click` to the S3 static website URL (`www.practiceaws.click.s3-website.ap-south-1.amazonaws.com`).
- The **Route 53 Name Server** returns the **S3 website's URL** back to the DNS resolver.

### **5. Browser Connects to S3 Static Website**
- The DNS resolver now has the **S3 URL** and forwards it to the user's browser.
- The **Browser** uses the URL to connect to the **S3 Bucket** where the static website is hosted.
- The **S3 bucket**, configured for static website hosting, serves the HTML, CSS, and other assets to the user, allowing them to view the website.

### **6. AWS Cloud Setup**
- In AWS, the following resources are set up:
  1. **S3 Bucket**: A bucket named `www.practiceaws.click` is created with **static website hosting enabled**. The website files (HTML, CSS, etc.) are uploaded to this bucket.
  2. **Route 53 Hosted Zone**: A hosted zone is created for the domain `www.practiceaws.click`. A **CNAME record** is added in the hosted zone, mapping the domain to the S3 static website URL.

---

## **Technical Setup Summary**

### **S3 Bucket Configuration**
- **Bucket Name**: `www.practiceaws.click`.
- **Static Website Hosting**: Enabled.
- **Website URL**: `www.practiceaws.click.s3-website.ap-south-1.amazonaws.com`.
- Files (HTML, CSS, etc.) are uploaded to this bucket, and it serves the static website content.

### **Route 53 Configuration**
- **Hosted Zone**: A hosted zone is created for `www.practiceaws.click`.
- **CNAME Record**: A CNAME record is added to Route 53 to resolve `www.practiceaws.click` to the S3 static website URL.

### **DNS Resolution Process**
1. User types `www.practiceaws.click`.
2. DNS resolver queries **Root Name Server** for `.click` TLD.
3. The **TLD Name Server** points to the **Route 53 Name Server**.
4. Route 53 returns the **S3 website URL** (`www.practiceaws.click.s3-website.ap-south-1.amazonaws.com`).
5. The user’s browser connects to the S3 bucket using the provided URL.

---

### **Phases of DNS Resolution and Static Website Hosting Process**

The diagram represents the flow of how DNS resolution occurs when a user accesses the static website hosted on Amazon S3. The process is divided into two key phases, each represented with different colored lines to distinguish the stages.

---

### **Phase 1: DNS Resolution Process (Blue Lines)**

The **blue lines** represent the **DNS resolution process** that occurs when a user types the domain name (`www.practiceaws.click`) into their browser. This phase involves the following steps:

1. **User Requests the Website**:
   - A user types the domain `www.practiceaws.click` into their browser.
   - The browser sends a DNS query to the **DNS Resolver** (often provided by the user’s ISP or a public DNS service like Google DNS).

2. **DNS Resolver Queries Root Name Server**:
   - The **DNS Resolver** forwards the request to the **DNS Root Name Server**, asking for the authoritative name server for the `.click` Top-Level Domain (TLD).

3. **Root Name Server Responds**:
   - The **Root Name Server** directs the query to the **TLD Name Server** for `.click`.

4. **TLD Name Server Points to Route 53**:
   - The **TLD Name Server** responds by directing the query to the **Route 53 Name Server**, which is the authoritative name server for `www.practiceaws.click`.

5. **Route 53 Provides the Website URL**:
   - The **Route 53 Name Server** contains a **CNAME record** that maps `www.practiceaws.click` to the Amazon S3 static website URL (`www.practiceaws.click.s3-website-ap-south-1.amazonaws.com`).
   - The **DNS Resolver** retrieves this information and passes it back to the user's browser.

---

### **Phase 2: Connection to the S3 Website (Black Lines)**

The **black lines** represent the second phase of the process, where the browser uses the resolved S3 URL to connect to the static website hosted in the Amazon S3 bucket.

1. **Browser Connects to S3 Website**:
   - Once the DNS resolver returns the S3 website URL (`www.practiceaws.click.s3-website-ap-south-1.amazonaws.com`), the browser directly connects to the **S3 Bucket** where the static website is hosted.
   - The S3 bucket is configured with **Static Website Hosting Enabled**, and it serves the HTML, CSS, and other website files to the user’s browser.

2. **User Views the Website**:
   - The website content is then loaded in the user’s browser, completing the process.

---

### **Summary**:
- The **blue lines** in the diagram trace the **DNS resolution phase**, showing how the query moves through the DNS system, starting from the user’s request and ending at Route 53.
- The **black lines** represent the **website connection phase**, where the browser uses the resolved S3 URL to load and display the static website.

This color-coding helps distinguish between the DNS resolution and the final connection to the hosted website on S3, making it easier to follow the process step by step.

