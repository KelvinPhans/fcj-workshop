---
title: "Amazon Cognito Configuration"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Configure Amazon Cognito for Startups Blogs

Amazon Cognito User Pool handles registration, email verification, sign-in, and JWT issuance for **Startups Blogs** in `us-east-1`. Cognito establishes identity; the NestJS backend verifies access tokens, links accounts through `cognitoSub`, and enforces application authorization.

#### Step 1 — Verify the User Pool

Open **Amazon Cognito → User pools → Overview**. The deployed pool uses the Essentials plan in US East (N. Virginia) and exposes OpenID Connect metadata plus a JWKS URL for RSA token-signature verification.

Record the User Pool ID for backend configuration. The backend must verify issuer, client, token use, signature, and expiration rather than merely decoding the JWT payload.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/overview.png" alt="Startups Blogs Amazon Cognito User Pool overview" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. User Pool overview in us-east-1 with OpenID Connect and JWKS URLs.</em></p>
</div>

#### Step 2 — Configure the App Client

Open **Applications → App clients → startups-blogs-app**. The evidence confirms a public client with no client secret, choice-based sign-in, username/password, SRP, authenticated-session token retrieval, 60-minute access and ID tokens, a five-day refresh token, token revocation, and user-existence-error prevention.

This is appropriate for browser-based React because no secret is embedded in frontend code. The frontend stores only the Client ID; never place secrets or AWS credentials in `VITE_*` variables.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/app-client.png" alt="Amazon Cognito startups-blogs-app App Client" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. Public App Client without a client secret and with the required authentication flows.</em></p>
</div>

#### Step 3 — Verify registered users

Open **User management → Users**. A user with verified email, `Confirmed` confirmation status, and `Enabled` status can sign in. An `Unconfirmed` record must finish email confirmation or be handled according to the administrative policy.

The application flow is: React registers the user; Cognito emails a confirmation code; the user confirms and signs in; React sends the Cognito access token to NestJS; the backend verifies it and synchronizes the PostgreSQL User through `cognitoSub`.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/users-redacted.png" alt="Privacy-redacted Cognito users list" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 3. User verification and activation states; UUIDs and emails are redacted for privacy.</em></p>
</div>

#### Step 4 — Manage the ADMIN group

Open **User management → Groups**, create the exact group name `ADMIN`, and add only approved administrators. The evidence shows two verified, confirmed, and enabled members.

For administrative requests, the backend verifies the access-token signature, issuer, client, and expiration, then checks current `ADMIN` membership through Cognito. It never trusts a role submitted by the frontend. Role changes use `AdminAddUserToGroup` and `AdminRemoveUserFromGroup`, with existing sessions revoked when required.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/group-admin-redacted.png" alt="Privacy-redacted Cognito ADMIN group" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 4. Cognito ADMIN group with verified members; personal identifiers are redacted.</em></p>
</div>

#### Step 5 — Configure and test the application

Configure both applications against the same User Pool:

```env
COGNITO_REGION=us-east-1
COGNITO_USER_POOL_ID=us-east-1_xxxxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

Do not define `COGNITO_CLIENT_SECRET` for the App Client shown because it has no secret. Keep production configuration in the protected runtime environment and never commit `.env` files.

Test successful registration and email verification, `401` for missing/invalid tokens, `403` for authenticated non-admin users on ADMIN routes, and successful access for a valid ADMIN member. Section **5.5 NestJS Backend Integration** covers JWT verification and RBAC in detail.
