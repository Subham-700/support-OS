# Entity Relationship Diagram (ERD)

## Overview

This document provides the Entity Relationship Diagram (ERD) for the SupportOS database. The diagram visualizes the relationships between all major entities and serves as a reference for database implementation using Prisma ORM and PostgreSQL.

---

# Entity Relationship Diagram

```mermaid
erDiagram

    Organization ||--o{ User : has
    Organization ||--o{ Role : defines
    Organization ||--o{ Ticket : owns
    Organization ||--o{ Invitation : sends
    Organization ||--o{ KnowledgeDocument : stores
    Organization ||--o{ AuditLog : records
    Organization ||--o{ AIInteraction : owns

    Role ||--o{ User : assigned_to

    User ||--o{ Session : creates
    User ||--o{ Ticket : creates
    User ||--o{ Message : writes
    User ||--o{ Notification : receives
    User ||--o{ AuditLog : performs

    Ticket ||--o{ Message : contains
    Ticket ||--o{ AIInteraction : analyzed_by

    Message ||--o{ Attachment : contains

    KnowledgeDocument ||--o{ KnowledgeChunk : split_into

    Organization {
        UUID id PK
        string name
        string slug
        string subscriptionPlan
        string status
        datetime createdAt
        datetime updatedAt
    }

    User {
        UUID id PK
        UUID organizationId FK
        UUID roleId FK
        string firstName
        string lastName
        string email
        string passwordHash
        boolean isEmailVerified
        string status
        datetime createdAt
        datetime updatedAt
    }

    Role {
        UUID id PK
        UUID organizationId FK
        string name
        string description
    }

    Invitation {
        UUID id PK
        UUID organizationId FK
        UUID roleId FK
        string email
        string token
        datetime expiresAt
    }

    Session {
        UUID id PK
        UUID userId FK
        string refreshToken
        string device
        string ipAddress
        datetime expiresAt
    }

    Ticket {
        UUID id PK
        UUID organizationId FK
        UUID customerId FK
        UUID assignedAgentId FK
        string title
        string priority
        string status
        datetime createdAt
    }

    Message {
        UUID id PK
        UUID ticketId FK
        UUID senderId FK
        text content
        string messageType
        datetime createdAt
    }

    Attachment {
        UUID id PK
        UUID messageId FK
        string fileName
        string storageKey
        string mimeType
        int size
    }

    KnowledgeDocument {
        UUID id PK
        UUID organizationId FK
        string title
        string source
        string status
    }

    KnowledgeChunk {
        UUID id PK
        UUID documentId FK
        text content
        int chunkIndex
    }

    AIInteraction {
        UUID id PK
        UUID organizationId FK
        UUID ticketId FK
        text prompt
        text response
        float confidence
        int tokenUsage
    }

    Notification {
        UUID id PK
        UUID userId FK
        string type
        string title
        boolean isRead
        datetime createdAt
    }

    AuditLog {
        UUID id PK
        UUID organizationId FK
        UUID userId FK
        string action
        string entityType
        UUID entityId
        datetime createdAt
    }
```

---

# Relationship Summary

| Parent Entity | Child Entity | Relationship |
|---------------|--------------|--------------|
| Organization | User | One-to-Many |
| Organization | Role | One-to-Many |
| Organization | Ticket | One-to-Many |
| Organization | Invitation | One-to-Many |
| Organization | KnowledgeDocument | One-to-Many |
| Organization | AuditLog | One-to-Many |
| Organization | AIInteraction | One-to-Many |
| Role | User | One-to-Many |
| User | Session | One-to-Many |
| User | Ticket | One-to-Many |
| User | Message | One-to-Many |
| User | Notification | One-to-Many |
| User | AuditLog | One-to-Many |
| Ticket | Message | One-to-Many |
| Ticket | AIInteraction | One-to-Many |
| Message | Attachment | One-to-Many |
| KnowledgeDocument | KnowledgeChunk | One-to-Many |

---

# Design Principles

The ERD follows these principles:

- UUID primary keys for all entities.
- Foreign key constraints ensure referential integrity.
- Organization is the root entity for tenant isolation.
- Business entities are normalized to reduce redundancy.
- Soft deletes are preferred for critical business data.
- Audit logging provides traceability across the system.

---

# Notes

- Every tenant-specific entity references an `organizationId`.
- All timestamps should use UTC.
- Relationships shown here map directly to Prisma `@relation` definitions.
- Future entities (Teams, Permissions, Workflows, Integrations) can be added without redesigning the existing schema.

---

# Summary

This Entity Relationship Diagram provides the structural foundation of the SupportOS database. It documents entity ownership, relationships, and key attributes, ensuring consistency between the architecture documentation and the future Prisma schema implementation.