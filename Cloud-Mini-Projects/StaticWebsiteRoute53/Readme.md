# Hosting a Static Website on S3 with Route 53 Domain Configuration

## Prerequisites
- An AWS account.
- A registered domain name in Route 53 or an external registrar pointing to Route 53.
- A static website prepared with HTML, CSS, and media files.

## Step 1: Set up an S3 Bucket for Hosting a Static Website

1. **Create an S3 bucket:**
   - Go to the AWS Management Console and navigate to the **S3** service.
   - Click **Create Bucket**, and name the bucket exactly as your domain name (e.g., `www.practiceaws.click`).
   - Select your preferred AWS region and leave the rest of the settings as default.
   - Ensure **Block all public access** is unchecked, and acknowledge the warning to allow public access.

2. **Upload your website files:**
   - Once the bucket is created, click on it and go to the **Objects** tab.
   - Click **Upload** and upload all the necessary files for your website (e.g., HTML, CSS, images).
   - Make sure that your **index document** (e.g., `Home.html`) and error document (e.g., `404.html`) are correctly named and uploaded.
     
<img width="960" alt="uploading_objects_on_S3" src="https://github.com/user-attachments/assets/5f3f1400-813e-4f7f-90be-d314c87836ef">

3. **Enable static website hosting:**
   - Inside your S3 bucket, go to the **Properties** tab.
   - Scroll down to **Static website hosting** and click **Edit**.
   - Choose **Enable** and configure the **Index document** (e.g., `Home.html`) and **Error document** (e.g., `404.html`).
   - Save the settings, and the bucket will generate a website endpoint URL.

<img width="960" alt="Enabling-static-website" src="https://github.com/user-attachments/assets/33f020b7-97be-44b2-ac38-12ce2dc90e90">


4. **Make the files publicly accessible:**
   - Go to the **Permissions** tab of your bucket.
   - Click **Edit bucket policy** and add the following policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::www.practiceaws.click/*"
       }
     ]
   }
   ```
<img width="960" alt="Edit-Policy" src="https://github.com/user-attachments/assets/63f45cc8-9288-4e6a-b9bf-261df385fc2a">

<img width="960" alt="Policy" src="https://github.com/user-attachments/assets/efcbc0da-763b-4edf-9d1e-5ca4dfaed319">

   - This allows public access to all objects in the bucket.

## Step 2: Configure a Route 53 Hosted Zone

1. **Create a Hosted Zone in Route 53:**
   - In the AWS Management Console, navigate to **Route 53**.
   - Click **Hosted Zones** and create a new zone.
   - Provide your domain name (e.g., `practiceaws.click`) and create the hosted zone.
     
 ### OR

1.  **Buy a Domain Name:**

   - Go to the Route 53 service in the AWS Management Console.
   - Under Domains, click Register Domain.
   - Search for your desired domain name (e.g., practiceaws.click) and select it if it's available.
   - Follow the registration process and purchase the domain for learning purposes. Once purchased, a public hosted zone will automatically be created for the domain.
   - Alternatively, if you already own a domain, you can transfer it to Route 53 by following the domain transfer instructions provided in the Route 53 console.
     
2. **Add a CNAME Record for S3 Website:**
   - Inside your hosted zone, click on **Create Record**.
   - Choose **CNAME** as the record type.
   - In the **Record Name**, type `www` (for subdomain `www.practiceaws.click`).
   - In the **Value** field, enter the S3 website URL provided in the static website settings (e.g., `http://www.practiceaws.click.s3-website.ap-south-1.amazonaws.com`).
   - Set the TTL (Time to Live) to 60 seconds or leave it as the default.

     <img width="960" alt="create-record-CNAME" src="https://github.com/user-attachments/assets/12d83bae-f428-4d4c-af1f-dc463a958d8e">

## Step 3: Test and Access the Website

1. **Testing via S3 URL:**
   - Once the static website is set up in S3, you can access it via the S3-generated URL (e.g., `http://www.practiceaws.click.s3-website.ap-south-1.amazonaws.com`).

<img width="960" alt="Accessing-via-this-url" src="https://github.com/user-attachments/assets/b2b3064d-1f4c-469b-9a6d-8e639ef43a2b">

<img width="960" alt="Website-using-url" src="https://github.com/user-attachments/assets/2aa7eb2a-fd0e-4711-a54b-ee3772e96dcc">


2. **Testing with Domain Name:**
   - Once the Route 53 DNS has propagated (this can take a few minutes to several hours), try accessing your site using the domain name `www.practiceaws.click`.

   <img width="960" alt="website-using-domain" src="https://github.com/user-attachments/assets/2e8361f2-0947-4942-a6e4-4a66d4469ea6">

## Troubleshooting

- Ensure that the bucket name matches exactly with the domain name or subdomain.
- DNS propagation might take some time; you can use tools like [DNS Checker](https://dnschecker.org/) to verify if the DNS records have propagated.
- Make sure all files are public, and the bucket policy is correctly set to allow public access to the S3 objects.

---

### Conclusion

You have successfully set up a static website hosted on Amazon S3 and connected it to your custom domain using Route 53. Your website can now be accessed using both the S3 URL and the custom domain.
