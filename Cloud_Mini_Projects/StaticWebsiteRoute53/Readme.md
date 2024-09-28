# **Hosting a Static Website on AWS with S3, Route 53, SSL, and CloudFront**

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Project Structure](#project-structure)
3. [Key Concepts](#key-concepts)
    - Amazon S3
    - Route 53
    - SSL (Secure Socket Layer)
    - CloudFront
4. [Tools Required](#tools-required)
5. [Project Setup](#project-setup)
    - [Step 1: Set up an S3 Bucket for Hosting a Static Website](#step-1-set-up-an-s3-bucket-for-hosting-a-static-website)
    - [Step 2: Configure a Route 53 Hosted Zone](#step-2-configure-a-route-53-hosted-zone)
    - [Step 3: Request and Configure an SSL Certificate](#step-3-request-and-configure-an-ssl-certificate)
    - [Step 4: Set up CloudFront Distribution](#step-4-set-up-cloudfront-distribution)
    - [Step 5: Connect SSL Certificate to CloudFront](#step-5-connect-ssl-certificate-to-cloudfront)
6. [Testing and Accessing Your Website](#testing-and-accessing-your-website)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)

---

## **Project Overview**

This project demonstrates how to host a static website using AWS services with secure HTTPS and a content delivery network (CDN). The major steps involve setting up an S3 bucket for the website, configuring CloudFront for global distribution, securing the site with SSL via AWS Certificate Manager (ACM), and using Route 53 to manage the domain and DNS settings.

**Domain**: `practiceaws.click` or `www.practiceaws.click`

---

## **Project Structure**

This project is composed of:
- **Amazon S3**: To store the static website content (HTML, CSS, JS, images).
- **Route 53**: To manage DNS resolution.
- **AWS Certificate Manager (ACM)**: To secure the website with an SSL certificate for HTTPS.
- **CloudFront**: To distribute the website globally and improve speed and security.

---

## **Key Concepts**

### **Amazon S3**
- **S3** stores your static website files.
- You enable **Static Website Hosting** in the S3 bucket to serve content publicly via a URL.

### **Route 53**
- **Route 53** manages the DNS of your domain.
- We will use an **Alias Record** to link your custom domain (e.g., `practiceaws.click`, `www.practiceaws.click`) to the S3 bucket or CloudFront distribution.

### **SSL (Secure Socket Layer)**
- **SSL Certificates** provide secure, encrypted communication over HTTPS.
- **AWS Certificate Manager (ACM)** offers free SSL certificates, which will be attached to the CloudFront distribution for this project.

### **CloudFront**
- **Amazon CloudFront** is a CDN that speeds up the website and ensures the content is distributed globally with low latency.
- CloudFront will distribute your S3-hosted website and ensure that it is served over HTTPS using the SSL certificate.

---

## **Tools Required**
1. AWS Management Console
2. Domain registered with Route 53 (practiceaws.click)
3. Static website files (HTML, CSS, images, etc.)
4. AWS Services: S3, CloudFront, Route 53, and ACM (for SSL)

---

## **Project Setup**

### **Step 1: Set up an S3 Bucket for Hosting a Static Website**

1. **Create an S3 bucket:**
   - In the **S3 Console**, create a bucket and name it after your domain, e.g., `practiceaws.click`.
   - Make sure **Block all public access** is unchecked to allow public access.
   - Click **Create bucket**.

2. **Upload your website files:**
   - Go to the **Objects** tab in your bucket and upload your static website files (HTML, CSS, JS, images).
   - Make sure you upload a default **index document** (e.g., `index.html`).
   - 
<img width="960" alt="uploading_objects_on_S3" src="https://github.com/user-attachments/assets/0e61ef5b-acb9-4270-a3eb-aef5526abaa3">

3. **Enable Static Website Hosting:**
   - In the **Properties** tab, under **Static website hosting**, enable hosting.
   - Set the **Index document** as `index.html` and the **Error document** as `404.html`.
   - Save the settings to generate the website endpoint.
     
<img width="960" alt="Enabling-static-website" src="https://github.com/user-attachments/assets/c262f4af-6b6d-45e6-909d-bed3f988d100">

4. **Make files publicly accessible:**
   - In the **Permissions** tab, edit the **Bucket Policy** to allow public access with this policy:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "PublicReadGetObject",
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::practiceaws.click/*"
         }
       ]
     }
     ```

---

### **Step 2: Configure a Route 53 Hosted Zone**

1. **Create a Hosted Zone in Route 53:**
   - In **Route 53**, create a new **Hosted Zone** for `practiceaws.click`.
   - Route 53 will provide **Name Servers (NS)** records that need to be configured in your domain registrar (if it’s not AWS).

---

### **Step 3: Request and Configure an SSL Certificate**

1. **Request an SSL Certificate in ACM**:
   - Go to **AWS Certificate Manager (ACM)** and request a new public SSL certificate.
   - Add the domain name `practiceaws.click` and `www.practiceaws.click`.
   - Use **DNS validation** to validate ownership of the domain by adding the CNAME records to Route 53.

2. **Wait for validation**:
   - The validation process might take a few minutes. Once it’s validated, the certificate will be available for use.

<img width="960" alt="ssl" src="https://github.com/user-attachments/assets/4fda2118-33eb-4d87-90c3-083b56ee07f9">

---

### **Step 4: Set up CloudFront Distribution**

1. **Create a CloudFront Distribution**:
   - In **CloudFront**, create a new distribution.
   - Set the **origin** as the S3 bucket URL (e.g., `http://practiceaws.click.s3-website.ap-south-1.amazonaws.com`).
   - In the **Viewer Protocol Policy**, choose **Redirect HTTP to HTTPS** to enforce secure connections.

2. **Connect the SSL Certificate**:
   - Under the **SSL Certificate** settings, choose **Custom SSL Certificate** and select the SSL certificate you generated in ACM.
   - Set the default root object to `index.html`.

<img width="960" alt="CLoud-FrontDistribution" src="https://github.com/user-attachments/assets/99077879-d232-4a8f-80fb-bc3a8bacb935">

---
### **Step 5: Update S3 Bucket Policy for CloudFront Access**

1. **Go to S3 Console**:
   - In the AWS Management Console, open the **S3** service and navigate to your bucket (`practiceaws.click`).

2. **Modify Bucket Policy**:
   - Go to the **Permissions** tab and click **Edit Bucket Policy**.
   - Add the following policy to allow CloudFront to access your bucket:

     ```json
     {
        "Version": "2008-10-17",
        "Id": "PolicyForCloudFrontPrivateContent",
        "Statement": [
          {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
              "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::practiceaws.click/*",
            "Condition": {
              "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::024848440143:distribution/E1L6CNGLC9QESZ"
              }
            }
          }
        ]
      }
     ```

   - This policy ensures that only requests coming from your CloudFront distribution (`E1L6CNGLC9QESZ`) can access the objects in your S3 bucket.

3. **Save the Policy**:
   - After adding the policy, click **Save**.

---

### **Step 6: Connect SSL Certificate to CloudFront**

- Ensure that your **Route 53 Alias Record** points to the CloudFront distribution domain (e.g., `d111111abcdef8.cloudfront.net`).
- CloudFront will now serve your website over HTTPS using the SSL certificate.

---



## **Testing and Accessing Your Website**

1. **Test via the S3 URL**:
   - Before propagation, you can access the website via the S3 URL (`http://practiceaws.click.s3-website.ap-south-1.amazonaws.com`).
  
     <img width="960" alt="Website-using-url" src="https://github.com/user-attachments/assets/4243a587-7892-4e52-876b-4b02b4d76e48">


2. **Test via Domain Name**:
   - Once DNS propagation completes (usually within 24-48 hours), visit your domain (`https://practiceaws.click` or `https://www.practiceaws.click`) to see your website served securely over HTTPS via CloudFront.

<img width="960" alt="webUsingDomain" src="https://github.com/user-attachments/assets/01d3156e-b182-43d6-ad38-94979ee6a3a4">

---

## **Best Practices**

- **DNS TTL Settings**: Set the TTL for DNS records to a reasonable value (e.g., 300 seconds) to balance propagation time and caching efficiency.
- **Cache Optimization in CloudFront**: Adjust cache settings to improve performance and lower costs.
- **Security**: Always use **HTTPS** with SSL/TLS for security and user trust.

---

## **Troubleshooting**

- **DNS Propagation Delays**: It might take up to 48 hours for DNS changes to propagate. Verify your DNS settings in Route 53.
- **SSL Issues**: Ensure your SSL certificate is validated in ACM and correctly attached to your CloudFront distribution.
- **CloudFront Delays**: CloudFront distribution changes might take a few minutes to propagate globally.

---

## **Final Thoughts**

By following this guide, you have successfully hosted a secure static website on **AWS S3**, managed DNS with **Route 53**, secured it with **SSL**, and distributed it globally using **CloudFront**.

