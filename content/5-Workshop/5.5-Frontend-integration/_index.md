---
title: "React Frontend Integration"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### React 19 Frontend Integration, AuthContext & Protected Capital Route

In this section, we examine how the **React 19 Frontend** application connects to NestJS Auth API and protects the capital raising wizard route (`RaiseCapital`).

#### 1. Authentication State Management (`AuthContext.tsx`)
`AuthProvider` automatically invokes `GET /api/v1/auth/me` upon app load to validate session cookies:

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

#### 2. Route Access Control (`ProtectedRoute.tsx`)
Protects sensitive routes (such as `/raise-capital`), restricting access strictly to authorized roles (`BUSINESS_OWNER` and `ENTERPRISE_PARTNER`):

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

#### 3. 8-Step Raise Capital Wizard (`RaiseCapital.tsx`)
The `/raise-capital` view renders an 8-step stepper wizard with step validation and `localStorage` draft auto-saving:

1. **Business Information (`BusinessStep`)**
2. **Funding Opportunity (`OpportunityStep`)**
3. **Funding Details & Allocation (`FundingStep`)**
4. **Market & Growth (`MarketGrowthStep`)**
5. **Financial Highlights (`FinancialStep`)**
6. **Documents & Attachments (`DocumentsStep`)**
7. **Privacy Settings (`VisibilityStep`)**
8. **Review & Submit (`ReviewStep`)**

> **Implementation Note**: The 8-step wizard form, input validation, `localStorage` draft persistence, and `ProtectedRoute` role guard are **fully IMPLEMENTED on the frontend**. PostgreSQL backend persistence (Write APIs) and real Amazon S3 file uploads are **PLANNED FOR FUTURE PHASES**.

> Screenshot required:
> Protected `/raise-capital` route showing 8-step wizard form with step indicator.
> Hide AWS account identifiers and sensitive values before capturing.
