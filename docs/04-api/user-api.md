# User API

## Overview

The User API manages user profiles, account settings, avatars, user search, and administrative user management. It enables authenticated users to manage their own profiles while allowing administrators to manage users within their organization.

All endpoints are tenant-aware and enforce Role-Based Access Control (RBAC).

---

# Base URL

```
/api/v1/users
```

---

# User Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| GET | /me | Get current user profile | Yes |
| PATCH | /me | Update current user profile | Yes |
| PATCH | /me/password | Change password | Yes |
| POST | /me/avatar | Upload avatar | Yes |
| DELETE | /me/avatar | Remove avatar | Yes |
| GET | / | List users | Yes |
| GET | /{userId} | Get user details | Yes |
| PATCH | /{userId} | Update user | Yes |
| PATCH | /{userId}/status | Update user status | Yes |
| DELETE | /{userId} | Deactivate user | Yes |

---

# Get Current User

## Endpoint

```
GET /users/me
```

Authentication required.

## Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "organizationId": "uuid",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "avatarUrl": null,
    "role": "SUPPORT_AGENT",
    "status": "ACTIVE",
    "isEmailVerified": true,
    "createdAt": "2026-08-04T10:30:00Z"
  }
}
```

---

# Update Current User

## Endpoint

```
PATCH /users/me
```

## Request

```json
{
  "firstName": "John",
  "lastName": "Smith"
}
```

Only supplied fields are updated.

---

# Change Password

## Endpoint

```
PATCH /users/me/password
```

## Request

```json
{
  "currentPassword": "CurrentPassword123!",
  "newPassword": "NewPassword123!"
}
```

Validation:

- Current password must match.
- New password must satisfy password policy.
- New password must differ from the current password.

---

# Upload Avatar

## Endpoint

```
POST /users/me/avatar
```

Content-Type

```
multipart/form-data
```

Accepted formats:

- JPG
- JPEG
- PNG
- WEBP

Maximum size:

```
5 MB
```

## Response

```json
{
  "success": true,
  "data": {
    "avatarUrl": "https://cdn.supportos.com/avatar/user-123.png"
  }
}
```

---

# Remove Avatar

## Endpoint

```
DELETE /users/me/avatar
```

Response

```
204 No Content
```

---

# List Users

## Endpoint

```
GET /users
```

Supports:

```
?page=1

&limit=20

&search=john

&role=SUPPORT_AGENT

&status=ACTIVE
```

Example response:

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "role": "SUPPORT_AGENT",
      "status": "ACTIVE"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 52,
    "totalPages": 3
  }
}
```

---

# Get User

## Endpoint

```
GET /users/{userId}
```

Returns detailed information about a specific user.

---

# Update User

## Endpoint

```
PATCH /users/{userId}
```

Required Role:

- Owner
- Administrator

## Request

```json
{
  "firstName": "Jane",
  "lastName": "Doe"
}
```

---

# Update User Status

## Endpoint

```
PATCH /users/{userId}/status
```

## Request

```json
{
  "status": "SUSPENDED"
}
```

Allowed values:

- ACTIVE
- INACTIVE
- SUSPENDED

Business Rules:

- Owners cannot suspend themselves.
- The last active Owner cannot be suspended.

---

# Deactivate User

## Endpoint

```
DELETE /users/{userId}
```

This performs a soft delete.

Response:

```
204 No Content
```

Business Rules:

- Users cannot delete themselves.
- The last Owner cannot be removed.
- Historical tickets remain associated with the user.

---

# Validation Rules

First Name

- Required
- Maximum 100 characters

Last Name

- Required
- Maximum 100 characters

Email

- Valid email format
- Maximum 255 characters
- Unique within organization

Avatar

- JPG
- PNG
- WEBP
- Maximum 5 MB

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full access |
| Administrator | Manage users |
| Support Manager | View users |
| Support Agent | Manage own profile |
| Customer | Manage own profile |

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | User not found |
| 409 | Email already exists |
| 413 | File too large |
| 415 | Unsupported media type |
| 422 | Validation failed |

---

# Audit Events

The following actions generate audit logs:

- User profile updated
- Password changed
- Avatar uploaded
- Avatar removed
- User status changed
- User deactivated

---

# Security Considerations

- Users can only modify their own profile unless authorized.
- All requests require authentication.
- Avatar uploads are validated and scanned.
- Passwords are never returned in API responses.
- All operations are restricted to the authenticated user's organization.

---

# Summary

The User API enables secure profile management, administrative user operations, avatar management, and account maintenance. It supports multi-tenant isolation, strong validation, RBAC, and comprehensive audit logging while providing a consistent RESTful interface.