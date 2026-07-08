# TODO

Single operational backlog for the `ai-guidelines` repository.

Rules:
- Track everything: every activity must be recorded here.
- Update status, date, and notes on every progress step.
- Attach evidence (updated files or documented decisions).
- Never push without explicit user request.

Status legend:
- `TODO`: not started
- `IN_PROGRESS`: currently being worked on
- `BLOCKED`: blocked by dependency
- `DONE`: completed

## Master TODO

| ID | Area | Task | Status | Evidence | Last Update |
|---|---|---|---|---|---|
| AG-001 | Foundation | Define complete project management guidelines | DONE | `guidelines.md` | 2026-07-08 |
| AG-002 | Agent Rules | Define operating rules for agents in `CLAUDE.md` | DONE | `CLAUDE.md` | 2026-07-08 |
| AG-003 | Repo Docs | Align `README.md` as official content map | DONE | `README.md` | 2026-07-08 |
| AG-004 | Tracking | Create changelog aligned with Keep a Changelog | DONE | `CHANGELOG.md` | 2026-07-08 |
| AG-005 | Governance | Add mandatory rule: always track everything | DONE | `guidelines.md` section 10.1 | 2026-07-08 |
| AG-006 | Governance | Add mandatory rule: never push without explicit request | DONE | `guidelines.md` section 10.1 + `CLAUDE.md` | 2026-07-08 |
| AG-007 | Process | Define operational templates (report, incident, ADR) | DONE | `guidelines.md` section 15 | 2026-07-08 |
| AG-008 | Improvement | Prepare `docs/` structure with starter templates | TODO | to be planned | 2026-07-08 |
| AG-009 | Improvement | Define backlog priority convention (`P0/P1/P2`) | TODO | to be planned | 2026-07-08 |
| AG-010 | Improvement | Add complete workflow example: task -> change -> changelog | TODO | to be planned | 2026-07-08 |
| AG-011 | Docs Policy | Remove repository references from documentation and keep wording generic | DONE | `README.md`, `guidelines.md` | 2026-07-08 |
| AG-012 | Software Docs | Create dedicated backend/frontend software development guidelines | DONE | `software-guidelines.md`, `README.md` | 2026-07-08 |
| AG-013 | Mobile Docs | Create dedicated mobile app guidelines (Swift/Kotlin, architecture pattern, mandatory TDD) | DONE | `mobile-guidelines.md`, `README.md` | 2026-07-08 |
| AG-014 | DevOps Docs | Create dedicated DevOps guidelines including Terraform and Ansible standards | DONE | `devops-guidelines.md`, `README.md` | 2026-07-08 |
| AG-015 | Agent Docs | Create `AGENTS.md` with contributor operating rules | DONE | `AGENTS.md`, `README.md` | 2026-07-08 |
| AG-018 | Tracking Docs | Add automatic issue tracking policy and backlog sync rules | DONE | `guidelines.md`, `AGENTS.md`, `README.md` | 2026-07-08 |
| AG-017 | Versioning Docs | Create dedicated versioning guidelines for releases, APIs, mobile builds, and artifacts | DONE | `versioning-guidelines.md`, `README.md` | 2026-07-08 |
| AG-019 | Repo Versioning | Assign initial repository version and record it consistently | DONE | `VERSION`, `CHANGELOG.md`, `README.md` | 2026-07-08 |
| AG-020 | Optimization | Restructure guides into core + domain-delta model, compress prose, add README router to reduce token usage | DONE | all `*-guidelines.md`, `README.md`, `CLAUDE.md`, `AGENTS.md`, `CHANGELOG.md` | 2026-07-08 |

## Permanent Rule

Traceability must always be active and complete in this file.
No push must be executed by agents/automations without explicit user request.
