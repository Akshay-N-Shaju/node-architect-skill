---
name: node-observability-ops
description: Adds production readiness to Node.js services through structured logs, metrics, tracing, health checks, graceful shutdown, containers, CI/CD, and incident runbooks.
---

# Node.js Observability and Operations

## Workflow
1. Inspect runtime topology, deployment platform, health checks, logging, metrics, traces, and current release process.
2. Identify service-level indicators and critical dependencies.
3. Add useful telemetry with redaction and correlation.
4. Implement lifecycle-safe startup/shutdown and readiness behavior.
5. Verify container/build/deploy behavior and write operational runbooks.

## Observability
- Use structured logs with timestamp, level, service, environment, request ID, trace ID, route, status, and duration.
- Propagate correlation IDs into background jobs.
- Track request rate, latency percentiles, errors, database pool saturation, dependency failures, queue depth, oldest job age, retries, and worker restarts.
- Use OpenTelemetry-compatible tracing when trace context can be propagated across services and jobs.
- Avoid high-cardinality metric labels such as arbitrary user IDs, URLs, or request IDs.
- Redact credentials, tokens, cookies, payment data, and sensitive personal information.
- Make liveness mean the process is alive; readiness means it can safely accept work. Don't expose internal dependency details publicly.

## Lifecycle
- Handle termination signals.
- Stop accepting new HTTP requests before closing the server.
- Stop workers from claiming new jobs; finish or safely release active jobs.
- Close database pools, Redis connections, telemetry exporters, timers, and sockets.
- Make startup fail fast on invalid required configuration.
- Avoid running migrations automatically in every replica unless that is explicitly the deployment strategy.

## Delivery
- Use reproducible installs and lockfiles.
- Build immutable artifacts.
- Run migrations as a controlled release step.
- Use least-privilege runtime credentials.
- Include smoke tests, health verification, rollback criteria, and schema compatibility planning.
- Test backups and restores, not just backup creation.

## Incident response
Capture impact, timeline, evidence, mitigation, root cause, contributing factors, and follow-up actions. Prefer reversible mitigations and preserve diagnostic evidence without exposing secrets.

## References
- https://opentelemetry.io/docs/
- https://getpino.io/
- https://12factor.net/
