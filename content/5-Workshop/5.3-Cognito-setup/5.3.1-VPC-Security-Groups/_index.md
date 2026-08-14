---
title: "VPC & Security Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Set Up Amazon VPC & Security Groups

This section creates the isolated network for the **Startups Blogs** infrastructure in **`us-east-1` (N. Virginia)**.

#### Step 1 — Create the VPC

Open **AWS Console → VPC → Your VPCs → Create VPC** and enter:

- **Name tag:** `Startups-Blogs-vpc`
- **IPv4 CIDR block:** `10.0.0.0/16`
- **Tenancy:** `Default`

Enable DNS resolution and DNS hostnames, then select **Create VPC**.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step1-create-vpc.png" alt="Startups Blogs VPC details" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. The Startups-Blogs-vpc is available with the 10.0.0.0/16 IPv4 CIDR block.</em></p>
</div>

#### Step 2 — Create the subnets

Open **VPC → Subnets → Create subnet**, select `Startups-Blogs-vpc`, and create:

- Public subnet in `us-east-1a`: `10.0.0.0/20`, used by internet-facing resources such as EC2.
- Private subnet: `10.0.128.0/20`, used by database resources.
- Additional private subnet: `10.0.200.0/24`, providing the second subnet required by the RDS DB subnet group.

Do not assign a public IP address to database resources.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step2-subnets.png" alt="Public and private subnets for Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. Public and private subnets created in the Startups Blogs VPC.</em></p>
</div>

#### Step 3 — Attach the Internet Gateway

Open **VPC → Internet gateways**, create `Startups-Blogs-igw`, and attach it to `Startups-Blogs-vpc`. Add a `0.0.0.0/0` route to the Internet Gateway in the public route table and associate that route table with the public subnet.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step3-igw.png" alt="Internet Gateway attached to Startups Blogs VPC" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 3. Startups-Blogs-igw attached to the project VPC.</em></p>
</div>

#### Step 4 — Configure the EC2 Security Group

Create `EC2-Security-Group` in `Startups-Blogs-vpc` and add these inbound rules:

- TCP `22` for SSH administration. Restrict the source to the administrator's trusted IP address.
- TCP `3000` for the NestJS application. In production, restrict the source to API Gateway or another approved upstream component.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step4-ec2-sg.png" alt="EC2 security group inbound rules" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 4. Inbound SSH and NestJS application rules in EC2-Security-Group.</em></p>
</div>

#### Step 5 — Configure the RDS Security Group

Create `RDS-Security-Group` in the same VPC. Add a PostgreSQL inbound rule on TCP port `5432` whose source is `EC2-Security-Group`. This allows the backend to reach PostgreSQL without exposing the database to the internet. Remove broad CIDR-based PostgreSQL rules that are not required.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step5-rds-sg.png" alt="RDS PostgreSQL security group inbound rules" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 5. PostgreSQL port 5432 rules in RDS-Security-Group.</em></p>
</div>
