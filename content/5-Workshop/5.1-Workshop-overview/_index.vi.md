---
title: "Tổng quan Workshop"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Tổng quan bài lab & Kiến trúc Đám mây AWS Enterprise

Trong bài hướng dẫn này, chúng ta sẽ tìm hiểu kiến trúc tổng thể của hệ thống **Startups Blogs** và luồng xác thực đám mây bằng **Amazon Cognito (`us-east-1`)**.

#### 1. Sơ đồ Kiến trúc AWS Enterprise
Hệ thống được thiết kế theo chuẩn Enterprise Microservices & Serverless kết hợp, tách biệt hoàn toàn giữa Frontend, Backend, Database và Hệ thống Xác thực. Toàn bộ hạ tầng được tự động hóa bằng **Terraform** (Infrastructure as Code).

![Kiến trúc AWS Enterprise của Startups Blogs](/images/5-Workshop/5.1-Workshop-overview/enterprise-aws-architecture.png)

*Hình 1. Kiến trúc AWS Enterprise của hệ thống Startups Blogs.*

#### 2. Phân định rõ các vai trò và phạm vi đăng ký
Hệ thống quản lý 4 vai trò người dùng chính (`UserRole`):
- **`BUSINESS_OWNER`**: Đăng ký công khai. Tạo và quản lý hồ sơ doanh nghiệp, công bố tin gọi vốn.
- **`INVESTOR`**: Đăng ký công khai. Tìm kiếm, tra cứu và đánh giá các cơ hội đầu tư.
- **`ENTERPRISE_PARTNER`**: Đăng ký công khai. Tham gia hợp tác chiến lược và đồng đầu tư.
- **`ADMIN`**: **Không mở đăng ký công khai**. Đồng bộ tự động với Cognito User Pool Group `ADMIN` qua `CognitoGroupsService`. Quản trị viên sử dụng Admin Dashboard để phê duyệt doanh nghiệp (`PUT /businesses/admin/:id/status`), kiểm duyệt bài viết và tạo Đề xuất thay đổi (`ChangeProposal`).

#### 3. Phân định tính năng Thực tế (Implemented) vs Tương lai (Planned)
- **ĐÃ TRIỂN KHAI VÀ KIỂM THỬ (IMPLEMENTED AND VERIFIED)**:
  - Hạ tầng mã nguồn Terraform IaC tại `terraform/` (Region: `us-east-1`).
  - Cơ sở dữ liệu Amazon RDS PostgreSQL & Prisma ORM Schema đầy đủ.
  - REST APIs Đọc & Ghi Doanh nghiệp (`POST/GET/PUT/DELETE /businesses`).
  - REST APIs Đăng tin Gọi vốn (`POST/GET/PUT/DELETE /businesses/:businessId/funding-opportunities`).
  - Tải ảnh đính kèm lên S3/MinIO (`POST /upload`).
  - Backend xác thực Amazon Cognito, SecretHash HMAC-SHA256, và đồng bộ Cognito Group `ADMIN`.
  - Bảo mật HttpOnly Signed Cookie & kiểm tra chữ ký RSA Token qua `aws-jwt-verify` từ JWKS `us-east-1`.
  - Giao diện React 19 Frontend, AuthStore Zustand, Admin Dashboard (`/admin/*`) và Đề xuất thay đổi (Change Proposals).
  - Yêu cầu liên hệ (`POST /businesses/:businessId/contact-requests`).
- **DỰ KIẾN TƯƠNG LAI (PLANNED)**:
  - Hệ thống Thông báo thời gian thực (Real-time Notification System với Notification Schema & WebSocket).
  - Tối ưu hóa luồng phê duyệt gọi vốn đa tầng.
  - Mở rộng bộ kiểm thử tự động E2E.
