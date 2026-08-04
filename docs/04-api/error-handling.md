# Error Handling

## Overview

The SupportOS API uses a standardized error response format across all endpoints. Every error response includes a status code, human-readable message, and additional details when applicable.

The goals of this strategy are:

- Consistent API responses
- Easy client-side error handling
- Better debugging
- Improved developer experience
- Secure error reporting

---

# Standard Error Response

Every failed request returns the following structure:

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "email",
      "message": "Email must be a valid email address."
    }
  ],
  "timestamp": "2026-08-04T12:00:00Z",
  "path": "/api/v1/auth/register"
}
```

---

# Error Response Fields

| Field | Description |
|--------|-------------|
| success | Always `false` |
| statusCode | HTTP status code |
| message | Human-readable error summary |
| errors | Optional list of validation errors |
| timestamp | Time of the error |
| path | Requested API endpoint |

---

# HTTP Status Codes

## 400 Bad Request

Returned when the request is malformed.

Example:

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Invalid request payload."
}
```

---

## 401 Unauthorized

Returned when authentication fails.

Example:

```json
{
  "success": false,
  "statusCode": 401,
  "message": "Authentication required."
}
```

Possible causes:

- Missing JWT
- Invalid JWT
- Expired JWT
- Invalid refresh token

---

## 403 Forbidden

Returned when the authenticated user lacks permission.

Example:

```json
{
  "success": false,
  "statusCode": 403,
  "message": "You do not have permission to perform this action."
}
```

---

## 404 Not Found

Returned when a requested resource does not exist.

Example:

```json
{
  "success": false,
  "statusCode": 404,
  "message": "Ticket not found."
}
```

---

## 409 Conflict

Returned when a business rule is violated.

Examples:

- Duplicate email
- Duplicate organization slug
- Ticket already closed
- Duplicate invitation

Example response:

```json
{
  "success": false,
  "statusCode": 409,
  "message": "Email already exists."
}
```

---

## 413 Payload Too Large

Returned when an uploaded file exceeds the maximum size.

Example:

```json
{
  "success": false,
  "statusCode": 413,
  "message": "Uploaded file exceeds the maximum allowed size."
}
```

---

## 415 Unsupported Media Type

Returned when an uploaded file format is not supported.

Example:

```json
{
  "success": false,
  "statusCode": 415,
  "message": "Unsupported file format."
}
```

---

## 422 Unprocessable Entity

Returned when validation fails.

Example:

```json
{
  "success": false,
  "statusCode": 422,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "password",
      "message": "Password must contain at least one uppercase letter."
    }
  ]
}
```

---

## 429 Too Many Requests

Returned when rate limits are exceeded.

Example:

```json
{
  "success": false,
  "statusCode": 429,
  "message": "Too many requests. Please try again later."
}
```

---

## 500 Internal Server Error

Returned when an unexpected server error occurs.

Example:

```json
{
  "success": false,
  "statusCode": 500,
  "message": "An unexpected error occurred."
}
```

Internal implementation details should never be exposed.

---

# Validation Errors

Multiple validation errors may be returned together.

Example:

```json
{
  "success": false,
  "statusCode": 422,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "email",
      "message": "Email is required."
    },
    {
      "field": "password",
      "message": "Password must be at least 8 characters."
    }
  ]
}
```

---

# Business Rule Errors

Examples include:

- Cannot delete the last organization owner
- Ticket already closed
- Duplicate invitation
- Email already registered
- Invalid ticket status transition

Business rule violations generally return:

```
409 Conflict
```

---

# Authentication Errors

Common authentication failures:

| Error | Status |
|---------|--------|
| Missing access token | 401 |
| Invalid token | 401 |
| Expired token | 401 |
| Invalid refresh token | 401 |

---

# Authorization Errors

Common authorization failures:

- Customer accessing another user's ticket
- Agent modifying organization settings
- Non-owner removing organization owner

Return:

```
403 Forbidden
```

---

# Resource Errors

Examples:

- Ticket not found
- User not found
- Document not found
- Notification not found

Return:

```
404 Not Found
```

---

# Rate Limiting Errors

Example limits:

| Endpoint | Limit |
|----------|-------|
| Login | 5 requests/minute |
| Password reset | 3 requests/hour |
| AI endpoints | 60 requests/minute |
| General API | 300 requests/minute |

Exceeded limits return:

```
429 Too Many Requests
```

---

# Logging Strategy

The backend logs:

- Request ID
- Timestamp
- User ID
- Organization ID
- Endpoint
- HTTP method
- Status code
- Execution time
- Exception details

Sensitive information such as passwords, tokens, and secrets must never be logged.

---

# Exception Handling

All exceptions are processed by a global exception filter.

Responsibilities:

- Format error responses
- Log unexpected exceptions
- Hide internal implementation details
- Return appropriate HTTP status codes

---

# Error Codes (Optional)

Applications may include machine-readable error codes.

Example:

```json
{
  "success": false,
  "statusCode": 409,
  "errorCode": "EMAIL_ALREADY_EXISTS",
  "message": "Email already exists."
}
```

---

# Best Practices

- Return the correct HTTP status code.
- Keep error messages concise and consistent.
- Do not expose stack traces or internal details.
- Return validation errors with field-level information.
- Log unexpected server errors for debugging.
- Use machine-readable error codes where appropriate.

---

# Summary

The SupportOS error-handling strategy provides a predictable and secure approach to reporting failures. By standardizing response formats, HTTP status codes, validation messages, and logging practices, the API ensures a consistent experience for both frontend developers and end users while protecting sensitive system information.