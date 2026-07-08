# AGENTS.md

Contributor guide for any agent (Claude, Copilot, or equivalent) editing this repository. Non-negotiables, quality bar, and priorities: `CLAUDE.md`.

## Repository Model

- `guidelines.md` = canonical core (shared, cross-cutting rules).
- `software-`, `mobile-`, `devops-`, `versioning-guidelines.md` = domain deltas that defer to the core.
- No rule is duplicated; extend the right file instead of restating.

## Workflow

1. Read the relevant docs first (core + target guide).
2. Make the smallest valid change set.
3. Shared rule → `guidelines.md`; domain-specific rule → domain guide with a cross-reference to the core.
4. Record changes in `TODO.md` and `CHANGELOG.md` (`Unreleased`).
5. Validate consistency across README, core, and domain guides.

## Deliverables Per Relevant Change

1. Updated target doc(s).
2. Updated `TODO.md` entry.
3. Updated `CHANGELOG.md` entry.
4. One-line summary of what changed and why.
5. Updated issue-tracking references when automatic sync is used.

## Do Not

Duplicate content across guides; introduce conflicting policies; leave work untracked; push without explicit user request.
