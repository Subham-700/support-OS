# Ticket API

## Overview

The Ticket API manages the complete lifecycle of support tickets. It enables customers to create tickets, agents to manage and resolve them, and administrators to monitor ticket operations. It also supports conversations, attachments, AI assistance, and audit logging.

All ticket operations are scoped to the authenticated user's organization.

---

# Base URL

```
/api/v1/tickets
```

---

# Ticket Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| GET | / | List tickets | Yes |
| POST | / | Create ticket | Yes |
| GET | /{ticketId} | Get ticket details | Yes |
| PATCH | /{ticketId} | Update ticket | Yes |
| DELETE | /{ticketId} | Archive ticket | Yes |
| PATCH | /{ticketId}/status | Update status | Yes |
| PATCH | /{ticketId}/priority | Update priority | Yes |
| PATCH | /{ticketId}/assign | Assign ticket | Yes |
| PATCH | /{ticketId}/unassign | Remove assignee | Yes |
| POST | /{ticketId}/close | Close ticket | Yes |
| POST | /{ticketId}/reopen | Reopen ticket | Yes |
| GET | /{ticketId}/messages | List messages | Yes |
| POST | /{ticketId}/messages | Add message | Yes |
| POST | /{ticketId}/attachments | Upload attachment | Yes |
| GET | /statistics | Ticket statistics | Yes |

---

# Ticket Model

A ticket contains:

- ID
- Title
- Description
- Status
- Priority
- Category
- Customer
- Assigned Agent
- Organization
- Created Date
- Updated Date
- Closed Date

---

# List Tickets

## Endpoint

```
GET /tickets
```

Supports filtering:

```
?page=1

&limit=20

&status=OPEN

&priority=HIGH

&assignedAgentId=uuid

&customerId=uuid

&category=Billing

&search=password

&sortBy=createdAt

&order=desc
```

---

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "title": "Unable to login",
      "status": "OPEN",
      "priority": "HIGH",
      "category": "Authentication",
      "assignedAgent": {
        "id": "uuid",
        "name": "Jane Smith"
      },
      "createdAt": "2026-08-04T10:30:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 145,
    "totalPages": 8
  }
}
```

---

# Create Ticket

## Endpoint

```
POST /tickets
```

## Request

```json
{
  "title": "Unable to log in",
  "description": "I cannot access my account after resetting my password.",
  "priority": "HIGH",
  "category": "Authentication"
}
```

## Success Response

```json
{
  "success": true,
  "message": "Ticket created successfully.",
  "data": {
    "id": "uuid",
    "ticketNumber": "SUP-1001"
  }
}
```

---

# Get Ticket

## Endpoint

```
GET /tickets/{ticketId}
```

Returns:

- Ticket details
- Customer
- Assigned agent
- Messages
- Attachments
- Timeline
- AI summary (if available)

---

# Update Ticket

## Endpoint

```
PATCH /tickets/{ticketId}
```

Request example:

```json
{
  "title": "Unable to login after password reset",
  "category": "Authentication"
}
```

Only supplied fields are updated.

---

# Update Status

## Endpoint

```
PATCH /tickets/{ticketId}/status
```

Request:

```json
{
  "status": "IN_PROGRESS"
}
```

Allowed values:

- OPEN
- IN_PROGRESS
- WAITING_CUSTOMER
- RESOLVED
- CLOSED

---

# Update Priority

## Endpoint

```
PATCH /tickets/{ticketId}/priority
```

Request:

```json
{
  "priority": "URGENT"
}
```

Allowed values:

- LOW
- MEDIUM
- HIGH
- URGENT

---

# Assign Ticket

## Endpoint

```
PATCH /tickets/{ticketId}/assign
```

Request:

```json
{
  "assignedAgentId": "uuid"
}
```

---

# Unassign Ticket

## Endpoint

```
PATCH /tickets/{ticketId}/unassign
```

Removes the current assignee.

---

# Close Ticket

## Endpoint

```
POST /tickets/{ticketId}/close
```

Marks the ticket as **CLOSED** and records the closing timestamp.

---

# Reopen Ticket

## Endpoint

```
POST /tickets/{ticketId}/reopen
```

Changes the ticket status back to **OPEN**.

---

# List Messages

## Endpoint

```
GET /tickets/{ticketId}/messages
```

Response:

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "sender": {
        "id": "uuid",
        "name": "Jane Smith"
      },
      "messageType": "PUBLIC",
      "content": "Can you provide a screenshot of the error?",
      "createdAt": "2026-08-04T11:00:00Z"
    }
  ]
}
```

---

# Add Message

## Endpoint

```
POST /tickets/{ticketId}/messages
```

Request:

```json
{
  "messageType": "PUBLIC",
  "content": "The issue has been resolved."
}
```

Allowed message types:

- PUBLIC
- INTERNAL_NOTE

---

# Upload Attachment

## Endpoint

```
POST /tickets/{ticketId}/attachments
```

Content-Type:

```
multipart/form-data
```

Supported formats:

- PDF
- DOCX
- PNG
- JPG
- JPEG
- ZIP

Maximum file size:

```
25 MB
```

---

# Ticket Statistics

## Endpoint

```
GET /tickets/statistics
```

Example response:

```json
{
  "success": true,
  "data": {
    "total": 542,
    "open": 82,
    "inProgress": 34,
    "waitingCustomer": 18,
    "resolved": 360,
    "closed": 48
  }
}
```

---

# Validation Rules

Title

- Required
- Maximum 200 characters

Description

- Required
- Maximum 10,000 characters

Priority

- LOW
- MEDIUM
- HIGH
- URGENT

Status

- OPEN
- IN_PROGRESS
- WAITING_CUSTOMER
- RESOLVED
- CLOSED

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full access |
| Administrator | Full access |
| Support Manager | Manage all tickets |
| Support Agent | Manage assigned tickets |
| Customer | Create and view own tickets |

---

# Business Rules

- Every ticket belongs to exactly one organization.
- Ticket numbers are unique within an organization.
- Only authorized users can assign or close tickets.
- Closed tickets can be reopened.
- Internal notes are visible only to staff.
- All ticket actions generate audit log entries.

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Ticket not found |
| 409 | Invalid state transition |
| 413 | File too large |
| 415 | Unsupported media type |
| 422 | Validation failed |

---

# Audit Events

The following actions generate audit records:

- Ticket created
- Ticket updated
- Ticket assigned
- Ticket unassigned
- Status changed
- Priority changed
- Ticket closed
- Ticket reopened
- Message added
- Attachment uploaded

---

# Security Considerations

- All ticket endpoints require authentication.
- Tenant isolation is enforced using `organizationId`.
- Customers can access only their own tickets.
- Internal notes are restricted to staff roles.
- Uploaded files are validated and scanned before storage.

---

# Summary

The Ticket API is the core of SupportOS. It provides comprehensive ticket lifecycle management, secure collaboration through messages and attachments, role-based permissions, advanced filtering and search, and complete auditability. Its design supports both customer-facing support workflows and internal operational processes while remaining scalable for future AI-powered enhancements.