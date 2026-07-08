# Project Rules

These rules capture conventions that should be preserved during refactors and new development.

## Utilities

- Shared, reusable data structures and generic helpers belong in the project utilities package.
- Prefer focused utility classes over broad catch-all classes.
- Avoid duplicating generic logic when a shared helper would express the behavior once.

## Constants

- Do not use raw domain strings in production code when a constant exists.
- Centralize domain literals in a constants class or equivalent module.
- The constants module should be the only production-code place where repeated domain string values are defined.
- Avoid hard-coded string comparisons for known domain values.

## DTOs and API Contracts

- DTOs should live in the DTO/API package.
- Do not define public API DTOs as nested classes or records inside services.
- Service-local records/classes are allowed only for private implementation details.
- Keep DTO mapping explicit at service boundaries so handlers/controllers stay thin.

## Handlers and Services

- HTTP/API handlers should depend on services, not repositories or low-level persistence objects.
- Repositories belong inside services or infrastructure composition code.
- Handlers should focus on request parsing, validation, response mapping, and exception handling.
- Business behavior belongs in services, resolvers, or domain components.

## Runtime State and Caching

- Be explicit about what each cache owns.
- Do not mix catalog/configuration caches with per-entity runtime state.
- When data can change at runtime, define a clear refresh or invalidation path.
- If cache refresh is cluster-wide or node-local, make that behavior obvious in naming and code.
- Avoid stale-state mechanisms unless they protect real runtime state that cannot be refreshed by another cache.

## Session and Protocol Behavior

- Session creation and session update paths should be handled distinctly.
- Requests that reference an existing session should return a clear unknown-session error when the session is missing.
- Duplicate session creation requests should have a defined behavior: either idempotent reuse, rejection, or replacement.
- Protocol error behavior should be documented and covered by tests.

## Reuse and Refactoring

- Prefer small, behavior-preserving refactors.
- Move common code only when the abstraction is already repeated or clearly reusable.
- Keep domain-specific filtering, ranking, and ordering near the component that owns that behavior.
- When rebuilding helper method signatures, put business/context arguments first and dependencies last.

## Dependencies

- Required dependencies should not be nullable in production code.
- Avoid fallback branches that silently skip persistence, external calls, or important behavior because a dependency is missing.
- If a dependency is optional, model that explicitly.

## Units and Boundaries

- Be explicit when values use different units or representations.
- Do not mix units silently.
- Put conversions at clear boundaries and cover them with tests.
