---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Tích hợp Xác thực Đám mây Amazon Cognito cho Ứng dụng Full-Stack Startups Blogs (React + NestJS + PostgreSQL)

#### Tổng quan

Trong bài thực hành (Workshop) này, bạn sẽ học cách thiết kế, cấu hình và triển khai hệ thống xác thực người dùng bảo mật chuẩn doanh nghiệp bằng **Amazon Cognito User Pool** cho nền tảng kết nối đầu tư **Startups Blogs**.

Hệ thống kết hợp kiến trúc Full-Stack bao gồm **React 19 (TypeScript, Vite)** ở Frontend, **NestJS REST API** ở Backend, cơ sở dữ liệu **PostgreSQL** kết hợp **Prisma ORM**, và đám mây **AWS Amazon Cognito** tại khu vực `ap-southeast-2`.

#### Điểm nổi bật của giải pháp
+ **Ủy quyền Quản lý Identity**: Không lưu trữ mật khẩu trực tiếp trong cơ sở dữ liệu PostgreSQL. Mật khẩu và xác thực người dùng được ủy quyền hoàn toàn cho Amazon Cognito.
+ **Xác thực Email qua Mã 6 Chữ số**: Tự động gửi email kích hoạt tài khoản ngay sau khi người dùng công khai đăng ký (`BUSINESS_OWNER`, `INVESTOR`, `ENTERPRISE_PARTNER`).
+ **Bảo mật Session qua HttpOnly Cookies**: Token xác thực (`sb_access_token`, `sb_id_token`, `sb_refresh_token`) được lưu trong HttpOnly Signed Cookies phía Server, chống lại nguy cơ đánh cắp token qua tấn công XSS.
+ **Kiểm tra Chữ ký JWT với aws-jwt-verify**: NestJS Backend thẩm định trực tiếp chữ ký RSA của token từ Cognito JWKS đối với mọi request được bảo vệ.
+ **Bảo vệ Tuyến đường Gọi vốn (Raise Capital)**: Phân quyền giao diện 8 bước lập hồ sơ gọi vốn (`RaiseCapital`), chỉ cho phép các vai trò được ủy quyền tiếp cận.

#### Nội dung các phần hướng dẫn

1. [Tổng quan về Workshop & Kiến trúc](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường & Cơ sở dữ liệu PostgreSQL](5.2-Prerequiste/)
3. [Cấu hình Amazon Cognito User Pool & App Client](5.3-Cognito-setup/)
4. [Tích hợp Backend NestJS & HttpOnly Cookie Session](5.4-Backend-integration/)
5. [Tích hợp Frontend React & Tuyến đường Gọi vốn Bảo vệ](5.5-Frontend-integration/)
6. [Rà soát Bảo mật & Định hướng Mở rộng AWS](5.6-Security-review/)
7. [Dọn dẹp tài nguyên & Tổng kết](5.7-Cleanup/)