---
title: "Resource Cleanup"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### AWS Resource Cleanup & Workshop Conclusion

To avoid incurring unexpected charges after completing the workshop, follow these steps to tear down testing resources.

#### 1. Delete Amazon Cognito User Pool
1. Log into the **AWS Management Console** and navigate to **Amazon Cognito** in `ap-southeast-2` region.
2. Select **User Pools** and click the test pool name.
3. Click **Delete User Pool**.
4. Enter the user pool name to permanently confirm deletion of the User Pool and App Client.

> Screenshot required:
> Confirmation modal for deleting the Cognito User Pool.
> Hide AWS account identifiers and sensitive values before capturing.

#### 2. Stop Local PostgreSQL Database
Tear down local development database containers:

```bash
docker-compose down -v
```

#### 3. Workshop Conclusion
In this workshop, you have successfully:
- Understood the full-stack architecture of the **Startups Blogs** business investment platform.
- Mastered configuring and integrating **Amazon Cognito User Pool** in `ap-southeast-2`.
- Implemented user registration, email verification, login, session refresh, and HttpOnly cookie sessions on **NestJS** and **React 19**.
- Verified token RSA cryptographic signatures using **`aws-jwt-verify`**.
- Established a clear boundary between implemented features and the future AWS services expansion roadmap.
