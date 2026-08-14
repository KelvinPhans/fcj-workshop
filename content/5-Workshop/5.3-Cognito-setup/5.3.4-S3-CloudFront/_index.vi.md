---
title: "S3 Bucket & CloudFront"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

### Lưu trữ Frontend và Media bằng Amazon S3 & CloudFront

Phần này tạo vùng lưu trữ S3 riêng cho Frontend và media, triển khai bản build React production, sau đó phân phối website qua Amazon CloudFront.

#### Bước 1 — Tạo các S3 bucket

Truy cập **Amazon S3 → Buckets → Create bucket** tại `us-east-1` và tạo các bucket riêng theo mục đích:

- `startups-blogs-frontend`: lưu bản React Frontend đã build.
- `startups-blogs-media`: lưu logo doanh nghiệp, avatar, cover và các media được tải lên.

Tên bucket phải duy nhất trên toàn AWS; thêm hậu tố phù hợp nếu tên đã được sử dụng. Giữ **Object Ownership** ở chế độ **Bucket owner enforced**.

Trong production, nên bật **Block Public Access** và cấp CloudFront quyền đọc frontend bucket bằng **Origin Access Control (OAC)**. Ảnh minh họa việc tắt chặn public access; chỉ sử dụng cách này khi chủ động thiết kế public hosting và có bucket policy giới hạn chặt chẽ. Không nên mở công khai toàn bộ media bucket theo mặc định.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-S3-CloudFront/5.3.4-step1-media-bucket.png" alt="Cấu hình public access của Amazon S3 bucket" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. Kiểm tra Block Public Access khi tạo S3 bucket.</em></p>
</div>

#### Bước 2 — Cấu hình CORS

Mở bucket phù hợp, chọn **Permissions → Cross-origin resource sharing (CORS)** và thêm rule cho đúng origin của ứng dụng. Ảnh thử nghiệm dùng wildcard origin cùng nhiều HTTP method; cấu hình production nên giới hạn như sau:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
    "AllowedOrigins": ["https://<CLOUDFRONT_DOMAIN>"],
    "ExposeHeaders": []
  }
]
```

Thay `<CLOUDFRONT_DOMAIN>` bằng origin Frontend đã triển khai. Chỉ thêm `http://localhost:5173` khi môi trường local cần truy cập trực tiếp từ trình duyệt. Tránh `AllowedOrigins: ["*"]` trong production khi có upload hoặc request xác thực.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-S3-CloudFront/5.3.4-step2-cors.png" alt="Cấu hình CORS của Amazon S3" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. Cấu hình CORS cho request xuất phát từ ứng dụng Frontend.</em></p>
</div>

#### Bước 3 — Kiểm tra các bucket của dự án

Quay lại danh sách S3 bucket và kiểm tra các bucket đã tồn tại tại `us-east-1`. Ảnh minh chứng hiển thị `startups-blogs-frontend`, `startups-blogs-media` và một bucket bổ sung của dự án.

Frontend bucket chỉ lưu các file tĩnh đã build. Media cần được tải qua endpoint NestJS `POST /upload` được bảo vệ để Backend kiểm tra quyền sở hữu, MIME type và dung lượng trước khi ghi vào S3.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-S3-CloudFront/5.3.4-step3-static-hosting.png" alt="Các S3 bucket của Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. Frontend bucket và media bucket đã được tạo tại Region N. Virginia.</em></p>
</div>

#### Bước 4 — Build và upload React Frontend

Cấu hình production API URL, build ứng dụng Vite và đồng bộ thư mục `dist` lên S3:

```bash
cd frontend
npm ci
npm run build
aws s3 sync dist/ s3://startups-blogs-frontend --delete
```

Không upload source Frontend, file `.env`, access key hoặc Backend secret. Sau những lần deploy tiếp theo, tạo CloudFront invalidation để người dùng nhận `index.html` và asset mới:

```bash
aws cloudfront create-invalidation \
  --distribution-id <DISTRIBUTION_ID> \
  --paths "/*"
```

#### Bước 5 — Tạo CloudFront distribution

Truy cập **Amazon CloudFront → Distributions → Create distribution** và cấu hình:

- **Origin:** S3 bucket `startups-blogs-frontend`.
- **Origin access:** Origin Access Control (khuyến nghị).
- **Viewer protocol policy:** Redirect HTTP to HTTPS.
- **Default root object:** `index.html`.
- **Allowed methods:** `GET` và `HEAD` cho Frontend tĩnh.

Cập nhật S3 bucket policy bằng OAC policy do CloudFront tạo. Chờ distribution chuyển thành **Enabled** và hoàn tất triển khai.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-S3-CloudFront/5.3.4-step5-distribution.png" alt="CloudFront distribution cho Frontend Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. CloudFront distribution đã Enabled với frontend S3 bucket làm origin.</em></p>
</div>

#### Bước 6 — Cấu hình fallback cho React Router

React Router xử lý điều hướng phía client nên request trực tiếp đến route của ứng dụng có thể nhận `403` hoặc `404` từ S3. Tại **CloudFront → Distribution → Error pages**, tạo các custom error response:

| HTTP error code | Response page path | HTTP response code | Minimum TTL |
|---|---|---:|---:|
| `403` | `/index.html` | `200` | `10` giây |
| `404` | `/index.html` | `200` | `10` giây |

Sau khi lưu, mở CloudFront domain và kiểm tra cả trang chủ lẫn route con như `/businesses`.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-S3-CloudFront/5.3.4-step6-enabled.png" alt="Custom error response của CloudFront dành cho React Router" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 5. CloudFront trả về index.html với HTTP 200 khi gặp phản hồi 403 hoặc 404.</em></p>
</div>
