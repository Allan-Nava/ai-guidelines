# Project Guidelines (Core)

Canonical, cross-cutting rules for any project. Domain guides (`software-`, `mobile-`, `devops-`, `versioning-guidelines.md`) add only their specifics and defer here for shared rules. Read this first.

## 1. Principles

- Repository is the source of truth: code, docs, decisions, and runbooks live together.
- Every relevant change leaves traceable evidence: what, why, how to validate.
- Automate repeatable work; keep critical steps manually verifiable.
- Separate facts, assumptions, and decisions.
- A green build is not a healthy system: validate real behavior.

## 2. Repository Structure

```text
/
|- README.md            # onboarding + repo map
|- CHANGELOG.md          # Keep a Changelog + SemVer
|- TODO.md               # single backlog with stable IDs
|- guidelines.md         # this core
|- CLAUDE.md / AGENTS.md # agent operating rules
|- docs/
|  |- architecture/  runbooks/  incidents/  reports/  decisions/
|- src/  test/  ci/
```

- `docs/incidents/`: dated post-mortems (`YYYY-MM-DD-slug.md`).
- `docs/reports/`: dated outputs (health checks, audits, verifications).
- `docs/decisions/`: ADRs or technical decision notes.

## 3. Governance & Ownership

- Every area/service has explicit ownership (team or person).
- Every service documents: technical owner, alert channel, triage runbook, health checks.
- Responsibilities are documented, never implicit.
- Tasks and technical debt live in one backlog with stable IDs (see §10).

## 4. Development Conventions

- **Utilities**: small, focused, shared modules; no duplicated generic logic.
- **Constants**: centralize literals; no hardcoded domain strings; use constants/enums for known-value checks.
- **DTOs/contracts**: in dedicated API/DTO modules, not nested service classes; explicit mapping at boundaries.
- **Layering**: handlers depend on services (not repositories); business logic in service/domain; handlers stay thin (parse, validate, map response, handle errors).
- **Dependencies**: required deps non-nullable; model optional deps explicitly; no silent fallbacks that skip persistence or critical integrations.
- **State/caching**: define cache ownership, invalidation/refresh paths, and node-local vs cluster-wide behavior; separate config cache from runtime state.
- **Units**: never mix units (ms/s, bytes/MB, UTC/local); convert at clear, test-covered boundaries.

## 5. Testing & Quality Gates

- Every bug fix ships a test that would have caught it.
- Every feature: unit tests (core logic), boundary integration tests, e2e smoke for critical flows.
- Cover happy and error paths; define warning-vs-failure behavior explicitly.
- Minimum CI gates: lint, tests, build, baseline security scan.
- A merge is valid only when gates pass and docs/changelog are updated when relevant.

## 6. CI/CD & Release

High-risk rollout sequence:

1. Preflight: baseline current state.
2. Dry-run: validate expected diff only.
3. Canary: one node/instance.
4. Rolling rollout: progressive.
5. Post-check: full validation.
6. Smoke tests: real functional verification.
7. Closure: report + changelog + follow-up.

Rules:

- Every release has changelog notes; tag with SemVer (see `versioning-guidelines.md`).
- Rollback strategy is mandatory for irreversible changes.
- No opaque deployments: every step must be reconstructable.
- Infrastructure/IaC specifics: see `devops-guidelines.md`.

## 7. Operational Documentation

Every significant intervention produces a `docs/` entry: Context, Goal, Actions executed, Evidence (key logs/outputs), Outcome, Residual risk & follow-up.

Runbook: prerequisites, step-by-step commands, success criteria, rollback, fast troubleshooting notes.

## 8. Observability & Monitoring

- Every service exposes cheap, repeatable health checks.
- Check scripts use a consistent interface: `--list` (targets), `--mode` (categories), documented exit codes.
- Distinguish checker/systemic failures, operational warnings, and real service failures.
- Archive periodic reports with dated naming; track uptime/MTTR/MTTA where useful.

## 9. Security & Secrets

- Never commit secrets/credentials; use secret managers or protected variables.
- Sanitize logs/reports of sensitive data.
- Validate/sanitize all external input; enforce authorization server-side.
- Least privilege for every integration/identity.
- Dependency scanning; patch vulnerable packages.
- Document credential rotation and emergency procedures.

## 10. Backlog Management

- One backlog source of truth with stable IDs; classify each item (bug/feature/hardening/tech-debt); set priority, owner, milestone.
- Keep planned work separate from operational findings.

### 10.1 Automatic Issue Tracking (when issue automation exists)

- Backlog file stays the source of truth unless documented otherwise.
- Sync is idempotent (no duplicates on rerun); each issue keeps a machine-readable backlink to its backlog item.
- Status transitions are explicit and auditable; closures reflected in both backlog and tracker.
- Automation never silently rewrites scope/priority/meaning and never bypasses TODO/changelog/doc rules.
- Sync jobs document required tokens/scopes/failure modes; on failure, surface it and provide a recoverable manual path.

### 10.2 TODO & Traceability (mandatory)

- All work tracked in `TODO.md` with: ID, description, status, last update, evidence/link.
- No off-record work. Every meaningful change updates `CHANGELOG.md`.
- Agents/automation never push without explicit user request. Track everything.

## 11. Decision Process

- Record important architectural decisions as ADRs: context, options, decision, consequences.
- Mark obsolete decisions superseded, not deleted.

## 12. Onboarding & Collaboration

- Onboarding: read README + guidelines + CLAUDE/AGENTS; set up env; run tests/lint; simulate change + test + changelog; read one recent incident and one runbook.
- Keep PRs small and intent-labeled; review for risk, regressions, and verifiability.

## 13. Definition of Done

A change is done only when: code updated; tests updated/passing; docs updated; changelog updated; risks & rollback clarified; validation evidence available. Domain guides add deltas.

## 14. Anti-Patterns

- Docs drifting from reality; decisions made only in chat; undocumented hotfixes; silent fallbacks hiding failures; monitoring reporting nominal status without context.

## 15. Templates

### Operational Report

```md
# <Title>
## Context
## Goal
## Actions Executed
## Evidence
## Outcome
## Follow-up
```

### Incident

```md
# YYYY-MM-DD - <slug>
## Symptom
## Impact
## Root Cause
## Timeline
## Mitigation
## Preventive Actions
```

### ADR

```md
# ADR-<n> - <title>
## Context
## Options
## Decision
## Consequences
```

### Release Readiness

```md
## Release Readiness
- Risk level:
- Rollout plan:
- Rollback plan:
- Validation checklist:
- Owner:
```

## 16. Final Rule

Prefer clarity, traceability, and repeatability over apparent speed. If a change cannot be explained and validated by someone who was not present, the process is not mature yet.
