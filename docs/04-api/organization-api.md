# Organization API

## Overview

The Organization API manages tenant workspaces within SupportOS. Every organization represents an isolated workspace containing users, tickets, knowledge documents, AI interactions, and settings.

Only authorized users (Owner and Administrator) can manage organization resources.

---

# Base URL

```
/api/v1/organizations
```

---

# Organization Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| GET | /current | Get current organization | Yes |
| PATCH | /current | Update organization | Yes |
| GET | /members | List organization members | Yes |
| POST | /members/invite | Invite a user | Yes |
| GET | /members/invitations | List pending invitations | Yes |
| DELETE | /members/invitations/{invitationId} | Cancel invitation | Yes |
| PATCH | /members/{userId}/role | Update user role | Yes |
| DELETE | /members/{userId} | Remove member | Yes |
| GET | /settings | Get organization settings | Yes |
| PATCH | /settings | Update organization settings | Yes |
| GET | /subscription | Get subscription details | Yes |

---

# Get Current Organization

## Endpoint

```
GET /organizations/current
```

Authentication required.

## Response

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Acme Inc",
    "slug": "acme-inc",
    "logoUrl": null,
    "subscriptionPlan": "PRO",
    "status": "ACTIVE",
    "createdAt": "2026-08-04T10:30:00Z"
  }
}
```

---

# Update Organization

## Endpoint

```
PATCH /organizations/current
```

Authentication required.

Required Role:

- Owner
- Administrator

## Request

```json
{
  "name": "Acme Corporation",
  "logoUrl": "https://example.com/logo.png"
}
```

---

# List Members

## Endpoint

```
GET /organizations/members
```

Authentication required.

Supports:

```
?page=1

&limit=20

&search=john

&role=SUPPORT_AGENT
```

---

# Invite Member

## Endpoint

```
POST /organizations/members/invite
```

Required Role:

- Owner
- Administrator

## Request

```json
{
  "email": "agent@example.com",
  "role": "SUPPORT_AGENT"
}
```

## Response

```json
{
  "success": true,
  "message": "Invitation sent successfully."
}
```

Business Rules:

- Email must be unique within the organization.
- Invitation expires after 7 days.
- Duplicate active invitations are not allowed.

---

# List Invitations

## Endpoint

```
GET /organizations/members/invitations
```

Returns all pending invitations.

Example response:

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "email": "agent@example.com",
      "role": "SUPPORT_AGENT",
      "expiresAt": "2026-08-11T10:30:00Z"
    }
  ]
}
```

---

# Cancel Invitation

## Endpoint

```
DELETE /organizations/members/invitations/{invitationId}
```

Required Role:

- Owner
- Administrator

Response:

```
204 No Content
```

---

# Update Member Role

## Endpoint

```
PATCH /organizations/members/{userId}/role
```

Required Role:

- Owner

## Request

```json
{
  "role": "SUPPORT_MANAGER"
}
```

Rules:

- Owners cannot demote themselves.
- Only one Owner is allowed by default.
- Target role must exist within the organization.

---

# Remove Member

## Endpoint

```
DELETE /organizations/members/{userId}
```

Required Role:

- Owner

Rules:

- Cannot remove yourself.
- Cannot remove the last Owner.
- Tickets remain associated with the removed user for audit purposes.

---

# Get Organization Settings

## Endpoint

```
GET /organizations/settings
```

Example response:

```json
{
  "success": true,
  "data": {
    "timezone": "UTC",
    "language": "en",
    "ticketPrefix": "SUP",
    "defaultTicketPriority": "MEDIUM",
    "allowPublicKnowledgeBase": false
  }
}
```

---

# Update Organization Settings

## Endpoint

```
PATCH /organizations/settings
```

Request example:

```json
{
  "timezone": "Asia/Kolkata",
  "language": "en",
  "defaultTicketPriority": "HIGH"
}
```

Only provided fields are updated.

---

# Get Subscription

## Endpoint

```
GET /organizations/subscription
```

Example response:

```json
{
  "success": true,
  "data": {
    "plan": "PRO",
    "status": "ACTIVE",
    "billingCycle": "MONTHLY",
    "renewalDate": "2026-09-01",
    "memberLimit": 50
  }
}
```

---

# Validation Rules

Organization Name

- Required
- Maximum 100 characters

Slug

- Unique
- Lowercase
- URL-safe

Invitation Email

- Required
- Valid email
- Maximum 255 characters

Role

Allowed values:

- OWNER
- ADMINISTRATOR
- SUPPORT_MANAGER
- SUPPORT_AGENT
- CUSTOMER

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full organization access |
| Administrator | Manage members and settings |
| Support Manager | View members |
| Support Agent | Read-only organization access |
| Customer | No organization management |

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Organization or user not found |
| 409 | Duplicate invitation or slug |
| 422 | Validation failed |

---

# Audit Events

The following actions generate audit logs:

- Organization updated
- Member invited
- Invitation cancelled
- Member removed
- Role changed
- Settings updated
- Subscription updated

---

# Security Considerations

- All requests require authentication.
- Tenant isolation is enforced using `organizationId`.
- Role-based access control (RBAC) is applied to every endpoint.
- Sensitive operations are logged for auditing.
- Organization data is never accessible across tenants.

---

# Summary

The Organization API enables secure management of tenant workspaces, including organization settings, member management, invitations, roles, and subscriptions. It forms the foundation of SupportOS's multi-tenant architecture while ensuring proper authorization, auditability, and data isolation.