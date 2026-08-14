---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Environment Setup, Terraform & Database Preparation

Before proceeding with Amazon Cognito configuration and full-stack cloud deployment, ensure the following local development environment is prepared.

#### 1. Software Requirements
- **Node.js**: Version 18+ or 20+ LTS.
- **npm** / **yarn** / **pnpm**: Package manager.
- **Terraform**: Version v1.5+ for automated cloud infrastructure provisioning.
- **AWS CLI**: Installed and configured with region **`us-east-1`** (N. Virginia).
- **PostgreSQL Database**: Running locally via Docker (`docker-compose.yml` on port `5433`) or Amazon RDS.
- **Docker**: For PostgreSQL and MinIO S3 local container orchestration.

#### 2. Database Schema Initialization via Prisma ORM
The Startups Blogs domain data model is declared inside `backend/prisma/schema.prisma`.

Execute the following commands to generate Prisma Client and apply migrations:

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.2-Prerequiste/prisma-push.png" alt="Successful Prisma database synchronization" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. Successfully synchronizing the PostgreSQL database with the Prisma schema using <code>npx prisma db push</code>.</em></p>
</div>

#### 3. Seed Reference Data
Seed taxonomy records for industries, business types, stages, articles, and sample accounts:

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.2-Prerequiste/prisma-seed.png" alt="Successful Prisma seed execution" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. Successfully seeding reference data and sample accounts using <code>npx prisma db seed</code>.</em></p>
</div>
