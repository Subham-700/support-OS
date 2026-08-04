# API Overview

## Overview

This document provides an overview of the SupportOS REST API. The API serves as the communication layer between the frontend, backend, third-party integrations, and AI services.

SupportOS follows RESTful principles, uses JSON for request and response payloads, and secures endpoints with JWT-based authentication.

---

# API Goals

The API is designed to:

- Provide a consistent REST interface
- Support multi-tenant architecture
- Enable secure authentication and authorization
- Facilitate communication between frontend and backend
- Support AI-powered features
- Allow future third-party integrations
- Maintain backward compatibility through versioning

---

# API Architecture

```
                    Client Applications
        ┌────────────────────────────────────┐
        │                                    │
        │  Next.js Dashboard                 │
        │  Customer Portal                   │
        │  Mobile App (Future)               │
        │  Third-Party Integrations          │
        │                                    │
        └────────────────────────────────────┘
                       │
                       ▼
                HTTPS / REST API
                       │
                       ▼
               NestJS Backend API
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
 Authentication   Business Logic   AI Services
      │                │                │
      └────────────────┼────────────────┘
                       ▼
                  PostgreSQL
```

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

# API Style

SupportOS follows REST conventions.

Resources include:

- Authentication
- Organizations
- Users
- Roles
- Tickets
- Messages
- Knowledge Base
- AI
- Notifications
- Audit Logs

---

# Content Type

Requests

```
Content-Type: application/json
```

Responses

```
Content-Type: application/json
```

File uploads use:

```
multipart/form-data
```

---

# Request Lifecycle

```
Client Request

↓

Authentication

↓

Authorization

↓

Validation

↓

Business Logic

↓

Database

↓

Response
```

---

# Response Format

Every successful response follows a consistent structure.

Example:

```json
{
  "success": true,
  "message": "Operation completed successfully.",
  "data": {
    "...": "..."
  }
}
```

---

# Error Response

Example:

```json
{
  "success": false,
  "statusCode": 400,
  "message": "Validation failed.",
  "errors": [
    {
      "field": "email",
      "message": "Invalid email address."
    }
  ]
}
```

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

# HTTP Methods

| Method | Purpose |
|----------|----------|
| GET | Retrieve resources |
| POST | Create resources |
| PUT | Replace resources |
| PATCH | Update resources |
| DELETE | Remove resources |

---

# Status Codes

| Code | Meaning |
|------|----------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

---

# API Modules

The API is organized into functional modules.

| Module | Purpose |
|---------|----------|
| Authentication | User authentication |
| Organizations | Tenant management |
| Users | User management |
| Roles | Role-based access |
| Tickets | Support tickets |
| Messages | Ticket conversations |
| Knowledge Base | Documentation |
| AI | AI-powered assistance |
| Notifications | Alerts and updates |
| Audit Logs | Security and compliance |

---

# Security

The API implements:

- HTTPS
- JWT Authentication
- Refresh Tokens
- Role-Based Access Control (RBAC)
- Request Validation
- Rate Limiting
- Input Sanitization
- Tenant Isolation

---

# Versioning

The API uses URL versioning.

Example:

```
/api/v1/auth/login
```

Future versions:

```
/api/v2/...
```

---

# Pagination

Collection endpoints support pagination.

Example:

```
GET /tickets?page=1&limit=20
```

---

# Filtering

Example:

```
GET /tickets?status=OPEN
```

---

# Sorting

Example:

```
GET /tickets?sortBy=createdAt&order=desc
```

---

# Search

Example:

```
GET /tickets?search=billing
```

---

# Rate Limiting

Example policy:

- Anonymous users: 60 requests/minute
- Authenticated users: 300 requests/minute

Limits may vary by endpoint.

---

# API Documentation

SupportOS provides interactive API documentation using Swagger.

Development URL:

```
http://localhost:3001/api/docs
```

---

# Best Practices

- Use HTTPS for all requests.
- Always include authentication tokens for protected endpoints.
- Validate request payloads.
- Handle errors gracefully.
- Use pagination for large datasets.
- Avoid exposing sensitive information in responses.

---

# Future Enhancements

Planned improvements include:

- GraphQL API
- WebSocket support
- Public API keys
- Webhooks
- SDKs for JavaScript and Python

---

# Summary

The SupportOS API provides a secure, scalable, and consistent REST interface for all platform functionality. By following standardized request and response formats, authentication mechanisms, and versioning strategies, the API enables seamless communication between clients and backend services while remaining extensible for future growth.