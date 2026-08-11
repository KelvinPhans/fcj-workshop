---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### Dọn dẹp Tài nguyên AWS & Tổng kết Workshop

Để tránh phát sinh chi phí không mong muốn sau khi hoàn thành bài thực hành, hãy tiến hành dọn dẹp các tài nguyên thử nghiệm.

#### 1. Dọn dẹp Amazon Cognito User Pool
1. Đăng nhập vào **AWS Management Console**, chuyển đến dịch vụ **Amazon Cognito** tại khu vực `ap-southeast-2`.
2. Chọn **User Pools** và nhấp vào tên User Pool thử nghiệm (`Startups Blogs User Pool`).
3. Chọn **Delete User Pool**.
4. Nhập xác nhận tên User Pool để xóa hoàn toàn User Pool và App Client đi kèm.

> Screenshot required:
> Confirmation modal for deleting the Cognito User Pool.
> Hide AWS account identifiers and sensitive values before capturing.

#### 2. Dọn dẹp Cơ sở dữ liệu Local PostgreSQL
Xóa cơ sở dữ liệu thử nghiệm hoặc dừng Docker container:

```bash
docker-compose down -v
```

#### 3. Kết luận bài Workshop
Thông qua bài thực hành này, bạn đã:
- Hiểu rõ kiến trúc hệ thống ứng dụng kết nối đầu tư **Startups Blogs**.
- Nắm vững quy trình cấu hình và tích hợp **Amazon Cognito User Pool** (`ap-southeast-2`).
- Triển khai thành công quy trình đăng ký, đăng nhập, xác thực email và quản lý phiên bằng **HttpOnly Signed Cookies** trên **NestJS** và **React 19**.
- Thẩm định tính an toàn của token xác thực qua chữ ký RSA với **`aws-jwt-verify`**.
- Phân định rõ ràng giữa các tính năng đã triển khai thực tế và lộ trình mở rộng các dịch vụ AWS trong tương lai.
