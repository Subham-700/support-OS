# Monorepo Structure

## Overview

SupportOS follows a monorepo architecture to manage multiple applications and shared packages within a single repository.

A monorepo simplifies dependency management, encourages code reuse, and ensures consistency across the entire platform.

---

# Why a Monorepo?

We chose a monorepo because it provides:

- Shared TypeScript types
- Shared UI components
- Centralized configuration
- Easier dependency management
- Simplified CI/CD pipelines
- Consistent coding standards
- Better developer experience

---

# Repository Structure

```text
support-os/
│
├── apps/
│   ├── api/
│   └── web/
│
├── packages/
│   ├── ui/
│   ├── shared/
│   ├── config/
│   └── types/
│
├── infrastructure/
│
├── docs/
│
├── scripts/
│
├── .github/
│
├── package.json
├── turbo.json
├── pnpm-workspace.yaml
└── README.md
```

---

# Applications

## apps/web

Frontend application built with Next.js.

Responsibilities:

- Authentication UI
- Dashboard
- Ticket Management
- Customer Portal
- Knowledge Base
- Analytics
- Organization Settings

---

## apps/api

Backend application built with NestJS.

Responsibilities:

- Authentication
- User Management
- Organization Management
- Ticket Management
- AI Services
- Notifications
- Analytics
- File Uploads

---

# Shared Packages

## packages/ui

Reusable UI components shared across frontend applications.

Examples:

- Buttons
- Forms
- Tables
- Dialogs
- Layout Components

---

## packages/shared

Shared business logic and utilities.

Examples:

- Helper functions
- Constants
- Validation utilities
- Common services

---

## packages/types

Shared TypeScript types and interfaces.

Examples:

- User
- Ticket
- Organization
- Message
- API Responses

---

## packages/config

Shared project configuration.

Examples:

- ESLint configuration
- Prettier configuration
- TypeScript configuration
- Tailwind configuration

---

# Infrastructure

The infrastructure directory contains deployment and operational resources.

Examples:

- Docker
- Docker Compose
- Kubernetes manifests (future)
- Terraform (future)

---

# Documentation

The docs directory contains all project documentation.

Examples:

- Vision
- Business Analysis
- Architecture
- Database Design
- API Design
- AI Documentation
- Security
- DevOps

---

# Scripts

Contains automation scripts.

Examples:

- Database seeding
- Backup scripts
- Build scripts
- Development utilities

---

# GitHub Configuration

The .github directory contains:

- GitHub Actions
- Pull Request templates
- Issue templates
- CODEOWNERS
- Dependabot configuration

---

# Package Management

SupportOS uses pnpm workspaces for dependency management.

Benefits:

- Faster installs
- Shared dependency store
- Efficient disk usage
- Better monorepo support

---

# Build System

TurboRepo is used to:

- Cache builds
- Cache tests
- Parallelize tasks
- Improve CI/CD performance

---

# Development Workflow

1. Clone repository
2. Install dependencies
3. Start development services
4. Run frontend
5. Run backend
6. Execute tests
7. Commit changes
8. Push to GitHub

---

# Benefits

The monorepo architecture provides:

- Better scalability
- Code reuse
- Consistent tooling
- Faster builds
- Easier maintenance
- Simplified deployments
- Improved developer experience

---

# Future Expansion

The repository structure allows adding new applications such as:

- Mobile App
- Admin Portal
- Public Documentation Site
- AI Worker Service
- Notification Worker
- Analytics Service

without major restructuring.

---

# Summary

The monorepo architecture enables SupportOS to scale efficiently while maintaining a clean, modular, and maintainable codebase. Shared packages reduce duplication, and centralized tooling improves the overall developer experience.