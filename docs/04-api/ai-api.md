# AI API

## Overview

The AI API provides intelligent assistance across the SupportOS platform. It leverages Large Language Models (LLMs) together with Retrieval-Augmented Generation (RAG) to generate context-aware responses based on an organization's knowledge base and ticket history.

All AI requests are scoped to the authenticated user's organization.

---

# Base URL

```
/api/v1/ai
```

---

# AI Endpoints

| Method | Endpoint | Description | Authentication |
|----------|----------|-------------|----------------|
| POST | /chat | AI assistant conversation | Yes |
| POST | /summarize | Generate ticket summary | Yes |
| POST | /reply-suggestion | Generate reply suggestion | Yes |
| POST | /knowledge-search | AI semantic knowledge search | Yes |
| POST | /classify-ticket | Predict category and priority | Yes |
| POST | /sentiment-analysis | Analyze customer sentiment | Yes |
| POST | /translate | Translate message | Yes |
| GET | /usage | AI usage statistics | Yes |

---

# AI Chat

## Endpoint

```
POST /ai/chat
```

## Request

```json
{
  "ticketId": "uuid",
  "message": "How can I help this customer?"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "response": "Based on the uploaded knowledge base, ask the customer to reset their password using the account recovery page.",
    "confidence": 0.95,
    "sources": [
      {
        "documentId": "uuid",
        "title": "Password Reset Guide"
      }
    ]
  }
}
```

---

# Ticket Summary

## Endpoint

```
POST /ai/summarize
```

## Request

```json
{
  "ticketId": "uuid"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "summary": "Customer experienced repeated login failures after enabling two-factor authentication. Suggested password reset and browser cache clearing."
  }
}
```

---

# Reply Suggestion

## Endpoint

```
POST /ai/reply-suggestion
```

## Request

```json
{
  "ticketId": "uuid"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "reply": "Thank you for contacting Support. Please follow the password reset instructions below..."
  }
}
```

---

# Knowledge Search

## Endpoint

```
POST /ai/knowledge-search
```

## Request

```json
{
  "query": "How do I reset my password?",
  "limit": 5
}
```

## Response

```json
{
  "success": true,
  "data": [
    {
      "documentTitle": "Password Reset Guide",
      "score": 0.96,
      "content": "Navigate to the login page and select 'Forgot Password'..."
    }
  ]
}
```

---

# Ticket Classification

## Endpoint

```
POST /ai/classify-ticket
```

## Request

```json
{
  "title": "Unable to access dashboard",
  "description": "Receiving an authorization error after login."
}
```

## Response

```json
{
  "success": true,
  "data": {
    "category": "Authentication",
    "priority": "HIGH",
    "confidence": 0.91
  }
}
```

---

# Sentiment Analysis

## Endpoint

```
POST /ai/sentiment-analysis
```

## Request

```json
{
  "message": "I've been waiting for two days and nothing works."
}
```

## Response

```json
{
  "success": true,
  "data": {
    "sentiment": "NEGATIVE",
    "confidence": 0.93
  }
}
```

Possible values:

```
POSITIVE

NEUTRAL

NEGATIVE
```

---

# Translation

## Endpoint

```
POST /ai/translate
```

## Request

```json
{
  "text": "Hello, how can I help you?",
  "targetLanguage": "es"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "translatedText": "Hola, ¿cómo puedo ayudarte?"
  }
}
```

---

# AI Usage

## Endpoint

```
GET /ai/usage
```

## Response

```json
{
  "success": true,
  "data": {
    "requestsToday": 184,
    "tokensUsed": 256000,
    "estimatedCost": 12.48
  }
}
```

---

# AI Response Metadata

AI responses may include:

- Confidence score
- Retrieved knowledge sources
- Processing time
- Model identifier
- Token usage

---

# Validation Rules

Message

- Required
- Maximum 10,000 characters

Search Query

- Required
- Maximum 1,000 characters

Translation Text

- Required
- Maximum 10,000 characters

---

# Authorization

| Role | Permission |
|------|------------|
| Owner | Full AI access |
| Administrator | Full AI access |
| Support Manager | AI features |
| Support Agent | AI features |
| Customer | Limited AI assistance (future) |

---

# Business Rules

- AI responses are organization-specific.
- Knowledge retrieval searches only the organization's indexed documents.
- AI suggestions are advisory and do not modify tickets automatically.
- Long-running AI operations should be processed asynchronously where appropriate.
- All AI interactions are recorded for analytics and auditing.

---

# Error Responses

| Status | Description |
|----------|-------------|
| 400 | Invalid request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Ticket or document not found |
| 422 | Validation failed |
| 429 | AI rate limit exceeded |
| 503 | AI service unavailable |

---

# Audit Events

The following actions generate audit records:

- AI chat request
- Ticket summarized
- Reply suggestion generated
- Knowledge search executed
- Ticket classified
- Sentiment analyzed
- Translation requested

---

# Security Considerations

- AI requests are isolated by `organizationId`.
- Retrieved knowledge is restricted to the authenticated organization.
- Sensitive information should be masked before being sent to external AI providers when possible.
- Token usage and AI costs are monitored.
- AI outputs should always be reviewed by users before taking action.

---

# Summary

The AI API enables intelligent assistance throughout SupportOS by combining LLMs with Retrieval-Augmented Generation (RAG). It provides conversational assistance, ticket summaries, response suggestions, semantic search, classification, sentiment analysis, and translation while maintaining tenant isolation, security, and auditability.