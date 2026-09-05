# AGENTS.md

## Global control

Before substantive cross-repository or development work, read:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

Then return to this repository and follow the rules below.

## Purpose

This repository is the public Source of Truth for the owner's personal software-engineering practices, lifecycle rules, checklists, and reusable templates.

It is designed to be usable not only in personal projects but also as the owner's working method inside company/client projects **when higher policy allows it**.

It is not an employer or client policy repository.

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

Never use this handbook to override a stricter project, employer, contractual, privacy, security, or data-boundary requirement.

## Mandatory review bootstrap

Before substantive engineering work, read:

1. `REVIEW_POLICY.md`
2. `OPERATING_MODEL.md`
3. the relevant lifecycle/standard/checklist documents

`REVIEW_POLICY.md` is **approved** and mandatory.

Core requirement:

- substantive code/config/schema/API/infra/security/deployment behavior changes require AGY/Gemini independent final review;
- MEDIUM/HIGH architecture/security/data/operation decisions require AGY/Gemini design review before implementation;
- AGY/Gemini findings are reconciled against official docs, runtime/test evidence and project requirements, not automatically accepted;
- company/client data may be supplied to AGY only when higher policy permits it;
- when AGY is prohibited by policy, use the project-approved independent reviewer and record `AGY_NOT_PERMITTED_BY_POLICY`;
- break-glass containment may defer review but must not silently skip it.

An agent must not report a substantive change as `Done` merely because implementation/tests passed if mandatory review/reconciliation evidence is missing.

## Status handling

- `approved`: use as a default engineering rule when relevant unless overridden by higher authority.
- `reviewed`: advisory; do not treat as mandatory unless an approved policy explicitly incorporates it.
- `draft`: proposal only.
- `deprecated` / `superseded`: do not use for new work unless explicitly requested.

If two approved rules conflict, prefer the more specific rule; if specificity is equal, prefer the newer explicit owner decision/review date.

## Public boundary

Before adding or revising content, read `PUBLICATION_POLICY.md`.

Do not publish:

- employer/client source code or modified copies;
- internal policies, templates, architecture documents, meeting notes, or runbooks;
- internal/customer IPs, URLs, accounts, credentials, schema/table names, or operational data;
- proprietary algorithms or business rules learned from non-public work;
- text that merely paraphrases a confidential source closely enough to reconstruct it.

Use independently created synthetic examples.

## Rule creation workflow

For substantive rules:

```text
identify problem / recurring decision
-> gather official / authoritative references
-> write draft with rationale and scope
-> map risks / exceptions
-> create synthetic example when useful
-> AGY/Gemini independent review
-> reconcile findings (accepted/modified/rejected/deferred)
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
> AGY/Gemini and other independent reviews
> implementation agent self-claim
```

## Relationship to projects

When an AI works on another software repository:

1. Read the target repository's `AGENTS.md`, requirements, architecture, tests, current state and data-boundary policy first.
2. Read this repo's `REVIEW_POLICY.md` and relevant approved defaults not already overridden by the project.
3. Do not copy all handbook rules into the project.
4. Apply risk-proportional documentation, but do not omit mandatory review for substantive changes.
5. Record project-specific deviations locally when intentional and durable.
6. If company/client policy forbids AGY, do not exfiltrate data; use an approved independent reviewer and record the exception accurately.
7. If project work reveals a reusable general rule, propose it here separately after sanitization, independent review and owner approval.
