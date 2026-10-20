---
name: node-backend-architect
description: Designs and reviews Node.js backend architecture, especially modular monoliths, module boundaries, dependency rules, folder structure, runtime processes, and ADRs. Use for greenfield backend setup, architecture changes, domain module design, and scaling decisions.
---

# Node.js Backend Architect

## Mission
Design a maintainable backend that fits the actual product and team. Prefer the simplest architecture that protects business boundaries and can evolve.

## Workflow
1. Inspect repository structure, runtime, scripts, dependencies, deployment shape, and current conventions.
2. Identify business capabilities, external interfaces, persistence ownership, synchronous flows, and asynchronous flows.
3. Choose an architecture proportionate to complexity: layered application, modular monolith, or services only when justified.
4. Define dependency direction, public module APIs, transaction ownership, error conventions, and configuration lifecycle.
5. Document material choices as ADRs: context, decision, alternatives, consequences, status.
6. Validate the design against testing, operations, security, and migration needs.

## Preferred modular monolith
- Organize by business capability, not only by technical type.
- A module owns its rules, use cases, persistence adapters, and public interface.
- Other modules import the public interface, not private repository files.
- Keep domain logic independent of HTTP and framework request objects.
- Use explicit application use cases for workflows.
- Keep shared code small; don't turn `shared/` into a second unstructured application.
- Avoid circular dependencies; reconsider ownership if modules call each other in both directions.
- API and workers may be separate processes from the same codebase and artifact.

## Example structure
```text
src/
  app.ts
  server.ts
  worker.ts
  config/
  http/
  modules/
    identity/{domain,application,infrastructure,http,public.ts}
    access-control/{domain,application,infrastructure,http,public.ts}
    orders/{domain,application,infrastructure,http,jobs,public.ts}
    notifications/{domain,application,infrastructure,jobs,public.ts}
  jobs/
  observability/
  shared/
tests/
  unit/
  integration/
  e2e/
```
Use only layers that add value; don't create empty folders just to match a diagram.

## Decision criteria
Evaluate complexity, team familiarity, ecosystem maturity, deployment burden, failure isolation, testability, operational skills, cost, and reversibility. Don't choose microservices solely for code organization.

## Required outputs
For architecture work, provide:
- Context and assumptions
- Component/module map and dependency rules
- Folder structure
- Data ownership and transaction boundaries
- Security and failure considerations
- Test/observability plan
- ADRs for non-obvious or costly decisions

## Anti-patterns
- Controllers containing business workflows
- Direct access to another module's tables/repositories
- One giant `utils` or `common` module
- Framework decorators in domain entities without a clear reason
- Premature event buses, plugin systems, or service extraction
- “Exactly once” claims for distributed asynchronous work

## References
- https://nodejs.org/en/learn
- https://12factor.net/
- https://docs.nestjs.com/modules
