# AGENTS.md

## Global control

Before substantive cross-repository or development work, read:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

Then return to this repository and follow the rules below.

## Purpose

This repository is the public Source of Truth for the owner's personal software-engineering practices, lifecycle rules, checklists, and reusable templates.

It is designed to be usable as the owner's working method in personal projects and, when higher policy permits, inside company/client projects. It is not an employer/client policy repository.

## Authority order

```text
latest explicit user instruction
> law / contract / client requirement
> employer policy and approved project standard
> current project's explicit AGENTS.md / requirements
> approved rules in this repository
> reviewed rules
> draft rules
> generic framework conventions
> AI preference
```

## Mandatory review bootstrap

Before substantive engineering work, read:

1. `REVIEW_POLICY.md` — approved v1.1
2. `OPERATING_MODEL.md`
3. relevant lifecycle/standard/checklist documents

Core rules:

- personal / explicitly AGY-authorized environment: substantive change requires AGY/Gemini final review; MEDIUM/HIGH design requires AGY/Gemini design review.
- company/client environment: independent review is mandatory, but external/personal AGY is DEFAULT DENY until explicit authorization for the service/repository/data classification is confirmed.
- if company AGY authorization is absent/unclear, record `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA` and use a company-approved human/internal-AI reviewer.
- AGY/Gemini findings are provisional and must be reconciled against evidence.
- AGY BLOCKER/MAJOR cannot be unilaterally rejected/downgraded by the implementer. Use the arbitration rules in `REVIEW_POLICY.md`.
- break-glass may defer review but requires `review_owner`, `review_due`, post-release review and remediation if a blocker is found.

An agent must not report a substantive change as Done merely because implementation/tests passed if mandatory review/arbitration evidence is missing.

## Status handling

- `approved`: default engineering rule when relevant unless overridden by higher authority.
- `reviewed`: advisory unless an approved policy explicitly incorporates it.
- `draft`: proposal only.
- `deprecated` / `superseded`: do not use for new work unless explicitly requested.

## Public/data boundary

Before publishing handbook content, read `PUBLICATION_POLICY.md`.

Do not publish employer/client source code, internal policies/templates/architecture/runbooks, internal URLs/IPs/accounts/schema names/operational data, proprietary algorithms/business rules, credentials or sensitive data.

For company project review, **do not send company/client content or derived context to personal Synology AGY or another external AI unless explicit authorization is confirmed.** Absence of an explicit prohibition is not treated as permission under this handbook.

## Rule creation workflow

```text
identify problem / recurring decision
-> gather official / authoritative references
-> write draft with rationale and scope
-> map risks / exceptions
-> synthetic example when useful
-> independent AGY/Gemini review when authorized
-> arbitrate/reconcile findings
-> verify with code/test/runtime when applicable
-> owner approval
-> mark approved + date
```

AI agreement alone is not verification.

## Review evidence priority

```text
law / contract / company / project policy
> actual test/runtime evidence
> official standards / official vendor documentation
> project requirements / architecture evidence
> reputable engineering practices
> AGY/Gemini or other independent review
> implementation agent self-claim
```

## Relationship to projects

When an AI works on another software repository:

1. Read target repo AGENTS, requirements, architecture, tests, current state and AI/data-boundary policy first.
2. Read this repo's `REVIEW_POLICY.md` and relevant approved defaults.
3. Apply risk-proportional documentation, but do not omit mandatory independent review.
4. In company projects, map handbook checkpoints into existing ticket/PR/CI/security/change-management rather than creating shadow governance.
5. Do not use personal AGY with company data unless explicitly authorized.
6. BLOCKER/MAJOR rejection/downgrade follows the arbitration rules; implementer-alone dismissal is invalid.
7. If project work reveals a reusable rule, propose it here separately after sanitization, independent review and owner approval.
