# Database Entities

## Overview

This document defines the core entities of the SupportOS database. Each entity represents a business object within the system and is designed to support a scalable, secure, and multi-tenant architecture.

---

# Entity Overview

SupportOS consists of the following primary entities:

- Organization
- User
- Role
- Invitation
- Session
- Ticket
- Message
- Attachment
- KnowledgeDocument
- KnowledgeChunk
- AIInteraction
- Notification
- AuditLog

---

# Organization

## Purpose

Represents a company or workspace using SupportOS.

## Attributes

| Field | Type | Description |
|--------|------|-------------|
| id | UUID | Primary Key |
| name | String | Organization name |
| slug | String | Unique URL identifier |
| logoUrl | String? | Organization logo |
| subscriptionPlan | Enum | Current plan |
| status | Enum | Active, Suspended |
| createdAt | DateTime | Created timestamp |
| updatedAt | DateTime | Updated timestamp |

---

# User

## Purpose

Represents an authenticated member of an organization.

## Attributes

| Field | Type | Description |
|--------|------|-------------|
| id | UUID | Primary Key |
| organizationId | UUID | Organization |
| firstName | String | First name |
| lastName | String | Last name |
| email | String | Unique email |
| passwordHash | String | Hashed password |
| avatarUrl | String? | Profile picture |
| isEmailVerified | Boolean | Verification status |
| status | Enum | Active, Suspended |
| createdAt | DateTime | Created timestamp |
| updatedAt | DateTime | Updated timestamp |

---

# Role

## Purpose

Defines user permissions.

## Default Roles

- Owner
- Administrator
- Support Manager
- Support Agent
- Customer

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| organizationId | UUID |
| name | String |
| description | String |

---

# Invitation

## Purpose

Stores pending organization invitations.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| organizationId | UUID |
| email | String |
| roleId | UUID |
| token | String |
| expiresAt | DateTime |
| acceptedAt | DateTime? |

---

# Session

## Purpose

Represents an authenticated user session.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| userId | UUID |
| refreshToken | String |
| device | String |
| ipAddress | String |
| lastActiveAt | DateTime |
| expiresAt | DateTime |

---

# Ticket

## Purpose

Represents a customer support request.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| organizationId | UUID |
| customerId | UUID |
| assignedAgentId | UUID? |
| title | String |
| description | String |
| priority | Enum |
| status | Enum |
| category | String |
| createdAt | DateTime |
| updatedAt | DateTime |

---

# Message

## Purpose

Represents messages exchanged within a ticket.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| ticketId | UUID |
| senderId | UUID |
| content | Text |
| messageType | Enum |
| createdAt | DateTime |

---

# Attachment

## Purpose

Stores uploaded files associated with messages or tickets.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| messageId | UUID |
| fileName | String |
| mimeType | String |
| size | Integer |
| storageKey | String |
| uploadedAt | DateTime |

---

# KnowledgeDocument

## Purpose

Represents a document uploaded to the knowledge base.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| organizationId | UUID |
| title | String |
| source | String |
| status | Enum |
| uploadedBy | UUID |
| createdAt | DateTime |

---

# KnowledgeChunk

## Purpose

Stores processed chunks of a knowledge document for semantic search.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| documentId | UUID |
| content | Text |
| embeddingId | UUID |
| chunkIndex | Integer |

---

# AIInteraction

## Purpose

Stores AI-generated outputs and metadata.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| ticketId | UUID |
| prompt | Text |
| response | Text |
| confidence | Decimal |
| tokenUsage | Integer |
| createdAt | DateTime |

---

# Notification

## Purpose

Stores notifications sent to users.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| userId | UUID |
| type | Enum |
| title | String |
| body | Text |
| isRead | Boolean |
| createdAt | DateTime |

---

# AuditLog

## Purpose

Tracks important system events.

## Attributes

| Field | Type |
|--------|------|
| id | UUID |
| organizationId | UUID |
| userId | UUID |
| action | String |
| entityType | String |
| entityId | UUID |
| metadata | JSON |
| createdAt | DateTime |

---

# Shared Fields

Most entities include the following fields:

| Field | Purpose |
|--------|---------|
| id | Unique identifier |
| createdAt | Creation timestamp |
| updatedAt | Last update timestamp |
| deletedAt | Soft delete timestamp (optional) |

---

# Entity Design Principles

SupportOS entities follow these principles:

- Every entity belongs to an organization where applicable.
- UUIDs are used as primary keys.
- Soft deletes preserve historical data.
- Audit fields provide traceability.
- Relationships are enforced with foreign keys.
- Business rules are enforced at the application layer.

---

# Summary

The entities defined in this document represent the core business objects of SupportOS. Together they provide the foundation for ticket management, user management, AI-assisted support, knowledge retrieval, notifications, and auditing. These entities will be translated directly into Prisma models during the schema design phase.