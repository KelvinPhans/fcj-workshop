---
title: "RDS PostgreSQL Database"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Thiết lập cơ sở dữ liệu Amazon RDS PostgreSQL

Phần này tạo cơ sở dữ liệu PostgreSQL được quản lý dành cho Backend NestJS.

#### Bước 1 — Chọn database engine

Truy cập **Amazon RDS → Databases → Create database**. Tại **Engine type**, chọn **PostgreSQL** và chọn **Full configuration** để có thể tự cấu hình instance, kết nối mạng, bảo mật, sao lưu và bảo trì. Trong Workshop này, sử dụng template **Production** như ảnh minh chứng.

{{< blog-image src="images/5-Workshop/5.3-Network-RDS/5.3.2-step1-engine.png" alt="Chọn PostgreSQL khi tạo cơ sở dữ liệu Amazon RDS" caption="Hình 1. Chọn PostgreSQL, Full configuration và template Production khi tạo Amazon RDS." >}}

#### Bước 2 — Cấu hình instance và thông tin đăng nhập

Nhập các thông tin database:

- **DB identifier:** `startups-blogs-db`
- **Master username:** `postgres`
- **Credentials management:** `Managed in AWS Secrets Manager`
- **Encryption key:** `aws/secretsmanager (default)` hoặc customer-managed KMS key được dự án phê duyệt

Không lưu mật khẩu production trong source code hoặc file `.env` được commit.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step2-instance.png" alt="Cấu hình tên và thông tin đăng nhập RDS" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. Cấu hình định danh database và master credential bằng AWS Secrets Manager.</em></p>
</div>

#### Bước 3 — Cấu hình kết nối riêng tư

Tại **Connectivity**, cấu hình:

- **Network type:** `IPv4`
- **VPC:** `Startups-Blogs-vpc`
- **DB subnet group:** nhóm chứa các private subnet của dự án
- **Public access:** `No`
- **VPC security group:** `RDS-Security-Group`

Ảnh minh họa các trường khi Default VPC đang được chọn. Khi triển khai chính thức, thay các giá trị mặc định bằng custom VPC, subnet group và Security Group đã tạo ở mục 5.3.1.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step3-connectivity.png" alt="Cấu hình kết nối Amazon RDS" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. Các trường kết nối riêng tư của RDS: IPv4, không public access, DB subnet group và VPC Security Group.</em></p>
</div>

#### Bước 4 — Kiểm tra trạng thái database

Nhấn **Create database** và chờ trạng thái chuyển thành **Available**. Tài nguyên hoàn chỉnh có tên `startups-blogs-db`, chạy PostgreSQL tại `us-east-1a` với instance class `db.t4g.micro`.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step4-available.png" alt="RDS PostgreSQL của Startups Blogs ở trạng thái Available" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. PostgreSQL instance startups-blogs-db đã ở trạng thái Available.</em></p>
</div>

#### Bước 5 — Ghi nhận database endpoint

Mở `startups-blogs-db` và chọn **Connectivity & security**. Ghi nhận endpoint cùng PostgreSQL port `5432`. Ảnh minh chứng hiển thị:

```text
startups-blogs-db.csp286seu5bu.us-east-1.rds.amazonaws.com:5432
```

Cấu hình NestJS runtime thông qua secret hoặc biến môi trường được bảo vệ:

```env
DATABASE_URL="postgresql://postgres:<password>@startups-blogs-db.csp286seu5bu.us-east-1.rds.amazonaws.com:5432/postgres?schema=public&sslmode=require"
```

Chỉ sử dụng private endpoint từ tài nguyên đã được cấp quyền như EC2 Backend. Không commit mật khẩu thật hoặc production connection string lên Git.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.2-step5-endpoint.png" alt="Endpoint PostgreSQL và hướng dẫn kết nối SSL" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 5. Private PostgreSQL endpoint và hướng dẫn kết nối SSL cho Backend.</em></p>
</div>
