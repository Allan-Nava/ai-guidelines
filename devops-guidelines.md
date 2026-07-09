# DevOps Guidelines

IaC, platform operations, reliable delivery. **Base: read `guidelines.md` first.** This file is the single source for Terraform + Ansible standards; shared security/observability/incident/CI-CD-sequence/DoD rules live in the core.

## 1. Scope

Infra provisioning & lifecycle, configuration management, release/rollback operations, platform observability & incident response.

## 2. Core DevOps Principles

- Infra reproducible & declarative; every change reviewable, testable, reversible.
- Production safety over speed; automate repeatable operations; operational evidence for every relevant intervention.

## 3. Repository Standards

```text
/
|- terraform/  modules/  environments/
|- ansible/    inventory/  group_vars/  host_vars/  roles/  playbooks/
|- docs/       runbooks/  incidents/  reports/
|- README.md  CHANGELOG.md  TODO.md
```

- Explicit environment separation (dev/staging/prod); secrets outside versioned files; consistent naming across tools.

## 4. Terraform

- **State**: remote + locking; isolated per environment/critical stack; least-privilege access; backed up with a documented restore procedure.
- **Modules**: focused & reusable; explicit typed inputs/outputs; no hidden inter-module dependencies; pinned provider versions + documented upgrade paths.
- **Workflow**: (1) `terraform fmt` + `validate`; (2) `plan` with clear diff + peer review for risky changes; (3) apply only reviewed plans with environment approval rules; (4) post-apply smoke checks + documented outcome/residual risk.
- **Policy/safety**: destructive changes need explicit approval; force-recreate needs a rollback plan; scheduled drift detection; mandatory resource tagging.
- **Testing**: fmt/validate/lints; security/compliance policy gates; module tests for critical shared modules; plan-level regression review for sensitive stacks.

## 5. Ansible

- **Inventory/vars**: environment-scoped inventories; consistent `group_vars`/`host_vars`; no variable shadowing or undocumented overrides; retired hosts in explicit archive/legacy inventory.
- **Roles**: idempotent; one capability each; handlers for controlled restarts/reloads; clear defaults + required vars.
- **Workflow**: (1) Preflight — confirm inventory, maintenance window, impact, baseline health; (2) Dry-run — `ansible-playbook --check --diff`, inspect unexpected changes; (3) Canary — `--limit` one node/batch; (4) Rolling — controlled `serial`; (5) Post-check — health + functional smoke; (6) Evidence — command, output summary, follow-up in docs/report.
- **Testing**: `ansible-lint` every change; syntax-check affected playbooks; role-level CI validation for critical roles; idempotency verification where feasible.

## 6. Secrets & Access (deltas over core §9)

- Vault/secret-manager integrations; rotate with documented runbooks; least-privilege automation identities; audit production access.

## 7. CI/CD for Infrastructure

- Every infra change runs through CI; separate validation jobs from apply jobs; require human approval for production apply; publish execution artifacts; link the rollback procedure in pipeline output.

## 8. Observability & Incidents (deltas over core §8 / §7)

- Alerts map to owner + runbook; reliability metrics include uptime + MTTR; scheduled operational reports.
- Every incident: dated post-mortem (core Incident template); every high-risk change: runbook entry; repeat incidents trigger process-hardening tasks.

## 9. Definition of Done (core §14 + deltas)

Also: Terraform/Ansible checks passed; rollout followed preflight/dry-run/canary/post-check; monitoring impact validated.

## 10. Templates

### Terraform Change

```md
## Terraform Change
- Environment:
- Modules/Resources impacted:
- Plan summary:
- Risk level:
- Rollback approach:
- Validation checks:
```

### Ansible Run

```md
## Ansible Run
- Inventory:
- Playbook:
- Check mode result:
- Canary target/result:
- Rollout result:
- Post-check evidence:
```

### Incident Follow-up

```md
## Incident Follow-up
- Root cause:
- Immediate fix:
- Permanent fix:
- Monitoring/rule updates:
- Backlog items created:
```

## 11. Final Rule

No infrastructure change is complete without validation, traceability, and a rollback path.
