# Versioning Guidelines

Versioning for apps, libraries, APIs, schemas, mobile, DB migrations, and artifacts. **Base: read `guidelines.md` first;** changelog and Definition of Done rules live in the core.

## 1. Scope

App releases, libraries/packages, APIs & event schemas, mobile app versions, DB/migration versioning, container images & infra artifacts.

## 2. Principles

- Version communicates change impact; every release traceable to code + changelog + validation evidence.
- Versioning supports safe rollout/rollback/compatibility; breaking changes explicit, documented, planned; consistent across repos/teams.

## 3. SemVer (default) — `MAJOR.MINOR.PATCH`

- **MAJOR**: incompatible/breaking change. **MINOR**: backward-compatible feature. **PATCH**: backward-compatible fix or safe internal change.
- Pre-release suffixes: `-alpha.N` (internal, incomplete), `-beta.N` (feature-complete, wider testing), `-rc.N` (release candidate, blocker fixes only).

## 4. Changelog & Traceability (deltas over core)

- Every release maps to a committed changelog state and describes user/operator-facing impact.
- Categories: Added, Changed, Fixed, Deprecated, Removed, Security.

## 5. Tags & Artifacts

- Annotated tags, consistent format `vX.Y.Z`; reproducible from source; artifacts carry exact version + commit reference; never reuse/move published tags.

## 6. API Versioning

- Version public APIs explicitly; prefer additive; deprecate before removal with documented period + migration path.
- **Breaking** if it: removes a field/endpoint/event, changes semantics of existing behavior, changes required validation, changes response shape incompatibly, or breaks auth behavior for clients.
- **Strategy**: pick one and use it consistently — URL (`/v1/...`) or header/media-type; don't mix without a clear reason.

## 7. Event & Schema Versioning

- Version schemas explicitly; consumers tolerate additive fields; breaking changes need coordinated rollout; compatibility matrix for critical flows.

## 8. Database & Migrations

- Every schema change has a tracked, ordered, deterministic, reviewable migration; prefer backward-compatible in rolling deploys; destructive migrations need approval + rollback; separate data vs schema migrations when risk is high.
- Rollout order: expand schema → deploy compatible code → migrate data if needed → contract/remove old schema only after a safe window.

## 9. Mobile Versioning

- Marketing version (user-facing, SemVer where practical) + monotonic build number/code (must always increase).
- Document breaking backend-contract dependencies in release notes; align app versioning with API compatibility windows.

## 10. Infrastructure & Artifacts

- **Images**: immutable version tag; `latest` is not a deployment contract; keep commit SHA/build metadata; preserve artifact identity across environment promotion.
- **Terraform modules**: version explicitly; pin module + provider in consumers; upgrade notes for breaking changes.
- **Ansible roles**: version when reused across repos/environments; track platform/OS compatibility; release notes + rollout guidance for breaking behavior.

## 11. Branching & Release Flow

- Merge reviewed changes into the main integration branch; stabilize on a short-lived release branch only when needed; tag after validation passes; record release notes + changelog before publication; patch production from the correct branch lineage.
- Avoid long-lived divergence; keep release branches short-lived; hotfixes update both release history and mainline.

## 12. Deprecation

- Mark deprecated behavior explicitly; document removal target version/window; communicate migration path early; monitor active usage before removal.

## 13. Validation (core §14 + deltas)

Also: version bump matches real impact; tag strategy respected.

## 14. Anti-Patterns

- Version numbers without defined meaning; breaking changes in patch/minor; reused tags; releases without changelog; `latest` as the only deployable reference; coupling API/app/DB changes without a compatibility strategy.

## 15. Templates

### Release Entry

```md
## Release X.Y.Z
- Type: major/minor/patch
- Summary:
- Breaking changes:
- Migration notes:
- Rollback notes:
```

### Deprecation Notice

```md
## Deprecation Notice
- Component:
- Deprecated in version:
- Removal target:
- Replacement:
- Migration path:
```

### Compatibility Note

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
