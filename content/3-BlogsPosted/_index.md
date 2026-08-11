---
title: "Blogs Posted"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Published technical articles index:

### 1. [Blog 1 - BUILDING SECURE AUTHENTICATION WITH AMAZON COGNITO FOR A REACT AND NESTJS APPLICATION](3.1-Blog1/)
This blog introduces how Amazon Cognito User Pools can be integrated into a React and NestJS application to provide secure registration, email verification, login, and password recovery. Using Startups Blogs as a practical case study, the article explains why authentication requests are routed through the NestJS backend instead of calling Cognito directly from the browser, allowing sensitive configuration such as the Cognito Client Secret to remain securely on the server.

### 2. [Blog 2 - SECURING AMAZON COGNITO AUTHENTICATION WITH SECRETHASH AND JWT VERIFICATION IN NESTJS](3.2-Blog2/)
This blog focuses on the backend security mechanisms required when using an Amazon Cognito confidential app client. It explains how Cognito SecretHash is generated using HMAC-SHA256 and how access tokens are validated with `aws-jwt-verify` before protected APIs are accessed. The article also discusses why decoding a JWT alone is not sufficient and why signature, expiration, token use, and client identity must be verified.

### 3. [Blog 3 - MANAGING AMAZON COGNITO SESSIONS WITH HTTPONLY COOKIES, REFRESH TOKENS, AND ROLE-BASED ACCESS CONTROL](3.3-Blog3/)
This blog explores secure session management for Amazon Cognito authentication. Instead of storing Cognito tokens in `localStorage` or `sessionStorage`, the Startups Blogs backend stores them in HttpOnly cookies that cannot be directly accessed by JavaScript. The article covers session restoration, refresh-token authentication, logout, cookie security, and how Cognito authentication can be combined with PostgreSQL user roles to protect application features.