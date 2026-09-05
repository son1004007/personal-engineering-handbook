# AGENTS.md

## Global control

Before substantive cross-repository or development work, read:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

Then return to this repository.

## Purpose

This repository is the public Source of Truth for the owner's personal software-engineering practices, lifecycle rules, checklists, and reusable templates. It may be used as the owner's working method inside company/client projects only within higher policy and approved data boundaries.

## Authority

```text
latest explicit user instruction
> law / contract / client requirement
> employer policy / approved project standard
> current project AGENTS / requirements
> approved handbook rules
> reviewed/draft guidance
> framework convention
> AI preference
```

## Mandatory review bootstrap

Before substantive engineering work read:

1. `REVIEW_POLICY.md` — **approved v1.2**
2. `OPERATING_MODEL.md`
3. relevant lifecycle/standard/checklist docs

Core rules:

- personal / explicitly AGY-authorized: substantive final change requires AGY/Gemini review; MEDIUM/HIGH design requires AGY/Gemini design review.
- company/client: independent review mandatory; external/personal AGY DEFAULT DENY until explicit authorization.
- company AGY authorization absent/unclear -> `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA` + company-approved reviewer.
- AGY findings are provisional; `BLOCKER/MAJOR` become pending blocking items until arbitration.
- directly falsifiable AI findings may be rejected/downgraded when decisive deterministic/official/runtime/contract evidence directly disproves the finding.
- interpretive/risk/design BLOCKER/MAJOR cannot be unilaterally dismissed: personal uses a fresh-context independent reviewer; company uses appropriate human concurrence/project governance.
- break-glass may explicitly defer both pre-design and final review, but requires `review_scope`, `review_owner`, `review_due`, and post-release review/reconciliation.

Do not report substantive work as Done merely because tests passed when mandatory review/arbitration evidence is missing.

## Public / company-data boundary

Read `PUBLICATION_POLICY.md` before publishing.

Do not publish employer/client source, internal policy/template/architecture/runbook, internal URL/IP/account/schema/data, proprietary rule, credential or sensitive data.

Company/client content or non-public derived context must not be sent to personal Synology AGY or unapproved external AI unless explicit authorization is confirmed. Absence of prohibition is not permission.

Independently formulated generic technology questions may be externalized only when they do not expose or permit reconstruction of non-public company/project facts; uncertainty defaults to no external transfer.

## Rule creation workflow

```text
identify recurring problem/decision
-> gather authoritative references
-> draft rationale/scope/exceptions
-> synthetic example when useful
-> independent review
-> arbitrate/reconcile findings
-> verify with test/runtime when applicable
-> owner approval
-> mark approved/date
```

AI agreement alone is not verification.

## Evidence priority

```text
law / contract / company / project policy
> actual test/runtime/reproduction
> official standards/vendor docs
> project requirements/architecture
> reputable engineering practice
> independent AI finding
> implementation-agent self-claim
```

## Relationship to projects

1. Read target repo AGENTS/requirements/architecture/tests/current state/data boundary first.
2. Read `REVIEW_POLICY.md` and relevant approved defaults.
3. Use risk-proportional documentation but mandatory independent review.
4. Map company work into existing ticket/PR/CI/security/change-management; do not create shadow governance.
5. Do not use personal AGY with company data unless explicitly authorized.
6. Apply v1.2 arbitration rules; implementer-alone dismissal of interpretive/risk BLOCKER/MAJOR is invalid.
7. Generalize reusable rules here only after sanitization, independent review and owner approval.
