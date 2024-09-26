# **DNS Resolution and Static Website Hosting on AWS**

The following architecture diagram illustrates the flow of DNS resolution and the process of hosting a static website using AWS S3 and Route 53.

<img width="820" alt="ArchitecturePic" src="https://github.com/user-attachments/assets/e4e20e59-a81a-4151-82a9-f49ca5a7d6ce">

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

### **Phases of the DNS Resolution and Static Website Hosting Process**

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

---

### **Additional Notes for the README**:
- **IAM**: For uploading the files to the S3 bucket, an IAM user with appropriate permissions is used to manage the bucket and enable static website hosting.
- **Route 53 Hosted Zone**: Be sure to configure the **TTL (Time to Live)** in Route 53 to control the caching behavior of the DNS records.
- **Security**: If necessary, implement bucket policies or access control lists (ACLs) to secure access to your S3 bucket.

---
