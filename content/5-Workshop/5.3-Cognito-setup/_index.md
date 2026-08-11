---
title: "Cognito Configuration"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Amazon Cognito User Pool & App Client Setup (`ap-southeast-2`)

In this section, we set up **Amazon Cognito User Pool** to manage user identities securely for the Startups Blogs platform.

#### 1. Create Cognito User Pool on AWS Management Console
1. Navigate to **Amazon Cognito** service on AWS Console in `ap-southeast-2` (Sydney) region.
2. Click **Create User Pool**.
3. Under **Authentication providers**: Select **Cognito User Pool**.
4. Under **Sign-in options**: Select **Email**.
5. Under **Password policy**: Choose **Cognito defaults** (minimum 8 characters, uppercase, lowercase, numbers, symbols).
6. Under **Multi-factor authentication (MFA)**: Select **No MFA** (or Optional MFA).
7. Under **User account recovery**: Select **Email only**.

> Screenshot required:
> Cognito User Pool Creation Step 1 - Sign-in Options & Password Policy.
> Hide AWS account identifiers and sensitive values before capturing.

#### 2. Configure User Attributes & Email Verification
1. Add standard required attributes:
   - `email` (Required)
   - `name` (Required - for Full Name)
2. Under **Email configuration**: Select **Send email with Cognito** for native 6-digit confirmation emails upon signup.

> Screenshot required:
> Cognito User Pool Attributes & Email Delivery Configuration.
> Hide AWS account identifiers and sensitive values before capturing.

#### 3. Create Confidential App Client with Secret Hash
1. Under **App client**: Select **Confidential client** (for NestJS server-side integration).
2. Set App Client Name: `startups-blogs-backend-client`.
3. Check **Generate a client secret**.
4. Under **Authentication flows**: Enable **ALLOW_USER_PASSWORD_AUTH** and **ALLOW_REFRESH_TOKEN_AUTH**.
5. Click **Create User Pool**.

> Screenshot required:
> Cognito App Client Settings & Authentication Flow options (USER_PASSWORD_AUTH enabled).
> Hide AWS account identifiers and sensitive values before capturing.

#### 4. Environment Parameters
Record parameters for backend environment configuration (ensure `.env` files are ignored by git):

```env
AWS_REGION=ap-southeast-2
COGNITO_USER_POOL_ID=ap-southeast-2_xxxxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
COGNITO_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> Screenshot required:
> Cognito User Pool Overview page showing User Pool ID and App Client ID.
> Hide AWS account identifiers and sensitive values before capturing.
