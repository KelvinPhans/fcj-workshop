---
title: "Terraform Automation & Security"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### Infrastructure Automation via Terraform (IaC), CloudWatch Monitoring & Security Review

In this section, we examine how the entire AWS Cloud Infrastructure of Startups Blogs is automated using **Terraform (Infrastructure as Code)** and monitored via **Amazon CloudWatch** in region **`us-east-1`**.

#### 1. Infrastructure Automation via Terraform (`terraform/`)

100% of AWS cloud resources are declared in the `terraform/` directory:
- **VPC Infrastructure (`vpc.tf`)**: Creates a Virtual Private Cloud with 2 Public Subnets and 2 Private Subnets spanning Availability Zones `us-east-1a` and `us-east-1b`.
- **RDS Database (`rds.tf`)**: Provisions Amazon RDS PostgreSQL in Private Subnets. Security Groups restrict database port 5432 access strictly to the EC2 instance.
- **EC2 Compute (`ec2.tf`)**: Provisions an Ubuntu EC2 instance running NestJS backend via PM2 24/7.
- **Cognito Auth (`cognito.tf`)**: Declares User Pool and Confidential App Client with Secret Key.
- **API Gateway (`apigateway.tf`)**: Routes public HTTPS API requests to the backend EC2 server.
- **S3 & CloudFront (`s3_cloudfront.tf`)**: Configures S3 static frontend hosting and CloudFront global CDN distribution.
- **CloudWatch & Alerts (`monitoring.tf`)**: Configures Log Groups, CloudWatch Alarms, and SNS Email notifications.

Automated deployment commands:

```bash
cd terraform
terraform init
terraform plan
terraform apply -auto-approve
```

---

#### 2. System Monitoring via Amazon CloudWatch (`monitoring.tf`)

CloudWatch provides continuous monitoring across the AWS infrastructure:
- **Log Groups**: Centralizes real-time logs from API Gateway and NestJS backend on EC2.
- **CloudWatch Dashboard**: Tracks CPU Utilization, Memory, and Network I/O metrics for EC2 and RDS instances.
- **SNS Alerts**: Automatically triggers email notifications (`alert_email`) if EC2 CPU usage exceeds 80% or RDS health degrades.

---

#### 3. Enterprise Security Best Practices Review

- **Network Security**: RDS PostgreSQL is strictly isolated within Private Subnets, preventing direct Internet access.
- **Server-Side Secret Isolation**: `COGNITO_CLIENT_SECRET` remains strictly on the NestJS backend, utilizing HMAC-SHA256 `SECRET_HASH` for all Cognito SDK commands.
- **RSA JWT Signature Verification (`us-east-1`)**: Uses `jwks-rsa` / `aws-jwt-verify` to cryptographically validate RSA signatures against Cognito JWKS endpoints.
- **Dual-Layer Authorization**: Enforces JWT verification and `ownerId` resource ownership validation for all business data modifications.
- **Zero Credentials Exposure**: AWS credentials, Client Secrets, and JWT tokens are excluded from public source repositories.

> Screenshot required:
> Terraform execution output showing successful creation of AWS infrastructure resources in `us-east-1`.
> Hide AWS account identifiers and sensitive values before capturing.
