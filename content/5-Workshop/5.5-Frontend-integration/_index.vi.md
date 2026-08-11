---
title: "Tích hợp Frontend React"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Tích hợp Frontend React 19, Zustand State, REST APIs & Admin Dashboard

Trong phần này, chúng ta sẽ xem xét cách ứng dụng **React 19 Frontend** tích hợp toàn diện với NestJS Backend REST APIs, quản lý trạng thái bằng Zustand, và triển khai giao diện **Admin Dashboard**.

#### 1. Quản lý Trạng thái Xác thực & Token (`authStore.ts` & Axios Interceptors)
Frontend sử dụng thư viện **Zustand** kết hợp với **Axios Interceptors** để tự động đính kèm Bearer JWT Token vào mọi API request gửi lên Backend:

```typescript
// Trong services/api.ts
api.interceptors.request.use((config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

#### 2. Tích hợp Luồng Đăng ký Gọi vốn & Backend Write APIs
Giao diện Đăng ký Gọi vốn (`PostIdea.tsx` / `RaiseCapital`) kết nối trực tiếp với backend write endpoints:
- Đăng tạo doanh nghiệp mới (`POST /api/v1/businesses`).
- Đăng tải tin gọi vốn (`POST /api/v1/businesses/:businessId/funding-opportunities`).
- Tải tệp hình ảnh đính kèm (`POST /api/v1/upload`).

#### 3. Giao diện Quản trị viên (Admin Dashboard UI - `/admin/*`)
Hệ thống Frontend triển khai khu vực Admin chuyên nghiệp tại tuyến đường `/admin`:
- **Overview (`/admin/overview`)**: Thống kê số lượng Users, Businesses, Articles, và Pending Submissions.
- **Businesses Management (`/admin/businesses`)**: Phê duyệt (`PUBLISHED`) hoặc Từ chối (`REJECTED`) doanh nghiệp, xem thông tin tài chính và lịch sử gọi vốn (`AdminViewBusiness`).
- **Users Management (`/admin/users`)**: Xem danh sách người dùng toàn hệ thống, cập nhật vai trò (`USER`, `MODERATOR`, `ADMIN`).
- **Articles Management (`/admin/articles`)**: Xem trước bài viết (Preview), quản lý bình luận rác và lọc bài viết nâng cao.
- **Change Proposals (`/admin/businesses/:id/edit`)**: Admin tạo đề xuất chỉnh sửa dữ liệu JSON, cho phép Founder kiểm tra Diff/Merge trước khi áp dụng.

> Screenshot required:
> Admin Dashboard Overview page (`/admin/overview`) showing stats widgets and business approval controls.
> Hide AWS account identifiers and sensitive values before capturing.
