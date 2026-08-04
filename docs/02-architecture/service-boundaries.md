# Service Boundaries

## Overview

This document defines the logical service boundaries within SupportOS. Although the MVP will be implemented as a modular monolith using NestJS, the system is designed so that each module can be extracted into an independent microservice in the future.

Each service has a single responsibility and communicates with other services through well-defined interfaces.

---

# Architecture Philosophy

SupportOS follows a **Modular Monolith** architecture.

Benefits:

- Easier development
- Simpler deployment
- Shared codebase
- Better debugging
- Lower infrastructure cost

As the platform grows, modules can evolve into microservices without major redesign.

---

# Service Overview

The platform consists of the following logical services:

- Authentication Service
- Organization Service
- User Service
- Ticket Service
- Conversation Service
- Knowledge Base Service
- AI Service
- Notification Service
- Analytics Service
- Audit Service
- File Storage Service

---

# Authentication Service

## Responsibilities

- User login
- User registration
- Password management
- JWT generation
- Refresh tokens
- Email verification
- Password reset

## Owns

- Credentials
- Sessions
- Tokens

## Does NOT Own

- User profile
- Organization information
- Tickets

---

# Organization Service

## Responsibilities

- Organization creation
- Organization settings
- Subscription information
- Tenant isolation
- Workspace configuration

## Owns

- Organization
- Billing settings
- Organization preferences

---

# User Service

## Responsibilities

- User profiles
- User invitations
- Role assignment
- Team management

## Owns

- Users
- Roles
- Memberships

---

# Ticket Service

## Responsibilities

- Ticket creation
- Ticket assignment
- Ticket lifecycle
- Priority management
- Status transitions

## Owns

- Tickets
- Assignments
- Priorities
- Categories

---

# Conversation Service

## Responsibilities

- Customer messages
- Agent replies
- Internal notes
- Attachments
- Conversation history

## Owns

- Messages
- Conversations
- Attachments

---

# Knowledge Base Service

## Responsibilities

- Upload documents
- Update documents
- Delete documents
- Search documents
- Document processing

## Owns

- Knowledge documents
- Categories
- Metadata

---

# AI Service

## Responsibilities

- Retrieve relevant knowledge
- Generate AI responses
- Summarize conversations
- Classify tickets
- Suggest replies
- Confidence scoring

## Owns

- Embeddings
- AI prompts
- AI summaries
- AI metadata

---

# Notification Service

## Responsibilities

- Email notifications
- In-app notifications
- Future SMS notifications
- Future WhatsApp notifications

## Owns

- Notification templates
- Notification history

---

# Analytics Service

## Responsibilities

- Dashboard metrics
- Ticket statistics
- Response times
- Agent performance
- AI performance

## Owns

- Reports
- Aggregated metrics

---

# Audit Service

## Responsibilities

- Track important system events
- Security auditing
- Compliance logging

## Records

- Login events
- Role changes
- Ticket assignments
- Knowledge uploads
- AI actions

Audit records are immutable.

---

# File Storage Service

## Responsibilities

- Upload files
- Store attachments
- Manage document versions
- Generate secure download URLs

## Owns

- Uploaded files
- File metadata

Actual files are stored in object storage (e.g., Amazon S3).

---

# Service Communication

```
Frontend
    │
    ▼
Backend API
    │
    ├── Authentication
    ├── Organization
    ├── Users
    ├── Tickets
    ├── Conversations
    ├── Knowledge Base
    ├── AI
    ├── Notifications
    ├── Analytics
    └── Audit
```

All communication occurs through internal module interfaces within the modular monolith.

---

# Future Event-Driven Communication

As the platform evolves, services can publish domain events.

Examples:

TicketCreated

↓

AI Analysis Started

↓

Knowledge Search

↓

AI Response Generated

↓

Notification Sent

Other possible events:

- UserInvited
- UserRegistered
- TicketAssigned
- TicketResolved
- KnowledgeUploaded
- AIReplyGenerated

---

# Dependency Rules

To maintain clean architecture:

- Services should not directly access another service's database tables.
- Services communicate through public interfaces.
- Shared utilities belong in the Shared module.
- Circular dependencies are not allowed.

---

# Scalability

The following services are expected to scale independently in the future:

- AI Service
- Notification Service
- Analytics Service
- File Storage Service

These services perform computationally intensive or asynchronous tasks and are good candidates for extraction into standalone microservices.

---

# Summary

SupportOS is organized into clearly defined service boundaries following a modular monolith architecture. Each service owns a specific business capability, reducing coupling and improving maintainability. This design supports rapid development today while providing a clear migration path toward microservices if future scale requires it.