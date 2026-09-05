# Mandatory Independent Review Policy

- status: `approved`
- version: `1.3`
- approved_date: `2026-09-06`
- last_reviewed: `2026-09-06`
- owner_decision: `Independent review is mandatory; AGY/Gemini is mandatory for personal or explicitly authorized environments; AI severity and findings are review inputs, not authority.`
- review_evidence:
  - `device-control issue #353 / workflow run 33996776013`
  - `device-control issue #354 / workflow run 33997032701`
  - `device-control issue #355 / workflow run 33997311911`

## Purpose

개인 프로젝트와 회사·고객 프로젝트에서 내가 수행하는 engineering work에 독립적인 반대 관점을 강제로 넣는다.

핵심:

1. substantive change는 independent review MUST.
2. personal / explicitly AGY-authorized environment는 AGY/Gemini review MUST.
3. AI finding과 AI가 붙인 severity는 **provisional**이다.
4. review 결과를 무조건 수용하지도, 근거 없이 무시하지도 않는다.
5. release gate는 **calibrated severity + evidence + governance**로 결정한다.
6. company/client data의 personal/external AGY 전송은 DEFAULT DENY.
7. company에서는 기존 governance에 mapping하고 shadow process를 만들지 않는다.

## 1. Authority

```text
법률 / 계약 / 고객 요구
> 회사 정책 / 승인된 프로젝트 표준
> 프로젝트 AGENTS / 보안 / AI-use / 데이터 반출 정책
> 이 REVIEW_POLICY.md
> 다른 handbook rules
> AI preference
```

## 2. Review unit and applicability

Review는 **개별 편집/commit마다가 아니라 하나의 logical change set / PR / release candidate**를 기본 단위로 한다. TDD 중 작은 test iteration마다 AGY를 다시 호출하지 않는다.

### `SUBSTANTIVE` — independent final review MUST

다음 중 하나라도 logical change set에서 달라지면 substantive다.

- runtime / externally observable behavior
- business or acceptance semantics
- authn/authz/security boundary
- persistent data/schema/query/migration/invariant
- API/event/file/interface contract
- runtime/config/profile behavior
- infra/network/container behavior
- CI/build/release/deployment/quality-gate behavior
- dependency change affecting build/runtime/security/support
- test change that weakens, removes, or materially redefines acceptance/regression protection
- operational safety/recovery behavior
- documentation-as-code/generated config affecting system/policy/deployment behavior

### `NON_SUBSTANTIVE`

아래가 모두 유지될 때만 review를 `NOT_APPLICABLE`로 둘 수 있다.

- runtime/build/release/security/data behavior unchanged
- externally visible contract unchanged
- acceptance/regression semantics unchanged
- meaning-preserving typo/format/dead-link/editorial-only change

**Test-only additions/refactors**가 product behavior와 기존 acceptance를 바꾸지 않고 보호 범위만 추가·정리하는 경우, 그 자체는 별도 AGY review unit으로 만들지 않고 관련 logical change의 final review에 포함하거나 standalone이면 risk에 따라 lightweight independent review로 처리할 수 있다.

회색 영역은 substantive로 보는 쪽을 기본값으로 한다.

## 3. Design review

MEDIUM/HIGH architecture/security/data/operation decision은 implementation 전에 independent design review MUST.

Personal / AGY-authorized 환경에서는 AGY/Gemini design review를 사용한다.

LOW substantive change는 separate pre-design review를 생략할 수 있지만 final review는 수행한다.

## 4. Review independence

첫 independent pass 입력:

```text
original requirement / acceptance / constraints
+ exact draft/commit/diff
+ necessary architecture/context
+ deterministic verification evidence
```

implementation-agent conclusion, expected finding, desired verdict를 truth로 seed하지 않는다.

## 5. AI finding severity is provisional

AI raw label은 release gate를 직접 결정하지 않는다.

```text
AI finding
-> PENDING_TRIAGE
-> severity rubric calibration
-> evidence/arbitration
-> final disposition
```

AI가 `BLOCKER`라고 썼다는 사실만으로 즉시 BLOCKER가 되지 않는다. 반대로 낮은 severity를 붙였어도 실제 영향이 크면 상향한다.

## 6. Severity rubric

### `BLOCKER`

다음 중 하나가 **credible failure path / evidence / explicit obligation**과 함께 존재하여, fix 또는 authorized waiver 없이 release하기 부적절한 경우.

- authn/authz/security boundary bypass 또는 high-impact exploit path
- data loss/corruption 또는 critical invariant breach
- explicit law/contract/project-policy violation
- core acceptance/required behavior failure로 release 목적을 달성할 수 없음
- destructive migration/release/recovery path가 demonstrably unsafe 또는 non-recoverable
- verification evidence가 무효라 변경 안전성을 판단할 수 없음
- active production safety/exposure가 크고 containment가 필요함

단순 architecture preference, 일반적인 HA 권고, future scalability 우려, style/maintainability 논쟁만으로 BLOCKER를 부여하지 않는다. explicit requirement나 실제 위험이 연결되면 그때 영향에 맞게 분류한다.

### `MAJOR`

release 전 해결이 기본인 substantial correctness/reliability/security/data/operability/test-adequacy risk. 다만 authorized risk acceptance가 가능한 수준.

### `MINOR`

현재 correctness/safety를 직접 깨지 않지만 개선 가치가 있음.

### `NIT`

style/naming/expression 등 선택적 개선.

### `QUESTION`

의도/근거 확인. 답변에 따라 재분류.

## 7. Severity triage anti-gaming

AI high-severity finding을 낮추려면 **finding 전체 scope와 counter-evidence scope가 일치**해야 한다.

- narrow test로 일부 symptom만 반증하면서 broader security/design risk를 Category A로 위장하지 않는다.
- finding에 factual claim과 interpretive risk가 섞여 있으면 분리한다.
  - factual portion -> direct evidence로 판정 가능
  - residual interpretive/risk portion -> independent arbitration 대상
- security/data-integrity finding은 threat/failure path가 남는 한 단순 happy-path test만으로 종결하지 않는다.

## 8. Arbitration

### 8.1 Directly falsifiable factual finding

다음이 finding의 **core factual premise 전체를 직접 반증**하면 owner가 second reviewer 없이 `REJECTED` 또는 severity-lowered `MODIFIED`를 기록할 수 있다.

- same-condition deterministic reproduction/test
- exact-version current official docs
- direct runtime probe
- explicit requirement/contract

`내 경험상`, broad best practice, preference는 충분하지 않다.

### 8.2 Interpretive / risk / design finding

architecture trade-off, exploitability, concurrency/partial-failure, operability, requirement ambiguity, conflicting evidence처럼 하나의 객관적 증거로 끝나지 않으면 implementer-alone disposition을 금지한다.

#### Personal

1. available evidence
2. eligible second independent semantic reviewer
3. **tie-break / risk decision**

Second reviewer 결과:

- **both reviewers substantially agree on the risk:** finding을 fix하거나 owner가 `RISK_ACCEPTED`/`WAIVED`로 명시한다. `REJECTED`라고 쓰지 않는다.
- **reviewers disagree:** owner는 evidence와 trade-off를 기록하고 `MODIFIED`, `RISK_ACCEPTED`, 또는 fix를 선택한다. 두 의견을 숨기지 않는다.
- **both inconclusive:** severity를 억지로 확정하지 말고 conservative risk statement와 bounded mitigation/test를 추가한 뒤 owner가 proceed/hold를 기록한다.

Personal owner는 external obligations가 없는 자신의 프로젝트에서 위험을 의식적으로 수용할 수 있다. 단, 이를 false-positive `REJECTED`로 위장하지 않고 `RISK_ACCEPTED` 또는 `WAIVED`로 남긴다.

#### Company/client

- company/project policy 우선
- BLOCKER/MAJOR downgrade/reject/risk acceptance는 appropriate human peer/tech lead/security owner/designated approver concurrence와 공식 절차를 따른다.
- 개인 handbook이 owner에게 회사 risk acceptance 권한을 부여하지 않는다.

## 9. Eligible independent reviewer

Reviewer는:

- implementation과 분리됐거나 fresh context임
- relevant source/evidence를 직접 검토 가능
- correctness/security/data/operation semantic reasoning 가능
- review result와 capability/identity 기록 가능

가능:
- human peer/lead/security reviewer
- company-approved internal AI reviewer
- 별도 fresh-context high-capability reasoning/review AI

불충분:
- same implementation-agent self-review
- formatter/linter/test runner/SAST alone
- context를 충분히 읽지 못하는 단순 classifier/small utility model

High-risk personal substitute AI는 **현재 작업의 full relevant context와 반례/보안/실패 분석을 수행할 수 있는 reasoning/review capability**가 있어야 한다. 모델 이름 자체로 품질을 보장하지 않는다.

## 10. Company/client data boundary — DEFAULT DENY

회사·고객의 source/diff/schema/design/log/API/internal context/sensitive data/non-public derived context를 personal Synology, public GitHub, personal external LLM 또는 unapproved AI에 보내지 않는다 unless explicit authorization covers:

- service/endpoint
- repository/project scope
- data classification
- applicable contractual/security conditions

현재 personal Synology AGY는 company data에 승인된 것으로 가정하지 않는다.

Authorization absent/unclear:

```text
AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA
-> company-approved independent reviewer
```

### Generic question

회사 사실 없이 독립적으로 formulation 가능한 일반 기술 질문은 external use가 가능할 수 있다. 그러나 non-public code/names/values/business rules/topology/project-specific failure/data shape를 포함하거나 재구성 가능하면 derived context로 취급한다. 불확실하면 외부 전송하지 않는다.

## 11. Company adoption — no shadow governance

Handbook discipline은 기존 artifact에 mapping한다.

```text
requirements/design -> ticket/design doc
verification -> CI/test
independent review -> PR reviewer/security review/approved internal AI
reconciliation -> PR discussion/review record
release -> existing change management
```

팀에 personal AGY sign-off나 별도 workflow를 강제하지 않는다 unless formally adopted.

## 12. Tool failure

### Personal

AGY unavailable:

```text
AGY_RUNTIME_UNAVAILABLE
```

AGY recovery 또는 eligible owner-approved substitute independent reviewer가 review할 때까지 normal Done을 보류한다.

### Company/client

personal/approved AGY failure를 organization delivery SPOF로 만들지 않는다. company-approved mandatory reviewer path가 완료되면 independent-review requirement를 충족할 수 있다.

## 13. Break-glass — both design and final review deferrable

Active incident/data-loss/security containment에서는 review delay가 더 위험하면 **pre-design과 final review 모두 defer 가능**.

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

- project policy deadline 우선
- open-ended defer 금지
- no-policy case: concrete due point
- 늦어도 incident/problem closure 또는 same-area next non-emergency production change 전에 review/reconciliation

### Post-release serious finding

- company: project incident/change process + human/security escalation
- personal: owner가 incident owner 역할을 맡아 exposure를 재평가하고 필요 시 rollback/disable/contain/forward-fix 후 evidence를 남김

## 14. Risk acceptance / waiver

`RISK_ACCEPTED` 또는 `WAIVED`는 `REJECTED`와 다르다.

최소 기록:

```text
finding
calibrated severity
reason not fixed now
known impact / blast radius
mitigation
reviewer opinions
owner/authorized approver
expiry/revisit trigger if relevant
```

Company/client risk acceptance는 company-authorized role/process만 수행할 수 있다.

## 15. Evidence hierarchy

```text
law/contract/company/project policy
> runtime/test/reproduction
> current official docs
> project requirements/architecture
> reputable engineering practice
> independent AI finding
> implementer self-claim
```

상위 evidence는 finding scope를 실제로 반증할 때만 dismissal 근거가 된다.

## 16. Done

Done requires, as applicable:

- deterministic verification
- required independent design/final review
- severity calibration
- arbitration/reconciliation
- no unresolved unwaived calibrated BLOCKER
- MAJOR fixed or policy-compliant risk acceptance
- company reviewer/data-boundary compliance

Test PASS alone is not Done.

## 17. Review history

- 2026-09-06 v1.0: owner mandatory-review intent.
- #353: self-arbitration, company default-deny, tool SPOF, shadow governance -> v1.1.
- #354: solo arbitration deadlock, break-glass pre-design ambiguity -> v1.2.
- #355: severity calibration, personal tie-break deadlock, Category A/B gaming, TDD review overhead, substitute capability, solo break-glass gaps -> v1.3.
- Adopted/modified rather than blindly copied: AGY raw severity no longer directly defines release blocking; personal owner can consciously waive a reviewed risk but must record it as risk acceptance, not pretend it was a false positive.
