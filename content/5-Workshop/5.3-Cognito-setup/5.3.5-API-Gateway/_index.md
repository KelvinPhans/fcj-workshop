---
title: "API Gateway"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

### Expose the NestJS Backend through Amazon API Gateway

This section creates an Amazon API Gateway **HTTP API** that forwards frontend requests to the NestJS backend running on EC2 port `3000`.

#### Step 1 — Create the HTTP API

Open **Amazon API Gateway → APIs → Create API**, select **HTTP API**, and enter:

- **API name:** `startups-blogs-api`
- **Endpoint type:** `Regional`
- **Region:** `us-east-1`

Create the API and open it from the APIs list. The evidence shows the API using the HTTP protocol and a Regional endpoint.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step1-create-api.png" alt="Startups Blogs HTTP API in Amazon API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 1. The startups-blogs-api HTTP API created with a Regional endpoint.</em></p>
</div>

#### Step 2 — Create the EC2 HTTP integration

Open **Integrations → Manage integrations**, create an HTTP integration, and use the NestJS backend address as the integration URI:

```text
http://<EC2_PUBLIC_IP>:3000/{proxy}
```

The screenshot verifies an `ANY` integration to EC2 port `3000` with a `30,000 ms` timeout. Confirm that PM2 reports the application as `online` before testing API Gateway.

> The public-IP integration shown here is suitable for demonstrating the workshop topology, but it leaves EC2 port `3000` internet-reachable. A production design should use a private integration through VPC Link and an internal load balancer, or otherwise restrict the EC2 Security Group to an approved upstream path.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step2-integration.png" alt="API Gateway HTTP integration with EC2 port 3000" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 2. HTTP proxy integration forwarding requests to the NestJS backend on EC2 port 3000.</em></p>
</div>

#### Step 3 — Create the proxy route

Open **Routes → Create** and configure:

- **Method:** `ANY`
- **Route:** `/{proxy+}`
- **Integration:** the EC2 HTTP integration from Step 2

The greedy path variable `{proxy+}` forwards nested paths such as `/articles`, `/businesses/example`, and `/admin/stats` to NestJS. The screenshot shows that no API Gateway authorizer is attached; authentication and authorization are therefore enforced by the backend's Cognito JWT and role guards.

If the application also needs the root path `/`, add a separate `ANY /` route because `/{proxy+}` only matches paths containing at least one segment.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step3-route.png" alt="ANY proxy route in API Gateway" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 3. The ANY /{proxy+} route attached to the backend HTTP integration.</em></p>
</div>

#### Step 4 — Configure CORS

Open **CORS** and configure the HTTP methods required by the frontend:

- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`
- `OPTIONS`

Allow the headers used by the application, including `Content-Type` and `Authorization`. Set the allowed origin to the actual CloudFront or custom frontend domain:

```text
https://<CLOUDFRONT_DOMAIN>
```

The screenshot uses wildcard origins, headers, and exposed headers for testing. Avoid `*` in production, especially when credentials or HttpOnly cookies are enabled. Cookie-based cross-origin sessions require an explicit origin and **Allow credentials = Yes**; wildcard origin cannot be combined safely with browser credentials.

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step4-cors.png" alt="API Gateway CORS configuration" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 4. HTTP API CORS methods, origins, and headers configured for frontend access.</em></p>
</div>

#### Step 5 — Deploy and test the Invoke URL

Open **Stages**, select `$default`, and enable **Automatic deployment** so route and integration changes are published automatically. Record the Invoke URL shown by API Gateway:

```text
https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com
```

This backend does not currently enable the documented `/api/v1` global prefix, so call the runtime routes directly until `setGlobalPrefix('api/v1')` is added:

```bash
curl -i https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com/articles
curl -i https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com/admin/stats
```

A protected endpoint may correctly return `401 Unauthorized` without a token; this still confirms that API Gateway reached the NestJS backend. Configure the frontend production variable with the Invoke URL and rebuild the frontend:

```env
VITE_API_URL=https://kk4wz3gg94.execute-api.us-east-1.amazonaws.com
```

<div style="width: 100%; margin: 16px 0;">
  <img src="/images/5-Workshop/5.3-API-Gateway/5.3.5-step5-invoke-url.png" alt="API Gateway default stage and Invoke URL" style="display: block; width: 100%; height: auto;">
  <p style="margin: 8px 0 0; text-align: left;"><em>Figure 5. The $default stage with automatic deployment and the public API Invoke URL.</em></p>
</div>
