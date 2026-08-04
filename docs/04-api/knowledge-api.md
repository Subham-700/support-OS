# Knowledge Base API

## Overview

The Knowledge Base API manages organizational knowledge used by both human support agents and AI services. It supports document uploads, document management, semantic search, chunk generation, and embedding creation.

Each organization maintains an isolated knowledge base.

---

# Base URL

```
/api/v1/knowledge
```

---

# Knowledge Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| GET | /documents | List documents | Yes |
| POST | /documents | Upload document | Yes |
| GET | /documents/{documentId} | Get document | Yes |
| PATCH | /documents/{documentId} | Update document | Yes |
| DELETE | /documents/{documentId} | Delete document | Yes |
| POST | /documents/{documentId}/process | Process document | Yes |
| GET | /documents/{documentId}/chunks | List document chunks | Yes |
| POST | /search | Semantic search | Yes |
| GET | /statistics | Knowledge statistics | Yes |

---

# Supported Document Types

Supported formats:

- PDF
- DOCX
- TXT
- Markdown (.md)

Maximum file size:

```
50 MB
```

---

# Upload Document

## Endpoint

```
POST /knowledge/documents
```

Content-Type

```
multipart/form-data
```

Additional fields:

```text
title
category
tags
```

Example Response

```json
{
  "success": true,
  "message": "Document uploaded successfully.",
  "data": {
    "id": "uuid",
    "status": "UPLOADED"
  }
}
```

---

# List Documents

## Endpoint

```
GET /knowledge/documents
```

Supports

```
?page=1

&limit=20

&category=HR

&status=READY

&search=password
```

---

# Get Document

## Endpoint

```
GET /knowledge/documents/{documentId}
```

Returns

- Metadata
- Upload date
- Status
- Tags
- Chunk count

---

# Update Document

## Endpoint

```
PATCH /knowledge/documents/{documentId}
```

Example

```json
{
  "title": "Employee Handbook",
  "category": "HR",
  "tags": [
    "employee",
    "policy"
  ]
}
```

---

# Delete Document

## Endpoint

```
DELETE /knowledge/documents/{documentId}
```

Soft deletes the document.

Associated chunks and embeddings are removed during background cleanup.

---

# Process Document

## Endpoint

```
POST /knowledge/documents/{documentId}/process
```

This endpoint starts an asynchronous processing job.

Pipeline

```
Upload

↓

Extract Text

↓

Clean Content

↓

Split Into Chunks

↓

Generate Embeddings

↓

Store in Database

↓

Ready
```

---

# Document Status

Possible values

```
UPLOADED

PROCESSING

READY

FAILED
```

---

# List Chunks

## Endpoint

```
GET /knowledge/documents/{documentId}/chunks
```

Example Response

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "chunkIndex": 1,
      "content": "Password reset instructions..."
    }
  ]
}
```

---

# Semantic Search

## Endpoint

```
POST /knowledge/search
```

Request

```json
{
  "query": "How do I reset my password?",
  "limit": 5
}
```

Example Response

```json
{
  "success": true,
  "data": [
    {
      "documentId": "uuid",
      "documentTitle": "Employee Handbook",
      "chunkId": "uuid",
      "score": 0.94,
      "content": "To reset your password..."
    }
  ]
}
```

Results are ranked by semantic similarity.

---

# Knowledge Statistics

## Endpoint

```
GET /knowledge/statistics
```

Example Response

```json
{
  "success": true,
  "data": {
    "documents": 85,
    "chunks": 2430,
    "embeddings": 2430,
    "storageUsedMB": 512
  }
}
```

---

# Validation Rules

Title

- Required
- Maximum 200 characters

Category

- Maximum 100 characters

Tags

- Maximum 20 tags
- Maximum 50 characters per tag

File Size

- Maximum 50 MB

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full access |
| Administrator | Full access |
| Support Manager | Manage documents |
| Support Agent | View and search |
| Customer | No access |

---

# Business Rules

- Every document belongs to one organization.
- Processing is asynchronous.
- Documents are chunked before embedding generation.
- Embeddings are regenerated after document updates.
- Deleted documents are excluded from AI search.

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Document not found |
| 413 | File too large |
| 415 | Unsupported file type |
| 422 | Validation failed |

---

# Audit Events

The following actions generate audit logs:

- Document uploaded
- Document updated
- Document deleted
- Processing started
- Processing completed
- Semantic search performed

---

# Security Considerations

- Documents are isolated by `organizationId`.
- Uploaded files are validated before processing.
- Background jobs run with restricted permissions.
- Embeddings are generated only for active documents.
- Search results are limited to the authenticated user's organization.

---

# Summary

The Knowledge Base API provides the foundation for SupportOS's AI-powered assistance. By managing document uploads, processing pipelines, chunk storage, embeddings, and semantic search, it enables fast and accurate Retrieval-Augmented Generation (RAG) while maintaining tenant isolation, security, and scalability.