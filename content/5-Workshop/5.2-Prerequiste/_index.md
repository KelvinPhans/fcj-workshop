---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Environment Preparation & Database Initialization

Before proceeding with Amazon Cognito configuration and full-stack integration, ensure the following local development setup is prepared.

#### 1. Software Requirements
- **Node.js**: Version 18+ or 20+ LTS.
- **npm** / **yarn** / **pnpm**: Package manager.
- **AWS CLI**: Installed and configured with region `ap-southeast-2`.
- **PostgreSQL Database**: Running locally or accessible via docker container.
- **Docker** *(Optional)*: To launch PostgreSQL container using `docker-compose`.

#### 2. Database Schema Generation via Prisma ORM
The Startups Blogs domain data model is declared inside `prisma/schema.prisma`.

Execute the following commands to apply migrations:

```bash
cd backend
npx prisma generate
npx prisma db push
```

#### 3. Seed Reference Data
Seed taxonomy records for industries, business types, stages, and sample listings:

```bash
npx prisma db seed
```

> Screenshot required:
> Successful execution of `npx prisma db push` showing PostgreSQL tables `users`, `businesses`, and `funding_opportunities`.
> Hide AWS account identifiers and sensitive values before capturing.