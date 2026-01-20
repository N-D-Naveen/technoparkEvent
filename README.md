
# 🚀 Technopark Event

An **event-driven web application** built using **AWS Serverless Architecture** to manage and display events efficiently.

---

## 📑 Table of Contents

* [Overview](#overview)
* [Architecture](#architecture)
* [Tech Stack](#tech-stack)
* [Key Learnings](#key-learnings)

  * [Permissions & Security](#permissions--security)
  * [API Gateway](#api-gateway)
  * [Architecture Decisions](#architecture-decisions)
* [Best Practices Followed](#best-practices-followed)
* [Future Improvements](#future-improvements)

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

---

### 🌐 API Gateway

* API Gateway generates the **invoke URL only after deployment**
* The deployed URL is used by the frontend to communicate with Lambda

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

       


    

