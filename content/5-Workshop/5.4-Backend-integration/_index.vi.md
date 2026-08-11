---
title: "Tích hợp Backend NestJS"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Tích hợp Backend NestJS, REST APIs, Amazon S3 Upload & Security Guards

Trong phần này, chúng ta sẽ xem xét chi tiết cách NestJS Backend triển khai hệ thống **REST APIs**, tích hợp **Amazon Cognito SDK**, thẩm định chữ ký **RSA JWT (`us-east-1`)**, và tích hợp **Amazon S3** để tải tệp hình ảnh.

#### 1. Các API Đã Triển khai & Bảo vệ (Implemented REST APIs)

- **Doanh nghiệp (`BusinessesController`)**:
  - `POST /businesses`: Tạo Doanh nghiệp mới (Yêu cầu `JwtAuthGuard`).
  - `GET /businesses`: Lấy danh sách Doanh nghiệp công khai.
  - `GET /businesses/:slug`: Lấy chi tiết Doanh nghiệp theo slug.
  - `PUT /businesses/:id`: Cập nhật Doanh nghiệp (Kiểm tra quyền sở hữu `ownerId`).
  - `DELETE /businesses/:id`: Xóa Doanh nghiệp (Kiểm tra quyền sở hữu `ownerId`).
  - `GET /businesses/admin/all` & `PUT /businesses/admin/:id/status`: Quản trị viên duyệt hồ sơ (`ADMIN`).

- **Tin Gọi vốn (`FundingOpportunitiesController`)**:
  - `POST /businesses/:businessId/funding-opportunities`: Đăng tin gọi vốn.
  - `GET /businesses/:businessId/funding-opportunities`: Lấy danh sách tin gọi vốn.
  - `PUT /businesses/:businessId/funding-opportunities/:id`: Cập nhật tin gọi vốn.
  - `DELETE /businesses/:businessId/funding-opportunities/:id`: Xóa tin gọi vốn.

- **Tải ảnh lên Amazon S3 (`UploadController` & `UploadService`)**:
  - `POST /upload`: Tải tệp hình ảnh (tối đa 5MB, hỗ trợ jpg, png, gif, webp) lên Amazon S3 / MinIO.
  - Sử dụng `@aws-sdk/client-s3` (`PutObjectCommand`, `PutBucketPolicyCommand`).

- **Quản trị & Đề xuất Thay đổi (`AdminController` & `ProposalsController`)**:
  - `GET /admin/stats`: Xem thống kê toàn hệ thống.
  - `POST /admin/proposals/business/:id`: Tạo Đề xuất thay đổi dữ liệu JSON.
  - `POST /proposals/:id/approve` & `POST /proposals/:id/reject`: Founder xem Diff/Merge và chấp thuận/từ chối.

#### 2. Thẩm định Chữ ký RSA JWT & Issuer `us-east-1` (`CognitoStrategy`)

Mọi request đến API bảo vệ được thẩm định bởi `CognitoStrategy` kết hợp thư viện `jwks-rsa`:

```typescript
@Injectable()
export class CognitoStrategy extends PassportStrategy(Strategy, 'cognito') {
  constructor() {
    const userPoolId = process.env.COGNITO_USER_POOL_ID;
    const region = process.env.AWS_REGION || 'us-east-1';

    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      audience: process.env.COGNITO_CLIENT_ID,
      issuer: `https://cognito-idp.${region}.amazonaws.com/${userPoolId}`,
      algorithms: ['RS256'],
      secretOrKeyProvider: passportJwtSecret({
        cache: true,
        rateLimit: true,
        jwksRequestsPerMinute: 5,
        jwksUri: `https://cognito-idp.${region}.amazonaws.com/${userPoolId}/.well-known/jwks.json`,
      }),
    });
  }
}
```

#### 3. Phân quyền Hai tầng (Dual-Layer Authorization Guard)

Hệ thống kết hợp `JwtAuthGuard` (xác minh danh tính người dùng) và `RolesGuard` (xác minh vai trò `UserRole` & Cognito Group `ADMIN`):

```typescript
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles(Role.ADMIN)
@Get('admin/all')
findAllForAdmin() {
  return this.businessesService.findAllForAdmin(...);
}
```

> Screenshot required:
> NestJS Swagger API Documentation showing protected endpoints for Businesses, Funding Opportunities, Upload, and Admin routes.
> Hide AWS account identifiers and sensitive values before capturing.
