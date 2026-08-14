---
title: "Chuẩn bị môi trường"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Chuẩn bị Môi trường, Terraform & Cơ sở dữ liệu

Trước khi thực hiện các bước cấu hình Amazon Cognito và hạ tầng Đám mây AWS, bạn cần chuẩn bị các công cụ môi trường sau.

#### 1. Yêu cầu công cụ (Prerequisites)
- **Node.js**: Phiên bản 18+ hoặc 20+ LTS.
- **npm** / **yarn** / **pnpm**: Trình quản lý gói phụ thuộc.
- **Terraform**: Phiên bản v1.5+ để khởi tạo hạ tầng Đám mây tự động.
- **AWS CLI**: Đã cài đặt và cấu hình credentials với khu vực **`us-east-1`** (N. Virginia).
- **PostgreSQL Database**: Đã khởi chạy locally qua Docker (`docker-compose.yml` tại cổng `5433`) hoặc dịch vụ Amazon RDS PostgreSQL.
- **Docker**: Khởi chạy container PostgreSQL và MinIO S3 simulation.

#### 2. Cấu hình cơ sở dữ liệu với Prisma ORM
Mô hình dữ liệu của Startups Blogs được định nghĩa chi tiết trong `backend/prisma/schema.prisma`. 

Khởi tạo và đồng bộ cơ sở dữ liệu bằng lệnh:

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.2-Prerequiste/prisma-push.png" alt="Đồng bộ cơ sở dữ liệu thành công bằng Prisma" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. Đồng bộ thành công cơ sở dữ liệu PostgreSQL với Prisma schema bằng lệnh <code>npx prisma db push</code>.</em></p>
</div>

#### 3. Khởi tạo dữ liệu mẫu (Seed Data)
Đăng ký dữ liệu mẫu cho danh mục ngành nghề (Industries), loại hình doanh nghiệp, giai đoạn phát triển, bài viết mẫu và tài khoản thử nghiệm:

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.2-Prerequiste/prisma-seed.png" alt="Khởi tạo dữ liệu mẫu thành công bằng Prisma" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. Khởi tạo thành công dữ liệu tham chiếu và các tài khoản mẫu bằng lệnh <code>npx prisma db seed</code>.</em></p>
</div>
