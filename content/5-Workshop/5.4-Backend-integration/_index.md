---
title: "NestJS Backend Integration"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### NestJS Backend Integration, REST APIs, Amazon S3 Upload & Security Guards

In this section, we examine how the NestJS backend implements **REST APIs**, integrates the **Amazon Cognito SDK**, verifies cryptographic **RSA JWT signatures (`us-east-1`)**, and handles image file uploads to **Amazon S3**.

#### 1. Implemented & Protected REST APIs

- **Businesses (`BusinessesController`)**:
  - `POST /businesses`: Create new Business (Requires `JwtAuthGuard`).
  - `GET /businesses`: Public business discovery listing.
  - `GET /businesses/:slug`: Detailed business profile by slug.
  - `PUT /businesses/:id`: Update Business (Validates `ownerId` authorization).
  - `DELETE /businesses/:id`: Delete Business (Validates `ownerId` authorization).
  - `GET /businesses/admin/all` & `PUT /businesses/admin/:id/status`: Admin listing & approval workflow (`ADMIN`).

- **Funding Opportunities (`FundingOpportunitiesController`)**:
  - `POST /businesses/:businessId/funding-opportunities`: Publish funding opportunity.
  - `GET /businesses/:businessId/funding-opportunities`: Retrieve funding opportunities list.
  - `PUT /businesses/:businessId/funding-opportunities/:id`: Update funding opportunity.
  - `DELETE /businesses/:businessId/funding-opportunities/:id`: Delete funding opportunity.

- **Amazon S3 Media Upload (`UploadController` & `UploadService`)**:
  - `POST /upload`: Uploads image files (up to 5MB, supports jpg, png, gif, webp) to Amazon S3 / MinIO.
  - Integrates `@aws-sdk/client-s3` (`PutObjectCommand`, `PutBucketPolicyCommand`).

- **Admin & Change Proposals (`AdminController` & `ProposalsController`)**:
  - `GET /admin/stats`: Overview system statistics.
  - `POST /admin/proposals/business/:id`: Create JSON change proposal.
  - `POST /proposals/:id/approve` & `POST /proposals/:id/reject`: Founder Diff/Merge review & approval.

##### Verify the public Businesses API

Start the NestJS backend and open `http://localhost:3000/businesses`, or send the equivalent request with an API client:

```bash
curl http://localhost:3000/businesses
```

The response confirms that the public `GET /businesses` route reads Business records through Prisma and returns structured JSON containing identifiers, slugs, business information, approval and verification states, financial highlights, counters, and owner data. The current runtime does not enable a global `/api/v1` prefix, so the local URL is `/businesses`, consistent with `API_LIST.md`.

The screenshot is a direct browser rendering of the JSON response, not the Swagger UI. Swagger/OpenAPI remains useful for interactive endpoint documentation when enabled separately.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.5-Backend/businesses-api-response.png" alt="JSON response from the public Businesses API" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. Successful JSON response from GET /businesses on the local NestJS server.</em></p>
</div>

#### 2. Cryptographic RSA JWT Signature Verification & Issuer `us-east-1` (`CognitoStrategy`)

Protected API requests are verified by `CognitoStrategy` using `jwks-rsa`:

```typescript
@Injectable()
export class CognitoStrategy extends PassportStrategy(Strategy, 'cognito') {
  constructor() {
    const userPoolId = process.env.COGNITO_USER_POOL_ID;
    const region = process.env.AWS_REGION || 'us-east-1';

    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      audience: process.env.COGNITO_CLIENT_ID,
      issuer: `https://cognito-idp.${region}.amazonaws.com/${userPoolId}`,
      algorithms: ['RS256'],
      secretOrKeyProvider: passportJwtSecret({
        cache: true,
        rateLimit: true,
        jwksRequestsPerMinute: 5,
        jwksUri: `https://cognito-idp.${region}.amazonaws.com/${userPoolId}/.well-known/jwks.json`,
      }),
    });
  }
}
```

#### 3. Dual-Layer Authorization Guard

The system combines `JwtAuthGuard` (verifies user identity) and `RolesGuard` (verifies `UserRole` & Cognito Group `ADMIN` permissions):

```typescript
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles(Role.ADMIN)
@Get('admin/all')
findAllForAdmin() {
  return this.businessesService.findAllForAdmin(...);
}
```
