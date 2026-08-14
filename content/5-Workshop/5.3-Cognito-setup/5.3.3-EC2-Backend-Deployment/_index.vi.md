---
title: "EC2 & Backend Deployment"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Triển khai Backend NestJS trên Amazon EC2

Phần này hướng dẫn khởi tạo EC2 Ubuntu, chuẩn bị môi trường Node.js, cấu hình Backend và duy trì API **Startups Blogs** bằng PM2.

#### Bước 1 — Khởi tạo EC2 instance

Truy cập **Amazon EC2 → Instances → Launch instance** và cấu hình:

- **Name:** `startups-blogs-backend`
- **AMI:** Ubuntu Server LTS, 64-bit (`x86`)
- **Instance type:** instance nhỏ phù hợp Workshop, chẳng hạn `t3.micro`
- **Storage:** tối thiểu 8 GiB EBS

Kiểm tra phần Summary và nhấn **Launch instance** sau khi hoàn thành cấu hình key pair và network ở bước tiếp theo.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step1-launch.png" alt="Khởi tạo EC2 instance cho Backend Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. Chọn Ubuntu AMI và instance type cho Backend Startups Blogs.</em></p>
</div>

#### Bước 2 — Cấu hình SSH key và network

Tạo hoặc chọn RSA key pair `startups-key` và lưu private key đã tải xuống tại vị trí an toàn. Trong quá trình khởi tạo instance, chọn:

- `Startups-Blogs-vpc`
- Public subnet đã tạo tại mục 5.3.1
- Cấp public IPv4 khi cần quản trị trực tiếp qua SSH
- `EC2-Security-Group`

Giới hạn SSH port `22` bằng địa chỉ IP tin cậy của quản trị viên. Không commit private key lên Git hoặc chia sẻ công khai.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step2-network.png" alt="EC2 key pair dùng để quản trị Backend" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. RSA key pair startups-key dùng cho kết nối SSH được cấp quyền.</em></p>
</div>

#### Bước 3 — Kiểm tra EC2 instance

Chờ Instance state chuyển thành **Running** và Status check hiển thị **2/2 checks passed**. Ghi nhận public DNS hoặc public IPv4 để kết nối SSH.

```bash
chmod 400 startups-key.pem
ssh -i startups-key.pem ubuntu@<EC2_PUBLIC_DNS>
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step3-running.png" alt="EC2 instance Startups Blogs đang chạy" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. EC2 Backend ở trạng thái Running và vượt qua cả hai status check.</em></p>
</div>

#### Bước 4 — Cài đặt và kiểm tra Node.js, PM2

Kết nối vào EC2 và cài đặt môi trường chạy Backend. Môi trường triển khai trong ảnh sử dụng Node.js `v20.20.2`, npm `10.8.2` và PM2 `7.0.3`.

```bash
sudo apt update
sudo apt install -y git curl
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2

node -v
npm -v
pm2 -v
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step4-nodejs.png" alt="Phiên bản Node.js npm và PM2 trên EC2" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. Node.js, npm và PM2 đã được cài đặt trên EC2 instance.</em></p>
</div>

#### Bước 5 — Cấu hình biến môi trường Backend

Clone hoặc chuyển mã nguồn Backend lên EC2, cài dependencies, tạo Prisma Client và tạo file `.env` runtime ngoài phạm vi quản lý của Git.

```bash
git clone <BACKEND_REPOSITORY_URL>
cd Startups_Blogs/backend
npm ci
npx prisma generate
```

Cấu hình kết nối RDS và các dịch vụ AWS. Nên sử dụng IAM Role của EC2 và AWS Secrets Manager thay cho access key dài hạn:

```env
DATABASE_URL="postgresql://postgres:<password>@<RDS_ENDPOINT>:5432/postgres?schema=public&sslmode=require"
AWS_S3_BUCKET=startups-blogs-media
AWS_S3_ENDPOINT=https://s3.us-east-1.amazonaws.com
AWS_S3_REGION=us-east-1
```

Ảnh minh chứng đã được che credential vì thông tin xác thực không được xuất hiện trong tài liệu Workshop, Git, log hoặc screenshot.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step5-env-redacted.png" alt="Cấu hình AWS S3 đã che thông tin nhạy cảm" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 5. Cấu hình môi trường S3 của Backend sau khi loại bỏ credential nhạy cảm.</em></p>
</div>

#### Bước 6 — Build và chạy Backend bằng PM2

Build ứng dụng NestJS và khởi chạy bằng PM2:

```bash
npm run build
pm2 start dist/main.js --name startups-backend
pm2 save
pm2 startup
```

Chạy lệnh được `pm2 startup` in ra, sau đó lưu lại danh sách process. Kiểm tra process và HTTP endpoint nội bộ:

```bash
pm2 status
curl -I http://localhost:3000
```

Kết quả mong đợi là process PM2 ở trạng thái `online` và NestJS/Express trả về HTTP response. Phản hồi `200 OK` xác nhận ứng dụng đang lắng nghe cục bộ tại port `3000`; truy cập từ bên ngoài vẫn được kiểm soát bằng EC2 Security Group và API Gateway.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-EC2-Backend/5.3.3-step6-pm2.png" alt="Backend NestJS online trong PM2 và phản hồi tại port 3000" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 6. Process startups-backend online trong PM2 và trả về HTTP 200 tại port 3000.</em></p>
</div>
