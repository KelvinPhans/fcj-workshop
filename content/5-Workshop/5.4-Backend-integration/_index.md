---
title: "NestJS Backend Integration"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### NestJS Backend Integration, Amazon Cognito SDK & HttpOnly Cookie Session

In this section, we examine how the NestJS backend integrates with **Amazon Cognito SDK** (`@aws-sdk/client-cognito-identity-provider`) and protects user sessions via **HttpOnly Signed Cookies**.

#### 1. Cognito Secret Hash Calculation (`cognito-secret-hash.ts`)
Because the Confidential App Client uses a Client Secret, all backend requests sent to Cognito must include a `SECRET_HASH` generated via HMAC-SHA256:

```typescript
import * as crypto from 'crypto';

export function generateCognitoSecretHash(username: string, clientId: string, clientSecret: string): string {
  return crypto
    .createHmac('SHA256', clientSecret)
    .update(username + clientId)
    .digest('base64');
}
```

#### 2. Registration & Authentication Handlers (`CognitoService`)
- **User Registration (`SignUpCommand`)**:
```typescript
async signUp(email: string, password: string, fullName: string) {
  const secretHash = this.getSecretHash(email);
  const command = new SignUpCommand({
    ClientId: this.clientId,
    SecretHash: secretHash,
    Username: email,
    Password: password,
    UserAttributes: [
      { Name: 'email', Value: email },
      { Name: 'name', Value: fullName },
    ],
  });
  return await this.cognitoClient.send(command);
}
```

- **User Authentication (`USER_PASSWORD_AUTH`)**:
```typescript
async login(email: string, password: string): Promise<CognitoTokens> {
  const secretHash = this.getSecretHash(email);
  const command = new InitiateAuthCommand({
    AuthFlow: AuthFlowType.USER_PASSWORD_AUTH,
    ClientId: this.clientId,
    AuthParameters: {
      USERNAME: email,
      PASSWORD: password,
      SECRET_HASH: secretHash,
    },
  });
  const response = await this.cognitoClient.send(command);
  return {
    accessToken: response.AuthenticationResult.AccessToken,
    idToken: response.AuthenticationResult.IdToken,
    refreshToken: response.AuthenticationResult.RefreshToken,
    expiresIn: response.AuthenticationResult.ExpiresIn,
  };
}
```

#### 3. HttpOnly Signed Cookie Token Transport (`auth.controller.ts`)
User passwords and JWT tokens are **never stored in PostgreSQL** nor exposed to client-side JavaScript. NestJS attaches signed HttpOnly cookies:

```typescript
response.cookie('sb_access_token', result.tokens.accessToken, {
  httpOnly: true,
  secure: process.env.COOKIE_SECURE === 'true',
  sameSite: 'lax',
  path: '/',
  signed: true,
  maxAge: 3600 * 1000,
});
```

#### 4. Cryptographic JWT Signature Verification (`CognitoAuthGuard` & `aws-jwt-verify`)
Protected routes use `CognitoAuthGuard`. The guard uses official `aws-jwt-verify` to fetch JWKS from Cognito and verify the Access Token RSA signature before fetching user records from PostgreSQL:

```typescript
const payload = await this.cognitoService.verifyAccessToken(accessToken);
const user = await this.prisma.user.findUnique({
  where: { email: payload.username },
});
```

> Screenshot required:
> NestJS AuthController endpoints in Swagger UI (`/auth/register`, `/auth/verify-email`, `/auth/login`, `/auth/me`).
> Hide AWS account identifiers and sensitive values before capturing.
