# DevOps Guidelines

Production-focused standards for infrastructure automation, platform operations, and reliable delivery.

This guide defines how to work with Infrastructure as Code and operational tooling, with explicit standards for Terraform and Ansible.

## 1. Scope

These rules apply to:

- infrastructure provisioning and lifecycle changes,
- configuration management,
- release and rollback operations,
- observability and incident response for platform services.

## 2. Core DevOps Principles

1. Infrastructure must be reproducible and declarative.
2. Every change must be reviewable, testable, and reversible.
3. Production safety is prioritized over speed.
4. Automation is mandatory for repeatable operations.
5. Operational evidence is required for every relevant intervention.

## 3. Repository Standards For DevOps

Recommended structure:

```text
/
|- terraform/
|  |- modules/
|  |- environments/
|- ansible/
|  |- inventory/
|  |- group_vars/
|  |- host_vars/
|  |- roles/
|  |- playbooks/
|- docs/
|  |- runbooks/
|  |- incidents/
|  |- reports/
|- README.md
|- CHANGELOG.md
|- TODO.md
|- devops-guidelines.md
```

Rules:

- Keep environment separation explicit (dev, staging, prod).
- Keep secrets outside versioned files.
- Keep naming conventions consistent across tools.

## 4. Terraform Standards

### 4.1 State And Environment Strategy

1. Use remote state with locking enabled.
2. Isolate state per environment and critical stack.
3. Restrict state access by least privilege.
4. Back up state and define restoration procedure.

### 4.2 Module Design

1. Prefer reusable modules with focused responsibility.
2. Keep module inputs/outputs explicit and typed.
3. Avoid hidden dependencies between modules.
4. Pin provider versions and document upgrade paths.

### 4.3 Execution Workflow

Required workflow for every Terraform change:

1. Format and validate:
   - terraform fmt
   - terraform validate
2. Plan and review:
   - terraform plan with clear diff output
   - peer review for risky changes
3. Apply:
   - apply only reviewed plans
   - use environment-specific approval rules
4. Verify:
   - post-apply smoke checks
   - document outcome and residual risk

### 4.4 Policy And Safety

- Destructive changes require explicit approval.
- Any force-recreate must include rollback plan.
- Drift detection must run on a schedule.
- Resource tagging standards are mandatory.

### 4.5 Terraform Testing

- Static checks: fmt, validate, lints.
- Policy checks: security/compliance gates.
- Module tests for critical shared modules.
- Plan-level regression checks for sensitive stacks.

## 5. Ansible Standards

### 5.1 Inventory And Variable Hygiene

1. Keep inventories environment-scoped.
2. Use group_vars and host_vars consistently.
3. Avoid variable shadowing and undocumented overrides.
4. Keep retired hosts in explicit archive/legacy inventory.

### 5.2 Role Design

1. Roles must be idempotent.
2. Keep one role focused on one capability.
3. Use handlers for controlled restarts/reloads.
4. Expose role defaults and required vars clearly.

### 5.3 Playbook Execution Workflow

Required workflow for every operational playbook run:

1. Preflight:
   - confirm target inventory,
   - confirm maintenance window and impact,
   - capture baseline health.
2. Dry-run:
   - ansible-playbook --check --diff
   - inspect unexpected changes.
3. Canary:
   - run with --limit on one node or one batch.
4. Rolling rollout:
   - apply with controlled serial strategy.
5. Post-check:
   - validate service health and functional smoke tests.
6. Evidence:
   - save command, output summary, and follow-up in docs/report.

### 5.4 Ansible Testing

- ansible-lint on every change.
- syntax-check for affected playbooks.
- role-level validation in CI for critical roles.
- idempotency verification where feasible.

## 6. Secrets And Access Control

1. Never commit credentials or private keys.
2. Use vault/secret manager integrations.
3. Rotate sensitive credentials with documented runbooks.
4. Restrict automation identities by least privilege.
5. Audit access to production environments.

## 7. CI/CD For Infrastructure

1. Every infrastructure change runs through CI checks.
2. Separate validation jobs from apply jobs.
3. Require human approval for production apply.
4. Publish execution artifacts for traceability.
5. Keep rollback procedure linked in pipeline output.

## 8. Observability And Operational Readiness

1. Every critical service must have health checks.
2. Monitoring must distinguish warning vs failure.
3. Alerts must map to owner + runbook.
4. Reliability metrics should include uptime and MTTR.
5. Weekly or scheduled operational reports are recommended.

## 9. Incident And Change Management

1. Every incident gets a dated post-mortem.
2. Every high-risk change gets a runbook entry.
3. Root cause and preventive action are mandatory fields.
4. Repeat incidents must trigger process hardening tasks.

## 10. Definition Of Done For DevOps Changes

A DevOps change is done only when:

1. Terraform/Ansible checks passed,
2. rollout followed preflight/dry-run/canary/post-check,
3. rollback strategy is clear,
4. monitoring impact is validated,
5. docs, changelog, and TODO are updated,
6. evidence is recorded.

## 11. Quick Templates

### 11.1 Terraform Change Template

```md
## Terraform Change
- Environment:
- Modules/Resources impacted:
- Plan summary:
- Risk level:
- Rollback approach:
- Validation checks:
```

### 11.2 Ansible Run Template

```md
## Ansible Run
- Inventory:
- Playbook:
- Check mode result:
- Canary target/result:
- Rollout result:
- Post-check evidence:
```

### 11.3 Incident Follow-up Template

```md
## Incident Follow-up
- Root cause:
- Immediate fix:
- Permanent fix:
- Monitoring/rule updates:
- Backlog items created:
```

## 12. Final Rule

No infrastructure change is complete without validation, traceability, and a rollback path.
