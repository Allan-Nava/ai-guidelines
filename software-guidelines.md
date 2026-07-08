# Software Development Guidelines

Practical guidelines for building and maintaining production-grade software systems, with dedicated standards for backend and frontend development.

## 1. Scope And Goals

Use this document to align software delivery across teams and repositories.

Primary goals:

- predictable architecture and code quality,
- reliable delivery with low regression risk,
- clear contracts between backend and frontend,
- strong operability, security, and maintainability.

## 2. Core Engineering Principles

1. Prefer simplicity over cleverness.
2. Make behavior explicit and testable.
3. Keep boundaries clear: API, domain, persistence, UI.
4. Optimize for maintainability before micro-optimizations.
5. Document decisions that change architecture or contracts.

## 3. Software Repository Baseline

Minimum recommended structure:

```text
/
|- src/
|- test/
|- docs/
|  |- architecture/
|  |- adr/
|  |- runbooks/
|- README.md
|- CHANGELOG.md
|- TODO.md
|- guidelines.md
|- software-guidelines.md
```

Rules:

- Keep one source of truth for tasks in TODO.
- Update changelog for every relevant change.
- Keep API contract docs close to implementation.

## 4. Architecture And Boundaries

### 4.1 Layering

- Presentation layer: controllers/handlers/UI adapters only.
- Application layer: orchestration and use-case logic.
- Domain layer: business rules and invariants.
- Infrastructure layer: databases, messaging, external APIs.

### 4.2 Dependency Direction

- Dependencies point inward toward domain/application.
- Infrastructure details must not leak into domain logic.
- Domain code must be portable and framework-light.

### 4.3 Contracts

- All external contracts are versioned (API, events, schemas).
- Breaking changes require migration plan and rollout steps.
- Prefer additive changes over destructive ones.

## 5. Backend Guidelines

### 5.1 API Design

- Use resource-oriented naming and stable identifiers.
- Return consistent error shape and machine-readable codes.
- Validate input at boundaries and fail fast on invalid requests.
- Enforce idempotency for retriable write operations when relevant.

### 5.2 Domain And Services

- Keep business logic in services/domain objects, not controllers.
- Model invariants explicitly.
- Avoid anemic services that only pass through repository calls.

### 5.3 Persistence And Data

- Use migrations for every schema change.
- Make migrations reversible when feasible.
- Treat data deletion and retention as explicit policy.
- Avoid hidden N+1 queries and unbounded scans.

### 5.4 Async And Integrations

- Use explicit retry policy with backoff and max attempts.
- Make message consumers idempotent.
- Track dead-letter handling and replay procedure.
- Apply timeouts/circuit breakers for external dependencies.

### 5.5 Observability

- Structured logs with correlation/request IDs.
- Metrics for throughput, latency, errors, saturation.
- Traces for cross-service workflows.
- Health/readiness endpoints must be meaningful and cheap.

## 6. Frontend Guidelines

### 6.1 Information Architecture

- Organize features by domain, not by technical type only.
- Keep UI state ownership explicit.
- Avoid global state for local concerns.

### 6.2 UI Engineering

- Build reusable components with clear contracts.
- Keep components focused and composable.
- Separate data fetching from presentational rendering when practical.

### 6.3 Accessibility And UX

- Follow semantic HTML and accessible labeling.
- Ensure keyboard navigation and focus management.
- Respect contrast and reduced-motion preferences.
- Design loading, empty, and error states intentionally.

### 6.4 Performance

- Control bundle size and avoid unnecessary dependencies.
- Use code splitting and lazy loading where it provides value.
- Prevent excessive re-renders with proper memoization strategy.
- Track web vitals and set performance budgets.

### 6.5 Security In The Browser

- Treat all client input and external content as untrusted.
- Prevent XSS via escaping/sanitization and safe rendering patterns.
- Protect session/token handling and avoid insecure storage patterns.
- Apply CSP and secure headers where supported.

## 7. Backend-Frontend Contract Practices

1. Maintain typed and versioned API contracts.
2. Define and document error taxonomy shared by both sides.
3. Use schema validation in CI for contract changes.
4. Coordinate deprecations with clear timeline and communication.
5. Add contract tests for critical user journeys.

## 8. Testing Strategy

### 8.1 Backend Testing

- Unit tests for domain logic.
- Integration tests for database and external adapters.
- API tests for contract and serialization behavior.
- Smoke tests for main production paths.

### 8.2 Frontend Testing

- Unit tests for business logic and utilities.
- Component tests for key UI states.
- End-to-end tests for critical journeys.
- Visual regression tests for stable design systems.

### 8.3 Quality Gate

A merge is valid only if:

1. lint and static analysis pass,
2. required test suite passes,
3. security checks pass,
4. changelog and docs are updated when relevant.

## 9. Security Requirements

1. Never commit secrets.
2. Validate and sanitize all external input.
3. Enforce authorization checks server-side.
4. Use dependency scanning and patch vulnerable packages.
5. Keep an incident-ready audit trail for critical operations.

## 10. CI/CD Expectations

1. Keep pipelines deterministic and fast enough for daily use.
2. Run tests in parallel where useful.
3. Use preview environments for high-impact UI/API changes.
4. Use canary/gradual rollout for risky backend changes.
5. Define rollback strategy before release.

## 11. Code Review Standards

Review focus order:

1. correctness and user impact,
2. security and data integrity,
3. regressions and edge cases,
4. maintainability and clarity,
5. performance and cost implications.

Review checklist:

- Is behavior explicit and test-covered?
- Are contracts backward compatible?
- Are failure modes and retries safe?
- Is documentation updated where needed?

## 12. Documentation Requirements

For relevant software changes, update:

- README for usage or setup changes,
- architecture docs for design shifts,
- ADR for significant decisions,
- changelog for release visibility,
- TODO for execution traceability.

## 13. Definition Of Done For Software Changes

A software change is done only when:

1. implementation is complete,
2. required tests pass,
3. observability impact is covered,
4. security checks are addressed,
5. docs and changelog are updated,
6. TODO status and evidence are updated.

## 14. Quick Templates

### 14.1 API Change Template

```md
## API Change
- Endpoint(s):
- Request/Response changes:
- Backward compatibility:
- Migration/Rollout plan:
- Monitoring impact:
```

### 14.2 Frontend Feature Template

```md
## Frontend Feature
- User story:
- UI states (loading/empty/error/success):
- Accessibility notes:
- Performance impact:
- Test coverage:
```

### 14.3 Release Readiness Template

```md
## Release Readiness
- Risk level:
- Rollout plan:
- Rollback plan:
- Validation checklist:
- Owner:
```

## 15. Final Rule

Ship software that is understandable, testable, observable, and safe to change.
