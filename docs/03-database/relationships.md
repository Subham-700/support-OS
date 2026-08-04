# Entity Relationships

## Overview

This document defines the relationships between the database entities used in SupportOS. Understanding these relationships is essential for designing the Prisma schema, enforcing referential integrity, and generating the Entity Relationship Diagram (ERD).

The database follows a normalized relational model using PostgreSQL with foreign key constraints.

---

# Relationship Types

SupportOS uses the following relationship types:

- One-to-One (1:1)
- One-to-Many (1:N)
- Many-to-Many (N:N)

---

# Organization Relationships

The Organization entity is the root of the multi-tenant architecture.

```
Organization
    │
    ├── Users
    ├── Roles
    ├── Tickets
    ├── Knowledge Documents
    ├── Invitations
    ├── Audit Logs
    └── AI Interactions
```

### Relationships

| Parent | Child | Type |
|---------|-------|------|
| Organization | User | One-to-Many |
| Organization | Role | One-to-Many |
| Organization | Ticket | One-to-Many |
| Organization | KnowledgeDocument | One-to-Many |
| Organization | Invitation | One-to-Many |
| Organization | AuditLog | One-to-Many |

---

# User Relationships

Each user belongs to one organization.

A user can:

- Create tickets
- Send messages
- Receive notifications
- Upload documents
- Create audit events

```
Organization

↓

Users

├── Sessions

├── Tickets

├── Messages

├── Notifications

└── Audit Logs
```

### Relationships

| Parent | Child | Type |
|---------|-------|------|
| User | Session | One-to-Many |
| User | Ticket | One-to-Many |
| User | Message | One-to-Many |
| User | Notification | One-to-Many |
| User | AuditLog | One-to-Many |

---

# Role Relationships

Each role belongs to one organization.

One role can be assigned to many users.

```
Role

↓

Users
```

Relationship:

| Parent | Child | Type |
|---------|-------|------|
| Role | User | One-to-Many |

---

# Invitation Relationships

Each invitation belongs to:

- Organization
- Role

One invitation creates one user after acceptance.

```
Organization

↓

Invitation

↓

User
```

---

# Ticket Relationships

Tickets are central to SupportOS.

Each ticket belongs to:

- Organization
- Customer
- Assigned Agent (optional)

Each ticket contains:

- Messages
- Attachments
- AI interactions

```
Organization

↓

Ticket

├── Messages

├── AI Interactions

└── Attachments
```

### Relationships

| Parent | Child | Type |
|---------|-------|------|
| Ticket | Message | One-to-Many |
| Ticket | AIInteraction | One-to-Many |

---

# Message Relationships

Messages belong to:

- Ticket
- Sender

Messages may contain multiple attachments.

```
Message

↓

Attachments
```

Relationship:

| Parent | Child | Type |
|---------|-------|------|
| Message | Attachment | One-to-Many |

---

# Knowledge Base Relationships

Each organization owns multiple knowledge documents.

Each document contains multiple chunks.

```
Organization

↓

KnowledgeDocument

↓

KnowledgeChunk
```

Relationship:

| Parent | Child | Type |
|---------|-------|------|
| KnowledgeDocument | KnowledgeChunk | One-to-Many |

---

# AI Relationships

AI interactions belong to:

- Organization
- Ticket

AI uses:

- Knowledge Chunks

AI does not own the knowledge data.

```
Knowledge

↓

Retrieved Context

↓

AI Interaction
```

---

# Notification Relationships

Notifications belong to users.

```
User

↓

Notifications
```

Relationship:

| Parent | Child | Type |
|---------|-------|------|
| User | Notification | One-to-Many |

---

# Audit Log Relationships

Audit logs belong to:

- Organization
- User

Audit logs may reference any entity.

```
AuditLog

↓

Entity Type

↓

Entity ID
```

Supported entity types include:

- Ticket
- User
- Organization
- KnowledgeDocument
- Message
- Role

---

# One-to-One Relationships

Current one-to-one relationships include:

| Entity A | Entity B |
|-----------|----------|
| User | Active Session (logical) |

Additional one-to-one relationships may be introduced in future releases.

---

# One-to-Many Relationships

| Parent | Child |
|---------|-------|
| Organization | Users |
| Organization | Roles |
| Organization | Tickets |
| Organization | Invitations |
| Organization | Knowledge Documents |
| User | Sessions |
| User | Messages |
| User | Notifications |
| Ticket | Messages |
| Ticket | AI Interactions |
| Message | Attachments |
| KnowledgeDocument | KnowledgeChunks |

---

# Many-to-Many Relationships

The initial MVP minimizes many-to-many relationships to reduce complexity.

Future examples include:

## Users ↔ Teams

```
Users

↓

UserTeams

↓

Teams
```

---

## Roles ↔ Permissions

```
Roles

↓

RolePermissions

↓

Permissions
```

This allows flexible permission management without changing application logic.

---

# Cascade Rules

Recommended cascade behavior:

| Parent | Child | Action |
|---------|-------|--------|
| Organization | Users | Restrict Delete |
| Ticket | Messages | Cascade |
| Message | Attachments | Cascade |
| KnowledgeDocument | KnowledgeChunks | Cascade |
| User | Sessions | Cascade |
| User | Notifications | Cascade |

Important business entities such as Organizations and Users should generally use soft deletes instead of hard deletes.

---

# Referential Integrity

All foreign keys enforce referential integrity.

Rules include:

- A ticket cannot exist without an organization.
- A message cannot exist without a ticket.
- A knowledge chunk cannot exist without a document.
- A notification cannot exist without a user.
- An AI interaction cannot exist without a ticket.

---

# Summary

SupportOS uses a normalized relational database structure with clearly defined one-to-one, one-to-many, and future many-to-many relationships. Every relationship is designed to maintain data integrity, support tenant isolation, and simplify future Prisma schema implementation.