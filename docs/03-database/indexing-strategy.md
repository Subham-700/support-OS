# Database Indexing Strategy

## Overview

This document defines the indexing strategy for the SupportOS database. Proper indexing improves query performance, reduces response times, and enables the application to scale efficiently while maintaining data integrity.

The strategy is designed for PostgreSQL and aligns with Prisma ORM's indexing capabilities.

---

# Objectives

The indexing strategy aims to:

- Improve query performance
- Reduce database latency
- Optimize JOIN operations
- Speed up sorting and filtering
- Support efficient pagination
- Improve AI knowledge retrieval
- Maintain scalability

---

# Index Types

SupportOS uses the following index types:

- Primary Index
- Unique Index
- Single-Column Index
- Composite Index
- Full-Text Search Index (Future)
- Vector Index (pgvector)

---

# Primary Keys

Every table uses a UUID primary key.

Example:

```prisma
id String @id @default(uuid())
```

PostgreSQL automatically creates a primary key index.

---

# Unique Indexes

Unique indexes prevent duplicate values.

Example:

```prisma
email String @unique
```

Used for:

- User email
- Organization slug
- Invitation token

---

# Single-Column Indexes

Single-column indexes improve filtering on frequently queried fields.

## Organization

```prisma
@@index([slug])
```

Queries:

- Find organization by slug

---

## User

```prisma
@@index([organizationId])

@@index([email])

@@index([status])
```

Queries:

- Users in an organization
- Login
- Active users

---

## Ticket

```prisma
@@index([organizationId])

@@index([status])

@@index([priority])

@@index([assignedAgentId])

@@index([createdAt])
```

Queries:

- Organization tickets
- Ticket queues
- Assigned tickets
- Recent tickets

---

## Message

```prisma
@@index([ticketId])

@@index([senderId])

@@index([createdAt])
```

Queries:

- Ticket conversations
- User messages

---

## KnowledgeDocument

```prisma
@@index([organizationId])

@@index([status])
```

Queries:

- Organization knowledge
- Published documents

---

## KnowledgeChunk

```prisma
@@index([documentId])
```

Queries:

- Retrieve document chunks

---

## AIInteraction

```prisma
@@index([ticketId])

@@index([organizationId])
```

Queries:

- AI history
- AI analytics

---

## Notification

```prisma
@@index([userId])

@@index([isRead])
```

Queries:

- User notifications
- Unread notifications

---

## AuditLog

```prisma
@@index([organizationId])

@@index([userId])

@@index([createdAt])
```

Queries:

- Audit history
- User activity
- Security reports

---

# Composite Indexes

Composite indexes optimize queries filtering on multiple columns.

## Tickets

```prisma
@@index([organizationId, status])
```

Used for:

- Open tickets in an organization

---

```prisma
@@index([organizationId, priority])
```

Used for:

- High-priority tickets

---

```prisma
@@index([organizationId, assignedAgentId])
```

Used for:

- Tickets assigned to an agent

---

```prisma
@@index([organizationId, createdAt])
```

Used for:

- Recent organization tickets

---

## Users

```prisma
@@index([organizationId, status])
```

Used for:

- Active users in an organization

---

## Knowledge Documents

```prisma
@@index([organizationId, status])
```

Used for:

- Published documents

---

# Unique Composite Indexes

Composite unique constraints prevent duplicate records.

Example:

```prisma
@@unique([organizationId, slug])
```

Ensures slugs are unique within an organization.

Additional examples:

```prisma
@@unique([organizationId, email])
```

```prisma
@@unique([organizationId, name])
```

---

# Sorting Optimization

Indexes improve ORDER BY performance.

Example:

```sql
ORDER BY createdAt DESC
```

Indexed field:

```prisma
@@index([createdAt])
```

---

# Pagination Optimization

SupportOS uses cursor-based pagination.

Preferred ordering:

```
createdAt

↓

id
```

Recommended composite index:

```prisma
@@index([createdAt, id])
```

Benefits:

- Stable pagination
- Fast page retrieval

---

# Full-Text Search

Future versions may use PostgreSQL Full-Text Search.

Candidate fields:

- Ticket title
- Ticket description
- Message content
- Knowledge document title
- Knowledge content

Benefits:

- Fast keyword search
- Ranking results
- Language support

---

# Vector Search

SupportOS uses pgvector for semantic search.

Indexed entity:

KnowledgeChunk

Stored:

- Embedding vector
- Metadata
- Organization ID

Queries:

- Similarity search
- AI knowledge retrieval

---

# Query Optimization Guidelines

- Always filter by `organizationId`.
- Avoid `SELECT *`; request only required columns.
- Use indexes for frequently filtered fields.
- Limit JOIN depth where possible.
- Prefer cursor-based pagination over OFFSET for large datasets.

---

# Monitoring Index Performance

Regularly monitor:

- Slow queries
- Index usage
- Sequential scans
- Missing indexes
- Query execution plans

Use PostgreSQL tools such as:

- `EXPLAIN`
- `EXPLAIN ANALYZE`
- `pg_stat_statements`

---

# Index Maintenance

Best practices:

- Review indexes periodically.
- Remove unused indexes.
- Rebuild fragmented indexes when necessary.
- Monitor storage overhead.
- Test performance after schema changes.

---

# Future Enhancements

Future optimization strategies include:

- Partial indexes
- Covering indexes
- Table partitioning
- Read replicas
- Materialized views
- Query result caching

---

# Summary

The indexing strategy for SupportOS is designed to support fast lookups, efficient filtering, scalable pagination, and AI-powered semantic search. By combining primary, unique, single-column, composite, and vector indexes, the database can efficiently handle growing workloads while maintaining excellent query performance.