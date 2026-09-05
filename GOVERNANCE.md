# Governance

- Status: **approved**
- Version: **1.0**
- Last reviewed: **2026-09-06**

## 1. Purpose

This document defines how rules in this handbook are proposed, reviewed, approved, revised, and retired.

## 2. Rule metadata

Substantive rule documents should declare:

```yaml
status: draft | reviewed | approved | deprecated | superseded
version: x.y
last-reviewed: YYYY-MM-DD
scope: general | java-spring | python-fastapi | database | security | testing | delivery | other
```

Optional metadata:

```yaml
sources:
  - official standard or vendor documentation
reviewed-by:
  - human
  - ChatGPT
  - Codex
  - Gemini
supersedes:
  - path/to/older-rule.md
```

AI reviewer names are evidence of review, not proof of correctness.

## 3. Lifecycle

```text
draft
-> reviewed
-> approved
-> deprecated
-> superseded
```

### draft

A proposal. It may contain unresolved questions and must not be treated as a default rule in other projects.

### reviewed

At least one independent review has occurred and major objections are recorded or resolved. Still advisory until owner approval.

### approved

The owner accepts it as a personal default engineering rule, subject to higher-priority project/company/contract requirements.

### deprecated

Kept for history but not recommended for new work.

### superseded

Replaced by a newer rule. The replacement path should be explicit.

## 4. Approval criteria

A rule should not become `approved` merely because it is popular or several AIs agree.

Approval requires, as applicable:

1. clear problem and scope;
2. explicit rationale and trade-offs;
3. authoritative sources for normative or technical claims;
4. project-independent wording;
5. public-safety review;
6. synthetic examples where examples are useful;
7. test/runtime evidence for behavior claims when practical;
8. independent review;
9. owner's final decision.

## 5. Review independence

For important rules, reviewers should first inspect the same draft independently.

Recommended roles:

- ChatGPT: structure, completeness, standards mapping, contradictions;
- Codex: implementation realism, code/test implications, technical consistency;
- Gemini or another independent model: counterexamples, ambiguity, maintainability, alternate interpretation;
- human/owner: practicality, experience fit, risk acceptance, final authority.

Do not prime later reviewers with the first reviewer's conclusion unless evaluating a specific disputed point.

## 6. Evidence priority

When evidence conflicts:

```text
actual reproducible test/runtime evidence
> applicable law/contract/company/project requirement
> final official standard / official vendor documentation
> reputable engineering practice
> independent human review
> multiple AI reviews
> single AI opinion
```

Actual runtime evidence does not override law, contract, security policy, or explicit business requirements.

## 7. Change discipline

- Change the smallest set of rules necessary.
- Preserve rationale when changing a long-lived rule.
- Record breaking changes in the document and version.
- Re-review rules when framework behavior, standards, security guidance, or personal goals materially change.
- Do not silently convert `draft` into a mandatory rule.

## 8. Project adoption

Projects should reference the handbook rather than copy it wholesale.

Project-specific exceptions belong in the project repository. A reusable exception may later become a handbook proposal after generalization and review.
