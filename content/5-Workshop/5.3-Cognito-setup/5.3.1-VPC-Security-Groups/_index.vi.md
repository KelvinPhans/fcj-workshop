---
title: "VPC & Security Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Thiết lập Amazon VPC & Security Groups

Phần này tạo mạng riêng biệt cho hạ tầng **Startups Blogs** tại Region **`us-east-1` (N. Virginia)**.

#### Bước 1 — Tạo VPC

Truy cập **AWS Console → VPC → Your VPCs → Create VPC** và điền:

- **Name tag:** `Startups-Blogs-vpc`
- **IPv4 CIDR block:** `10.0.0.0/16`
- **Tenancy:** `Default`

Bật DNS resolution và DNS hostnames, sau đó nhấn **Create VPC**.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step1-create-vpc.png" alt="Thông tin VPC của Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. Startups-Blogs-vpc ở trạng thái Available với IPv4 CIDR 10.0.0.0/16.</em></p>
</div>

#### Bước 2 — Tạo các subnet

Truy cập **VPC → Subnets → Create subnet**, chọn `Startups-Blogs-vpc` và tạo:

- Public subnet tại `us-east-1a`: `10.0.0.0/20`, dùng cho tài nguyên kết nối Internet như EC2.
- Private subnet: `10.0.128.0/20`, dùng cho tài nguyên cơ sở dữ liệu.
- Private subnet bổ sung: `10.0.200.0/24`, cung cấp subnet thứ hai cho DB subnet group của RDS.

Không cấp public IP cho tài nguyên cơ sở dữ liệu.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step2-subnets.png" alt="Public subnet và private subnet của Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. Các public subnet và private subnet trong VPC của dự án.</em></p>
</div>

#### Bước 3 — Gắn Internet Gateway

Truy cập **VPC → Internet gateways**, tạo `Startups-Blogs-igw` và gắn vào `Startups-Blogs-vpc`. Thêm route `0.0.0.0/0` trỏ đến Internet Gateway trong public route table, sau đó liên kết route table với public subnet.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step3-igw.png" alt="Internet Gateway gắn với VPC Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. Startups-Blogs-igw đã được gắn vào VPC của dự án.</em></p>
</div>

#### Bước 4 — Cấu hình Security Group cho EC2

Tạo `EC2-Security-Group` trong `Startups-Blogs-vpc` và thêm các inbound rule:

- TCP `22` cho quản trị qua SSH. Giới hạn Source bằng địa chỉ IP tin cậy của quản trị viên.
- TCP `3000` cho ứng dụng NestJS. Trong production, giới hạn Source về API Gateway hoặc upstream được phê duyệt.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step4-ec2-sg.png" alt="Inbound rule của EC2 Security Group" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. Inbound rule SSH và cổng ứng dụng NestJS trong EC2-Security-Group.</em></p>
</div>

#### Bước 5 — Cấu hình Security Group cho RDS

Tạo `RDS-Security-Group` trong cùng VPC. Thêm inbound rule PostgreSQL tại TCP port `5432` với Source là `EC2-Security-Group`. Cấu hình này cho phép Backend truy cập PostgreSQL mà không công khai database ra Internet. Xóa các rule PostgreSQL dùng dải CIDR rộng nếu không cần thiết.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-Network-RDS/5.3.1-step5-rds-sg.png" alt="Inbound rule PostgreSQL của RDS Security Group" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 5. Các rule PostgreSQL port 5432 trong RDS-Security-Group.</em></p>
</div>
