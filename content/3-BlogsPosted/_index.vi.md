---
title: "Các bài viết Blog"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Danh mục các bài viết kỹ thuật đã đăng tải:

### 1. [Blog 1 - XÂY DỰNG HỆ THỐNG XÁC THỰC BẢO MẬT VỚI AMAZON COGNITO CHO ỨNG DỤNG REACT VÀ NESTJS](3.1-Blog1/)
Bài viết giới thiệu giải pháp tích hợp Amazon Cognito User Pool vào ứng dụng Full-Stack React và NestJS để cung cấp các tính năng đăng ký, xác thực email qua mã 6 chữ số, đăng nhập và khôi phục mật khẩu. Lấy dự án Startups Blogs làm ca nghiên cứu thực tế, bài viết giải thích lý do tại sao các request xác thực được điều hướng qua NestJS Backend thay vì gọi trực tiếp từ trình duyệt, giúp bảo vệ an toàn Client Secret trên Server.

### 2. [Blog 2 - TĂNG CƯỜNG BẢO MẬT AMAZON COGNITO VỚI SECRETHASH VÀ KIỂM TRA CHỮ KÝ JWT TRONG NESTJS](3.2-Blog2/)
Bài viết tập trung vào các cơ chế bảo mật Backend khi sử dụng Amazon Cognito Confidential App Client. Hướng dẫn chi tiết cách tính toán Cognito `SECRET_HASH` bằng thuật toán HMAC-SHA256 và cách thẩm định chữ ký RSA của Access Token bằng `aws-jwt-verify` trước khi cho phép truy cập API. Đồng thời phân tích lý do tại sao việc chỉ decode chuỗi JWT là chưa đủ và bắt buộc phải xác minh chữ ký, thời hạn, token_use và client_id.

### 3. [Blog 3 - QUẢN LÝ PHIÊN ĐĂNG NHẬP AMAZON COGNITO VỚI HTTPONLY COOKIES, REFRESH TOKENS VÀ PHÂN QUYỀN RBAC](3.3-Blog3/)
Bài viết khám phá giải pháp quản lý phiên làm việc an toàn cho Amazon Cognito. Thay vì lưu trữ token trong `localStorage` hay `sessionStorage` dễ bị tấn công XSS, NestJS Backend lưu token vào HttpOnly Signed Cookies phía Server. Bài viết trình bày chi tiết cơ chế duy trì phiên, làm mới token (`REFRESH_TOKEN_AUTH`), đăng xuất, cài đặt thuộc tính bảo mật cookie và cách kết hợp xác thực Cognito với vai trò người dùng trong PostgreSQL để bảo vệ tính năng gọi vốn.