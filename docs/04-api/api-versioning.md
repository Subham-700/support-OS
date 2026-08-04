# API Versioning

## Overview

SupportOS uses URL-based API versioning to ensure backward compatibility while allowing new features and breaking changes to be introduced safely.

Every API endpoint belongs to a specific version.

Current version:

```
v1
```

---

# Versioning Strategy

SupportOS follows **URL versioning**.

Example:

```
/api/v1/auth/login

/api/v1/tickets

/api/v1/users
```

Future versions:

```
/api/v2/auth/login

/api/v2/tickets

/api/v3/users
```

This approach makes API versions explicit, easy to document, and simple for clients to adopt.

---

# Current API Version

```
v1
```

Base URL

Development

```
http://localhost:3001/api/v1
```

Production

```
https://api.supportos.com/api/v1
```

---

# Version Lifecycle

Each API version progresses through the following stages:

| Stage | Description |
|--------|-------------|
| Development | Internal development and testing |
| Beta | Available for early adopters |
| Stable | Recommended production version |
| Deprecated | Scheduled for removal |
| Retired | No longer available |

---

# Breaking Changes

A new API version is required when introducing breaking changes.

Examples include:

- Removing endpoints
- Renaming endpoints
- Changing request formats
- Changing response structures
- Removing response fields
- Modifying authentication mechanisms
- Changing validation rules incompatibly

Example:

```
v1

GET /tickets
```

becomes

```
v2

GET /support-tickets
```

This requires a new version.

---

# Non-Breaking Changes

The following changes do **not** require a new version:

- Adding optional request fields
- Adding optional response fields
- Performance improvements
- Bug fixes
- Security enhancements
- Internal implementation changes
- Documentation updates

---

# Deprecation Policy

Deprecated endpoints remain available for a defined period before removal.

Typical lifecycle:

```
Stable

↓

Deprecated

↓

Retired
```

Clients should migrate before the retirement date.

---

# Deprecation Response Headers

Deprecated endpoints may include headers such as:

```
Deprecation: true

Sunset: Wed, 01 Dec 2027 00:00:00 GMT
```

These headers inform clients about upcoming removals.

---

# Version Compatibility

SupportOS aims to support at least one previous stable version during major upgrades.

Example:

| Version | Status |
|----------|--------|
| v1 | Stable |
| v2 | Beta |

After v2 becomes stable:

| Version | Status |
|----------|--------|
| v1 | Deprecated |
| v2 | Stable |

---

# Migration Guidelines

When upgrading to a new version:

1. Review release notes.
2. Update endpoint URLs.
3. Update request payloads if necessary.
4. Update response handling.
5. Test all integrations.
6. Deploy changes before the deprecated version is retired.

---

# Example Versioned Endpoints

Authentication

```
POST /api/v1/auth/login
```

Users

```
GET /api/v1/users
```

Tickets

```
POST /api/v1/tickets
```

Knowledge Base

```
GET /api/v1/knowledge/documents
```

AI

```
POST /api/v1/ai/chat
```

Notifications

```
GET /api/v1/notifications
```

---

# Release Process

Each major API release includes:

- Updated documentation
- Changelog
- Migration guide
- Deprecation notices
- Automated compatibility tests

---

# Semantic Versioning

SupportOS follows Semantic Versioning (SemVer) for releases.

Format:

```
MAJOR.MINOR.PATCH
```

Examples:

```
1.0.0

1.1.0

1.2.5

2.0.0
```

Meaning:

| Component | Description |
|-----------|-------------|
| MAJOR | Breaking changes |
| MINOR | Backward-compatible features |
| PATCH | Bug fixes and small improvements |

---

# Documentation Versioning

Each API version has its own documentation.

Example:

```
docs/

04-api/

v1/

v2/
```

Swagger documentation is also versioned.

Examples:

```
/api/docs

/api/v2/docs
```

---

# Best Practices

- Always specify the API version in the URL.
- Avoid breaking changes within a version.
- Deprecate before removing endpoints.
- Provide migration guides for every major version.
- Maintain backward compatibility whenever possible.
- Keep documentation synchronized with the implementation.

---

# Future Considerations

Potential enhancements include:

- API version negotiation using headers.
- Long-Term Support (LTS) API versions.
- Automated client SDK generation for each version.
- GraphQL schema versioning.

---

# Summary

SupportOS uses URL-based API versioning to deliver a stable and predictable developer experience. By separating breaking changes into new major versions, maintaining backward compatibility, and providing clear migration paths, the API can evolve without disrupting existing integrations.