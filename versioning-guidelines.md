# Versioning Guidelines

Practical standards for versioning software, APIs, infrastructure artifacts, and releases in a predictable and low-risk way.

## 1. Scope

These rules apply to:

- application releases,
- libraries and shared packages,
- APIs and event schemas,
- mobile app versions,
- database and migration versioning,
- container images and infrastructure artifacts.

## 2. Core Principles

1. Version numbers must communicate change impact clearly.
2. Every released version must be traceable to code, changelog, and validation evidence.
3. Versioning must support safe rollout, rollback, and compatibility decisions.
4. Breaking changes must be explicit, documented, and planned.
5. Versioning rules must be consistent across repositories and teams.

## 3. Default Versioning Model

Use Semantic Versioning by default:

`MAJOR.MINOR.PATCH`

- `MAJOR`: incompatible or breaking change.
- `MINOR`: backward-compatible feature addition.
- `PATCH`: backward-compatible fix, correction, or safe internal improvement.

### 3.1 SemVer Decision Rules

Increase:

- `PATCH` when fixing defects without changing supported behavior contracts.
- `MINOR` when adding backward-compatible functionality.
- `MAJOR` when changing or removing supported behavior in a way that breaks consumers.

### 3.2 Pre-release Versions

Use pre-release suffixes for unstable builds:

- `1.4.0-alpha.1`
- `1.4.0-beta.2`
- `1.4.0-rc.1`

Rules:

- `alpha`: incomplete, internal validation only.
- `beta`: feature-complete, wider testing.
- `rc`: release candidate, only release-blocker fixes allowed.

## 4. Changelog And Release Traceability

1. Every relevant release must have a changelog entry.
2. Changelog entries must describe user-facing or operator-facing impact.
3. Every version tag must map to a committed changelog state.
4. Release notes must reference validation, rollout, and rollback information when relevant.

Minimum changelog categories:

- Added
- Changed
- Fixed
- Deprecated
- Removed
- Security

## 5. Git Tags And Release Artifacts

1. Use annotated tags for releases.
2. Tag format should be consistent, typically `vX.Y.Z`.
3. Every tagged release must be reproducible from source.
4. Build artifacts must include the exact version and commit reference.
5. Never reuse or move release tags after publication.

## 6. API Versioning

### 6.1 Public API Rules

1. Version public APIs explicitly.
2. Prefer additive changes over breaking changes.
3. Deprecate before removal whenever possible.
4. Document deprecation period and migration path.

### 6.2 Breaking Change Policy

A change is breaking if it:

- removes a field/endpoint/event,
- changes semantics of existing behavior,
- changes required validation rules,
- changes response shape incompatibly,
- changes authentication/authorization behavior in a way that breaks clients.

### 6.3 Recommended API Versioning Strategy

Prefer one stable strategy and use it consistently:

- URL versioning: `/v1/...`
- header/media-type versioning when the platform already supports it

Do not mix strategies without a clear reason.

## 7. Event And Schema Versioning

1. Version event schemas explicitly.
2. Consumers must tolerate additive fields where possible.
3. Breaking schema changes require coordinated rollout.
4. Producers and consumers must have compatibility matrix documentation for critical flows.

## 8. Database And Migration Versioning

1. Every schema change must have a tracked migration.
2. Migrations must be ordered, deterministic, and reviewable.
3. Prefer backward-compatible migrations in rolling deployments.
4. Destructive migrations require explicit approval and rollback planning.
5. Data migrations and schema migrations must be separated when risk is high.

Recommended rollout order:

1. expand schema,
2. deploy compatible application code,
3. migrate data if needed,
4. contract/remove old schema only after safe window.

## 9. Mobile Application Versioning

Use two separate concepts:

- marketing version: user-facing release (`2.3.0`)
- build number/code: internal monotonic build identifier

Rules:

1. Marketing version follows SemVer where practical.
2. Build number/code must always increase.
3. Breaking backend contract dependencies must be documented in release notes.
4. Coordinate app release versioning with API compatibility windows.

## 10. Infrastructure And Artifact Versioning

### 10.1 Container Images

1. Every image must have an immutable version tag.
2. `latest` is not a deployment contract.
3. Keep commit SHA or build metadata traceable.
4. Promotion across environments should preserve artifact identity.

### 10.2 Terraform And Infrastructure Modules

1. Version reusable Terraform modules explicitly.
2. Pin module and provider versions in consumers.
3. Document upgrade notes for breaking infrastructure module changes.

### 10.3 Ansible Roles And Playbooks

1. Version shared Ansible roles when reused across multiple repositories/environments.
2. Track role compatibility with supported platforms or OS versions.
3. Breaking operational behavior requires release notes and rollout guidance.

## 11. Branching And Release Flow

Recommended release flow:

1. Merge reviewed changes into the main integration branch.
2. Stabilize on a release branch only when needed for coordination or hardening.
3. Tag the release after validation passes.
4. Record release notes and changelog before publication.
5. Patch production from the correct branch lineage to keep history coherent.

Rules:

- Avoid long-lived divergence without reason.
- Keep release branches short-lived.
- Hotfixes must update both release history and ongoing mainline where relevant.

## 12. Deprecation Policy

1. Mark deprecated behavior explicitly.
2. Document removal target version or time window.
3. Communicate migration path early.
4. Monitor active usage before removal when possible.

## 13. Validation And Governance

A versioned release is valid only when:

1. version bump matches real impact,
2. changelog is updated,
3. tests and validation checks passed,
4. release tag strategy is respected,
5. rollback path is known,
6. TODO and documentation are updated when relevant.

## 14. Anti-Patterns To Avoid

- Using version numbers without defined meaning.
- Shipping breaking changes in patch/minor releases.
- Reusing tags.
- Publishing releases without changelog entries.
- Using `latest` as the only deployable artifact reference.
- Coupling API, app, and database changes without compatibility strategy.

## 15. Quick Templates

### 15.1 Release Entry Template

```md
## Release X.Y.Z
- Type: major/minor/patch
- Summary:
- Breaking changes:
- Migration notes:
- Rollback notes:
```

### 15.2 Deprecation Template

```md
## Deprecation Notice
- Component:
- Deprecated in version:
- Removal target:
- Replacement:
- Migration path:
```

### 15.3 Compatibility Note Template

```md
## Compatibility Note
- Component version:
- Depends on:
- Compatible with:
- Not compatible with:
- Rollout order:
```

## 16. Final Rule

A version is part of the product contract, not just a label. Treat it as an operational and engineering promise.
