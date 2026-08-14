---
title: "API Gateway"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

### Công khai Backend NestJS qua Amazon API Gateway

Phần này tạo Amazon API Gateway **HTTP API** để chuyển tiếp request từ Frontend đến Backend NestJS đang chạy trên EC2 port `3000`.

#### Bước 1 — Tạo HTTP API

Truy cập **Amazon API Gateway → APIs → Create API**, chọn **HTTP API** và nhập:

- **API name:** `startups-blogs-api`
- **Endpoint type:** `Regional`
- **Region:** `us-east-1`

Tạo API và mở API từ danh sách. Ảnh minh chứng cho thấy API sử dụng giao thức HTTP và Regional endpoint.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step1-create-api.png" alt="HTTP API của Startups Blogs trong Amazon API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. HTTP API startups-blogs-api được tạo với Regional endpoint.</em></p>
</div>

#### Bước 2 — Tạo HTTP integration đến EC2

Mở **Integrations → Manage integrations**, tạo HTTP integration và dùng địa chỉ Backend NestJS làm integration URI:

```text
http://<EC2_PUBLIC_IP>:3000/{proxy}
```

Ảnh minh chứng xác nhận integration `ANY` đến EC2 port `3000` với timeout `30.000 ms`. Trước khi kiểm tra API Gateway, bảo đảm PM2 hiển thị ứng dụng ở trạng thái `online`.

> Integration qua public IP trong ảnh phù hợp để minh họa topology Workshop nhưng khiến EC2 port `3000` có thể truy cập từ Internet. Kiến trúc production nên dùng private integration qua VPC Link và internal load balancer, hoặc giới hạn EC2 Security Group về upstream được phê duyệt.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step2-integration.png" alt="HTTP integration giữa API Gateway và EC2 port 3000" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. HTTP proxy integration chuyển request đến Backend NestJS trên EC2 port 3000.</em></p>
</div>

#### Bước 3 — Tạo proxy route

Mở **Routes → Create** và cấu hình:

- **Method:** `ANY`
- **Route:** `/{proxy+}`
- **Integration:** HTTP integration đến EC2 ở Bước 2

Greedy path variable `{proxy+}` chuyển tiếp các đường dẫn lồng nhau như `/articles`, `/businesses/example` và `/admin/stats` đến NestJS. Ảnh cho thấy route chưa gắn API Gateway authorizer; việc xác thực và phân quyền vì thế do Cognito JWT Guard và Role Guard của Backend thực thi.

Nếu ứng dụng cần xử lý root path `/`, tạo thêm route `ANY /` vì `/{proxy+}` chỉ khớp đường dẫn có ít nhất một segment.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step3-route.png" alt="ANY proxy route trong API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. Route ANY /{proxy+} đã được gắn với HTTP integration của Backend.</em></p>
</div>

#### Bước 4 — Cấu hình CORS

Mở **CORS** và chọn các HTTP method Frontend cần sử dụng:

- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`
- `OPTIONS`

Cho phép các header ứng dụng sử dụng, bao gồm `Content-Type` và `Authorization`. Đặt allowed origin bằng CloudFront hoặc custom domain thực tế của Frontend:

```text
https://<CLOUDFRONT_DOMAIN>
```

Ảnh minh họa sử dụng wildcard cho origins, headers và exposed headers trong lúc kiểm thử. Tránh `*` trong production, đặc biệt khi bật credential hoặc HttpOnly cookie. Session cookie cross-origin yêu cầu origin cụ thể và **Allow credentials = Yes**; wildcard origin không thể kết hợp an toàn với browser credential.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step4-cors.png" alt="Cấu hình CORS của API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. Cấu hình HTTP method, origin và header CORS để Frontend gọi API.</em></p>
</div>

#### Bước 5 — Deploy và kiểm tra Invoke URL

Mở **Stages**, chọn `$default` và bật **Automatic deployment** để tự động phát hành những thay đổi về route và integration. Ghi nhận Invoke URL do API Gateway cung cấp:

```text
https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com
```

Backend hiện chưa bật global prefix `/api/v1` như tài liệu quy ước, vì vậy gọi trực tiếp runtime route cho đến khi bổ sung `setGlobalPrefix('api/v1')`:

```bash
curl -i https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com/articles
curl -i https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com/admin/stats
```

Endpoint được bảo vệ có thể trả về đúng `401 Unauthorized` khi thiếu token; kết quả này vẫn chứng minh API Gateway đã kết nối đến Backend NestJS. Cấu hình biến production của Frontend bằng Invoke URL và build lại Frontend:

```env
VITE_API_URL=https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step5-invoke-url.png" alt="Default stage và Invoke URL của API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 5. Stage $default bật automatic deployment và public API Invoke URL.</em></p>
</div>
