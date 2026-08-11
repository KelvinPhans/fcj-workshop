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

```bash
cd backend
npx prisma generate
npx prisma db push
```

#### 3. Seed Reference Data
Seed taxonomy records for industries, business types, stages, articles, and sample accounts:

```bash
npx prisma db seed
```

> Screenshot required:
> Successful execution of `npx prisma db push` showing PostgreSQL tables `users`, `businesses`, `funding_opportunities`, `articles`, and `change_proposals`.
> Hide AWS account identifiers and sensitive values before capturing.