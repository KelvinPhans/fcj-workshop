---
title: "Cấu hình Amazon Cognito"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Cấu hình Amazon Cognito User Pool & App Client (`ap-southeast-2`)

Trong phần này, chúng ta sẽ thiết lập **Amazon Cognito User Pool** để quản lý danh tính người dùng bảo mật cho nền tảng Startups Blogs.

#### 1. Tạo Cognito User Pool trên AWS Management Console
1. Truy cập dịch vụ **Amazon Cognito** trên AWS Console, chọn Region `ap-southeast-2` (Sydney).
2. Nhấn **Create User Pool**.
3. Tại phần **Authentication providers**: Chọn **Cognito User Pool**.
4. Tại phần **Sign-in options**: Chọn **Email**.
5. Tại phần **Password policy**: Chọn **Cognito defaults** (yêu cầu tối thiểu 8 ký tự, bao gồm chữ hoa, chữ thường, chữ số và ký tự đặc biệt).
6. Tại phần **Multi-factor authentication (MFA)**: Chọn **No MFA** (hoặc Optional MFA tùy môi trường thử nghiệm).
7. Tại phần **User account recovery**: Chọn **Email only**.

> Screenshot required:
> Cognito User Pool Creation Step 1 - Sign-in Options & Password Policy.
> Hide AWS account identifiers and sensitive values before capturing.

#### 2. Cấu hình Thuộc tính Người dùng & Gửi Email Xác thực
1. Thêm các thuộc tính người dùng tiêu chuẩn (**Standard Attributes**):
   - `email` (Bắt buộc)
   - `name` (Bắt buộc - dùng cho Họ tên đầy đủ)
2. Tại phần **Email configuration**: Chọn **Send email with Cognito** để Cognito tự động gửi mã xác thực 6 chữ số qua email người dùng ngay sau khi đăng ký.

> Screenshot required:
> Cognito User Pool Attributes & Email Delivery Configuration.
> Hide AWS account identifiers and sensitive values before capturing.

#### 3. Tạo Cognito App Client với Secret Hash
1. Tại phần **App client**: Chọn **Confidential client** (Server-side application NestJS).
2. Đặt tên App Client: `startups-blogs-backend-client`.
3. Bật tùy chọn **Generate a client secret**.
4. Tại phần **Authentication flows**: Chọn **ALLOW_USER_PASSWORD_AUTH** và **ALLOW_REFRESH_TOKEN_AUTH**.
5. Nhấn **Create User Pool**.

> Screenshot required:
> Cognito App Client Settings & Authentication Flow options (USER_PASSWORD_AUTH enabled).
> Hide AWS account identifiers and sensitive values before capturing.

#### 4. Ghi nhận tham số cấu hình Môi trường
Sau khi hoàn tất tạo User Pool, ghi nhận các tham số để cấu hình file `.env` phía Backend NestJS (chú ý **không bao giờ commit file `.env` chứa secret lên Git**):

```env
AWS_REGION=ap-southeast-2
COGNITO_USER_POOL_ID=ap-southeast-2_xxxxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
COGNITO_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> Screenshot required:
> Cognito User Pool Overview page showing User Pool ID and App Client ID.
> Hide AWS account identifiers and sensitive values before capturing.
