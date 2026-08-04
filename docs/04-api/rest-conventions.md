# REST API Conventions

## Overview

This document defines the REST conventions used throughout the SupportOS API. Following these standards ensures consistency, readability, maintainability, and predictable behavior across all API endpoints.

SupportOS follows RESTful design principles and communicates using JSON over HTTPS.

---

# REST Principles

SupportOS APIs follow these principles:

- Resource-oriented URLs
- Stateless requests
- Standard HTTP methods
- Consistent response structure
- Proper HTTP status codes
- Predictable endpoint naming
- Versioned APIs

---

# Base URL

Development

```
http://localhost:3001/api/v1
```

Production

```
https://api.supportos.com/api/v1
```

---

# Resource Naming

Resources use plural nouns.

Examples:

```
/users

/tickets

/organizations

/messages

/notifications
```

Avoid:

```
/getUsers

/createTicket

/deleteMessage
```

---

# URL Structure

General format:

```
/api/v1/{resource}

/api/v1/{resource}/{id}
```

Examples:

```
GET /tickets

GET /tickets/{ticketId}

PATCH /users/{userId}

DELETE /notifications/{notificationId}
```

---

# Nested Resources

Nested resources represent relationships.

Examples:

```
GET /tickets/{ticketId}/messages

POST /tickets/{ticketId}/messages

GET /organizations/{organizationId}/users

GET /knowledge/documents/{documentId}/chunks
```

Keep nesting shallow whenever possible.

---

# HTTP Methods

## GET

Retrieve resources.

Example:

```
GET /tickets
```

Safe and idempotent.

---

## POST

Create a new resource.

Example:

```
POST /tickets
```

Returns:

```
201 Created
```

---

## PUT

Replace an entire resource.

Example:

```
PUT /users/{userId}
```

Use when sending the complete representation.

---

## PATCH

Update part of a resource.

Example:

```
PATCH /tickets/{ticketId}
```

Preferred for partial updates.

---

## DELETE

Delete a resource.

Example:

```
DELETE /notifications/{notificationId}
```

Soft deletion is recommended for business entities.

---

# HTTP Status Codes

Success:

| Code | Meaning |
|------|----------|
| 200 | OK |
| 201 | Created |
| 202 | Accepted |
| 204 | No Content |

Client Errors:

| Code | Meaning |
|------|----------|
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 429 | Too Many Requests |

Server Errors:

| Code | Meaning |
|------|----------|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

# Request Format

Requests use JSON.

Example:

```json
{
  "title": "Unable to log in",
  "priority": "HIGH"
}
```

File uploads use:

```
multipart/form-data
```

---

# Response Format

Successful responses:

```json
{
  "success": true,
  "message": "Request completed successfully.",
  "data": {}
}
```

---

# Error Format

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "email",
      "message": "Email is invalid."
    }
  ]
}
```

---

# Resource Identifiers

Resources use UUIDs.

Example:

```
GET /tickets/8b2bafc8-6cb4-4e73-aec8-f55df83d3d95
```

Sequential integer IDs are avoided.

---

# Query Parameters

Used for filtering, sorting, searching, and pagination.

Example:

```
GET /tickets?page=1&limit=20

GET /tickets?status=OPEN

GET /tickets?priority=HIGH

GET /tickets?search=invoice

GET /tickets?sortBy=createdAt&order=desc
```

---

# Pagination

Collection endpoints support pagination.

Example response:

```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 145,
    "totalPages": 8
  }
}
```

---

# Filtering

Examples:

```
GET /tickets?status=OPEN

GET /tickets?priority=URGENT

GET /users?status=ACTIVE
```

Multiple filters:

```
GET /tickets?status=OPEN&priority=HIGH
```

---

# Sorting

Ascending:

```
GET /tickets?sortBy=createdAt&order=asc
```

Descending:

```
GET /tickets?sortBy=createdAt&order=desc
```

---

# Searching

Example:

```
GET /tickets?search=password
```

Search behavior depends on the resource.

---

# Idempotency

Idempotent methods:

- GET
- PUT
- DELETE

Non-idempotent:

- POST

PATCH may or may not be idempotent depending on implementation.

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <access_token>
```

Public endpoints include:

- Login
- Register
- Forgot Password
- Reset Password
- Verify Email
- Health Check

---

# API Versioning

Versioning is included in the URL.

Example:

```
/api/v1/users

/api/v2/users
```

Breaking changes require a new version.

---

# Naming Conventions

Resources:

```
users

tickets

messages

roles
```

Fields:

```
firstName

lastName

createdAt

organizationId
```

Enums:

```
OPEN

CLOSED

HIGH

LOW
```

---

# Validation

All incoming requests are validated.

Examples:

- Required fields
- Email format
- UUID format
- Enum values
- String length
- Numeric ranges

Invalid requests return:

```
422 Unprocessable Entity
```

---

# Security

REST APIs enforce:

- HTTPS
- JWT authentication
- RBAC
- Rate limiting
- Input sanitization
- Tenant isolation

---

# Best Practices

- Use nouns instead of verbs in URLs.
- Return meaningful status codes.
- Keep response structures consistent.
- Use UUIDs for identifiers.
- Validate every request.
- Avoid exposing internal implementation details.
- Document all endpoints with Swagger.

---

# Summary

SupportOS follows RESTful API conventions that emphasize consistency, predictability, and security. By standardizing URLs, HTTP methods, response formats, validation, and versioning, the API provides a reliable interface for frontend applications, mobile clients, and future third-party integrations.