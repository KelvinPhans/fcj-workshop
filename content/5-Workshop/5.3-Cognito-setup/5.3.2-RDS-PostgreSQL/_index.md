---
title: "RDS PostgreSQL Database"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Set Up the Amazon RDS PostgreSQL Database

This section creates the managed PostgreSQL database used by the NestJS backend.

#### Step 1 — Select the database engine

Open **Amazon RDS → Databases → Create database**. Select **PostgreSQL** as the engine type and choose **Full configuration** so that the instance, connectivity, security, backup, and maintenance settings can be configured manually. For this workshop, use the **Production** template as shown in the evidence image.

{{< blog-image src="images/5-Workshop/5.3-Network-RDS/5.3.2-step1-engine.png" alt="Selecting the PostgreSQL engine when creating an Amazon RDS database" caption="Figure 1. Select PostgreSQL, Full configuration, and the Production template when creating Amazon RDS." >}}

#### Step 2 — Configure the instance and credentials

Enter the database settings:

- **DB identifier:** `startups-blogs-db`
- **Master username:** `postgres`
- **Credentials management:** `Managed in AWS Secrets Manager`
- **Encryption key:** `aws/secretsmanager (default)` or a project-approved customer-managed KMS key

Never store the production database password in source code or a committed `.env` file.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step2-instance.png" alt="RDS database identifier and credential settings" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. Database identity and master credentials managed by AWS Secrets Manager.</em></p>
</div>

#### Step 3 — Configure private connectivity

In **Connectivity**, configure:

- **Network type:** `IPv4`
- **VPC:** `Startups-Blogs-vpc`
- **DB subnet group:** the group containing the project private subnets
- **Public access:** `No`
- **VPC security group:** `RDS-Security-Group`

The screenshot illustrates these fields while Default VPC is selected. For the final deployment, replace the default values with the custom VPC, subnet group, and Security Group created in Section 5.3.1.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step3-connectivity.png" alt="Amazon RDS connectivity settings" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 3. RDS private connectivity fields: IPv4, no public access, DB subnet group, and VPC Security Group.</em></p>
</div>

#### Step 4 — Verify the database status

Select **Create database** and wait until the status changes to **Available**. The completed resource is `startups-blogs-db`, running PostgreSQL in `us-east-1a` with the `db.t4g.micro` instance class.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step4-available.png" alt="Startups Blogs RDS PostgreSQL database available" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 4. The startups-blogs-db PostgreSQL instance is available.</em></p>
</div>

#### Step 5 — Record the database endpoint

Open `startups-blogs-db` and select **Connectivity & security**. Record the endpoint and PostgreSQL port `5432`. The screenshot shows:

```text
startups-blogs-db.csp286seu5bu.us-east-1.rds.amazonaws.com:5432
```

Configure the NestJS runtime through a protected secret or environment variable:

```env
DATABASE_URL="postgresql://postgres:<password>@startups-blogs-db.csp286seu5bu.us-east-1.rds.amazonaws.com:5432/postgres?schema=public&sslmode=require"
```

Use this private endpoint only from authorized resources such as the EC2 backend. Do not commit the real password or production connection string to Git.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step5-endpoint.png" alt="RDS PostgreSQL endpoint and SSL connection instructions" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 5. Private PostgreSQL endpoint and SSL connection instructions for the backend.</em></p>
</div>
