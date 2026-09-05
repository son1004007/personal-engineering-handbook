# Mandatory Review Policy — AGY/Gemini Governance Review

- date: `2026-09-06`
- final_policy: `REVIEW_POLICY.md v1.4.1`
- final_verdict: `READY`
- final_blocker: `0`
- final_major: `0`
- final_review: `device-control #358 / workflow 33997636021`
- reviewer_runtime: `AGY 1.1.27 / read-only / settings restored / read_only_violation=no`

## Purpose

Owner decision:

> Independent review is mandatory. AGY/Gemini is the mandatory independent reviewer for personal or explicitly authorized environments. AGY findings are not automatically accepted; they must be calibrated and reconciled against evidence.

This policy is intended as the owner's engineering discipline for personal work and company/client work, while preserving higher organization policy and data boundaries.

## Review sequence

### #353 — policy v1.0

Result: `REJECTED / revision required`

Important findings selectively incorporated:

- implementer self-arbitration loophole
- company/client data exfiltration risk
- external/personal AGY default-deny requirement
- personal AGY tool outage must not become company delivery SPOF
- break-glass remediation/accountability
- avoid shadow governance inside company workflows

Not blindly adopted:

- universal fixed 24h/next-business-day remediation SLA was not accepted as a global rule; project policy or explicit concrete due point is used instead.

### #354 — policy v1.1

Result: `NOT_READY`

BLOCKERs incorporated:

- personal direct-evidence false positives must not always require a second reviewer
- break-glass must explicitly permit deferral of both pre-design and final review

Additional changes:

- company-approved internal AI governance clarified
- substantive/non-substantive boundary clarified
- substitute reviewer requirements added
- generic technology question vs confidential derived context clarified

### #355 — policy v1.2

Result: `NOT_READY`

BLOCKERs incorporated:

- raw AI `BLOCKER` label must not directly own the release gate
- objective severity rubric required
- personal interpretive tie-break must not deadlock

Additional changes:

- review unit changed to logical change set/PR/release candidate, not every edit/TDD iteration
- counter-evidence scope must match finding scope
- substitute AI must have real semantic reasoning/context capability
- personal break-glass owner handling defined

### #356 — policy v1.3

Result: `NOT_READY`

BLOCKER incorporated:

- true calibrated BLOCKER cannot be waived during normal release
- AI-overstated BLOCKER can be disproved/recalibrated through evidence and arbitration

Additional changes:

- interpretive false positive may be `REJECTED` when second independent reviewer and evidence concur
- triage authority clarified
- personal AGY outage may create bounded `REVIEW_DEBT`: development/testing on non-release branch may continue, but Done/main/release/production remain gated

### #357 — policy v1.4

Result: `READY`

- BLOCKER: `0`

AGY assessed solo operation as practical/enforceable and company/client governance as safe/conservative.

Three remaining MAJOR suggestions were evaluated.

#### Major 1 — fixed review-cycle count

AGY suggested an example hard cap such as two refinement cycles.

Disposition: `MODIFIED`.

Reason: a universal iteration count is arbitrary across risk classes. Instead v1.4.1 requires an explicit **bounded evidence plan** with unresolved question, decision-changing evidence, bounded probes and stop condition. Time/iteration exhaustion alone cannot downgrade a credible BLOCKER.

#### Major 2 — overdue break-glass debt

Disposition: `ACCEPTED`.

A subsequent non-emergency release in the same affected area cannot proceed while prior break-glass `review_due` is overdue and review/triage remains open. Organization policy may define a broader gate.

#### Major 3 — fresh reviewer prompt provenance

Disposition: `MODIFIED`.

Reason: blindly archiving raw prompts/payloads could duplicate confidential company/client data. v1.4.1 instead records reviewer/session, source refs, initial prompt template/reference, hash where practical and fresh-context mode. Full prompt retention is allowed only where policy permits; public handbook never stores confidential review payloads.

### #358 — v1.4.1 post-amendment review

Result:

```text
VERDICT: READY
BLOCKER: 0
MAJOR: 0
```

AGY concluded that the three post-READY amendments were coherent with mandatory independent review, default-deny company-data handling and non-deadlocking solo governance.

## Final model

```text
logical change
-> deterministic verification
-> independent review
-> PENDING_TRIAGE
-> objective severity calibration
-> evidence / arbitration
-> final disposition
-> release decision
```

### Raw AI severity is not authority

An AI `BLOCKER` label is not itself a blocker. It is calibrated against the rubric.

### Calibrated BLOCKER

- no normal-release waiver
- fix, disprove or legitimate recalibration required
- break-glass is temporary emergency deferral, not waiver

### Calibrated MAJOR

- fix by default
- explicit authorized `RISK_ACCEPTED` permitted

### Personal arbitration

- directly falsifiable whole premise -> decisive evidence can reject without extra reviewer
- interpretive/risk/design dispute -> second independent semantic reviewer
- if concern is proven invalid -> `REJECTED`
- if material risk remains but not BLOCKER -> MAJOR/MINOR calibration
- owner decision is audit record, not artificial separation-of-duties

### Company/client

- independent review mandatory
- personal/external AGY default-deny unless explicitly authorized
- current personal Synology AGY is not presumed approved for company data
- existing company PR/security/change governance is authoritative
- no personal shadow governance

## Evidence hierarchy

```text
law / contract / company / project policy
> runtime / deterministic test / reproduction
> current official standard/vendor docs
> project requirements / architecture
> reputable engineering practice
> independent AI finding
> implementer self-claim
```

## Conclusion

`REVIEW_POLICY.md v1.4.1` is the owner-approved current Source of Truth as of `2026-09-06`.

The policy deliberately enforces **review execution without granting AI sovereign authority**.
