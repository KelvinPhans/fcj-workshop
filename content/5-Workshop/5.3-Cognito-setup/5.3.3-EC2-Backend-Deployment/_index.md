---
title: "EC2 & Backend Deployment"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Deploy the NestJS Backend on Amazon EC2

This section launches an Ubuntu EC2 instance, prepares the Node.js runtime, configures the backend environment, and runs the **Startups Blogs** API continuously with PM2.

#### Step 1 — Launch the EC2 instance

Open **Amazon EC2 → Instances → Launch instance** and configure:

- **Name:** `startups-blogs-backend`
- **AMI:** Ubuntu Server LTS, 64-bit (`x86`)
- **Instance type:** a small instance suitable for the workshop, such as `t3.micro`
- **Storage:** at least 8 GiB EBS

Review the summary and select **Launch instance** after completing the key pair and network settings in the next step.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step1-launch.png" alt="Launch the Startups Blogs backend EC2 instance" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. Ubuntu AMI and instance type selected for the Startups Blogs backend.</em></p>
</div>

#### Step 2 — Configure the SSH key and network

Create or select the RSA key pair `startups-key` and securely store the downloaded private key. During instance launch, select:

- `Startups-Blogs-vpc`
- The public subnet created in Section 5.3.1
- Public IPv4 assignment when direct SSH administration is required
- `EC2-Security-Group`

Restrict SSH port `22` to the administrator's trusted IP. Do not commit the private key to Git or share it publicly.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step2-network.png" alt="EC2 key pair for backend administration" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. The startups-key RSA key pair used for authorized SSH access.</em></p>
</div>

#### Step 3 — Verify the EC2 instance

Wait until the instance state is **Running** and both EC2 status checks report **2/2 checks passed**. Record the public DNS name or public IPv4 address for the SSH connection.

```bash
chmod 400 startups-key.pem
ssh -i startups-key.pem ubuntu@<EC2_PUBLIC_DNS>
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step3-running.png" alt="Startups Blogs EC2 instance running" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 3. The backend EC2 instance is running and has passed both status checks.</em></p>
</div>

#### Step 4 — Install and verify Node.js and PM2

Connect to EC2 and install the backend runtime. The deployed environment shown in the evidence uses Node.js `v20.20.2`, npm `10.8.2`, and PM2 `7.0.3`.

```bash
sudo apt update
sudo apt install -y git curl
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2

node -v
npm -v
pm2 -v
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step4-nodejs.png" alt="Node.js npm and PM2 versions on EC2" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 4. Node.js, npm, and PM2 installed on the EC2 instance.</em></p>
</div>

#### Step 5 — Configure backend environment variables

Clone or transfer the backend source, install dependencies, generate Prisma Client, and create the runtime `.env` file outside version control.

```bash
git clone <BACKEND_REPOSITORY_URL>
cd Startups_Blogs/backend
npm ci
npx prisma generate
```

Configure the RDS connection and AWS service settings. Prefer an EC2 IAM role and AWS Secrets Manager instead of long-lived access keys:

```env
DATABASE_URL="postgresql://postgres:<password>@<RDS_ENDPOINT>:5432/postgres?schema=public&sslmode=require"
AWS_S3_BUCKET=startups-blogs-media
AWS_S3_ENDPOINT=https://s3.us-east-1.amazonaws.com
AWS_S3_REGION=us-east-1
```

The evidence image has been redacted because credentials must never appear in workshop documentation, Git, logs, or screenshots.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step5-env-redacted.png" alt="Redacted AWS S3 environment configuration" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 5. Backend S3 environment configuration with sensitive credentials removed.</em></p>
</div>

#### Step 6 — Build and run the backend with PM2

Build the NestJS application and start it under PM2:

```bash
npm run build
pm2 start dist/main.js --name startups-backend
pm2 save
pm2 startup
```

Run the command printed by `pm2 startup`, then save the process list again. Verify both the process and local HTTP endpoint:

```bash
pm2 status
curl -I http://localhost:3000
```

The expected result is an `online` PM2 process and an HTTP response from the NestJS/Express server. A `200 OK` response verifies that the application is listening locally on port `3000`; external access remains controlled by the EC2 Security Group and API Gateway.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step6-pm2.png" alt="NestJS backend online in PM2 and responding on port 3000" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 6. The startups-backend process is online in PM2 and returns HTTP 200 on port 3000.</em></p>
</div>
