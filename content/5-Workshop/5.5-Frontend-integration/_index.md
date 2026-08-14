---
title: "React Frontend Integration"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### React 19 Frontend Integration, Zustand State, REST APIs & Admin Dashboard

In this section, we examine how the **React 19 Frontend** application connects to NestJS backend REST APIs, manages state via Zustand, and presents the **Admin Dashboard UI**.

#### 1. Authentication State & Interceptors (`authStore.ts` & Axios Interceptors)
The frontend utilizes **Zustand** combined with **Axios Interceptors** to automatically attach Bearer JWT Tokens to API requests:

```typescript
// Inside services/api.ts
api.interceptors.request.use((config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

#### 2. Capital Raising Integration & Backend Write APIs
The capital submission view (`PostIdea.tsx` / `RaiseCapital`) connects directly to backend write endpoints:
- Business creation (`POST /api/v1/businesses`).
- Funding opportunity publishing (`POST /api/v1/businesses/:businessId/funding-opportunities`).
- Image file upload (`POST /api/v1/upload`).

#### 3. Admin Dashboard UI (`/admin/*`)
The frontend features a comprehensive Admin area under `/admin`:
- **Overview (`/admin/overview`)**: Statistical metrics for Users, Businesses, Articles, and Pending Submissions.
- **Businesses Management (`/admin/businesses`)**: Approving (`PUBLISHED`) or Rejecting (`REJECTED`) businesses, viewing financial metrics (`AdminViewBusiness`).
- **Users Management (`/admin/users`)**: Managing system-wide users and role assignments (`USER`, `MODERATOR`, `ADMIN`).
- **Articles Management (`/admin/articles`)**: Article preview, comment moderation, and advanced filtering.
- **Change Proposals (`/admin/businesses/:id/edit`)**: Admin JSON change proposal generation with founder Diff/Merge approval interfaces.

> Screenshot required:
> Admin Dashboard Overview page (`/admin/overview`) showing stats widgets and business approval controls.
> Hide AWS account identifiers and sensitive values before capturing.
