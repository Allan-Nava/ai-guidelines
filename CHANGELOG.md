# Changelog

All notable changes to this repository are documented in this file.

The format follows Keep a Changelog and SemVer.

## [Unreleased]

### Changed
- Restructured the guidelines into a **core + domain-delta** model to cut token usage for AI-agent consumption: shared rules (testing, security, CI/CD, Definition of Done, docs, templates) now live only in `guidelines.md`; `software-`, `mobile-`, `devops-`, and `versioning-guidelines.md` keep only domain-specific deltas and cross-reference the core.
- Removed the duplicated DevOps/Terraform/Ansible section from `guidelines.md`; `devops-guidelines.md` is now the single source for Infrastructure-as-Code standards.
- Compressed prose across all guides into dense, bullet-first form without dropping any rule.
- Reworked `README.md` into a router/index (read-order + which-guide table) enabling progressive disclosure.
- Deduplicated `CLAUDE.md` and `AGENTS.md` into complementary roles (non-negotiables/quality vs repository model/workflow).

### Added
- Added a **Spec-Driven Change Flow** to the core (`guidelines.md` §11) plus a Change Proposal template (§16): non-trivial changes are agreed as a versioned spec before code, then archived into living `specs/` and reconciled with the backlog, ADRs, and changelog. Documents the flow (tool-agnostic; aligns with tools such as OpenSpec without mandating any) and adds `specs/`/`changes/` to the repository structure. Renumbered subsequent core sections to §12–§17 and updated the `core §13`→`core §14` (Definition of Done) cross-references in the domain guides.
- Anti-duplication rule and the core/delta repository model, documented in `CLAUDE.md` and `AGENTS.md`.
- Reusable Release Readiness template in the core (`guidelines.md`), extended by mobile releases.

## [0.1.0] - 2026-07-08

### Added
- Expanded `guidelines.md` into a complete end-to-end framework for project management: governance, development, quality gates, CI/CD, documentation, incident management, security, backlog, and decision process.
- Added `CLAUDE.md` with operating instructions for AI agents contributing to this repository.
- Updated `README.md` with repository goal and document map.
- Added `TODO.md` as the single operational backlog with full tracking of tasks, status, and evidence.
- Added explicit policy in the guidelines: always track everything and never push without explicit user request.
- Added `software-guidelines.md` with dedicated software engineering standards for backend and frontend development.
- Added `mobile-guidelines.md` with dedicated standards for mobile application development in Swift/Kotlin, including a default architecture pattern and mandatory TDD.
- Added `devops-guidelines.md` with dedicated DevOps standards, including explicit Terraform and Ansible workflows and validation gates.
- Added `AGENTS.md` as the contributor agent operating guide for quality, traceability, and no-push rules.
- Added `versioning-guidelines.md` with dedicated standards for SemVer, API versioning, mobile release versioning, migrations, and artifact versioning.
- Added automatic issue-tracking policy to the main guidelines and agent rules, including stable IDs, idempotent sync, and auditability requirements.

### Changed
- Converted the main documentation files to English (`README.md`, `guidelines.md`, `CLAUDE.md`, `TODO.md`, `CHANGELOG.md`).
- Removed direct references to external repository names in documentation and kept wording generic.
