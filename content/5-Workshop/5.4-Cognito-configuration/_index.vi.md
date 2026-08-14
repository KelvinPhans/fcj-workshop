---
title: "Cấu hình Amazon Cognito"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

### Cấu hình Amazon Cognito cho Startups Blogs

Amazon Cognito User Pool đảm nhiệm đăng ký, xác minh email, đăng nhập và phát hành JWT cho **Startups Blogs** tại Region `us-east-1`. Theo kiến trúc dự án, Cognito xác định người dùng là ai; Backend NestJS xác minh access token, liên kết tài khoản bằng `cognitoSub` và quyết định người dùng được phép làm gì.

#### Bước 1 — Kiểm tra User Pool

Truy cập **Amazon Cognito → User pools → Overview**. User Pool thực tế có:

- **Region:** US East (N. Virginia), `us-east-1`
- **Feature plan:** Essentials
- **OpenID Connect configuration URL** để client khám phá issuer metadata
- **Token signing key URL (JWKS)** để Backend xác minh chữ ký RSA của JWT

Ghi nhận `User pool ID` để cấu hình Backend nhưng không đưa mật khẩu, token hoặc secret vào tài liệu. Backend phải kiểm tra đúng issuer, client, loại token và thời hạn token; không chỉ decode payload.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/overview.png" alt="Tổng quan Amazon Cognito User Pool của Startups Blogs" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 1. Tổng quan User Pool tại us-east-1 cùng OpenID Connect và JWKS URL.</em></p>
</div>

#### Bước 2 — Cấu hình App Client

Mở **Applications → App clients** và chọn `startups-blogs-app`. Ảnh minh chứng xác nhận:

- **App client name:** `startups-blogs-app`
- **Client secret:** Không có (`-`)
- **Authentication flows:** Choice-based sign-in, Username and password, Secure Remote Password (SRP), và lấy token từ phiên đã xác thực
- **Access token expiration:** 60 phút
- **ID token expiration:** 60 phút
- **Refresh token expiration:** 5 ngày
- **Token revocation:** Được bật
- **Prevent user existence errors:** Được bật

Đây là public client phù hợp cho React chạy trong trình duyệt vì không nhúng client secret. Frontend chỉ lưu Client ID; không được đưa secret hoặc AWS credential vào biến `VITE_*`.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/app-client.png" alt="App Client startups-blogs-app của Amazon Cognito" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 2. App Client không có client secret và bật các authentication flow cần thiết.</em></p>
</div>

#### Bước 3 — Kiểm tra đăng ký và xác minh người dùng

Mở **User management → Users**. Cognito hiển thị trạng thái email, trạng thái xác nhận và trạng thái tài khoản. Người dùng có `Email verified = Yes`, `Confirmation status = Confirmed` và `Status = Enabled` có thể đăng nhập; bản ghi `Unconfirmed` cần hoàn tất mã xác nhận hoặc được xử lý theo chính sách quản trị.

Luồng tích hợp của dự án:

1. React gửi yêu cầu đăng ký đến Cognito.
2. Cognito gửi mã xác minh email.
3. Người dùng xác nhận mã và đăng nhập.
4. Frontend gửi Cognito access token đến NestJS.
5. Backend xác minh token và đồng bộ User trong PostgreSQL bằng `cognitoSub`.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/users-redacted.png" alt="Danh sách người dùng Cognito đã che thông tin cá nhân" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 3. Trạng thái xác minh và kích hoạt người dùng; UUID và email đã được che để bảo vệ dữ liệu cá nhân.</em></p>
</div>

#### Bước 4 — Tạo và quản lý Cognito Group ADMIN

Mở **User management → Groups**, tạo nhóm chính xác tên `ADMIN` và chỉ thêm tài khoản quản trị được phê duyệt. Ảnh minh chứng cho thấy nhóm có hai thành viên, đều đã xác minh email, `Confirmed` và `Enabled`.

Theo `BACKEND_ARCHITECTURE.md` và `AWS_ARCHITECTURE.md`, Backend không tin role do Frontend gửi lên. Với request quản trị, Backend:

1. Xác minh chữ ký, issuer, client và expiration của Cognito access token.
2. Kiểm tra membership `ADMIN` hiện tại qua Cognito.
3. Từ chối tài khoản `LOCKED` hoặc người dùng không còn thuộc nhóm.
4. Đồng bộ thay đổi role bằng `AdminAddUserToGroup`, `AdminRemoveUserFromGroup` và thu hồi phiên cũ khi cần.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.4-Cognito/group-admin-redacted.png" alt="Nhóm ADMIN của Cognito đã che thông tin thành viên" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Hình 4. Cognito Group ADMIN và các thành viên đã xác minh; thông tin cá nhân đã được che.</em></p>
</div>

#### Bước 5 — Cấu hình ứng dụng và kiểm tra

Frontend sử dụng Region, User Pool ID và Client ID của public client. Backend sử dụng cùng User Pool để xác minh token:

```env
COGNITO_REGION=us-east-1
COGNITO_USER_POOL_ID=us-east-1_xxxxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

Không khai báo `COGNITO_CLIENT_SECRET` cho App Client trong ảnh vì client này không có secret. Lưu cấu hình production trong môi trường runtime được bảo vệ, không commit file `.env`.

Cuối cùng, kiểm tra các trường hợp: đăng ký và xác minh email thành công; token thiếu/sai trả `401`; user hợp lệ nhưng không thuộc `ADMIN` trả `403`; Admin hợp lệ truy cập được endpoint quản trị. Mục **5.5 Tích hợp Backend NestJS** trình bày chi tiết cơ chế xác minh JWT và RBAC.
