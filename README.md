# AWS Static Website CI/CD

A static website hosted on **Amazon S3** and delivered globally through **Amazon CloudFront**, with automated deployments using **GitHub Actions** and **AWS IAM OIDC**.

Every push to the `main` branch automatically deploys the latest website version to S3 and invalidates the CloudFront cache.

---

## 🚀 Project Overview

This project demonstrates a simple, secure, and automated CI/CD pipeline for deploying a static website to AWS.

Instead of manually uploading website files to S3, GitHub Actions handles the deployment whenever changes are pushed to the repository.

### Deployment Flow

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    │ OIDC Authentication
    ▼
AWS IAM Role
    │
    ├──────────────► Amazon S3
    │                 │
    │                 │ Website files
    │                 ▼
    │              S3 Bucket
    │
    └──────────────► Amazon CloudFront
                      │
                      │ Cache Invalidation
                      ▼
                   Live Website
```

---

## 🛠️ Technologies Used

| Technology            | Purpose                               |
| --------------------- | ------------------------------------- |
| **Amazon S3**         | Stores static website files           |
| **Amazon CloudFront** | CDN for global content delivery       |
| **AWS IAM**           | Controls AWS permissions              |
| **GitHub Actions**    | Automates CI/CD deployment            |
| **GitHub OIDC**       | Enables keyless authentication to AWS |
| **Git**               | Version control                       |
| **HTML5**             | Website structure                     |
| **CSS3**              | Website styling                       |

---

## 🔐 Security

A key part of this project is the authentication mechanism between GitHub Actions and AWS.

### GitHub OIDC

The workflow uses **OpenID Connect (OIDC)** to authenticate with AWS.

No long-term AWS access keys or secret access keys are stored in GitHub.

```text
GitHub Actions
      │
      │ OIDC Token
      ▼
AWS IAM
      │
      │ AssumeRoleWithWebIdentity
      ▼
IAM Role
      │
      ▼
S3 + CloudFront
```

The IAM role uses a trust policy that restricts access to this repository and the `main` branch.

### Least-Privilege Permissions

The deployment role is granted only the permissions required for the deployment:

* Upload objects to S3
* Read objects from S3
* List the S3 bucket
* Delete objects during synchronization
* Create CloudFront invalidations

This avoids using broad administrative permissions for the deployment workflow.

---

## ⚙️ CI/CD Pipeline

The GitHub Actions workflow is triggered whenever code is pushed to the `main` branch.

### Pipeline Steps

1. **Checkout Repository**

   * Retrieves the latest source code.

2. **Configure AWS Credentials**

   * Uses GitHub OIDC to assume the AWS IAM deployment role.

3. **Sync Website to S3**

   * Uploads the latest website files.
   * Removes files that no longer exist in the repository.

4. **Invalidate CloudFront Cache**

   * Creates a CloudFront invalidation so visitors receive the latest version of the website.

### Workflow Trigger

```yaml
on:
  push:
    branches:
      - main
```

---

## 📁 Project Structure

```text
aws-static-site-cicd/
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── index.html
│
└── README.md
```

---

## 🌐 Live Website

The website is deployed through Amazon CloudFront.

**Live URL:**
https://d27r5ywouue2ki.cloudfront.net

---

## 💻 Local Development & Deployment

### 1. Clone the repository

```bash
git clone https://github.com/itzrax/aws-static-site-cicd.git
cd aws-static-site-cicd
```

### 2. Make changes to the website

Edit `index.html` using your preferred editor.

For example:

```bash
code index.html
```

### 3. Commit and trigger automated deployment

```bash
git add .
git commit -m "feat: update landing page content"
git push origin main
```

### 4. Verify the deployment

After pushing the changes:

1. Open the repository's **Actions** tab.
2. Check that the deployment workflow completes successfully.
3. Open the CloudFront URL.
4. Refresh the page and verify the updated website.

---

## 📊 AWS Architecture

```text
                 ┌─────────────────────┐
                 │   GitHub Repository │
                 └──────────┬──────────┘
                            │
                         git push
                            │
                            ▼
                 ┌─────────────────────┐
                 │   GitHub Actions    │
                 └──────────┬──────────┘
                            │
                       OIDC Token
                            │
                            ▼
                 ┌─────────────────────┐
                 │      AWS IAM        │
                 │    Deployment Role  │
                 └───────┬───────┬─────┘
                         │       │
                  S3 Sync│       │CloudFront
                         ▼       ▼
                  ┌─────────┐  ┌────────────┐
                  │   S3    │  │ CloudFront │
                  │ Bucket  │  │    CDN     │
                  └─────────┘  └─────┬──────┘
                                      │
                                      ▼
                                🌐 Live Website
```

---

## 🎯 Project Objectives

This project was built to demonstrate practical understanding of:

* AWS S3 static website hosting
* Amazon CloudFront
* AWS IAM roles and policies
* GitHub Actions
* CI/CD automation
* GitHub OIDC authentication
* Least-privilege IAM permissions
* Automated cache invalidation
* Git and GitHub workflows

---

## 🔄 Deployment Example

A typical deployment looks like this:

```text
Edit index.html
      ↓
git add .
      ↓
git commit
      ↓
git push origin main
      ↓
GitHub Actions starts
      ↓
AWS authentication via OIDC
      ↓
Files synced to S3
      ↓
CloudFront cache invalidated
      ↓
Updated website goes live
```

---

## 📌 Key Takeaway

This project demonstrates how a static website can be deployed to AWS using a fully automated CI/CD workflow while avoiding long-lived AWS credentials in GitHub.

It combines **cloud infrastructure, IAM security, CI/CD automation, and CDN delivery** into a single practical AWS project.

---

## 👤 Author

**Rahsheetha**

BE CSE — Cybersecurity
Cloud & AWS Learning Project

---

⭐ If you found this project useful, feel free to explore the repository and workflow configuration.

