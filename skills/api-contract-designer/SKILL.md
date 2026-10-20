---
name: api-contract-designer
description: Designs consistent REST APIs and OpenAPI contracts for Node.js services, including resource modeling, validation, pagination, idempotency, error envelopes, and compatibility.
---

# API Contract Designer

## Workflow
1. Identify consumers, use cases, resources, privacy boundaries, and expected traffic.
2. Define resource names, operations, status codes, schemas, and error codes before implementation.
3. Specify authentication and authorization requirements per operation.
4. Document pagination, filtering, sorting, idempotency, rate limits, and concurrency behavior.
5. Validate OpenAPI and add contract/e2e tests.
6. Review compatibility before changing released endpoints.

## REST conventions
- Use nouns for resources and HTTP methods for standard operations.
- Use consistent plural resource names and nested routes only when the relationship is meaningful.
- Separate transport DTOs from internal domain models.
- Validate path, query, header, and body inputs.
- Use stable machine-readable error codes and a request/correlation ID.
- Use cursor pagination for large or rapidly changing collections when appropriate; document ordering and cursor semantics.
- Apply request size limits and rate limits.
- Use idempotency keys for retry-prone operations with costly side effects.
- Use optimistic concurrency or preconditions for conflicting updates where necessary.
- Avoid leaking whether a sensitive resource exists if that distinction creates a security risk.

## Error envelope example
```json
{
  "error": {
    "code": "RESOURCE_CONFLICT",
    "message": "The requested change conflicts with current state.",
    "requestId": "req_123"
  }
}
```

## Compatibility
- Prefer additive changes.
- Don't silently rename/remove fields or change meaning.
- Define deprecation and migration windows for breaking changes.
- Keep OpenAPI schemas aligned with runtime validation.
- Treat generated clients as contract consumers and test them where possible.

## Deliverables
For API design, include endpoint table, auth requirements, request/response schemas, error cases, pagination semantics, OpenAPI changes, and contract tests.

## References
- https://spec.openapis.org/oas/latest.html
- https://owasp.org/www-project-api-security/
