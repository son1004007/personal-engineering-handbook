# AGENTS.md

## Global control

Before substantive cross-repository or development work, read:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

Then return to this repository.


> 2026-11-20 owner policy: Independent AI review is optional. See REVIEW_POLICY.md v1.5. Actual governance, security, verification and approval rules remain unchanged.

## Purpose

This repository is the public Source of Truth for the owner's personal software-engineering practices, lifecycle rules, deliverables, checklists, and reusable templates. It may be used as the owner's working method inside company/client projects only within higher policy and approved data boundaries.

## Authority

```text
latest explicit user instruction
> law / contract / client requirement
> employer policy / approved project standard
> current project AGENTS / requirements / approved decisions
> approved handbook rules
> reviewed/draft guidance
> framework convention
> AI preference
```

## Quality and review bootstrap

Before substantive engineering work read:

1. `REVIEW_POLICY.md` — **approved v1.5**
2. `OPERATING_MODEL.md`
3. relevant lifecycle/standard/checklist docs
4. for deliverables/handover, `lifecycle/03-deliverables-and-handover.md`

Core rules:

- personal: self-review and risk-based verification required; independent AI design/final review optional.
- company/client: actual governance review remains mandatory when required; external/personal AI DEFAULT DENY until explicit authorization.
- raw AI finding/severity is provisional; calibrate against evidence and the v1.4.1 severity rubric.
- calibrated BLOCKER cannot be waived for normal release; calibrated MAJOR may use authorized risk acceptance.
- directly falsifiable finding may be rejected/down-calibrated only with decisive scope-matching evidence.
- disputed credible interpretive/risk/design findings require evidence-based reconciliation; independent AI arbitration optional, unsupported dismissal invalid.
- break-glass may defer design/final review only with tracked review debt and post-release reconciliation.

Do not claim Done without required verification, unresolved-blocker disposition and actual governance evidence. Optional AI review alone does not block Done.

## Deliverable model

Default project deliverable classes:

1. DLV-01 requirements and traceability
2. DLV-02 UI publishing and screen guide
3. DLV-03 system/software design
4. DLV-04 database/data specification
5. DLV-05 source/config/migration/test code
6. DLV-06 installation/build/deployment guide
7. DLV-07 operation/acceptance/handover guide and results

Files may be combined for small projects, but required truth/evidence must remain traceable. Update changed truth, not ceremonial documents.

## Public / company-data boundary

Read `PUBLICATION_POLICY.md` before publishing.

Do not publish employer/client source, internal policy/template/architecture/runbook, internal URL/IP/account/schema/data, proprietary rule, credential or sensitive data.

Company/client content or non-public derived context must not be sent to personal Synology AGY or unapproved external AI unless explicit authorization is confirmed. Absence of prohibition is not permission.

## Rule creation workflow

```text
identify recurring problem/decision
-> gather authoritative references
-> draft rationale/scope/exceptions
-> synthetic example when useful
-> self-review / optional independent AI review
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
3. Apply the DLV-01~07 model with risk-proportional tailoring.
4. Use risk-proportional documentation, mandatory verification, and optional independent AI review.
5. Map company work into existing ticket/PR/CI/security/change-management; do not create shadow governance.
6. Do not use personal AGY with company data unless explicitly authorized.
7. Generalize reusable rules here only after sanitization, verification and owner approval; independent AI review optional.
