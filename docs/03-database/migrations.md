# Database Migration Strategy

## Overview

This document defines the migration strategy for SupportOS. Database migrations provide a controlled and versioned approach for evolving the database schema while preserving existing data and ensuring compatibility across development, staging, and production environments.

SupportOS uses **Prisma Migrate** as the primary migration tool.

---

# Objectives

The migration strategy aims to:

- Version all database schema changes
- Ensure consistent environments
- Prevent schema drift
- Enable automated deployments
- Support rollback planning
- Maintain data integrity

---

# Migration Tool

SupportOS uses:

| Component | Technology |
|-----------|------------|
| ORM | Prisma ORM |
| Migration Tool | Prisma Migrate |
| Database | PostgreSQL |

---

# Migration Workflow

Every schema change follows the same workflow.

```
Update Prisma Schema

↓

Generate Migration

↓

Review Generated SQL

↓

Apply Migration Locally

↓

Run Tests

↓

Commit Migration

↓

Deploy

↓

Apply Migration in Production
```

---

# Development Workflow

### Step 1

Update the Prisma schema.

Example:

```prisma
model Ticket {
    id          String @id @default(uuid())
    title       String
    description String
}
```

---

### Step 2

Generate a migration.

```bash
pnpm prisma migrate dev --name add-ticket-table
```

---

### Step 3

Review the generated SQL migration.

Verify:

- Tables
- Columns
- Constraints
- Indexes
- Foreign keys

---

### Step 4

Run application tests.

Verify:

- Database queries
- API endpoints
- Relations
- Seed scripts

---

### Step 5

Commit both:

- schema.prisma
- migration files

---

# Production Workflow

Production deployments use:

```bash
pnpm prisma migrate deploy
```

Characteristics:

- Applies pending migrations only
- No interactive prompts
- Safe for CI/CD
- Does not generate new migrations

---

# Migration Naming

Migration names should describe the change.

Examples:

```
create-users

create-ticket-table

add-ticket-priority

add-notification-table

add-ai-interaction

add-ticket-status-index
```

Avoid generic names such as:

```
update

changes

fix

migration1
```

---

# Migration Versioning

Prisma stores migration history in:

```
_prisma_migrations
```

This table records:

- Migration name
- Checksum
- Applied timestamp
- Execution status

---

# Schema Changes

Typical schema changes include:

- Create table
- Drop table
- Rename column
- Add column
- Remove column
- Add relation
- Add index
- Add constraint

All changes should be tracked through migrations.

---

# Data Migrations

Some schema changes require updating existing data.

Examples:

- Populate default values
- Convert legacy data
- Split columns
- Merge columns
- Normalize records

These operations should be scripted and tested before production deployment.

---

# Rollback Strategy

Prisma does not provide automatic rollback for applied migrations.

Instead:

1. Restore database backup if necessary.
2. Create a corrective migration.
3. Apply the new migration.

Rollback plans should be prepared for high-risk schema changes.

---

# Backup Policy

Before applying production migrations:

- Create a database backup.
- Verify backup integrity.
- Ensure restore procedures are tested.

Recommended:

- Daily backups
- Point-in-time recovery
- Weekly restore verification

---

# Deployment Pipeline

```
Developer

↓

Git Commit

↓

GitHub Actions

↓

Install Dependencies

↓

Run Tests

↓

Build Application

↓

Run Prisma Migrate Deploy

↓

Start Application
```

Migrations should complete successfully before the application begins serving requests.

---

# Seed Data

Development environments may require seed data.

Example:

```bash
pnpm prisma db seed
```

Seed data may include:

- Default roles
- Admin account
- Sample organization
- Sample tickets
- Categories

Production seed scripts should be limited to essential system data.

---

# Best Practices

- Keep migrations small and focused.
- Apply one logical change per migration.
- Review generated SQL before merging.
- Never edit an applied migration.
- Test migrations in staging before production.
- Avoid destructive changes without backups.
- Use transactions where supported.

---

# Common Migration Types

| Migration | Example |
|-----------|----------|
| Create Table | User |
| Add Column | phoneNumber |
| Remove Column | legacyField |
| Add Relation | Ticket → User |
| Add Index | organizationId |
| Add Enum | TicketPriority |
| Rename Column | status → ticketStatus |

---

# Schema Drift Prevention

Schema drift occurs when the database differs from the Prisma schema.

To prevent drift:

- Never modify production schema manually.
- Always use Prisma Migrate.
- Review migration history regularly.
- Keep all environments synchronized.

---

# Monitoring

Monitor migrations for:

- Execution time
- Errors
- Failed constraints
- Lock duration
- Database availability

Migration logs should be retained for troubleshooting and auditing.

---

# Future Enhancements

Future improvements include:

- Zero-downtime migrations
- Blue-Green deployments
- Online schema changes
- Automated migration validation
- Database compatibility testing

---

# Summary

SupportOS uses Prisma Migrate to manage all database schema changes in a consistent, version-controlled manner. By following a structured migration workflow, reviewing generated SQL, testing changes before deployment, and maintaining backups, the platform ensures safe and reliable database evolution throughout its lifecycle.