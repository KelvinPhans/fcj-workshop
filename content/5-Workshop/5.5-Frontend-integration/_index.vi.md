---
title: "Tích hợp Frontend React"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Tích hợp Frontend React 19, AuthContext & Tuyến đường Bảo vệ Gọi vốn

Trong phần này, chúng ta sẽ xem xét cách ứng dụng **React 19 Frontend** tích hợp với NestJS Auth API và bảo vệ tuyến đường đăng ký gọi vốn (`RaiseCapital`).

#### 1. Quản lý Trạng thái Xác thực (`AuthContext.tsx`)
`AuthProvider` tự động gọi endpoint `GET /api/v1/auth/me` khi khởi chạy ứng dụng để kiểm tra phiên đăng nhập từ HttpOnly Cookie:

```typescript
export const AuthProvider = ({ children }: { children: ReactNode }) => {
  const [user, setUser] = useState<UserProfile | null>(null);
  const [isLoading, setIsLoading] = useState(true);

  const fetchUser = async () => {
    try {
      setIsLoading(true);
      const currentUser = await authApi.getCurrentUser();
      setUser(currentUser);
    } catch {
      setUser(null);
    } finally {
      setIsLoading(false);
    }
  };

  useEffect(() => {
    fetchUser();
  }, []);

  return (
    <AuthContext.Provider value={{ user, isAuthenticated: !!user, isLoading, login, register, logout }}>
      {children}
    </AuthContext.Provider>
  );
};
```

#### 2. Phân quyền Tuyến đường (`ProtectedRoute.tsx`)
Bảo vệ các route nhạy cảm (như `/raise-capital`), chỉ cho phép các vai trò được ủy quyền tiếp cận (`BUSINESS_OWNER` và `ENTERPRISE_PARTNER`):

```typescript
export function ProtectedRoute({ children, allowedRoles }: ProtectedRouteProps) {
  const { user, isAuthenticated, isLoading } = useAuth();

  if (isLoading) return <LoadingSpinner />;
  if (!isAuthenticated) return <Navigate to="/login" replace />;
  if (allowedRoles && user && !allowedRoles.includes(user.role)) {
    return <Navigate to="/" replace />;
  }

  return <>{children}</>;
}
```

#### 3. Quy trình Đăng ký Gọi vốn 8 Bước (Raise Capital Wizard)
Giao diện `/raise-capital` triển khai form wizard 8 bước với tính năng kiểm tra dữ liệu (validation) và tự động lưu bản nháp vào `localStorage`:

1. **Thông tin Doanh nghiệp (`BusinessStep`)**
2. **Thông tin Cơ hội Gọi vốn (`OpportunityStep`)**
3. **Chi tiết Gọi vốn & Phân bổ Vốn (`FundingStep`)**
4. **Thị trường & Chiến lược Phát triển (`MarketGrowthStep`)**
5. **Điểm tin Tài chính (`FinancialStep`)**
6. **Tài liệu & Hồ sơ Đính kèm (`DocumentsStep`)**
7. **Cài đặt Quyền Riêng tư (`VisibilityStep`)**
8. **Tổng duyệt & Gửi Hồ sơ (`ReviewStep`)**

> **Lưu ý về trạng thái triển khai**: Form 8 bước, kiểm tra dữ liệu đầu vào, lưu bản nháp `localStorage` và bảo vệ tuyến đường bằng `ProtectedRoute` **đã được triển khai trên Frontend**. Việc lưu dữ liệu vào cơ sở dữ liệu PostgreSQL (Write APIs) và tải tệp lên Amazon S3 thuộc **định hướng nâng cấp trong tương lai (PLANNED)**.

> Screenshot required:
> Protected `/raise-capital` route showing 8-step wizard form with step indicator.
> Hide AWS account identifiers and sensitive values before capturing.
