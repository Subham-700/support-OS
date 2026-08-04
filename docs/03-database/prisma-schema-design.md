# Prisma Schema Design

## Overview

This document defines the design principles, conventions, and organization of the Prisma schema used by SupportOS. The Prisma schema serves as the source of truth for the application's data model and is responsible for generating the database schema, Prisma Client, and migrations.

SupportOS uses PostgreSQL as its primary database and Prisma ORM for type-safe database access.

---

# Objectives

The Prisma schema is designed to:

- Represent all business entities
- Maintain referential integrity
- Support multi-tenancy
- Enable type-safe queries
- Simplify database migrations
- Provide scalability for future features

---

# Database Provider

SupportOS uses PostgreSQL.

Example datasource:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

---

# Prisma Client

The Prisma Client is generated automatically.

Example generator:

```prisma
generator client {
  provider = "prisma-client-js"
}
```

---

# Schema Organization

Models are organized by business domains.

```
Authentication

├── User
├── Session
├── Invitation
└── Role

Organization

└── Organization

Support

├── Ticket
├── Message
└── Attachment

Knowledge Base

├── KnowledgeDocument
└── KnowledgeChunk

AI

└── AIInteraction

Notifications

└── Notification

Audit

└── AuditLog
```

---

# Model Design Principles

Each Prisma model should:

- Represent one business entity
- Use UUID as the primary key
- Include timestamps
- Use relations instead of duplicate data
- Avoid nullable fields unless required
- Follow consistent naming conventions

---

# Naming Conventions

## Models

Models use PascalCase.

Examples:

```
Organization
User
Ticket
KnowledgeDocument
```

---

## Fields

Fields use camelCase.

Examples:

```
createdAt
updatedAt
organizationId
ticketId
```

---

## Enums

Enums use PascalCase.

Examples:

```
TicketStatus
TicketPriority
UserStatus
```

---

# Common Fields

Most models include:

```prisma
id        String   @id @default(uuid())
createdAt DateTime @default(now())
updatedAt DateTime @updatedAt
deletedAt DateTime?
```

Benefits:

- Consistency
- Auditing
- Soft deletes

---

# Primary Keys

Every model uses UUID.

Example:

```prisma
id String @id @default(uuid())
```

Advantages:

- Globally unique
- Secure
- Distributed-system friendly

---

# Foreign Keys

Relationships are represented using Prisma relations.

Example:

```prisma
organizationId String

organization Organization
    @relation(fields: [organizationId], references: [id])
```

Every relation should explicitly define:

- fields
- references

---

# One-to-Many Relationships

Example:

Organization

↓

Users

Prisma:

```prisma
model Organization {
  id    String @id @default(uuid())
  users User[]
}
```

```prisma
model User {
  organizationId String
  organization Organization
}
```

---

# One-to-One Relationships

Example:

User

↓

Session

Prisma relation:

```prisma
session Session?
```

---

# Many-to-Many Relationships

Future features such as Teams and Permissions will use explicit join tables.

Example:

```
User

↓

UserTeam

↓

Team
```

Explicit join tables provide:

- Better querying
- Additional metadata
- Easier auditing

---

# Enums

The schema defines reusable enums.

Examples:

## TicketStatus

```
OPEN
IN_PROGRESS
WAITING_CUSTOMER
RESOLVED
CLOSED
```

---

## TicketPriority

```
LOW
MEDIUM
HIGH
URGENT
```

---

## UserStatus

```
ACTIVE
INACTIVE
SUSPENDED
```

---

## NotificationType

```
EMAIL
IN_APP
SYSTEM
```

---

# Soft Deletes

Business entities should support soft deletion.

Example:

```prisma
deletedAt DateTime?
```

Instead of deleting records permanently:

- Mark deletedAt
- Exclude deleted records from queries

---

# Index Strategy

Frequently queried fields should be indexed.

Examples:

```prisma
@@index([organizationId])

@@index([email])

@@index([status])

@@index([createdAt])
```

Composite indexes:

```prisma
@@index([organizationId, status])

@@index([organizationId, createdAt])
```

---

# Unique Constraints

Examples:

```prisma
email String @unique
```

Composite uniqueness:

```prisma
@@unique([organizationId, slug])
```

This allows multiple organizations while preventing duplicate slugs within the same tenant.

---

# Multi-Tenant Design

Every tenant-specific model includes:

```prisma
organizationId String
```

Application queries always filter by:

```
organizationId
```

This ensures tenant isolation.

---

# Audit Fields

Critical entities may include:

```prisma
createdBy String?

updatedBy String?
```

These fields support traceability and compliance.

---

# Schema Evolution

Changes should be made through Prisma migrations.

Workflow:

```
Update Schema

↓

Generate Migration

↓

Review SQL

↓

Apply Migration

↓

Deploy
```

Avoid manual database changes outside the migration process.

---

# Best Practices

- Keep models focused on a single responsibility.
- Prefer explicit relations over duplicated fields.
- Use enums for fixed values.
- Add indexes for frequently queried columns.
- Avoid storing derived data when it can be calculated.
- Keep migrations small and reviewable.

---

# Future Enhancements

The schema is designed to support future additions such as:

- Teams
- Permissions
- Workflows
- Integrations
- API Keys
- Webhooks
- Billing
- Feature Flags
- AI Agents

These can be introduced without major restructuring.

---

# Summary

The Prisma schema for SupportOS follows consistent naming conventions, strong relational modeling, and multi-tenant design principles. By using UUID primary keys, explicit relationships, enums, indexes, and Prisma Migrate, the schema provides a robust and maintainable foundation for application development.