---
name: express-engineer
description: Designs, implements, and reviews Express.js applications with secure middleware ordering, route/module boundaries, validation, error handling, request lifecycle, testing, and production operations. Use for Express APIs or services.
---

# Express Engineer

## Mission
Build Express applications with explicit boundaries, predictable middleware order, safe defaults, testable handlers, and production-grade failure handling. Verify behavior against the Express major version installed in the project.

## Workflow
1. Inspect `package.json`, Express version, Node.js version, ESM/CommonJS mode, route layout, error handling, tests, and proxy topology.
2. Establish the HTTP contract, validation strategy, authentication and authorization requirements.
3. Configure app-level middleware in a deliberate order.
4. Keep route handlers thin; delegate business operations to use cases/services.
5. Add unit and HTTP integration tests for success, invalid input, authorization denial, not-found, and unexpected errors.
6. Run lint, typecheck if applicable, tests, build, and dependency audit.

## Recommended app structure
```text
src/
  app.ts                 # create/configure Express app; do not listen here
  server.ts              # startup, listener, graceful shutdown
  config/
  http/
    middleware/
      request-id.ts
      authenticate.ts
      authorize.ts
      not-found.ts
      error-handler.ts
    routes/
      index.ts
  modules/
    users/
      users.routes.ts
      users.controller.ts
      users.service.ts
      users.repository.ts
      users.schemas.ts
      users.test.ts
  jobs/
  observability/
```
For larger applications, use business modules with explicit public APIs. Don't force every small project into many layers.

## Middleware order
A common baseline is:
1. Request ID and safe request context
2. Security headers (`helmet`)
3. Explicit CORS policy, if browser clients need cross-origin access
4. Request/body limits and parsers
5. Rate limiting and abuse controls (placed thoughtfully; login routes may need specific policies)
6. Authentication where routes require it (or route-level guards)
7. Routes and route-level validation/authorization
8. Custom 404 handler
9. Central error handler as the final middleware

Actual order depends on middleware behavior. Avoid parsing large bodies before enforcing limits. Do not apply authentication globally if public routes need a deliberate, explicit policy.

## Secure production baseline
- Use a supported Node.js LTS and maintained Express version; patch promptly.
- Use TLS at the edge or trusted ingress for sensitive traffic.
- Apply `helmet()` and configure Content Security Policy when serving browser content.
- Disable fingerprinting with `app.disable("x-powered-by")`.
- Set explicit JSON and URL-encoded body limits.
- Configure `trust proxy` only for the actual known proxy topology; do not blindly trust all forwarded headers.
- Restrict CORS origins, methods, and credentials to actual requirements. CORS is not access control.
- Rate-limit login, password recovery, expensive routes, and other abuse-prone operations using a store appropriate for multi-instance deployments.
- Validate params, query, headers, and body at runtime.
- Prevent open redirects, SSRF, path traversal, unsafe file uploads, command injection, and SQL injection.
- Use secure cookie settings (`Secure`, `HttpOnly`, `SameSite`) appropriate to the flow. Use CSRF protections where cookie-authenticated browser requests require them.
- Never use in-memory session storage as production shared session storage.
- Redact authorization headers, cookies, passwords, tokens, and sensitive data from logs.
- Audit dependencies and maintain an update process.

## Request validation
Treat all request data as `unknown` until validated. Choose one validation library or JSON Schema approach and use it consistently. Validation should produce typed values for the handler; don't assume TypeScript types validate runtime JSON.

Validate:
- `req.params`
- `req.query`
- `req.headers` where relevant
- `req.body`
- webhook payloads and signatures
- file metadata and upload size/type

Do not trust a filename extension or client-provided MIME type as proof of file content.

## Error handling
- In Express 5, rejected promises returned by route handlers are forwarded to error handling; still verify the project's installed version and handler wrappers.
- In Express 4, async handlers generally need an explicit wrapper or `next(error)` strategy.
- Use typed application errors or stable error codes and map them centrally.
- Add a custom 404 handler before the error handler.
- The error handler has four arguments: `(err, req, res, next)`.
- If headers have already been sent, delegate to Express's default handler as documented.
- Return stable, non-sensitive production errors; don't send stack traces, SQL, secrets, or internal topology.
- Log unexpected failures with request ID and redacted context; do not log the entire request object.

Example shape:
```ts
import type { ErrorRequestHandler } from "express";

export const errorHandler: ErrorRequestHandler = (err, req, res, next) => {
  if (res.headersSent) return next(err);

  const status = Number.isInteger(err?.status) ? err.status : 500;
  const code = status >= 500 ? "INTERNAL_ERROR" : "REQUEST_ERROR";

  if (status >= 500) {
    req.log?.error?.({ err, requestId: req.id }, "Unhandled request error");
  }

  res.status(status).json({
    error: {
      code,
      message: status >= 500 ? "An unexpected error occurred." : err.message,
      requestId: req.id
    }
  });
};
```
Adapt this illustrative example to your actual logger, typed error model, and request augmentation. Do not blindly trust arbitrary `err.status` values; production code should map only known application errors to public status codes.

## Controllers and services
- Controllers parse validated input, call one use case, and map the result to HTTP.
- Keep database transactions and business invariants in application/domain layers.
- Do not pass the Express `Request` or `Response` objects deep into domain logic.
- Avoid a generic base controller/service abstraction unless repeated behavior is real and stable.
- Use explicit dependency construction or a DI container only when it improves lifecycle management and testability.

## Proxy and client IP
Configure `trust proxy` according to the exact network topology and trusted proxy count/subnets. Incorrect settings can let clients spoof IP-related headers and bypass rate limits or audit controls. Verify behavior in the deployed environment.

## Testing
- Export `createApp()` separately from `server.listen()` to make tests straightforward.
- Test routes over HTTP using the project's chosen test client.
- Include invalid body/query, malformed JSON, oversized body, unauthorized request, forbidden resource, not-found, and unexpected-error cases.
- Test CORS and proxy assumptions in the deployed topology where relevant.
- Verify error responses don't leak stack traces or secrets.

## References
- Express production security: https://expressjs.com/en/advanced/best-practice-security/
- Express performance and reliability: https://expressjs.com/en/advanced/best-practice-performance/
- Express error handling: https://expressjs.com/en/guide/error-handling.html
- Express behind proxies: https://expressjs.com/en/guide/behind-proxies.html
- Express API reference: https://expressjs.com/en/5x/api.html
- Node.js security best practices: https://nodejs.org/en/learn/getting-started/security-best-practices
