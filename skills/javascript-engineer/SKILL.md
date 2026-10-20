---
name: javascript-engineer
description: Builds, debugs, and refactors modern JavaScript on Node.js, including ESM/CommonJS, asynchronous control flow, streams, errors, resource lifecycles, and runtime performance.
---

# JavaScript Engineer for Node.js

## Workflow
1. Inspect Node.js version, `package.json`, module type, lint rules, and existing style.
2. Confirm whether the project uses ESM or CommonJS; keep imports, scripts, tests, and build tooling consistent.
3. Trace async flow and resource lifecycle before changing code.
4. Implement readable code with explicit errors, cancellation/timeouts where appropriate, and cleanup.
5. Add tests for success, failure, concurrency, and edge cases.
6. Run lint, tests, and the project's actual runtime/build command.

## JavaScript guidance
- Prefer `async`/`await` for readable asynchronous workflows, but understand Promise scheduling and rejection handling.
- Do not leave floating promises. Await them, return them, or explicitly handle their rejection.
- Avoid sequential `await` when independent operations can safely run concurrently; use bounded concurrency for large sets.
- Use `Promise.all` only when fail-fast behavior is intended. Consider `allSettled` when every result must be inspected.
- Set timeouts for network and external dependency calls; propagate cancellation when supported.
- Do not block the event loop with large synchronous work. Use streams, worker threads, or background jobs when justified.
- Respect backpressure when reading/writing streams.
- Close database pools, queue connections, timers, sockets, and server listeners during shutdown.
- Avoid mutable global state that makes tests and multi-tenant behavior unsafe.
- Prefer explicit error classes or stable error codes over parsing error-message strings.
- Use `Map`/`Set` when their semantics fit; avoid clever coercion and implicit type conversion.
- Keep ESM/CJS boundaries intentional; don't blindly mix `require`, dynamic import, and transpiler assumptions.

## Node runtime concerns
- Understand process signals and unhandled rejection behavior for the deployed Node version.
- Use `process.env` only in configuration bootstrap, not throughout business logic.
- Avoid logging full request objects, headers, or arbitrary error payloads.
- Treat filesystem paths and user-controlled URLs as untrusted input.
- Measure before optimizing; use CPU/heap profiles and event-loop delay metrics for performance issues.

## Review checklist
- Are promises handled?
- Can failures leave partial state?
- Can concurrent calls race?
- Are external calls bounded by timeouts?
- Is cleanup guaranteed?
- Is input validated?
- Is the code testable without global mutation?

## References
- https://nodejs.org/en/learn
- https://nodejs.org/api/
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
