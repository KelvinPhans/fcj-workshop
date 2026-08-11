---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Integrating Amazon Cognito Cloud Authentication for Startups Blogs Full-Stack Application (React + NestJS + PostgreSQL)

#### Overview

In this workshop, you will learn how to design, configure, and implement an enterprise-grade secure user authentication system using **Amazon Cognito User Pool** for the **Startups Blogs** platform.

The system combines a full-stack architecture comprising **React 19 (TypeScript, Vite)** on the frontend, **NestJS REST API** on the backend, **PostgreSQL** with **Prisma ORM** for business data, and **AWS Amazon Cognito** in the `ap-southeast-2` region.

#### Key Highlights
+ **Identity Management Delegation**: Passwords are never stored in the PostgreSQL database. Credential handling is entirely delegated to Amazon Cognito.
+ **6-Digit Email Verification**: Native verification emails sent upon public self-registration (`BUSINESS_OWNER`, `INVESTOR`, `ENTERPRISE_PARTNER`).
+ **HttpOnly Cookie Session Security**: Auth tokens (`sb_access_token`, `sb_id_token`, `sb_refresh_token`) are stored in server-side HttpOnly signed cookies, eliminating token theft risks from XSS attacks.
+ **JWT Verification via aws-jwt-verify**: NestJS backend cryptographically validates token RSA signatures against Cognito JWKS for all protected routes.
+ **Protected Raise Capital Route**: Role-based access control protecting the 8-step Raise Capital wizard (`RaiseCapital`), restricted to authorized business roles.

#### Workshop Modules

1. [Workshop Overview & Architecture](5.1-Workshop-overview/)
2. [Environment Preparation & PostgreSQL Setup](5.2-Prerequiste/)
3. [Configuring Amazon Cognito User Pool & App Client](5.3-Cognito-setup/)
4. [NestJS Backend Integration & HttpOnly Cookie Session](5.4-Backend-integration/)
5. [React Frontend Integration & Protected Raise Capital Route](5.5-Frontend-integration/)
6. [Security Review & Future AWS Expansion Roadmap](5.6-Security-review/)
7. [Resource Cleanup & Summary](5.7-Cleanup/)