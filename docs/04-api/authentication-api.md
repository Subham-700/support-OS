# Authentication API

## Overview

The Authentication API manages user identity, authentication, authorization, session management, and account recovery. It uses JWT access tokens and refresh tokens to provide secure authentication for SupportOS.

---

# Base URL

```
/api/v1/auth
```

---

# Authentication Flow

```
User Login

↓

Validate Credentials

↓

Generate Access Token

↓

Generate Refresh Token

↓

Store Refresh Token

↓

Return Tokens

↓

Authenticated Requests

↓

Access Token Expires

↓

Refresh Token Endpoint

↓

New Access Token
```

---

# Authentication Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| POST | /register | Register a new user | No |
| POST | /login | User login | No |
| POST | /refresh | Refresh access token | No (Refresh Token) |
| POST | /logout | Logout current session | Yes |
| POST | /forgot-password | Send password reset email | No |
| POST | /reset-password | Reset password | No |
| POST | /verify-email | Verify email address | No |
| POST | /resend-verification | Resend verification email | Yes |
| GET | /me | Get authenticated user | Yes |
| PATCH | /change-password | Change password | Yes |

---

# Register

## Endpoint

```
POST /auth/register
```

## Request

```json
{
  "organizationName": "Acme Inc",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "StrongPassword123!"
}
```

## Success Response

```json
{
  "success": true,
  "message": "Account created successfully.",
  "data": {
    "userId": "uuid",
    "organizationId": "uuid"
  }
}
```

## Validation

- Organization name required
- First name required
- Last name required
- Valid email
- Password length ≥ 8 characters
- Password must include uppercase, lowercase, number, and special character

---

# Login

## Endpoint

```
POST /auth/login
```

## Request

```json
{
  "email": "john@example.com",
  "password": "StrongPassword123!"
}
```

## Success Response

```json
{
  "success": true,
  "message": "Login successful.",
  "data": {
    "accessToken": "...",
    "refreshToken": "...",
    "expiresIn": 900,
    "user": {
      "id": "uuid",
      "email": "john@example.com",
      "role": "OWNER"
    }
  }
}
```

---

# Refresh Token

## Endpoint

```
POST /auth/refresh
```

## Request

```json
{
  "refreshToken": "..."
}
```

## Response

```json
{
  "success": true,
  "data": {
    "accessToken": "...",
    "expiresIn": 900
  }
}
```

---

# Logout

## Endpoint

```
POST /auth/logout
```

Authentication required.

The refresh token is revoked and the current session is invalidated.

---

# Forgot Password

## Endpoint

```
POST /auth/forgot-password
```

## Request

```json
{
  "email": "john@example.com"
}
```

Response:

```json
{
  "success": true,
  "message": "Password reset instructions have been sent if the email exists."
}
```

This response should be identical whether or not the email exists to avoid account enumeration.

---

# Reset Password

## Endpoint

```
POST /auth/reset-password
```

## Request

```json
{
  "token": "reset-token",
  "password": "NewStrongPassword123!"
}
```

---

# Verify Email

## Endpoint

```
POST /auth/verify-email
```

## Request

```json
{
  "token": "verification-token"
}
```

---

# Resend Verification Email

## Endpoint

```
POST /auth/resend-verification
```

Authentication required.

---

# Get Current User

## Endpoint

```
GET /auth/me
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
    "role": "OWNER",
    "isEmailVerified": true
  }
}
```

---

# Change Password

## Endpoint

```
PATCH /auth/change-password
```

Authentication required.

## Request

```json
{
  "currentPassword": "CurrentPassword123!",
  "newPassword": "NewStrongPassword123!"
}
```

Validation:

- Current password must be correct.
- New password must meet password policy.
- New password must differ from the current password.

---

# Authentication Headers

Protected endpoints require:

```
Authorization: Bearer <access_token>
```

---

# JWT Payload

Example payload:

```json
{
  "sub": "user-uuid",
  "organizationId": "org-uuid",
  "role": "OWNER",
  "email": "john@example.com"
}
```

---

# Token Expiration

| Token | Lifetime |
|---------|----------|
| Access Token | 15 minutes |
| Refresh Token | 7 days |

Refresh tokens are stored securely and can be revoked on logout or password changes.

---

# Validation Rules

Email:

- Required
- Valid email format
- Maximum 255 characters

Password:

- Minimum 8 characters
- Maximum 128 characters
- One uppercase letter
- One lowercase letter
- One number
- One special character

Names:

- Required
- Maximum 100 characters

---

# Authentication Errors

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Invalid credentials |
| 401 | Token expired |
| 401 | Invalid refresh token |
| 403 | Email not verified |
| 404 | Resource not found |
| 409 | Email already exists |
| 422 | Validation failed |

---

# Security Considerations

- Passwords are hashed using Argon2.
- JWTs are signed with a secure secret.
- Refresh tokens are stored securely.
- Password reset tokens expire after a limited time.
- Email verification tokens are single-use.
- Rate limiting is applied to login and password reset endpoints.
- Failed login attempts are logged for auditing.

---

# Summary

The Authentication API provides secure account management using JWT authentication, refresh tokens, email verification, and password recovery. It is designed to support multi-tenant authentication, strong password policies, and secure session management while integrating with the SupportOS authorization system.