# Authorization Architecture

## Overview

This document defines the authorization architecture for SupportOS. Authorization determines what authenticated users are allowed to access and perform within the system.

SupportOS implements **Role-Based Access Control (RBAC)** combined with **tenant isolation** to ensure users only access resources within their organization.

---

# Authorization Goals

The authorization system is designed to:

- Enforce role-based permissions
- Maintain tenant isolation
- Protect sensitive resources
- Prevent unauthorized actions
- Support future custom roles and permissions

---

# Authorization Model

SupportOS uses Role-Based Access Control (RBAC).

```
User

↓

Role

↓

Permissions

↓

Resource Access
```

A user's permissions are determined by their assigned role within an organization.

---

# User Roles

The system defines the following roles:

## Organization Owner

Highest level of access.

Permissions include:

- Manage organization
- Manage billing
- Invite users
- Assign roles
- View analytics
- Delete organization

---

## Administrator

Responsible for platform administration within an organization.

Permissions include:

- Manage users
- Manage knowledge base
- Configure integrations
- View analytics
- Assign roles (except Owner)

---

## Support Manager

Responsible for overseeing support operations.

Permissions include:

- Assign tickets
- Monitor ticket queues
- View reports
- Manage workflows
- Review agent performance

---

## Support Agent

Handles customer support requests.

Permissions include:

- View assigned tickets
- Reply to customers
- Update ticket status
- Add internal notes
- Request AI assistance

---

## Customer

External user requesting support.

Permissions include:

- Create tickets
- View own tickets
- Reply to own conversations
- Upload attachments

Customers cannot access internal organization data.

---

## AI Assistant

System role used internally.

Capabilities include:

- Search knowledge base
- Suggest replies
- Summarize conversations
- Classify tickets

AI cannot modify data directly unless explicitly permitted by business rules.

---

# Permission Categories

Permissions are grouped into categories.

## Organization

- organization.read
- organization.update
- organization.delete

---

## User Management

- users.read
- users.create
- users.update
- users.delete
- users.invite

---

## Tickets

- tickets.read
- tickets.create
- tickets.assign
- tickets.update
- tickets.close
- tickets.reopen

---

## Conversations

- messages.read
- messages.create
- notes.create

---

## Knowledge Base

- knowledge.read
- knowledge.upload
- knowledge.update
- knowledge.delete

---

## Analytics

- analytics.read
- analytics.export

---

## AI

- ai.generate
- ai.summarize
- ai.search

---

# Authorization Flow

```
Incoming Request

↓

Authenticate JWT

↓

Extract User & Role

↓

Determine Organization

↓

Load Required Permission

↓

Check Permission

↓

Access Granted / Denied
```

---

# Tenant Isolation

SupportOS is a multi-tenant platform.

Every request includes an Organization ID.

Rules:

- Users can only access resources belonging to their organization.
- Cross-organization access is prohibited.
- Database queries must always filter by Organization ID.

Example:

```
Organization A

↓

Users

↓

Tickets

↓

Knowledge

↓

Analytics
```

These resources are isolated from every other organization.

---

# Resource Ownership

Some resources require ownership checks in addition to role checks.

Examples:

### Customer

Can only view:

- Their own tickets
- Their own messages

---

### Support Agent

Can only modify:

- Assigned tickets
- Messages they are authorized to access

---

### Manager

Can access:

- All tickets within the organization

---

# Authorization Guards

NestJS Guards enforce authorization.

Example guards:

- JwtAuthGuard
- RolesGuard
- PermissionsGuard
- OrganizationGuard

Each protected endpoint may use one or more guards.

---

# Example Access Rules

| Action | Owner | Admin | Manager | Agent | Customer |
|---------|:----:|:-----:|:-------:|:-----:|:--------:|
| View Organization | ✅ | ✅ | ✅ | ✅ | ❌ |
| Invite Users | ✅ | ✅ | ❌ | ❌ | ❌ |
| Create Ticket | ❌ | ❌ | ❌ | ❌ | ✅ |
| Assign Ticket | ✅ | ✅ | ✅ | ❌ | ❌ |
| Reply to Ticket | ✅ | ✅ | ✅ | ✅ | ✅ |
| Upload Knowledge | ✅ | ✅ | ❌ | ❌ | ❌ |
| View Analytics | ✅ | ✅ | ✅ | Limited | ❌ |

---

# Access Denied

When authorization fails:

```
Request

↓

Permission Check

↓

Failed

↓

HTTP 403 Forbidden
```

The system logs the failed authorization attempt for auditing.

---

# Audit Logging

Authorization-related events recorded include:

- Permission denied
- Role changes
- Failed access attempts
- Organization switching attempts

These logs support security monitoring and compliance.

---

# Future Enhancements

Planned improvements include:

- Custom roles
- Fine-grained permissions
- Attribute-Based Access Control (ABAC)
- Temporary permissions
- Approval workflows
- Delegated administration

---

# Summary

SupportOS uses a secure Role-Based Access Control (RBAC) model combined with strict tenant isolation to protect organizational data. Authorization is enforced through NestJS Guards, ownership validation, and permission checks, ensuring that every request is evaluated against the user's role and organization before access is granted.