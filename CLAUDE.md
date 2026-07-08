# CLAUDE.md

Operating instructions for AI agents working on **this** repository. Full contributor workflow: `AGENTS.md`.

## Mission

Keep this repo a reliable, token-efficient reference of engineering guidelines. Every change must improve clarity, operational quality, decision traceability, or cross-document consistency.

## Non-negotiables

1. Track all work in `TODO.md` (ID, status, last update, evidence).
2. Update `CHANGELOG.md` (`Unreleased`) for every relevant change.
3. Never commit secrets/credentials.
4. Never push without explicit user request.
5. Keep all content in English.
6. No duplication: shared rules live only in `guidelines.md`; domain guides defer to it. Extend, don't restate.
7. No unverifiable or conflicting content.

## Content Quality

Specific, actionable, measurable, consistent. Action-oriented technical writing; descriptive titles; concrete bullets; explicit trade-offs; no empty buzzwords.

## Priority Order For Improvements

1. Definition of Done, quality gates, test strategy.
2. Incident management, runbooks, rollback.
3. Changelog, release process, ownership.
4. Security & secret handling.
5. Onboarding & maintainability.

## Before Delivering

1. Consistency across README, `guidelines.md`, domain guides, and CLAUDE/AGENTS.
2. `CHANGELOG.md` updated under `Unreleased`.
3. Templates/examples valid and reusable.
4. Redundancy reduced (no content that belongs in the core).
5. `TODO.md` updated for touched tasks.

## Do Not

Rewrite whole docs without need; change tone/structure without reason; remove a rule without an equivalent replacement; add conflicting policies.
