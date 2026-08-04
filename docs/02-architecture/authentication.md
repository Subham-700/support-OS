# Authentication Architecture

## Overview

This document describes the authentication architecture for SupportOS. Authentication is responsible for verifying user identities and securing access to the platform.

SupportOS uses JWT-based authentication with refresh tokens, secure password hashing, and email verification. The architecture is designed to support future integrations such as OAuth and Single Sign-On (SSO).

---

# Authentication Goals

The authentication system is designed to provide:

- Secure user authentication
- Stateless API authentication
- Multi-tenant support
- Session management
- Password recovery
- Email verification
- Future OAuth support
- Future SSO support

---

# Authentication Flow

```
User

↓

Login Request

↓

Authentication Service

↓

Verify Credentials

↓

Generate JWT Access Token

↓

Generate Refresh Token

↓

Store Refresh Token

↓

Return Tokens

↓

Authenticated User
```

---

# Components

## Authentication Service

Responsibilities:

- User login
- User registration
- Email verification
- Password reset
- Refresh token management
- Logout
- Session validation

---

## Identity Provider

During MVP, SupportOS manages user identities internally.

Future support:

- Google OAuth
- Microsoft OAuth
- GitHub OAuth
- SAML
- OpenID Connect

---

# Registration Flow

```
User Registers

↓

Validate Input

↓

Check Email Availability

↓

Hash Password

↓

Create User

↓

Generate Verification Token

↓

Send Verification Email

↓

User Verifies Email

↓

Account Activated
```

---

# Login Flow

```
User Enters Credentials

↓

Validate Input

↓

Find User

↓

Verify Password

↓

Generate JWT

↓

Generate Refresh Token

↓

Store Refresh Token

↓

Return Tokens
```

---

# Logout Flow

```
User Requests Logout

↓

Invalidate Refresh Token

↓

Remove Session

↓

Logout Successful
```

---

# Password Reset Flow

```
User Requests Password Reset

↓

Generate Reset Token

↓

Send Email

↓

User Opens Link

↓

Validate Token

↓

Set New Password

↓

Invalidate Old Sessions

↓

Success
```

---

# Email Verification Flow

```
Registration

↓

Generate Verification Token

↓

Email Sent

↓

User Opens Link

↓

Verify Token

↓

Activate Account
```

---

# JWT Tokens

SupportOS uses two token types.

## Access Token

Purpose:

- Authenticate API requests

Lifetime:

- 15 minutes

Contents:

- User ID
- Organization ID
- Role
- Issued At
- Expiration

---

## Refresh Token

Purpose:

- Obtain new access tokens

Lifetime:

- 30 days

Stored:

- Securely in database
- Hashed before storage

---

# Password Security

Passwords are never stored in plain text.

Algorithm:

- bcrypt

Requirements:

- Minimum 8 characters
- Uppercase letter
- Lowercase letter
- Number
- Special character

Passwords are hashed before database storage.

---

# Session Management

Each login creates a session.

Session contains:

- Device information
- IP address
- Login timestamp
- Last activity
- Refresh token reference

Future enhancements:

- Multiple device management
- Session revocation
- Device trust

---

# Authentication Middleware

Every protected request follows:

```
Request

↓

Extract JWT

↓

Verify Signature

↓

Check Expiration

↓

Load User

↓

Verify Organization

↓

Continue Request
```

If validation fails:

- Return HTTP 401 Unauthorized

---

# Token Refresh

When the access token expires:

```
Access Token Expired

↓

Client Sends Refresh Token

↓

Validate Refresh Token

↓

Generate New Access Token

↓

Return New Access Token
```

Invalid refresh tokens require re-authentication.

---

# Multi-Tenant Authentication

Every authenticated user belongs to exactly one organization.

JWT includes:

- User ID
- Organization ID
- Role

The Organization ID is used to enforce tenant isolation across the application.

---

# Security Best Practices

SupportOS follows these practices:

- HTTPS only
- Password hashing with bcrypt
- Short-lived access tokens
- Refresh token rotation
- Email verification
- Rate limiting on login endpoints
- Account lockout after repeated failed attempts
- Secure HTTP-only cookies (optional for web clients)

---

# Error Responses

| Error | HTTP Status |
|--------|-------------|
| Invalid credentials | 401 |
| Invalid token | 401 |
| Expired token | 401 |
| Email not verified | 403 |
| Account locked | 403 |
| Too many login attempts | 429 |

---

# Future Enhancements

Future authentication features include:

- Google Sign-In
- Microsoft Sign-In
- GitHub Sign-In
- Magic Links
- Multi-Factor Authentication (MFA)
- Single Sign-On (SSO)
- Biometric authentication for mobile
- Passkeys (WebAuthn)

---

# Summary

SupportOS uses a secure JWT-based authentication system with refresh tokens, bcrypt password hashing, email verification, and tenant-aware identity management. The architecture is designed to be scalable, secure, and extensible while supporting future authentication methods such as OAuth, SSO, and MFA.