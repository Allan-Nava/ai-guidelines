# Project Guidelines

Complete guidelines for managing software projects in a scalable, readable, and operable way.

This document captures practices from production-oriented environments, with focus on:

- repository as the source of truth,
- process and output standardization,
- delivery quality,
- incident management and continuous improvement.

## 1. Foundational Principles

1. The repository is the official working system: code, documentation, decisions, and runbooks must live together.
2. Every relevant change must leave traceable evidence: what was done, why, and how to validate it.
3. Automate whenever possible, but keep critical steps manually verifiable.
4. Clearly separate facts, assumptions, and decisions.
5. A green build is not automatically a healthy system: always validate real behavior.

## 2. Minimum Repository Structure

Recommended structure:

```text
/
|- README.md
|- CHANGELOG.md
|- TODO.md
|- guidelines.md
|- CLAUDE.md
|- docs/
|  |- architecture/
|  |- runbooks/
|  |- incidents/
|  |- reports/
|  |- decisions/
|- src/
|- test/
|- ci/
```

Rules:

- `README.md`: quick onboarding and repository map.
- `CHANGELOG.md`: Keep a Changelog + SemVer.
- `TODO.md`: single operational backlog with complete tracking.
- `docs/runbooks/`: planned operational procedures.
- `docs/incidents/`: dated post-mortems (`YYYY-MM-DD-slug.md`).
- `docs/reports/`: periodic or ad-hoc outputs (health checks, audits, verifications).
- `docs/decisions/`: ADRs or technical decision notes.

## 3. Governance And Ownership

1. Every area must have explicit ownership (team or person).
2. Every service must have:
   - technical owner,
   - alert channel,
   - triage runbook,
   - health checks.
3. Responsibilities must be documented, never implicit.
4. TODO items and technical debt must live in one project backlog with stable IDs.

## 4. Development Conventions

### 4.1 Utilities

- Place shared helpers in dedicated modules.
- Prefer small, focused utilities over catch-all classes.
- Avoid duplicating generic logic.

### 4.2 Constants

- Do not hardcode domain strings when constants exist.
- Centralize literals in a single constants module.
- Use constants/enums for known value comparisons.

### 4.3 DTOs And API Contracts

- Keep DTOs in dedicated API/DTO modules.
- Do not define public DTOs as nested service classes.
- Keep mapping explicit at boundaries (handler/service).

### 4.4 Layering

- Handlers/APIs depend on services, not repositories.
- Business logic belongs in service/domain layers.
- Handlers stay thin: parsing, validation, response mapping, error handling.

### 4.5 Dependencies

- Required dependencies must not be nullable.
- Do not allow silent fallbacks that skip critical integrations or persistence.
- Model optional dependencies explicitly.

### 4.6 Runtime State And Caching

- Define ownership of every cache.
- Separate configuration caches from runtime state.
- Define explicit invalidation/refresh paths.
- Make node-local vs cluster-wide behavior explicit.

### 4.7 Units And Conversion Boundaries

- Do not mix units (ms/s, bytes/MB, UTC/local time).
- Keep conversions at clear boundaries and cover them with tests.

## 5. Testing And Quality Gates

1. Every bug fix must include at least one test that would have caught the bug.
2. Every new feature should include:
   - unit tests for core logic,
   - boundary integration tests,
   - end-to-end smoke tests for critical workflows.
3. Cover both happy paths and error paths.
4. Define warning vs failure behavior explicitly.
5. Add minimum CI gates:
   - lint,
   - tests,
   - build,
   - baseline security scanning.

## 6. CI/CD And Release

### 6.1 Deployment Strategy

Recommended pipeline for high-risk changes:

1. Preflight: baseline current system state.
2. Dry-run: validate expected diff only.
3. Canary: roll out to one node/instance.
4. Rolling rollout: progressive deployment.
5. Post-check: full validation.
6. Smoke tests: real functional verification.
7. Closure: report + changelog + follow-up.

### 6.2 Operational Rules

- Every release must have changelog notes.
- Tag releases with SemVer when applicable.
- Rollback strategy is mandatory for irreversible changes.
- Avoid opaque deployments: all steps must be reconstructable.

## 7. Operational Documentation

### 7.1 Documentation Standard

Every significant intervention must produce a document in `docs/` with:

1. Context.
2. Goal.
3. Actions executed.
4. Evidence (key logs/outputs).
5. Outcome.
6. Residual risks and follow-up.

### 7.2 Runbooks

A runbook must include:

- prerequisites,
- step-by-step commands,
- success criteria,
- rollback,
- fast troubleshooting notes.

### 7.3 Incident Post-Mortem

Minimum format:

1. Date and severity.
2. Observed symptom.
3. Impact.
4. Root cause.
5. Timeline.
6. Immediate mitigation.
7. Preventive actions.
8. Links to logs/evidence.

## 8. Observability And Monitoring

1. Every service must expose repeatable health checks.
2. Check scripts must have consistent interfaces:
   - `--list` for targets,
   - `--mode` for check categories,
   - documented exit codes.
3. Clearly distinguish:
   - checker/systemic failures,
   - operational warnings,
   - real service failures.
4. Archive periodic reports using dated naming.
5. Track reliability metrics where useful (uptime, MTTR, MTTA).

## 9. Security And Secrets

1. Never commit secrets or credentials.
2. Use secret managers or protected variables for tokens/keys.
3. Sanitize logs and reports for sensitive data.
4. Apply least privilege to every integration.
5. Document credential rotation and emergency procedures.

## 10. Backlog Management

1. Use one source of truth for backlog (file or board with stable IDs).
2. Classify each item (bug, feature, hardening, technical debt).
3. Define priority, owner, and milestone.
4. Keep planned work separate from operational findings.
5. Keep sync idempotent when issue automation exists.

### 10.1 Mandatory Operating Rule: TODO And Full Traceability

1. All work must be tracked in `TODO.md` with updated status.
2. Every task must include at least: ID, description, status, last update, and evidence/link.
3. No off-record work (no chat-only tasks without backlog entry).
4. Every meaningful content change must update `CHANGELOG.md`.
5. Automatic pushes by agents/automations are forbidden unless explicitly requested by the user.
6. Permanent rule: track everything, never push without explicit user instruction.

## 11. Decision Process

1. Important architectural decisions must be recorded (ADR).
2. Every ADR must include:
   - context,
   - options considered,
   - decision,
   - consequences.
3. Obsolete decisions should be marked as superseded, not deleted.

## 12. Onboarding And Collaboration

Minimum onboarding checklist:

1. Read `README.md`, `guidelines.md`, and `CLAUDE.md`.
2. Configure local environment.
3. Run tests and lint.
4. Simulate a standard flow (change + test + changelog update).
5. Read at least one recent incident and one runbook.

Collaboration rules:

- Keep PRs small and focused.
- Use clear, intent-based commit messages.
- Review with focus on risk, regressions, and verifiability.

## 13. Definition Of Done

A change is complete only when:

1. code is updated,
2. relevant tests are updated/passing,
3. documentation is updated,
4. changelog is updated,
5. risks and rollback are clarified,
6. validation evidence is available.

## 14. Anti-Patterns To Avoid

- Documentation that drifts from code/reality.
- Critical decisions made only in chat and not recorded.
- Undocumented hotfixes.
- Silent fallbacks that hide real failures.
- Monitoring that reports nominal status without context.

## 15. Quick Templates

### 15.1 Operational Report Template

```md
# <Title>

## Context

## Goal

## Actions Executed

## Evidence

## Outcome

## Follow-up
```

### 15.2 Incident Template

```md
# YYYY-MM-DD - <slug>

## Symptom

## Impact

## Root Cause

## Timeline

## Mitigation

## Preventive Actions
```

### 15.3 ADR Template

```md
# ADR-<number> - <title>

## Context

## Options

## Decision

## Consequences
```

## 16. Final Rule

Always prefer clarity, traceability, and repeatability over apparent speed.

If a change cannot be explained and validated by someone who was not present, the process is not mature yet.
