---
title: "Cognito Configuration"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Amazon Cognito User Pool, App Client & Cognito Groups Setup (`us-east-1`)

In this section, we set up **Amazon Cognito User Pool** to manage user identities securely for the Startups Blogs platform in the **`us-east-1`** region.

#### 1. Create Cognito User Pool on AWS Management Console / Terraform
1. Navigate to **Amazon Cognito** service on AWS Console in **`us-east-1`** (N. Virginia) region (or automate via `terraform/cognito.tf`).
2. Click **Create User Pool**.
3. Under **Authentication providers**: Select **Cognito User Pool**.
4. Under **Sign-in options**: Select **Email**.
5. Under **Password policy**: Choose **Cognito defaults** (minimum 8 characters, uppercase, lowercase, numbers, symbols).
6. Under **User account recovery**: Select **Email only**.

> Screenshot required:
> Cognito User Pool Creation Step 1 - Sign-in Options & Password Policy in `us-east-1`.
> Hide AWS account identifiers and sensitive values before capturing.

#### 2. Configure User Attributes & Email Verification
1. Add standard required attributes:
   - `email` (Required)
   - `name` (Required - for Full Name)
2. Under **Email configuration**: Select **Send email with Cognito** for native 6-digit confirmation emails upon signup.

> Screenshot required:
> Cognito User Pool Attributes & Email Delivery Configuration.
> Hide AWS account identifiers and sensitive values before capturing.

#### 3. Create Confidential App Client with Client Secret Key
1. Under **App client**: Select **Confidential client** (for NestJS server-side integration).
2. Set App Client Name: `startups-blogs-backend-client`.
3. Check **Generate a client secret**.
4. Under **Authentication flows**: Enable **ALLOW_USER_PASSWORD_AUTH** and **ALLOW_REFRESH_TOKEN_AUTH**.

> Screenshot required:
> Cognito App Client Settings & Authentication Flow options (USER_PASSWORD_AUTH enabled).
> Hide AWS account identifiers and sensitive values before capturing.

#### 4. Configure Cognito User Pool Groups for ADMIN Role
Create a User Pool Group named **`ADMIN`**. The system `ADMIN` role is synchronized with Cognito Group `ADMIN` using `CognitoGroupsService`:
- `AdminAddUserToGroupCommand`
- `AdminRemoveUserFromGroupCommand`

```typescript
// Inside cognito-groups.service.ts
const command = isAdmin
  ? new AdminAddUserToGroupCommand({ GroupName: 'ADMIN', Username: username, UserPoolId: userPoolId })
  : new AdminRemoveUserFromGroupCommand({ GroupName: 'ADMIN', Username: username, UserPoolId: userPoolId });
await this.client.send(command);
```

#### 5. Environment Parameters
Record parameters for backend environment configuration (ensure `.env` files are ignored by git):

```env
AWS_REGION=us-east-1
COGNITO_USER_POOL_ID=us-east-1_xxxxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
COGNITO_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> Screenshot required:
> Cognito User Pool Overview page showing User Pool ID and App Client ID in region `us-east-1`.
> Hide AWS account identifiers and sensitive values before capturing.
