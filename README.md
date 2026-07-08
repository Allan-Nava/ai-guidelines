# ai-guidelines

Reference guidelines for software, mobile, and DevOps projects. Optimized for AI-agent consumption: read the core once, then load only the domain guide your task needs. All content is in English.

Repository version: `0.1.0`

## Read order

1. [guidelines.md](guidelines.md) — core, cross-cutting rules (read first, always).
2. The one domain guide matching your task (table below).
3. [AGENTS.md](AGENTS.md) / [CLAUDE.md](CLAUDE.md) — rules for contributing to **this** repo.

## Which guide

| Task | Read |
|---|---|
| Any project (baseline) | [guidelines.md](guidelines.md) |
| Backend / frontend code | [software-guidelines.md](software-guidelines.md) |
| iOS / Android apps | [mobile-guidelines.md](mobile-guidelines.md) |
| Infra, Terraform, Ansible | [devops-guidelines.md](devops-guidelines.md) |
| Releases, APIs, migrations, artifacts | [versioning-guidelines.md](versioning-guidelines.md) |

Domain guides contain only their specifics and defer to [guidelines.md](guidelines.md) for shared rules (testing, security, CI/CD, Definition of Done, docs, templates). No rule is duplicated across files.

## Non-negotiables

- Track all work in [TODO.md](TODO.md); update [CHANGELOG.md](CHANGELOG.md) for every relevant change.
- Never commit secrets. Never push without explicit user request.
- Keep all content in English.

## Governance

- [TODO.md](TODO.md) — single backlog with stable IDs, status, evidence.
- [CHANGELOG.md](CHANGELOG.md) — Keep a Changelog + SemVer.
- Backlog may sync to an issue tracker, but `TODO.md` stays the governed source of truth unless documented otherwise.
