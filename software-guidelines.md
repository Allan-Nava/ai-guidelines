# Software Development Guidelines

Backend/frontend specifics. **Base: read `guidelines.md` first.** Shared rules (testing gates, security, CI/CD, Definition of Done, docs, templates) live in the core; this file adds only software-specific deltas.

## 1. Scope & Goals

Align backend/frontend delivery: predictable architecture, low-regression delivery, clear backend↔frontend contracts, strong operability.

## 2. Engineering Principles

- Simplicity over cleverness; behavior explicit and testable.
- Clear boundaries: API, domain, persistence, UI.
- Maintainability before micro-optimization.
- Document decisions that change architecture or contracts (ADR).

## 3. Architecture & Boundaries

- Layers: presentation (controllers/handlers/UI adapters) → application (use cases) → domain (rules/invariants) → infrastructure (db/messaging/external APIs).
- Dependencies point inward toward domain/application; infra details never leak into domain; domain stays portable and framework-light.
- All external contracts versioned (API, events, schemas); breaking changes need migration plan + rollout; prefer additive changes.

## 4. Backend

- **API**: resource-oriented naming, stable IDs, consistent machine-readable error shape, validate at boundaries (fail fast), idempotency for retriable writes.
- **Domain/services**: business logic in services/domain, not controllers; model invariants explicitly; avoid anemic pass-through services.
- **Persistence**: migrations for every schema change (reversible when feasible); explicit deletion/retention policy; avoid N+1 and unbounded scans.
- **Async/integrations**: retry with backoff + max attempts; idempotent consumers; dead-letter + replay procedure; timeouts/circuit breakers for external deps.
- **Observability**: structured logs with correlation IDs; metrics (throughput/latency/errors/saturation); traces for cross-service flows; meaningful, cheap health/readiness endpoints.

## 5. Frontend

- **Information architecture**: organize by domain; explicit UI state ownership; no global state for local concerns.
- **UI**: reusable components with clear contracts; separate data fetching from presentational rendering.
- **A11y/UX**: semantic HTML + accessible labeling; keyboard nav + focus management; contrast + reduced-motion; intentional loading/empty/error states.
- **Performance**: control bundle size; code-split/lazy-load where valuable; prevent excess re-renders; track web vitals with budgets.
- **Browser security**: treat client input/external content as untrusted; prevent XSS (escape/sanitize/safe rendering); protect session/token storage; apply CSP + secure headers.

## 6. Backend↔Frontend Contracts

- Typed, versioned contracts; shared error taxonomy; schema validation in CI for contract changes; coordinate deprecations with a clear timeline; contract tests for critical journeys.

## 7. Testing (deltas over core §5)

- **Backend**: unit (domain), integration (db/adapters), API (contract/serialization), smoke (main prod paths).
- **Frontend**: unit (logic/utils), component (key UI states), e2e (critical journeys), visual regression (stable design systems).

## 8. Code Review Order

1. correctness & user impact → 2. security & data integrity → 3. regressions & edge cases → 4. maintainability & clarity → 5. performance & cost.

Checklist: behavior explicit & test-covered? contracts backward compatible? failure/retry safe? docs updated?

## 9. Definition of Done (core §14 + deltas)

Also: observability impact covered; API/contract compatibility addressed.

## 10. Templates

### API Change

```md
## API Change
- Endpoint(s):
- Request/Response changes:
- Backward compatibility:
- Migration/Rollout plan:
- Monitoring impact:
```

### Frontend Feature

```md
## Frontend Feature
- User story:
- UI states (loading/empty/error/success):
- Accessibility notes:
- Performance impact:
- Test coverage:
```

Release readiness template: see `guidelines.md` → Templates → Release Readiness.

## 11. Final Rule

Ship software that is understandable, testable, observable, and safe to change.
