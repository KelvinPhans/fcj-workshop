---
title: "Chuẩn bị môi trường"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Chuẩn bị Môi trường & Khởi tạo Cơ sở dữ liệu

Trước khi thực hiện các bước cấu hình Amazon Cognito và kết nối Full-Stack, bạn cần chuẩn bị các công cụ môi trường sau.

#### 1. Yêu cầu công cụ (Prerequisites)
- **Node.js**: Phiên bản 18+ hoặc 20+ LTS.
- **npm** / **yarn** / **pnpm**: Trình quản lý gói phụ thuộc.
- **AWS CLI**: Đã cài đặt và cấu hình credentials với khu vực `ap-southeast-2`.
- **PostgreSQL Database**: Đã khởi chạy locally hoặc trên một instance thử nghiệm.
- **Docker** *(Tùy chọn)*: Để khởi chạy PostgreSQL container qua `docker-compose`.

#### 2. Cấu hình cơ sở dữ liệu với Prisma ORM
Mô hình dữ liệu của Startups Blogs được định nghĩa chi tiết trong `prisma/schema.prisma`. 

Khởi tạo các bảng dữ liệu bằng lệnh:

```bash
cd backend
npx prisma generate
npx prisma db push
```

#### 3. Chạy dữ liệu mẫu (Seed Data)
Đăng ký dữ liệu mẫu cho danh mục ngành nghề (Industries), loại hình doanh nghiệp, giai đoạn phát triển và hồ sơ mẫu:

```bash
npx prisma db seed
```

> Screenshot required:
> Kết quả lệnh `npx prisma db push` và cơ sở dữ liệu PostgreSQL đã được khởi tạo các bảng `users`, `businesses`, `funding_opportunities`.
> Hide AWS account identifiers and sensitive values before capturing.