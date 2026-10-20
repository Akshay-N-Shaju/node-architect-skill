---
name: nestjs-engineer
description: Implements and reviews NestJS backends using modules, dependency injection, controllers, providers, guards, pipes, interceptors, filters, configuration, validation, and tests.
---

# NestJS Engineer

## Workflow
1. Inspect NestJS version, module layout, adapter (Express/Fastify), providers, global pipes/guards, config, and test setup.
2. Map the feature to a business module and identify its public provider/interface.
3. Define DTO validation, controller contract, application service/use case, persistence adapter, and authorization.
4. Wire providers with explicit tokens when abstractions are needed; avoid accidental circular module imports.
5. Test providers and HTTP behavior; verify startup and dependency injection.
6. Run lint, typecheck, unit tests, e2e tests, and build.

## Responsibilities by Nest construct
- **Module:** Composition and provider visibility; keep modules capability-oriented.
- **Controller:** HTTP mapping only; no business workflows or raw database orchestration.
- **Provider/service:** Application use cases or infrastructure adapters, with responsibilities named clearly.
- **Pipe:** Transform/validate request input at the boundary.
- **Guard:** Authentication and coarse authorization decisions.
- **Interceptor:** Cross-cutting request/response behavior such as timing or serialization; don't hide business logic.
- **Exception filter:** Map known errors into stable HTTP responses without leaking internals.
- **Middleware:** Request IDs and transport-level setup where appropriate.
- **Lifecycle hooks:** Initialize/close resources; ensure shutdown behavior is tested.

## Design rules
- Use `ConfigModule` or the project's established config mechanism and validate environment values at startup.
- Use global validation only when its behavior is documented and consistent with DTOs.
- Keep domain logic framework-independent when practical.
- Prefer constructor injection and explicit provider tokens for ports.
- Export only providers intended as module public API.
- Avoid making every provider global.
- Use `forwardRef()` only as a deliberate temporary bridge; recurring circular dependencies usually indicate a boundary problem.
- Ensure guards don't replace resource-level authorization in application use cases.
- Use the project's selected database library consistently.
- For queues, register queue consumers and lifecycle behavior in a predictable module.

## Tests
- Unit-test use cases and policies.
- Test controller/guard/pipe behavior with Nest testing utilities.
- E2E-test auth, validation, status codes, response shapes, and forbidden access.
- Use integration tests for real database constraints and transaction behavior.

## Common pitfalls
- Request-scoped providers without a demonstrated need
- Business logic in decorators, guards, or interceptors
- Hidden side effects in module constructors
- `@Global()` used to avoid explicit dependency wiring
- DTO validation configured but not actually enforced
- E2E tests that mock the behavior they are supposed to verify

## References
- https://docs.nestjs.com/
- https://docs.nestjs.com/guards
- https://docs.nestjs.com/techniques/validation
- https://docs.nestjs.com/fundamentals/testing
