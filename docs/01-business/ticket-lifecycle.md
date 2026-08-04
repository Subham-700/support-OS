# Ticket Lifecycle

## Overview

This document defines the lifecycle of a support ticket in SupportOS, including all possible states, transitions, and business rules.

---

# Ticket States

## 1. New

### Description
A ticket has been created but has not yet been reviewed.

### Entered When
- Customer creates a ticket
- External integration creates a ticket

### Allowed Actions
- AI analyzes the request
- Assign ticket
- Merge duplicate tickets

---

## 2. Open

### Description
The ticket has been accepted and is waiting to be processed.

### Allowed Actions
- Assign an agent
- Respond to the customer
- Change priority

---

## 3. In Progress

### Description
A support agent is actively working on the issue.

### Allowed Actions
- Reply to customer
- Add internal notes
- Escalate
- Request more information

---

## 4. Waiting for Customer

### Description
The support team needs additional information before continuing.

### Allowed Actions
- Customer replies
- Send reminder
- Close after timeout (configurable)

---

## 5. Resolved

### Description
The issue has been solved and is awaiting customer confirmation.

### Allowed Actions
- Customer confirms resolution
- Reopen ticket
- Automatically close after a configurable period

---

## 6. Closed

### Description
The ticket is completed.

### Allowed Actions
- View history
- Reopen (authorized users only)

---

# Lifecycle Diagram

Customer Creates Ticket
        │
        ▼
      New
        │
        ▼
      Open
        │
        ▼
   In Progress
        │
   ┌────┴───────────┐
   ▼                ▼
Waiting for      Resolved
Customer            │
   │                ▼
   └────────────► Closed

---

# State Transitions

| From | To | Trigger |
|------|----|----------|
| New | Open | Ticket accepted |
| Open | In Progress | Agent starts work |
| In Progress | Waiting for Customer | More information required |
| Waiting for Customer | In Progress | Customer replies |
| In Progress | Resolved | Issue fixed |
| Resolved | Closed | Customer confirms or timeout |
| Closed | Open | Authorized user reopens |

---

# Business Rules

- Every ticket starts in the **New** state.
- A ticket can only have one current state.
- Closed tickets become read-only.
- Only authorized users can reopen a closed ticket.
- All state changes must be recorded in the audit log.
- Customers cannot directly change ticket status.
- AI cannot close tickets automatically unless explicitly configured.

---

# Future Enhancements

- Custom workflows per organization
- SLA timers
- Automatic escalation
- Priority-based routing
- AI-generated status recommendations