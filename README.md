
# 🚀 Technopark Event

An **event-driven web application** built using **AWS Serverless Architecture** to manage and display events efficiently.

---

## 📑 Table of Contents

* [Overview](#-overview)
* [Architecture](#-architecture)
* [Tech Stack](#-tech-stack)
* [Key Learnings](#-key-learnings)

  * [Permissions & Security](#-permissions--security)
  * [API Gateway](#-api-gateway)
  * [Architecture Decisions](#-architecture-decisions)
* [Best Practices Followed](#-best-practices-followed)
* [Future Improvements](#-future-improvements)

---

## 📖 Overview

Technopark Event is a **serverless, event-driven application** where the client communicates with AWS services through a secure and scalable architecture.

The application uses **AWS Lambda** for business logic, **DynamoDB** for data storage, and **Amazon S3** for static website hosting and image handling.

---

## 🏗️ Architecture Diagram

![Technopark Event Architecture](docs/architecture.png)

### Flow Explanation:

* 🌐 Client sends requests to **API Gateway**
* ⚙️ API Gateway triggers **Lambda**
* 🗄️ Lambda interacts with **DynamoDB** and **S3**
* 🖼️ Images are uploaded/downloaded directly from S3 using **Pre-Signed URLs**

---

## 🛠️ Tech Stack

* **Frontend**: Static Website (Hosted on Amazon S3)
* **Backend**: AWS Lambda (Node.js)
* **API**: Amazon API Gateway
* **Database**: Amazon DynamoDB
* **Storage**: Amazon S3
* **Authentication**: IAM Roles & Policies

---

## 📚 Key Learnings

### 🔐 Permissions & Security

1. **Lambda Execution Role**

   * Lambda must have an **Execution Role** with access to:

     * 📦 Amazon S3
     * 🗄️ Amazon DynamoDB
   * Without proper permissions, Lambda cannot read or write data.

2. **S3 Static Website Hosting**

   * Disabled **Block Public Access** (bucket level)
   * Added a **Bucket Policy** to allow public `GET` access

3. **CORS Configuration for Pre-Signed URLs**

   * Pre-signed S3 image URLs were blocked by browser CORS policy
   * Fixed by adding **CORS headers** in Lambda responses:

```js
const headers = {
  "Access-Control-Allow-Origin": "http://technopark-events-web.s3-website-us-east-1.amazonaws.com",
  "Access-Control-Allow-Headers": "Content-Type",
  "Access-Control-Allow-Methods": "OPTIONS,GET"
};
```
4. **Why POST requests are more strict:**
     * GET requests are "simple requests" - no preflight needed
     * POST with JSON is a "non-simple request" - browser sends OPTIONS preflight first
     * If API doesn't handle OPTIONS, the POST request fails with CORS error

   * **Understanding CORS in this architecture:**
     * **API Gateway CORS** = permission to SEND the API call to Lambda
     * **Lambda CORS headers** = permission to READ the data in the browser returned by Lambda
     * **S3 CORS** = permission to enter the storage room
     * Both API Gateway CORS and Lambda CORS headers are needed for the request-response cycle
     * S3 CORS is additionally needed if your front-end uploads to S3
---

### 🌐 API Gateway

* API Gateway generates the **invoke URL only after deployment**
* The deployed URL is used by the frontend to communicate with Lambda

**Throttling Configuration:**

* Throttling can be set at the **stage level** to control API request rates
* **Rate Limit** = Expected number of requests per second
* **Burst Limit** = Maximum allowed requests per second/per IP
* When limits are exceeded, AWS API Gateway returns a **503 error** (Service Unavailable)

---

### 🧩 Architecture Decisions

1. ❌ Images are **not processed inside Lambda**

   * Lambda has payload and execution time limits

2. ✅ Image handling is done directly by the client using **S3 Pre-Signed URLs**

   * Faster uploads/downloads
   * Lower Lambda cost
   * Improved scalability

---

## ✅ Best Practices Followed

* 🔑 Least-privilege IAM roles
* ⚡ Serverless & event-driven design
* 📦 Direct S3 access via pre-signed URLs
* 🌍 Proper CORS configuration
* 💰 Cost-efficient architecture

---

## 🚧 Future Improvements

* 🔐 Add authentication using Amazon Cognito
* 📊 Add monitoring with CloudWatch dashboards
* 🧪 Add unit tests for Lambda functions
* 🖼️ Add CloudFront for faster content delivery

---

⭐ **If you find this project useful, feel free to star the repository!**

---

       


    

