# Mobile Application Guidelines

Native iOS (Swift) + Android (Kotlin). **Base: read `guidelines.md` first;** `software-guidelines.md` applies to shared backend↔frontend contracts. This file adds mobile-specific deltas; shared testing/security/CI-CD/DoD/templates live in the core.

## 1. Scope

Native iOS (Swift), native Android (Kotlin), parallel teams sharing product behavior.

## 2. Default Architecture

Unless a strong, documented reason to diverge: Clean Architecture (Domain/Data/Presentation) + MVVM (Presentation) + Repository (Data) + Use Case/Interactor (Domain) + Coordinator/Router (navigation) + Dependency Injection at composition root.

- **Presentation**: UI rendering, state management, view models, navigation intent.
- **Domain**: business rules, use cases, platform-independent models/validation.
- **Data**: network clients, persistence/cache, repository implementations, DTO mapping.
- **Dependency rule**: Presentation → Domain; Data → Domain abstractions; Domain depends on nothing; no platform/framework code in Domain.

## 3. Project Structure

```text
mobile-app/
|- ios/     App/  Features/<Feature>/{Presentation,Domain,Data}  Core/  Tests/
|- android/ app/  feature-<feature>/{presentation,domain,data}  core/  tests/
|- docs/  README.md  CHANGELOG.md  TODO.md
```

- Aligned domain naming/behavior across platforms; isolate platform-specific adapters in data/presentation; explicit feature boundaries.

## 4. TDD (mandatory)

- **Red → Green → Refactor.** No production logic without a failing test first.
- Every bug fix starts with a failing regression test; refactors preserve behavior via existing tests; test names describe behavior, not implementation.
- Pyramid: unit (majority, fast/deterministic) > integration (repo/network/persistence) > UI/flow (critical journeys only).

## 5. iOS (Swift)

- **Stack**: Swift 6+ (or project baseline), SwiftUI default for new features, UIKit interop only when needed, Combine or async/await (consistent per project).
- Views contain no business logic; ViewModels expose immutable state + explicit intents; Use Cases pure/testable; Coordinators own navigation.
- **Tests**: XCTest (unit/integration); snapshot tests where useful; UI tests only for high-value flows (login, checkout, critical forms).

## 6. Android (Kotlin)

- **Stack**: stable Kotlin aligned with toolchain, Jetpack Compose default, XML only for legacy interop, Coroutines + Flow default.
- Composables stateless where possible; ViewModel owns screen state + intent; Use Cases framework-independent; centralized navigation.
- **Tests**: JUnit + coroutine test utils (domain/presentation); integration (Room/network/repo); UI (Compose/Espresso) critical flows only.

## 7. Unidirectional State

UI Intent/Action → ViewModel → Use Case → result → ViewModel emits new immutable UI State → UI re-renders. Predictable, testable, fewer side effects, simpler debugging.

## 8. Mobile-Specific Concerns

- **Contracts**: versioned; shared error taxonomy across iOS/Android; explicit DTO→domain mapping; consistent offline/timeout/retry; contract tests for critical endpoints.
- **Offline/cache/sync**: per-feature data policy (remote-first/cache-first/hybrid); explicit documented invalidation; deterministic conflict resolution; observable, retry-safe background sync.
- **Performance**: startup/render budgets; track launch time + frame drops; never block main thread; lazy-load heavy resources; monitor crashes, ANR (Android), hangs (iOS).
- **Security/privacy** (deltas over core §9): secure storage for tokens (Keychain/Encrypted storage); validate server certs / secure transport; minimize PII collection; consent/privacy flows when required.
- **Observability** (deltas over core §8): crash reporting with release/build metadata; standard analytics taxonomy for key events; auditable feature-flag/remote-config changes.
- **CI/CD** (deltas over core §6): lint + tests each PR; build signed artifacts with secure secret handling; staged rollout; release notes linked to changelog.

## 9. Code Review Order

1. correctness → 2. architecture boundaries → 3. TDD evidence/test quality → 4. performance/memory → 5. security/privacy → 6. maintainability.

Checklist: failing-test-first history for new logic? business logic outside views/composables? unidirectional explicit state? docs/changelog/TODO updated?

## 10. Definition of Done (core §14 + deltas)

Also: TDD cycle complete with passing tests; architecture boundaries respected; critical UI states (loading/empty/error/success); observability hooks added; security/privacy reviewed.

## 11. Templates

### Mobile Feature

```md
## Feature
- User value:
- iOS scope:
- Android scope:
- Domain rules:
- API dependencies:
- Risk level:
```

### TDD Evidence

```md
## TDD Evidence
- Failing test(s) first:
- Minimal implementation commit:
- Refactor commit:
- Final passing suite:
```

### Mobile Release Readiness (extends core Release Readiness)

```md
- Build numbers (iOS/Android):
- Monitoring dashboard links:
```

## 12. Final Rule

Ship mobile software that is test-driven, architecturally disciplined, observable, and safe to evolve.
