---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Integrating AWS Cloud Infrastructure & Amazon Cognito Authentication for Startups Blogs (React + NestJS + PostgreSQL + Terraform)

#### Overview

In this workshop, you will learn how to design, build, and automate a full enterprise cloud infrastructure on **AWS (Region: `us-east-1`)** for the **Startups Blogs** platform.

The system combines a full-stack architecture comprising **React 19 (TypeScript, Vite)** on the frontend, **NestJS REST API** on the backend, **Amazon RDS PostgreSQL**, **Amazon S3** media storage, **Amazon CloudFront** CDN, **Amazon API Gateway**, **Amazon Cognito** authentication, **Amazon CloudWatch** monitoring, and 100% Infrastructure as Code powered by **Terraform**.

#### Key Highlights
+ **Infrastructure as Code via Terraform**: All AWS resources (VPC, EC2, RDS, Cognito, API Gateway, S3, CloudFront, CloudWatch) are declared inside the `terraform/` directory.
+ **Identity Management Delegation**: Passwords are never stored in PostgreSQL. Credential handling is entirely offloaded to Amazon Cognito.
+ **Admin Role Synchronization via Cognito Groups**: Automatic synchronization of the `ADMIN` role between NestJS and Cognito User Pool Group `ADMIN`.
+ **HttpOnly Cookie Session Security**: Auth tokens (`sb_access_token`, `sb_id_token`, `sb_refresh_token`) are stored in server-side HttpOnly signed cookies, mitigating XSS risks.
+ **JWT Verification via aws-jwt-verify**: NestJS backend cryptographically validates RSA signatures against Cognito JWKS in `us-east-1`.
+ **S3 Media Uploads (`POST /upload`)**: Uses `@aws-sdk/client-s3` for uploading business logos, avatars, and cover images to S3/MinIO.

#### Workshop Modules

1. [Workshop Overview & AWS Enterprise Architecture](5.1-Workshop-overview/)
2. [Environment Setup, Terraform & PostgreSQL Preparation](5.2-Prerequiste/)
3. [AWS Infrastructure Configuration](5.3-Cognito-setup/)
4. [Configuring Amazon Cognito](5.4-Cognito-configuration/)
5. [NestJS Backend Integration, REST APIs & HttpOnly Cookie Session](5.4-Backend-integration/)
6. [React 19 Frontend Integration, Zustand State & Admin Dashboard](5.5-Frontend-integration/)
7. [Automating Infrastructure with Terraform & CloudWatch Monitoring](5.6-Security-review/)
8. [Resource Cleanup & Workshop Summary](5.7-Cleanup/)
