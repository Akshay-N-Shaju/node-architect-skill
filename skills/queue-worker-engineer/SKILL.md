---
name: queue-worker-engineer
description: Designs reliable Node.js background processing with queues, workers, typed job payloads, retries, idempotency, dead-letter handling, graceful shutdown, and transactional outbox patterns.
---

# Queue and Worker Engineer

## Use queues when
Work is slow, retryable, bursty, provider-dependent, scheduled, or should not extend HTTP latency. Keep synchronous work synchronous when that is simpler and reliable.

## Workflow
1. Define job purpose, payload schema/version, owner module, retry policy, timeout, concurrency, and operational SLO.
2. Choose a queue technology compatible with infrastructure and delivery semantics.
3. Validate job payloads at worker entry.
4. Implement idempotent handlers and explicit retryable/permanent error classification.
5. Add metrics, structured logs, correlation IDs, graceful shutdown, and safe replay operations.
6. Test duplicate delivery, timeout, worker restart, provider outage, and malformed payloads.

## Reliability rules
- Assume at-least-once delivery unless the actual system proves otherwise.
- A job may run more than once; use deduplication, unique constraints, idempotency keys, or state transitions.
- Retry transient failures with bounded exponential backoff and jitter.
- Do not retry invalid payloads or permanent business errors forever.
- Use provider idempotency keys for payments and other irreversible side effects.
- Keep payloads small and versioned; pass entity IDs rather than stale copies of whole records.
- Avoid secrets and unnecessary personal data in payloads.
- Set per-job timeouts and per-queue concurrency limits.
- Handle shutdown by stopping new claims, finishing or safely releasing in-flight work, and closing connections.
- Monitor queue depth, oldest waiting age, attempts, duration, failures, and stalled workers.
- Protect dead-letter replay with permissions, validation, and audit logs.

## Database and queue consistency
A database commit and queue publish usually aren't one atomic transaction. For critical workflows, consider a transactional outbox:
1. Write business state and outbox record in the same DB transaction.
2. Dispatcher publishes unpublished records.
3. Mark records published only after dispatch succeeds.
4. Consumers remain idempotent because duplicates can occur.

## Job contract example
```ts
import { z } from "zod";

export const GenerateReportJob = z.object({
  version: z.literal(1),
  reportId: z.string().min(1),
  requestedBy: z.string().min(1),
  correlationId: z.string().min(1),
});
```

## Operational checklist
- Is job ownership clear?
- Is the payload validated?
- Can it safely execute twice?
- Are retry limits and backoff appropriate?
- Can operators see and replay failures safely?
- Are queue and provider outages observable?
- Are shutdown and deployment drain behavior tested?

## References
- https://docs.bullmq.io/
- https://docs.nestjs.com/techniques/queues
- https://www.postgresql.org/docs/
