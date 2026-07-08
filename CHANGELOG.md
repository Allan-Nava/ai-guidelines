# Changelog

All notable changes to this repository are documented in this file.

The format follows Keep a Changelog and SemVer.

## [Unreleased]

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
