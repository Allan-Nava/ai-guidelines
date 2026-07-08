# Mobile Application Development Guidelines

Practical standards for building production-grade mobile applications with Swift (iOS) and Kotlin (Android), using a clear architectural pattern and mandatory TDD.

## 1. Scope

These guidelines apply to:

- native iOS applications (Swift),
- native Android applications (Kotlin),
- teams working in parallel on both platforms with shared product behavior.

## 2. Default Architecture Pattern

Use this default pattern unless there is a strong, documented reason to diverge:

- Clean Architecture (Domain, Data, Presentation),
- MVVM in Presentation,
- Repository pattern in Data,
- Use Case / Interactor pattern in Domain,
- Coordinator/Router for navigation flow,
- Dependency Injection at composition root.

### 2.1 Layer Responsibilities

- Presentation:
  - UI rendering and user interaction,
  - state management,
  - view models and navigation intent.
- Domain:
  - business rules,
  - use cases,
  - platform-independent models and validation.
- Data:
  - network clients,
  - persistence/cache,
  - repository implementations,
  - DTO mapping.

### 2.2 Dependency Rule

- Presentation depends on Domain.
- Data depends on Domain abstractions.
- Domain depends on nothing outside itself.
- Platform/framework code must not leak into Domain.

## 3. Project Structure

Recommended feature-first structure:

```text
mobile-app/
|- ios/
|  |- App/
|  |- Features/
|  |  |- <Feature>/
|  |  |  |- Presentation/
|  |  |  |- Domain/
|  |  |  |- Data/
|  |- Core/
|  |- Tests/
|- android/
|  |- app/
|  |- feature-<feature>/
|  |  |- presentation/
|  |  |- domain/
|  |  |- data/
|  |- core/
|  |- tests/
|- docs/
|- README.md
|- CHANGELOG.md
|- TODO.md
```

Rules:

- Keep domain naming and behavior aligned across platforms.
- Keep platform-specific adapters isolated in data/presentation.
- Keep feature boundaries explicit.

## 4. TDD Is Mandatory

All production logic must follow TDD.

### 4.1 TDD Cycle

1. Red: write a failing test for expected behavior.
2. Green: implement the smallest code to pass the test.
3. Refactor: improve design while keeping tests green.

### 4.2 TDD Rules

- No production logic without a failing test first.
- Every bug fix starts with a failing regression test.
- Refactors must preserve behavior via existing tests.
- Test names must describe behavior, not implementation details.

### 4.3 Test Pyramid

- Unit tests: majority, fast and deterministic.
- Integration tests: repository, network boundaries, persistence.
- UI/flow tests: critical user journeys only.

## 5. iOS (Swift) Standards

### 5.1 Stack

- Swift 6+ (or project baseline),
- SwiftUI by default for new features,
- UIKit interoperability only when needed,
- Combine or async/await for async flows (project decision must be consistent).

### 5.2 iOS Architecture Rules

- Views contain no business logic.
- ViewModels expose immutable state + explicit intents/actions.
- Use Cases are pure and testable.
- Coordinators own navigation orchestration.

### 5.3 iOS Testing

- XCTest for unit/integration tests.
- Snapshot tests for stable visual components where useful.
- UI tests only for high-value flows (login, checkout, critical forms).

## 6. Android (Kotlin) Standards

### 6.1 Stack

- Kotlin stable version aligned with toolchain,
- Jetpack Compose by default for new features,
- XML views only for legacy interop,
- Coroutines + Flow as default async model.

### 6.2 Android Architecture Rules

- Composables are stateless whenever possible.
- ViewModel owns screen state and intent handling.
- Use Cases remain framework-independent.
- Navigation is centralized (Navigation component/router abstraction).

### 6.3 Android Testing

- JUnit + coroutine test utilities for domain/presentation tests.
- Integration tests for Room/network/repository boundaries.
- UI tests (Compose UI/Espresso) only for critical flows.

## 7. State Management Pattern

Use a unidirectional flow on both platforms:

1. UI emits Intent/Action.
2. ViewModel processes action.
3. ViewModel calls Use Case.
4. Use Case returns result.
5. ViewModel emits new immutable UI State.
6. UI re-renders from state.

Benefits:

- predictable behavior,
- easier testing,
- fewer side effects,
- simpler debugging.

## 8. API And Contract Consistency

1. Maintain versioned API contracts.
2. Share error taxonomy across iOS and Android.
3. Keep DTO-to-domain mapping explicit.
4. Handle offline, timeout, and retry consistently.
5. Add contract tests for critical endpoints.

## 9. Offline, Cache, And Sync

1. Define per-feature data source policy (remote-first, cache-first, or hybrid).
2. Cache invalidation must be explicit and documented.
3. Sync conflicts require deterministic resolution strategy.
4. Background sync must be observable and retry-safe.

## 10. Performance And Reliability

1. Define startup and screen rendering budgets.
2. Measure and track app launch time and frame drops.
3. Avoid blocking the main thread.
4. Use lazy loading for heavy resources.
5. Monitor crashes, ANR (Android), and hangs (iOS).

## 11. Security And Privacy

1. Never store secrets in source code.
2. Use secure storage for tokens/credentials (Keychain/Encrypted storage).
3. Validate server certificates and secure transport.
4. Minimize personally identifiable data collection.
5. Ensure privacy policy and consent flows are implemented when required.

## 12. Observability

1. Structured client logs with correlation IDs where possible.
2. Standard analytics taxonomy for key events.
3. Crash reporting with release/build metadata.
4. Feature flags and remote config changes must be auditable.

## 13. CI/CD For Mobile

1. Run lint + tests on every pull request.
2. Build signed artifacts in CI with secure secret handling.
3. Use staged rollout for production releases.
4. Keep rollback plan for each release.
5. Publish release notes linked to changelog entries.

## 14. Code Review Checklist

Review in this order:

1. behavior correctness,
2. architecture boundary respect,
3. TDD evidence and test quality,
4. performance and memory impact,
5. security/privacy impact,
6. readability and maintainability.

Mandatory checks:

- Is there a failing-test-first history for new logic?
- Is business logic outside views/composables/controllers?
- Is state flow unidirectional and explicit?
- Are docs/changelog/TODO updated?

## 15. Definition Of Done For Mobile Features

A mobile feature is done only when:

1. TDD cycle completed with passing tests,
2. architecture boundaries respected,
3. critical UI states implemented (loading, empty, error, success),
4. observability hooks added,
5. security/privacy review completed,
6. docs, changelog, and TODO updated.

## 16. Quick Templates

### 16.1 Mobile Feature Template

```md
## Feature
- User value:
- iOS scope:
- Android scope:
- Domain rules:
- API dependencies:
- Risk level:
```

### 16.2 TDD Evidence Template

```md
## TDD Evidence
- Failing test(s) first:
- Minimal implementation commit:
- Refactor commit:
- Final passing suite:
```

### 16.3 Release Readiness Template

```md
## Mobile Release Readiness
- Build numbers (iOS/Android):
- Test summary:
- Rollout plan:
- Rollback plan:
- Monitoring dashboard links:
```

## 17. Final Rule

Ship mobile software that is test-driven, architecturally disciplined, observable, and safe to evolve.
