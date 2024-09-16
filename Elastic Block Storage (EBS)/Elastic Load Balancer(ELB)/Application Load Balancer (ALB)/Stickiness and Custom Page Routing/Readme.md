# Stickiness and Custom Page Routing in AWS Application Load Balancer (ALB)

This document explains two critical features of AWS Application Load Balancer (ALB) — **Session Stickiness** and **Custom Page Routing**. You will find information on how these features work, their use cases, and how to configure them for better performance and user experience. It also includes an example of handling error pages with a **Fixed Response**.

---

## Table of Contents
1. [What is Session Stickiness in ALB?](#what-is-session-stickiness-in-alb)
2. [How Session Stickiness Works](#how-session-stickiness-works)
3. [Benefits of Using Stickiness](#benefits-of-using-stickiness)
4. [Use Cases for Stickiness](#use-cases-for-stickiness)
5. [How to Enable Stickiness in ALB](#how-to-enable-stickiness-in-alb)
6. [What is Custom Page Routing in ALB?](#what-is-custom-page-routing-in-alb)
7. [How Custom Page Routing Works](#how-custom-page-routing-works)
8. [Benefits of Custom Page Routing](#benefits-of-custom-page-routing)
9. [How to Configure Custom Page Routing](#how-to-configure-custom-page-routing)
10. [Handling Error Pages with Fixed Response](#handling-error-pages-with-fixed-response)
11. [Q&A Section](#qna-section)

---

## What is Session Stickiness in ALB?

**Session Stickiness**, also called **Session Affinity**, is a feature in AWS Application Load Balancer that allows the ALB to bind a user’s session to a specific target within a target group. This ensures that all requests from a particular user during their session are consistently routed to the same instance.

### Key Points:
- Stickiness is achieved using cookies (either ALB-generated or application-generated).
- The ALB ensures that each user's subsequent requests go to the same target server for the duration of the session.
- It’s most useful in stateful applications where user sessions need consistency.

---

## How Session Stickiness Works

1. **Cookie-based Routing**:
   - When a user first accesses the application, the ALB generates or reads a cookie that identifies the user’s session.
   - The ALB then routes all future requests for that user session to the same target instance.
   - The **AWSALB** cookie is automatically updated with every interaction to extend the session.

2. **Cookie Duration**:
   - You can configure how long the session should be sticky by setting the **stickiness duration** (1 second to 7 days).
   - The session continues as long as the user keeps interacting with the application.

---

## Benefits of Using Stickiness

- **Ensures Session Consistency**: Keeps users connected to the same instance throughout their session, which is essential for stateful applications like e-commerce checkouts.
- **Reduces Backend Complexity**: By routing users to the same instance, the need for session replication across multiple instances is minimized.
- **Improved User Experience**: Users experience faster and smoother interactions since they are consistently routed to the same server.
- **Session Management Control**: The cookie-based stickiness allows control over session duration and handling.

---

## Use Cases for Stickiness

1. **E-commerce**: Users should be routed to the same server throughout their shopping and checkout process to ensure no session data (like cart contents) is lost.
2. **Interactive Dashboards**: Applications with long-running user sessions, like dashboards, where consistency is important.
3. **Applications with State Management**: Any stateful application where backend instances need to maintain session-specific information.

---

## How to Enable Stickiness in ALB

### Step-by-Step Guide:

1. **Navigate to Target Group**:
   - Go to the **EC2 Dashboard** > **Target Groups**.
   
2. **Select the Target Group**:
   - Choose the target group where you want to enable stickiness.

3. **Edit Stickiness Settings**:
   - Click **Actions** > **Edit Stickiness**.
   
4. **Enable Stickiness**:
   - Check the box for **Turn on Stickiness**.
   - Choose **Stickiness type** (either Load Balancer generated cookie or Application-based cookie).
   
5. **Set the Stickiness Duration**:
   - Specify the **Stickiness Duration** (e.g., 1 day, 2 hours).

6. **Save the Changes**:
   - Click **Save**.

Now, stickiness is enabled, and users will be bound to the same instance during their session.

---

## What is Custom Page Routing in ALB?

**Custom Page Routing** is the feature that allows AWS Application Load Balancer to route user requests based on specific rules. The routing is determined by factors like URL path, host headers, or query strings.

### Key Points:
- It operates at Layer 7 (Application Layer) of the OSI model.
- Custom routing rules can be defined based on **hostnames**, **URL paths**, and **HTTP headers**.
- It helps in forwarding traffic to different backend servers depending on the request content.

---

## How Custom Page Routing Works

1. **Rule-based Routing**:
   - ALB checks incoming requests against listener rules to determine which target group to route the request to.
   - For example, requests with `/api/*` can be routed to an API target group, while requests to `/web/*` can be routed to a frontend group.

2. **Listener Rules**:
   - Listener rules are defined to examine incoming traffic and take actions (forward, authenticate, or redirect).
   - These rules can use path-based, host-based, and header-based conditions.

3. **Target Groups**:
   - Based on the routing rules, the ALB forwards traffic to appropriate target groups that host the backend services.

---

## Benefits of Custom Page Routing

- **Supports Microservices**: Easily route traffic to different microservices within your architecture, such as API, Web, and Authentication services.
- **Flexible Content-based Routing**: Customize traffic routing based on application-specific requirements, allowing you to efficiently utilize your backend servers.
- **Efficient Traffic Distribution**: Helps in distributing traffic more efficiently across backend instances and services based on their specific responsibilities.

---

## How to Configure Custom Page Routing

### Step-by-Step Guide:

1. **Navigate to Listeners**:
   - Go to **EC2 Dashboard** > **Load Balancers** > **Listeners**.

2. **Add Rule**:
   - In the listener configuration, click on **Add Rule**.

3. **Define Conditions**:
   - Set up conditions like **Path-based Routing** (e.g., `/api/*` for API traffic) or **Host-based Routing** (e.g., `api.example.com` for API traffic).

4. **Set Actions**:
   - Define the action, such as **Forward** to a specific target group.

5. **Save Rule**:
   - Save the rule and ensure it is correctly prioritized.

---

## Handling Error Pages with Fixed Response

You can configure ALB to return custom error pages using the **Fixed Response** action. For example, you can return a custom HTML response whenever users access a certain path, such as `/error`.

### Example:

You want users who access the `/error` path to see a custom HTML error message:

1. **Navigate to Listeners** in the ALB configuration.
2. **Add Rule**: Define a rule that matches the `/error` path.
3. **Choose Action**: In the **Action** section, select **Return Fixed Response**.
4. **Response Code**: Set the **Response Code** to `503` (Service Temporarily Unavailable).
5. **Content Type**: Choose `text/html`.
6. **Response Body**: Add your custom HTML, for example:

```html
<h1 align="center">An error page</h1>
<p align="center">Sorry, something went wrong!</p>
```

7. **Save the Rule**.

Now, whenever a user accesses the `/error` path, they will receive this custom response with a 503 status code.

---

## Q&A Section

### Q: What happens if a sticky session target goes down?
   - **Answer**: If the instance (target) handling a sticky session goes down, the load balancer will route the session to a healthy target. However, some session data may be lost unless the application uses session replication across instances.

### Q: Can I use stickiness with any routing algorithm?
   - **Answer**: Stickiness is not compatible with weighted random routing algorithms, but it works with round-robin and other simpler routing mechanisms.

### Q: How do custom page routing and stickiness interact?
   - **Answer**: Custom page routing decides which target group handles the request, while stickiness ensures that subsequent requests for the same session are routed to the same instance within that target group.

### Q: How do I troubleshoot if stickiness isn’t working properly?
   - **Answer**: Check if the **AWSALB** cookie is being set and updated on each interaction. Ensure that the stickiness duration is configured properly and that the target group health checks are passing.

---

## Conclusion

AWS Application Load Balancer (ALB) provides powerful features such as **Session Stickiness** and **Custom Page Routing** that enhance the user experience by maintaining consistent sessions and efficiently routing traffic to the correct backend instances. Whether you're running a stateful application, microservices, or an e-commerce website, these features can help optimize both performance and operational efficiency. Following the best practices and proper configurations can ensure high availability and smooth user interactions.
