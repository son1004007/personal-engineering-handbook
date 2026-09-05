# AGENTS.md

## Global control

Before substantive cross-repository or development work, read:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

Then return to this repository and follow the rules below.

## Purpose

This repository is the public Source of Truth for the owner's personal software-engineering practices, lifecycle rules, checklists, and reusable templates.

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

Never use this handbook to override a stricter project, employer, contractual, privacy, or security requirement.

## Status handling

- `approved`: may be used as a default engineering rule when relevant.
- `reviewed`: advisory; do not treat as mandatory.
- `draft`: proposal only.
- `deprecated` / `superseded`: do not use for new work unless explicitly requested.

If two approved rules conflict, prefer the more specific rule; if specificity is equal, prefer the newer `last-reviewed` date.

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
-> independent review
-> verify with code/test/runtime when applicable
-> owner approval
-> mark approved + date
```

AI agreement alone is not verification.

## Review evidence priority

```text
actual test/runtime evidence
> official standards / official vendor documentation
> reputable engineering practices
> independent human review
> multiple independent AI reviews
> single AI opinion
```

## Relationship to projects

When an AI works on another software repository:

1. Read the target repository's `AGENTS.md`, requirements, architecture, tests, and current state first.
2. Consult this handbook only for relevant approved defaults not already overridden by the project.
3. Do not copy all handbook rules into the project.
4. Record project-specific deviations locally when they are intentional and durable.
5. If project work reveals a reusable general rule, propose it here separately after sanitization and review.
