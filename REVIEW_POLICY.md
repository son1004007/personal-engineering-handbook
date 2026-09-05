# Mandatory Independent Review Policy

- status: `approved`
- version: `1.4.1`
- approved_date: `2026-09-06`
- last_reviewed: `2026-09-06`
- owner_decision: `Independent review is mandatory; AGY/Gemini is mandatory for personal or explicitly authorized environments; raw AI findings and severity are provisional.`
- review_evidence:
  - `device-control #353 / run 33996776013`
  - `device-control #354 / run 33997032701`
  - `device-control #355 / run 33997311911`
  - `device-control #356 / run 33997410654`
  - `device-control #357 / run 33997542247 — READY, BLOCKER 0`

## 1. Core policy

1. Substantive engineering change -> independent review MUST.
2. Personal / explicitly AGY-authorized environment -> AGY/Gemini final review MUST.
3. MEDIUM/HIGH design -> independent pre-design review MUST.
4. Raw AI finding/severity is provisional, not authority.
5. Findings are calibrated against objective severity criteria and evidence.
6. A **true calibrated BLOCKER cannot be waived in normal release**.
7. A false/overstated AI BLOCKER may be rejected or downgraded through evidence/arbitration.
8. `RISK_ACCEPTED` is available for calibrated MAJOR, not calibrated BLOCKER.
9. Company/client data to personal/external AGY -> DEFAULT DENY unless explicitly authorized.
10. Company governance outranks this handbook; no shadow process.

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

TDD micro-iterations and work-in-progress commits do not each trigger AGY. Final substantive logical change still requires review before declared Done/merge/release.

## 4. Applicability

### SUBSTANTIVE

Independent final review required when a logical change changes any of:

- runtime / externally observable behavior
- business or acceptance semantics
- authn/authz/security boundary
- persistent data/schema/query/migration/invariant
- API/event/file/interface contract
- runtime config/profile
- infra/network/container behavior
- CI/build/release/deployment/quality-gate behavior
- dependency behavior/security/support characteristics
- test semantics that weaken/remove/materially redefine acceptance/regression protection
- operational safety/recovery
- documentation-as-code/generated config affecting actual behavior

### NON_SUBSTANTIVE

May be `NOT_APPLICABLE` only when all remain unchanged:

- runtime/build/release/security/data behavior
- external contract
- acceptance/regression semantics

and the change is meaning-preserving editorial/format/dead-link work.

Test-only additions/refactors that only add/reorganize protection without weakening/redefining acceptance are reviewed with the surrounding logical change. If standalone, they may use a lightweight final review rather than full design-review ceremony.

Gray area defaults substantive.

## 5. Design review

MEDIUM/HIGH architecture/security/data/operation decisions require pre-implementation independent design review.

- personal / AGY-authorized -> AGY/Gemini
- company/client -> company-approved reviewer unless AGY explicitly authorized

LOW substantive change may skip separate pre-design review but still requires final review.

## 6. Independent review input and provenance

First independent review receives:

```text
original requirement / acceptance / constraints
+ exact draft/commit/diff
+ necessary architecture/context
+ deterministic verification evidence
```

Do not seed implementation conclusion, expected finding or desired verdict as truth.

### Review provenance record — MUST

Keep enough evidence to show what the independent reviewer actually received and that the first pass was not conclusion-seeded.

Record at minimum:

```text
reviewer / capability
review session or issue/run id
input source refs / commit / diff range
initial prompt template or prompt reference
prompt/input hash when practical
review mode: fresh-context / read-only / other
```

- Personal/private review may retain the full initial prompt when useful.
- Company/client environments follow their retention/classification policy.
- **Do not duplicate confidential code/log/data into a new audit artifact merely to prove neutrality.** Prefer existing approved audit logs, immutable source refs, private review IDs, hashes or policy-approved records.
- Public handbook records must never contain confidential review payloads.

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

Use only when credible failure path, evidence or explicit obligation connects to:

- high-impact auth/security bypass or exploit
- data loss/corruption/critical invariant breach
- explicit law/contract/project-policy violation
- core required/acceptance behavior failure defeating release purpose
- demonstrably unsafe/nonrecoverable destructive migration/release/recovery
- invalid verification evidence such that safety/correctness cannot be judged
- active high-impact production exposure requiring containment

Generic HA/scalability advice, architecture preference, style/maintainability debate alone is not BLOCKER.

**Calibrated BLOCKER is not waivable in normal release.** It must be fixed, disproved, or legitimately recalibrated lower before release.

### MAJOR

Substantial correctness/reliability/security/data/operability/test-adequacy risk. Blocking by default but eligible for authorized `RISK_ACCEPTED`.

### MINOR / NIT / QUESTION

Nonblocking by default unless evidence raises severity.

## 9. Triage authority and anti-gaming

Owner/reconciler performs initial rubric calibration.

### Facial mislabel

If AI high-severity finding does not allege a credible failure path/obligation matching the rubric, owner may down-calibrate with recorded rubric-mismatch rationale.

### Credible failure-path dispute

If a plausible path could meet BLOCKER/MAJOR rubric and validity is disputed, do not unilaterally down-calibrate; use arbitration unless Section 10 evidence completely disproves it.

### Scope matching

- counter-evidence scope must match entire finding scope
- narrow happy-path test cannot dismiss broader security/design/data risk
- mixed factual + interpretive finding is split
- residual threat/failure path remains arbitration subject

## 10. Directly falsifiable finding

Owner may `REJECTED` or down-calibrate without second reviewer only when evidence directly disproves the **whole core factual premise**:

- same-condition deterministic reproduction/test
- exact-version current official docs
- direct runtime probe
- explicit requirement/contract

Experience/preference/general best practice is insufficient.

## 11. Interpretive / risk / design arbitration

Implementer cannot unilaterally dismiss a disputed credible risk.

### Personal

Use:

1. available evidence
2. eligible second independent semantic reviewer
3. rubric-based owner disposition

Outcomes:

- second reviewer concurs concern invalid/non-credible -> `REJECTED` allowed with concurrence + evidence
- both agree risk exists -> calibrate severity
  - BLOCKER -> FIX/HOLD; no normal waiver
  - MAJOR -> FIX or `RISK_ACCEPTED`
- reviewers disagree -> record both views
  - credible BLOCKER path unresolved -> HOLD/FIX or gather bounded evidence
  - BLOCKER rubric not met but material risk remains -> `MODIFIED` to MAJOR/MINOR
  - second reviewer + evidence establishes concern invalid -> `REJECTED`
- both inconclusive -> use bounded evidence process below

### Bounded evidence process — MUST

Do not enter unlimited AI/review loops.

Before another investigation cycle, write a small evidence plan:

```text
unresolved question
what evidence could change the decision
bounded tests/docs/probes to run
stop condition
```

After the planned evidence is exhausted:

- if credible BLOCKER rubric path remains -> HOLD/FIX; do not down-calibrate merely because investigation took too long
- if BLOCKER rubric is not established but material residual risk remains -> calibrate MAJOR and FIX or `RISK_ACCEPTED`
- if evidence + second reviewer supports no material risk -> REJECTED/MINOR as justified

No universal fixed number of review cycles is mandated; the bound must be explicit and risk-proportional.

### Company/client

Company policy controls arbitration/risk acceptance.

BLOCKER/MAJOR reject/downgrade/risk acceptance requires appropriate human peer/tech lead/security owner/designated approver concurrence and official project process.

Personal handbook grants no company risk-acceptance authority.

## 12. Eligible reviewer / substitute

Reviewer must:

- be separated from implementation or use genuine fresh context
- directly inspect relevant source/evidence
- support semantic correctness/security/data/operation reasoning
- record identity/capability/result

Eligible examples:

- human peer/lead/security reviewer
- company-approved internal AI reviewer
- separate fresh-context high-capability reasoning/review AI able to ingest the full relevant context and analyze counterexamples/failure/security paths

Not sufficient alone:

- same implementation-agent self-review
- formatter/linter/test runner/SAST
- simple classifier/utility model lacking semantic review capability

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
- commit on non-release branch
- collect evidence

Record:

```text
REVIEW_DEBT
review_required: yes
reason: AGY_RUNTIME_UNAVAILABLE
review_target: <exact commit/range>
review_due: before release/main integration
```

Until independent review completes, do not declare Done, merge/update declared release/main baseline, publish release or deploy production.

### Company/client

Personal AGY outage must not be organization SPOF. Use company-approved mandatory review path.

## 16. Risk acceptance

`RISK_ACCEPTED` / `WAIVED` is permitted for **calibrated MAJOR only** in normal release.

It is not `REJECTED`.

Record finding, calibrated severity, reason not fixed, impact/blast radius, mitigation, reviewer opinions, authorized approver and revisit trigger if relevant.

**Calibrated BLOCKER cannot be risk-accepted/waived for normal release.**

Company risk acceptance only by company-authorized role/process.

## 17. Break-glass

When review delay would worsen active incident/data-loss/security containment, both pre-design and final review can be deferred.

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

### Expired review-debt gate — MUST

A subsequent **non-emergency release in the same affected area** must not be approved while a prior break-glass `review_due` is overdue and its review/triage is still open.

Organization policy may define a broader gate.

### Post-release serious finding

- company -> official incident/change escalation
- personal -> owner acts as incident owner, reassesses exposure and uses rollback/disable/contain/forward-fix as appropriate, then records evidence

Break-glass is temporary emergency deferral, not normal-release BLOCKER waiver.

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

Higher evidence matters only when it addresses the finding scope.

## 19. Done

Done requires, as applicable:

- deterministic verification
- required independent design/final review
- severity triage/calibration
- arbitration/reconciliation
- no unresolved calibrated BLOCKER
- calibrated MAJOR fixed or properly risk-accepted
- no overdue same-area break-glass review debt blocking this non-emergency release
- reviewer provenance evidence appropriate to policy
- reviewer/data-boundary/project-governance compliance

Test PASS alone is not Done.

## 20. Review history

- v1.0 owner mandatory-review intent.
- #353 -> v1.1: self-arbitration, company default-deny, tool-SPOF, shadow governance.
- #354 -> v1.2: solo arbitration deadlock, design-review break-glass deferral, substitute criteria.
- #355 -> v1.3: raw severity calibration, tie-break, review-unit/TDD overhead, substitute capability, solo incident handling.
- #356 -> v1.4: calibrated BLOCKER non-waivable; interpretive false-positive rejection path; triage authority; non-emergency review debt.
- #357 -> READY, BLOCKER 0. Post-READY majors selectively applied in v1.4.1: bounded evidence plan without arbitrary fixed cycle count, overdue break-glass debt gate, and policy-safe reviewer-input provenance rather than mandatory raw confidential-payload duplication.
