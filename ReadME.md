# CD12352 - Infrastructure as Code Project Solution  
# Harsh Raj  

---

## 🌐 Live Demo Links

| Resource | URL |
|-----------|-----|
| **Application Load Balancer (Udagram App)** | [http://udagra-applo-iuoza2341sho-920439548.us-east-2.elb.amazonaws.com/] |
| **S3 Static Website (Static Content)** | [http://udagram-udagram-bucket-313766952812.s3-website.us-east-2.amazonaws.com/] |



---

## 🏗️ Spin up instructions

Follow these steps to deploy the full Udagram infrastructure using AWS CloudFormation.

---

### 1️⃣ Network Stack

Creates the base network (VPC, subnets, Internet/NAT gateways, and route tables):

```powershell
aws cloudformation create-stack `
  --stack-name udagram-network `
  --template-body file://network.yml `
  --parameters file://network-parameters.json `
  --capabilities CAPABILITY_NAMED_IAM `
  --region us-east-2
```

---

### 2️⃣ Application Stack

Deploys EC2 instances (Auto Scaling Group + Application Load Balancer):

```powershell
aws cloudformation create-stack `
  --stack-name udagram-app `
  --template-body file://app.yml `
  --parameters file://app-parameters.json `
  --capabilities CAPABILITY_NAMED_IAM `
  --region us-east-2
```

---

### 3️⃣ S3 Stack

Creates a public S3 bucket to host static content:

```powershell
aws cloudformation create-stack `
  --stack-name udagram-s3 `
  --template-body file://s3.yml `
  --parameters file://s3-parameters.json `
  --capabilities CAPABILITY_NAMED_IAM `
  --region us-east-2
```

---

### ✅ After Deployment

Once all stacks are **CREATE_COMPLETE**, go to **CloudFormation → Outputs tab** for each stack and note:
- Network: VPC ID, Public & Private Subnets
- App: LoadBalancer URL (open in browser)
- S3: BucketWebsiteURL (open in browser)

---

## 🧹 Tear down instructions

To delete all stacks and avoid ongoing charges:

### 1️⃣ Empty the S3 bucket first

```powershell
aws s3 rm s3://<your-bucket-name> --recursive --region us-east-2
```

### 2️⃣ Delete stacks in reverse order

```powershell
aws cloudformation delete-stack `
  --stack-name udagram-s3 `
  --region us-east-2

aws cloudformation delete-stack `
  --stack-name udagram-app2 `
  --region us-east-2

aws cloudformation delete-stack `
  --stack-name udagram-network2 `
  --region us-east-2
```

### 3️⃣ Verify deletion

```powershell
aws cloudformation describe-stacks --region us-east-2
```

All stacks should show **DELETE_COMPLETE**.

---

## 🧾 Verification checklist

✅ **Network Stack Outputs**
- VPC ID  
- Public and Private subnets  

✅ **App Stack Outputs**
- LoadBalancer URL — opens “Udagram is Live!” page  

✅ **S3 Stack Outputs**
- Bucket Name and Bucket Website URL  

✅ **Browser Tests**
- ALB URL shows Udagram NGINX page  
- S3 URL shows static site  

✅ **All stacks:** `CREATE_COMPLETE`

---

## 📸 Evidence of Deployment

Include these screenshots in your `/screenshots` folder:

1. `1_all_stacks_overview.png` — all stacks in CREATE_COMPLETE  
2. `2_network_outputs.png` — network stack outputs  
3. `3_app_outputs.png` — app stack outputs  
4. `4_app_browser.png` — ALB URL in browser  
5. `5_s3_outputs.png` — S3 stack outputs  
6. `6_s3_website.png` — S3 static website in browser  

---

## ⚙️ Other details

- **Region:** us-east-2 (Ohio)  
- **Environment Name:** udagram  
- **Instance Type:** t3.micro  
- **AMI:** Amazon Linux 2  
- **Security Groups:** Least privilege, HTTP (80) only  
- **Deployment Method:** AWS CLI via PowerShell  
- **Verification:** All templates validated with `aws cloudformation validate-template`

---

**Author:** Harsh Raj  
**Project:** Udagram - Infrastructure as Code  
**Course:** Cloud DevOps Engineer Nanodegree
