# Mandatory Independent Review Policy

- status: `approved`
- version: `1.4`
- approved_date: `2026-09-06`
- last_reviewed: `2026-09-06`
- owner_decision: `Independent review is mandatory; AGY/Gemini is mandatory for personal or explicitly authorized environments; raw AI findings and severity are provisional.`
- review_evidence:
  - `device-control #353 / run 33996776013`
  - `device-control #354 / run 33997032701`
  - `device-control #355 / run 33997311911`
  - `device-control #356 / run 33997410654`

## 1. Core policy

1. Substantive engineering change -> independent review MUST.
2. Personal / explicitly AGY-authorized environment -> AGY/Gemini final review MUST.
3. MEDIUM/HIGH design -> independent pre-design review MUST.
4. Raw AI finding/severity is provisional, not authority.
5. Findings are calibrated against objective severity criteria and evidence.
6. A **true calibrated BLOCKER cannot be waived in normal release**.
7. A false/overstated AI BLOCKER may be rejected or downgraded through evidence/arbitration.
8. `RISK_ACCEPTED/WAIVED` is available for calibrated MAJOR, not calibrated BLOCKER.
9. Company/client data to personal/external AGY -> DEFAULT DENY unless explicitly authorized.
10. Company governance always outranks this handbook; no shadow process.

## 2. Authority

```text
law / contract / client requirement
> employer/project policy
> project AI/data/security rules
> this REVIEW_POLICY.md
> other handbook guidance
> AI preference
```

## 3. Review unit

Review is performed on a **logical change set / PR / release candidate**, not every edit or commit.

TDD micro-iterations, intermediate local commits and work-in-progress test edits do not each trigger AGY. The final logical substantive change still requires review before declared completion/merge/release.

## 4. Applicability

### SUBSTANTIVE

Independent final review required when a logical change changes any of:

- runtime or externally observable behavior
- business/acceptance semantics
- authn/authz/security boundary
- persistent data/schema/query/migration/invariant
- API/event/file/interface contract
- runtime config/profile
- infra/network/container behavior
- CI/build/release/deployment/quality-gate behavior
- dependency behavior/security/support characteristics
- test semantics that weaken/remove/materially redefine acceptance/regression protection
- operational safety/recovery
- documentation-as-code/generated configuration that affects actual behavior

### NON_SUBSTANTIVE

May be `NOT_APPLICABLE` only when all are unchanged:

- runtime/build/release/security/data behavior
- external contract
- acceptance/regression semantics

and the change is meaning-preserving editorial/format/dead-link work.

Test-only additions/refactors that only add/reorganize protection without weakening or redefining existing acceptance are reviewed with the surrounding logical change. If standalone, they may use a lightweight final review rather than a full design-review cycle.

Gray area defaults substantive.

## 5. Design review

MEDIUM/HIGH architecture/security/data/operation decisions require pre-implementation independent design review.

Personal/AGY-authorized -> AGY/Gemini.
Company/client -> company-approved reviewer unless AGY is explicitly authorized.

## 6. Independent review input

```text
original requirement / acceptance / constraints
+ exact draft/commit/diff
+ necessary architecture/context
+ deterministic verification evidence
```

Do not seed implementation-agent conclusions, desired verdict or expected findings as truth.

## 7. Finding pipeline

```text
AI finding
-> PENDING_TRIAGE
-> severity calibration
-> evidence/arbitration
-> final disposition
```

Raw AI label never directly owns the release gate.

## 8. Severity rubric

### BLOCKER

Use only when a credible failure path, evidence or explicit obligation connects the issue to at least one of:

- high-impact auth/security bypass or exploit
- data loss/corruption/critical invariant breach
- explicit law/contract/project-policy violation
- core required/acceptance behavior failure that defeats release purpose
- demonstrably unsafe/nonrecoverable destructive migration/release/recovery
- invalid verification evidence such that safety/correctness cannot be judged
- active high-impact production exposure requiring containment

Generic HA/scalability advice, architecture preference, style or maintainability debate is not BLOCKER by itself.

**Calibrated BLOCKER is not waivable in normal release.** It must be fixed, disproved, or legitimately recalibrated to a lower severity before normal release.

Break-glass may temporarily defer review/fix only under Section 17.

### MAJOR

Substantial correctness/reliability/security/data/operability/test-adequacy risk. Blocking by default, but may be accepted through authorized `RISK_ACCEPTED` process.

### MINOR / NIT / QUESTION

Nonblocking by default unless later evidence raises severity.

## 9. Triage authority

The owner/reconciler performs initial rubric calibration.

### Facial mislabel

If an AI-labelled BLOCKER/MAJOR **does not even allege** a credible failure path/explicit obligation matching the rubric (for example, generic style or scalability advice), owner may down-calibrate during triage with a short rubric-mismatch rationale. This is severity calibration, not dismissal of a demonstrated risk.

### Credible failure-path dispute

If the finding alleges a plausible path that could satisfy BLOCKER/MAJOR criteria and the validity of that path is disputed, do not unilaterally down-calibrate. Use Section 11 arbitration unless Section 10 direct evidence completely disproves the path.

## 10. Directly falsifiable finding

Owner may `REJECTED` or down-calibrate without second reviewer only when evidence **directly disproves the whole core factual premise**:

- same-condition deterministic reproduction/test
- exact-version current official docs
- direct runtime probe
- explicit requirement/contract

Counter-evidence scope must cover finding scope.

A narrow happy-path test cannot dismiss broader security/design/data risk. Mixed finding must be split into factual and residual interpretive parts.

## 11. Interpretive / risk / design arbitration

Implementer cannot unilaterally dismiss a disputed credible risk.

### Personal

Use:

1. available evidence
2. eligible second independent semantic reviewer
3. rubric-based owner disposition

Outcomes:

- **Second reviewer concurs finding is invalid/non-credible:** owner may `REJECTED` with reviewer concurrence + evidence.
- **Both reviewers agree risk exists:** calibrate severity using rubric.
  - BLOCKER -> FIX/HOLD; no normal-release waiver.
  - MAJOR -> FIX or `RISK_ACCEPTED`.
- **Reviewers disagree:** owner records both views and applies rubric/evidence.
  - if credible BLOCKER failure path remains unresolved -> HOLD/FIX or gather more evidence.
  - if BLOCKER rubric is not met but material risk remains -> `MODIFIED` to MAJOR/MINOR as justified.
  - calibrated MAJOR may be `RISK_ACCEPTED`.
  - if second reviewer plus evidence establishes the concern itself is invalid -> `REJECTED`.
- **Both inconclusive:** collect bounded additional evidence/mitigation. If credible BLOCKER path remains, HOLD. Otherwise calibrate residual risk and proceed only under the resulting severity rule.

Owner disposition is an auditable decision record, not fake separation-of-duties.

### Company/client

Company policy controls arbitration/risk acceptance.

BLOCKER/MAJOR reject/downgrade/risk acceptance requires appropriate human peer/tech lead/security owner/designated approver concurrence and official project process.

Personal handbook grants no company risk-acceptance authority.

## 12. Eligible reviewer / substitute

Reviewer must:

- be separated from implementation or use a genuine fresh context
- directly inspect relevant source/evidence
- support semantic correctness/security/data/operation reasoning
- record identity/capability/result

Eligible examples:

- human peer/lead/security reviewer
- company-approved internal AI reviewer
- separate fresh-context **high-capability reasoning/review AI** able to ingest the full relevant context and analyze counterexamples/failure/security paths

Not sufficient alone:

- same implementation agent self-review
- formatter/linter/test runner/SAST
- simple classifier/utility model unable to perform semantic review

Model name alone does not guarantee qualification.

## 13. Company/client data boundary — DEFAULT DENY

Do not send company/client source, diff, schema, design, log, API/internal context, sensitive data or reconstructable non-public derived context to personal Synology, public GitHub, personal external LLM or unapproved AI unless explicit authorization covers:

- service/endpoint
- repository/project
- data classification
- applicable contract/security conditions

Current personal Synology AGY is not presumed approved for company data.

Authorization absent/unclear:

```text
AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA
-> company-approved independent reviewer
```

Generic technology questions are externalizable only when independently formulatable without revealing/reconstructing non-public company facts. Uncertainty -> no external transfer.

## 14. Company adoption — no shadow governance

Map to existing organization artifacts:

```text
requirements/design -> ticket/design doc
verification -> CI/test
independent review -> PR/security/approved internal-AI review
reconciliation -> PR/review record
release -> existing change management
```

Do not impose personal AGY sign-off on a team unless formally adopted.

## 15. Offline / tool-outage review debt

### Personal non-emergency

If AGY and eligible substitute are temporarily unavailable, work may continue on a **non-release branch/worktree**:

- implement
- test
- commit locally/branch
- collect evidence

Record:

```text
REVIEW_DEBT
review_required: yes
reason: AGY_RUNTIME_UNAVAILABLE
review_target: <exact commit/range>
```

Until independent review completes, do **not**:

- declare Done
- merge/update the declared release/main baseline
- publish release
- deploy to production

When reviewer becomes available, review the exact accumulated change set before release/main integration.

### Company/client

Personal AGY outage must not be organization SPOF. Use company-approved mandatory review path.

## 16. Risk acceptance

`RISK_ACCEPTED` / `WAIVED` is permitted for **calibrated MAJOR only** under normal release.

It is not `REJECTED`.

Record:

```text
finding
calibrated severity: MAJOR
reason not fixed
impact/blast radius
mitigation
reviewer opinions
owner/authorized approver
revisit trigger / issue / due if relevant
```

**Calibrated BLOCKER cannot be risk-accepted/waived for normal release.**

Company risk acceptance only by company-authorized role/process.

## 17. Break-glass

When review delay would worsen an active incident/data-loss/security containment, both pre-design and final review can be deferred.

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> DEFERRED DESIGN/FINAL REVIEW
-> RETRO / RECONCILE
```

Record:

```text
REVIEW_DEFERRED_BREAK_GLASS
review_scope: DESIGN | FINAL | BOTH
review_owner: ...
review_due: project-policy deadline or explicit concrete deadline
```

No open-ended defer.

Post-release serious finding:

- company -> official incident/change escalation
- personal -> owner acts as incident owner, reassesses exposure and uses rollback/disable/contain/forward-fix as appropriate, then records evidence

Break-glass is not a normal-release BLOCKER waiver; it is temporary emergency deferral followed by review/remediation.

## 18. Evidence hierarchy

```text
law/contract/company/project policy
> runtime/test/reproduction
> current official docs
> project requirements/architecture
> reputable engineering practice
> independent AI finding
> implementer self-claim
```

Higher evidence matters only when it actually addresses the finding's scope.

## 19. Done

Done requires, as applicable:

- deterministic verification
- required independent design/final review
- severity triage/calibration
- arbitration/reconciliation
- **no unresolved calibrated BLOCKER**
- calibrated MAJOR fixed or properly risk-accepted
- reviewer/data-boundary/project-governance compliance

Test PASS alone is not Done.

## 20. Review history

- v1.0: owner mandatory-review intent.
- #353 -> v1.1: self-arbitration, company default-deny, tool-SPOF, shadow governance.
- #354 -> v1.2: solo arbitration deadlock, explicit design-review break-glass deferral, substitute criteria.
- #355 -> v1.3: raw AI severity calibration, tie-break, TDD review-unit, substitute capability, solo incident handling.
- #356 -> v1.4: calibrated BLOCKER made non-waivable in normal release; interpretive false positive can be REJECTED with independent concurrence; triage authority defined; non-emergency review debt allows development but gates main/release.
- AGY suggestions are incorporated selectively; wording/thresholds are changed when needed to preserve evidence hierarchy, owner intent and practical operation.
