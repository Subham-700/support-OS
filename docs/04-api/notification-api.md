# Notification API

## Overview

The Notification API manages in-app and email notifications within SupportOS. It informs users about important events such as ticket updates, assignments, mentions, organization invitations, AI processing, and system announcements.

Notifications are delivered securely and are scoped to the authenticated user's organization.

---

# Base URL

```
/api/v1/notifications
```

---

# Notification Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| GET | / | List notifications | Yes |
| GET | /unread-count | Get unread notification count | Yes |
| PATCH | /{notificationId}/read | Mark notification as read | Yes |
| PATCH | /read-all | Mark all notifications as read | Yes |
| DELETE | /{notificationId} | Delete notification | Yes |
| GET | /preferences | Get notification preferences | Yes |
| PATCH | /preferences | Update notification preferences | Yes |
| POST | /test-email | Send test email notification | Yes (Admin) |

---

# Notification Types

Supported notification types:

```
SYSTEM

TICKET_CREATED

TICKET_ASSIGNED

TICKET_UPDATED

TICKET_CLOSED

NEW_MESSAGE

MENTION

INVITATION

KNOWLEDGE_UPDATED

AI_PROCESSING_COMPLETE
```

---

# List Notifications

## Endpoint

```
GET /notifications
```

Supports:

```
?page=1

&limit=20

&type=TICKET_ASSIGNED

&isRead=false

&sortBy=createdAt

&order=desc
```

---

## Response

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "type": "TICKET_ASSIGNED",
      "title": "Ticket Assigned",
      "message": "You have been assigned ticket SUP-1001.",
      "isRead": false,
      "createdAt": "2026-08-04T12:00:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 18,
    "totalPages": 1
  }
}
```

---

# Get Unread Count

## Endpoint

```
GET /notifications/unread-count
```

## Response

```json
{
  "success": true,
  "data": {
    "count": 7
  }
}
```

---

# Mark Notification as Read

## Endpoint

```
PATCH /notifications/{notificationId}/read
```

Response:

```json
{
  "success": true,
  "message": "Notification marked as read."
}
```

---

# Mark All Notifications as Read

## Endpoint

```
PATCH /notifications/read-all
```

Marks every unread notification for the authenticated user as read.

---

# Delete Notification

## Endpoint

```
DELETE /notifications/{notificationId}
```

Response:

```
204 No Content
```

---

# Get Notification Preferences

## Endpoint

```
GET /notifications/preferences
```

Example response:

```json
{
  "success": true,
  "data": {
    "email": true,
    "inApp": true,
    "ticketAssignments": true,
    "ticketReplies": true,
    "mentions": true,
    "systemAnnouncements": true
  }
}
```

---

# Update Notification Preferences

## Endpoint

```
PATCH /notifications/preferences
```

Example request:

```json
{
  "email": true,
  "inApp": true,
  "ticketAssignments": false,
  "mentions": true
}
```

Only supplied fields are updated.

---

# Send Test Email

## Endpoint

```
POST /notifications/test-email
```

Required Role:

- Owner
- Administrator

Example response:

```json
{
  "success": true,
  "message": "Test email sent successfully."
}
```

---

# Real-Time Notifications

SupportOS supports real-time notification delivery using WebSockets.

Examples:

- Ticket assigned
- New ticket message
- User mention
- AI processing completed
- System announcement

Clients subscribe after successful authentication.

---

# Notification Channels

Supported delivery channels:

- In-app notifications
- Email
- WebSocket (real-time)

Future channels:

- Push notifications
- SMS
- Slack
- Microsoft Teams

---

# Validation Rules

Notification ID

- Must be a valid UUID

Preferences

- Boolean values only

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full access |
| Administrator | Full access |
| Support Manager | Own notifications |
| Support Agent | Own notifications |
| Customer | Own notifications |

---

# Business Rules

- Users can access only their own notifications.
- Read status is stored per user.
- Deleted notifications cannot be recovered.
- Email delivery respects user preferences.
- Real-time notifications are delivered only to connected authenticated clients.

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Notification not found |
| 422 | Validation failed |

---

# Audit Events

The following actions generate audit records:

- Notification delivered
- Notification read
- Notification deleted
- Preferences updated
- Test email sent

---

# Security Considerations

- Notifications are isolated by `organizationId`.
- Users cannot access notifications belonging to others.
- WebSocket connections require authentication.
- Sensitive notification content should be minimized in email previews.
- Email sending should be rate-limited to prevent abuse.

---

# Summary

The Notification API provides a flexible and secure notification system for SupportOS. It supports in-app notifications, email delivery, user preferences, unread tracking, and real-time updates while ensuring tenant isolation, role-based access control, and scalable communication across the platform.