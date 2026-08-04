# Pagination, Filtering & Sorting

## Overview

SupportOS uses a consistent approach for pagination, filtering, sorting, and searching across all collection endpoints. This standard improves API usability, performance, and predictability.

All endpoints returning collections should implement these conventions.

---

# Pagination

Pagination is supported using query parameters.

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| page | Integer | 1 | Current page number |
| limit | Integer | 20 | Number of records per page |

Example:

```
GET /tickets?page=1&limit=20
```

---

# Pagination Response

Every paginated response includes metadata.

Example:

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "title": "Unable to login"
    }
  ],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 143,
    "totalPages": 8,
    "hasNextPage": true,
    "hasPreviousPage": false
  }
}
```

---

# Pagination Rules

- Default page is **1**
- Default limit is **20**
- Maximum limit is **100**
- Values less than 1 return validation errors
- Empty pages return an empty array

Example:

```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 15,
    "limit": 20,
    "total": 143,
    "totalPages": 8,
    "hasNextPage": false,
    "hasPreviousPage": true
  }
}
```

---

# Filtering

Filtering allows clients to retrieve only relevant resources.

Example:

```
GET /tickets?status=OPEN
```

Multiple filters:

```
GET /tickets?status=OPEN&priority=HIGH
```

Example:

```
GET /users?role=SUPPORT_AGENT
```

Example:

```
GET /knowledge/documents?category=HR
```

---

# Common Filter Parameters

Examples include:

```
status

priority

role

category

type

assignedAgentId

customerId

createdBy

isRead
```

Only supported filters should be processed.

Unknown filters should return:

```
400 Bad Request
```

---

# Searching

Search uses the `search` query parameter.

Example:

```
GET /tickets?search=password
```

Example:

```
GET /users?search=john
```

Search should perform case-insensitive matching where applicable.

Typical searchable fields:

Tickets

- Title
- Description

Users

- First name
- Last name
- Email

Knowledge

- Title
- Content
- Tags

Organizations

- Name

---

# Sorting

Sorting is controlled using:

```
sortBy

order
```

Example:

```
GET /tickets?sortBy=createdAt&order=desc
```

Ascending:

```
GET /users?sortBy=firstName&order=asc
```

---

# Sort Order

Allowed values:

```
asc

desc
```

Invalid values should return:

```
422 Unprocessable Entity
```

---

# Default Sorting

Unless otherwise specified:

```
createdAt DESC
```

Newest records appear first.

---

# Combining Query Parameters

All query options may be combined.

Example:

```
GET /tickets?page=2&limit=25&status=OPEN&priority=HIGH&search=login&sortBy=updatedAt&order=desc
```

---

# Date Filtering

Example:

```
GET /tickets?createdAfter=2026-08-01
```

Example:

```
GET /tickets?createdBefore=2026-08-31
```

Combined:

```
GET /tickets?createdAfter=2026-08-01&createdBefore=2026-08-31
```

---

# Response Metadata

Every paginated endpoint returns:

| Field | Description |
|--------|-------------|
| page | Current page |
| limit | Records per page |
| total | Total matching records |
| totalPages | Total pages |
| hasNextPage | Whether another page exists |
| hasPreviousPage | Whether a previous page exists |

---

# Performance Guidelines

- Use indexed database columns for filtering.
- Limit maximum page size to 100 records.
- Apply filters before pagination.
- Avoid returning unnecessary fields.
- Use database-level sorting whenever possible.

---

# Validation Rules

Page

- Integer
- Minimum 1

Limit

- Integer
- Minimum 1
- Maximum 100

Order

Allowed values:

```
asc

desc
```

Sort Field

Must be one of the supported sortable fields for the resource.

---

# Error Responses

Invalid page:

```json
{
  "success": false,
  "statusCode": 422,
  "message": "Page must be greater than zero."
}
```

Invalid sort field:

```json
{
  "success": false,
  "statusCode": 422,
  "message": "Unsupported sort field."
}
```

---

# Best Practices

- Always paginate collection endpoints.
- Apply filtering before pagination.
- Return metadata with every paginated response.
- Use indexed fields for search and sorting.
- Keep default page sizes reasonable.
- Document supported filters for each endpoint.

---

# Summary

SupportOS standardizes pagination, filtering, searching, and sorting across all collection endpoints. This consistency improves performance, simplifies frontend integration, and provides a predictable API experience for developers while supporting scalable data access patterns.