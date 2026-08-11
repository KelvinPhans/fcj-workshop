---
title: "Tích hợp Backend NestJS"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Tích hợp Backend NestJS, Amazon Cognito SDK & HttpOnly Cookie Session

Trong phần này, chúng ta sẽ xem xét chi tiết cách NestJS Backend tích hợp trực tiếp với **Amazon Cognito SDK** (`@aws-sdk/client-cognito-identity-provider`) và bảo mật phiên bằng **HttpOnly Signed Cookies**.

#### 1. Tính toán Cognito Secret Hash (`cognito-secret-hash.ts`)
Do App Client được cấu hình Secret Key để bảo vệ server-side, mọi request từ NestJS tới Cognito cần đi kèm tham số `SECRET_HASH` tính theo thuật toán HMAC-SHA256:

```typescript
import * as crypto from 'crypto';

export function generateCognitoSecretHash(username: string, clientId: string, clientSecret: string): string {
  return crypto
    .createHmac('SHA256', clientSecret)
    .update(username + clientId)
    .digest('base64');
}
```

#### 2. Xử lý Đăng ký & Đăng nhập trong `CognitoService`
- **Đăng ký (`SignUpCommand`)**:
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

- **Đăng nhập (`USER_PASSWORD_AUTH`)**:
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

#### 3. Lưu trữ Token trong HttpOnly Signed Cookies (`auth.controller.ts`)
Mật khẩu và Tokens tuyệt đối **không được lưu trữ trong cơ sở dữ liệu PostgreSQL** và không trả về JavaScript localStorage để phòng chống tấn công XSS. NestJS thiết lập HttpOnly Signed Cookies:

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

#### 4. Kiểm tra Chữ ký RSA JWT (`CognitoAuthGuard` & `aws-jwt-verify`)
Mọi API yêu cầu đăng nhập được bảo vệ bởi `CognitoAuthGuard`. Guard sử dụng thư viện chính thức `aws-jwt-verify` để tải JWKS từ Cognito và xác minh chữ ký RSA của Access Token trước khi truy vấn thông tin người dùng từ PostgreSQL:

```typescript
const payload = await this.cognitoService.verifyAccessToken(accessToken);
const user = await this.prisma.user.findUnique({
  where: { email: payload.username },
});
```

> Screenshot required:
> NestJS AuthController endpoints in Swagger UI (`/auth/register`, `/auth/verify-email`, `/auth/login`, `/auth/me`).
> Hide AWS account identifiers and sensitive values before capturing.
