# Permissions Matrix

## Overview

This document defines the permissions for each role in SupportOS. It serves as the foundation for Role-Based Access Control (RBAC).

---

## Roles

- Organization Owner
- Administrator
- Support Manager
- Support Agent
- Customer
- AI Assistant

---

# Organization Management

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| Create Organization | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Update Organization | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Delete Organization | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| View Organization | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

---

# User Management

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| Invite Users | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Remove Users | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Assign Roles | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| View Users | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

---

# Ticket Management

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| Create Ticket | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| View Tickets | ✅ | ✅ | ✅ | Assigned | Own | ❌ |
| Assign Ticket | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Reply to Ticket | ✅ | ✅ | ✅ | ✅ | ✅ | Suggest |
| Change Status | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Close Ticket | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Reopen Ticket | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

---

# Knowledge Base

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| Upload Documents | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Update Documents | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Delete Documents | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Search Knowledge | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |

---

# Analytics

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| View Dashboard | ✅ | ✅ | ✅ | Limited | ❌ | ❌ |
| Export Reports | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

---

# AI Features

| Permission | Owner | Admin | Manager | Agent | Customer | AI |
|------------|:-----:|:-----:|:-------:|:-----:|:--------:|:--:|
| Generate Replies | ❌ | ❌ | ❌ | Request | ❌ | ✅ |
| Summarize Tickets | ❌ | ❌ | ❌ | Request | ❌ | ✅ |
| Search Knowledge Base | ❌ | ❌ | ❌ | Request | ❌ | ✅ |

---

# Notes

- "Assigned" means only tickets assigned to the agent.
- "Own" means only tickets created by the customer.
- "Suggest" means the AI generates a draft, but a human agent decides whether to send it.
- Organization Owners always have full access within their organization.