# Risk-based Optional Independent AI Review Policy

- status: `approved`
- version: `1.5`
- approved_date: `2026-11-20`
- last_reviewed: `2026-09-06`
- owner_decision: `Independent AI review is optional; objective verification and actual organizational obligations remain mandatory. AGY/Gemini must not be automatically delegated.`
- review_evidence (historical v1.4.1; not review of v1.5):
  - `device-control #353 / run 33996776013`
  - `device-control #354 / run 33997032701`
  - `device-control #355 / run 33997311911`
  - `device-control #356 / run 33997410654`
  - `device-control #357 / run 33997542247 — READY, BLOCKER 0`

**2026-11-20 superseding rule:** Independent AI quality review is optional across owner-governed engineering workflows. No missing optional AI review alone shall cause REVIEW_DEBT, approval requests or release hold. Use `NOT_RUN_OPTIONAL`; never report it as PASS. Legacy mandatory-review language elsewhere is superseded for optional AI review only. Existing legal, contractual, employer/client governance, human approvals, security boundaries and actual BLOCKER/MAJOR handling remain intact. Implementation AI may self-review and perform real verification. This update is owner-approved, not independently reviewed.

## 1. Core policy

1. Substantive engineering change -> self-review and risk-based functional/security/regression verification MUST; independent AI review MAY.
2. Personal / explicitly authorized environment -> use optional independent review only when useful and authorized. AGY/Gemini automatic delegation remains disabled until explicit reapproval.
3. MEDIUM/HIGH design -> risk-based design/security analysis MUST; separate AI pre-design/final review MAY. Actual law/contract/company/client/project-required human or independent review remains mandatory where applicable.
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

TDD micro-iterations and work-in-progress commits do not each trigger AGY. Final substantive logical change requires verification and self-review; independent AI review is optional unless an actual external governance requirement applies.

## 4. Applicability

### SUBSTANTIVE

Risk-proportional verification required, and optional independent AI review may be useful, when a logical change changes any of:

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

For non-substantive work `NOT_APPLICABLE` may be used when all remain unchanged:

- runtime/build/release/security/data behavior
- external contract
- acceptance/regression semantics

and the change is meaning-preserving editorial/format/dead-link work.

Test-only additions/refactors that only add/reorganize protection without weakening/redefining acceptance are reviewed with the surrounding logical change. If standalone, they may use a lightweight final review rather than full design-review ceremony.

Gray area defaults substantive.

## 5. Design review

MEDIUM/HIGH architecture/security/data/operation decisions require risk-based design/security analysis. Separate AI review is optional; use only authorized reviewers/data. Company/client actual review obligations remain in force. LOW substantive change still requires relevant functional and security verification.

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
2. optional eligible second independent semantic reviewer or owner-authorized evidence-based risk reconciliation
3. rubric-based owner disposition

Outcomes:

- decisive evidence establishes concern invalid/non-credible -> `REJECTED` with recorded rationale; optional reviewer concurrence if available
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
- if evidence supports no material risk -> REJECTED/MINOR as justified, with optional reviewer concurrence where obtained

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

If an optional reviewer is unavailable, record `NOT_RUN_OPTIONAL`; no review debt/release block solely for missing independent AI review. Within existing approvals, proceed with implementation, self-review, verification and deployment. The following historical debt procedure applies only to an actual mandatory review obligation:

Work may continue on a **non-release branch/worktree**:

- implement
- test
- commit on non-release branch
- collect evidence

Record:

```text
REVIEW_DEBT
review_required: yes
reason: ACTUAL_REQUIRED_REVIEW_UNAVAILABLE
review_target: <exact commit/range>
review_due: before release/main integration
```

When actual required review remains outstanding, follow that governance's release restrictions. Missing optional AI review alone never prevents release.

### Company/client

Personal AGY outage must not be organization SPOF. Follow the company's actual approved review path, if required.

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

A subsequent non-emergency release in the same affected area must not be approved while an actual mandatory review obligation or credible unresolved BLOCKER from break-glass remains overdue. Missing optional AI review alone does not block release.

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
- self-review, and independent design/final review only if actually required; optional AI review status recorded
- severity triage/calibration
- arbitration/reconciliation
- no unresolved calibrated BLOCKER
- calibrated MAJOR fixed or properly risk-accepted
- no actual mandatory overdue same-area break-glass review obligation or unresolved calibrated BLOCKER
- reviewer provenance evidence if independent review performed
- reviewer/data-boundary/project-governance compliance

Test PASS alone is not Done.

## 20. Review history

- v1.0 owner mandatory-review intent.
- #353 -> v1.1: self-arbitration, company default-deny, tool-SPOF, shadow governance.
- #354 -> v1.2: solo arbitration deadlock, design-review break-glass deferral, substitute criteria.
- #355 -> v1.3: raw severity calibration, tie-break, review-unit/TDD overhead, substitute capability, solo incident handling.
- #356 -> v1.4: calibrated BLOCKER non-waivable; interpretive false-positive rejection path; triage authority; non-emergency review debt.
- #357 -> READY, BLOCKER 0. Post-READY majors selectively applied in v1.4.1: bounded evidence plan without arbitrary fixed cycle count, overdue break-glass debt gate, and policy-safe reviewer-input provenance rather than mandatory raw confidential-payload duplication.

- v1.5 (2026-11-20): owner makes independent AI review optional, retains evidence-based quality/security and actual governance requirements; independent AI review NOT_RUN_OPTIONAL.
