# Mandatory Independent Review Policy

- status: `approved`
- version: `1.2`
- approved_date: `2026-09-06`
- last_reviewed: `2026-09-06`
- owner_decision: `Independent review is mandatory; AGY/Gemini is mandatory for personal or explicitly authorized environments; findings are rebuttable evidence, not automatic truth.`
- review_evidence:
  - `device-control issue #353 / workflow run 33996776013`
  - `device-control issue #354 / workflow run 33997032701`

## Purpose

이 정책은 개인 프로젝트와 회사·고객 프로젝트에서 내가 수행하는 기획·설계·구현·테스트·검수에 **독립적인 반대 관점**을 강제로 넣기 위한 quality gate다.

핵심 원칙:

1. **Substantive engineering change의 independent review는 필수다.**
2. **개인 또는 명시적으로 AGY 사용이 승인된 환경에서는 AGY/Gemini review를 필수로 사용한다.**
3. **AGY/Gemini finding은 자동 정답이 아니다.**
4. **BLOCKER/MAJOR를 근거 없이 기각하거나 하향할 수 없다.**
5. **명확한 deterministic/official counter-evidence가 AI finding을 직접 반증하면 불필요한 추가 ceremony를 강제하지 않는다.**
6. **회사·고객 데이터의 개인/외부 AGY 전송은 DEFAULT DENY다.**
7. **회사에서는 기존 PR/CI/security/change-management에 이 원칙을 mapping하며 shadow governance를 만들지 않는다.**

## 1. Authority

```text
법률 / 계약 / 고객 요구
> 회사 정책 / 승인된 프로젝트 표준
> 프로젝트 AGENTS / 보안 / AI-use / 데이터 반출 정책
> 이 REVIEW_POLICY.md
> handbook의 다른 approved/reviewed rules
> AI preference
```

이 handbook은 조직의 공식 review/change-management 절차를 대체하거나 축소하지 않는다.

## 2. Review applicability

### 2.1 `SUBSTANTIVE` — independent final review MUST

다음 중 하나라도 변경하면 substantive로 본다.

- runtime 또는 externally observable application behavior
- business/acceptance semantics
- authentication/authorization/security boundary
- persistent data, schema, query, migration, integrity/ownership rule
- API/event/file/interface contract
- configuration/runtime profile behavior
- infrastructure/network/container behavior
- CI/CD, build, release, deployment 또는 quality gate behavior
- dependency 추가·교체·version 변경으로 build/runtime/security/support 특성이 달라짐
- test/fixture/assertion 변경으로 실제 acceptance 또는 regression protection 의미가 달라짐
- 운영/장애/복구/권한 절차의 안전성 또는 결과가 달라짐
- documentation-as-code/generated config처럼 문서 변경이 실제 시스템/정책/배포 결과를 바꿈

### 2.2 `NON_SUBSTANTIVE` — review MAY be NOT_APPLICABLE

아래 조건을 **모두** 만족할 때만 non-substantive로 본다.

- runtime/build/release/security/data behavior가 바뀌지 않음
- acceptance/test semantics가 바뀌지 않음
- externally visible contract가 바뀌지 않음
- 오탈자, 표현, formatting, dead link 교정 등 의미 보존 변경임

회색 영역은 substantive로 분류하는 쪽을 기본값으로 한다.

Routine dependency bump, test refactor, developer tooling, CI 변경은 이름만으로 제외하지 않고 위 기준으로 판단한다.

## 3. Design review

MEDIUM/HIGH의 다음 결정은 구현 전에 independent design review를 MUST 수행한다.

- architecture/component responsibility
- authentication/authorization model
- data ownership/invariant/migration
- external API/integration contract
- transaction/concurrency/idempotency
- deployment/recovery
- security policy/control
- 되돌리기 어려운 선택

개인 또는 AGY-authorized 환경에서는 AGY/Gemini design review를 사용한다.

LOW substantive change는 별도 pre-design review를 생략할 수 있지만 final review는 수행한다.

## 4. Review independence

첫 independent review 입력 기본값:

```text
original requirement / acceptance / constraints
+ exact draft/commit/diff
+ 필요한 architecture/context
+ deterministic verification evidence
```

첫 pass에는 implementation agent의 결론, 예상 finding, `문제가 없다고 확인해 달라` 같은 유도 문구를 정답처럼 넣지 않는다.

## 5. Review output

최소 요구:

- overall verdict
- `BLOCKER / MAJOR / MINOR / NIT / QUESTION`
- 대상 file/section/behavior
- failure impact
- 가능한 reproduction/verification
- suggested fix
- confidence / official-doc-check-needed 표시

## 6. Finding state

AGY 또는 equivalent independent AI finding은 provisional이다.

```text
BLOCKER -> PENDING_BLOCKER
MAJOR   -> PENDING_MAJOR
MINOR/NIT/QUESTION -> non-blocking review item by default
```

`PENDING_BLOCKER`와 `PENDING_MAJOR`는 arbitration이 끝날 때까지 기본 blocking이다.

Disposition:

- `ACCEPTED`
- `MODIFIED`
- `REJECTED`
- `DEFERRED`

## 7. BLOCKER/MAJOR arbitration

### 7.1 Directly falsifiable finding

다음처럼 finding의 핵심 사실을 **직접 반증하는 결정적 evidence**가 있으면 additional reviewer 없이 owner가 `REJECTED` 또는 severity-lowered `MODIFIED` disposition을 기록할 수 있다.

예:

- 같은 조건의 deterministic reproduction/test가 finding의 전제를 명확히 반증
- 사용 중인 정확한 version의 current official documentation이 주장과 직접 충돌
- 실제 runtime probe가 해당 path/behavior가 존재하지 않음을 직접 입증
- 명시적 requirement/contract가 finding이 가정한 requirement가 존재하지 않음을 명확히 입증

조건:

- evidence가 finding의 핵심을 **직접** 반증해야 한다.
- 단순 `내 경험상`, 일반적인 best practice, 구현자의 해석만으로는 충분하지 않다.
- disposition record에 evidence를 재현 가능하게 남긴다.

이 경우 owner disposition은 **독립성 장벽이 아니라 의사결정 기록**이다.

### 7.2 Interpretive / risk / design finding

다음처럼 객관적 evidence가 하나의 결론을 강제하지 않는 BLOCKER/MAJOR는 구현자가 단독 기각·하향할 수 없다.

- architecture trade-off
- security exploitability/risk 판단
- concurrency/partial-failure 해석이 복수 가능
- maintainability/operability blast radius
- requirement ambiguity
- 여러 evidence가 충돌함

#### Personal project

필요 조건:

1. available counter-evidence
2. **second independent reviewer**
3. owner disposition record

Eligible second independent reviewer:

- 구현에 참여하지 않은 human reviewer, 또는
- **fresh context**에서 원 source/evidence를 독립적으로 검토하는 별도 AI reviewer

동일 implementation agent의 자기 재판정, 단순 formatter/linter/static scanner는 independent semantic reviewer로 계산하지 않는다. 자동 도구 결과는 evidence로 사용한다.

#### Company/client project

필요 조건:

1. objective/counter evidence
2. human peer / tech lead / security owner / designated reviewer의 명시적 concurrence
3. 공식 project review/risk-acceptance process

회사에서 사용하는 approved internal AI finding의 blocking/arbitration은 **회사 정책을 우선**한다. 별도 조직 규칙이 없고 그 AI가 independent reviewer 역할을 수행한다면 이 handbook의 pending/disposition 원칙을 fallback으로 사용하되, BLOCKER/MAJOR 기각·하향은 human concurrence 없이 처리하지 않는다.

### 7.3 DEFERRED

- BLOCKER는 backlog 이동만으로 releaseable이 되지 않는다.
- MAJOR도 기본적으로 merge/release 전 해결한다.
- authorized risk acceptance가 허용되는 경우 owner/issue/due point를 기록한다.

## 8. Evidence hierarchy

```text
법률/계약/회사·프로젝트 policy
> runtime/test/reproduction evidence
> current official standard/vendor/framework documentation
> project requirement/architecture evidence
> reputable engineering practice
> independent AI finding
> implementation agent self-claim
```

상위 evidence가 있다고 해서 자동으로 finding이 무효가 되는 것은 아니다. **실제로 finding의 전제를 반증하는지**를 확인한다.

## 9. Company/client data boundary — DEFAULT DENY

### 9.1 Default

회사·고객의 다음 내용을 개인 Synology AGY, 개인 외부 Gemini/LLM, public GitHub 또는 미승인 외부 AI에 보내는 것은 **명시적 authorization 전에는 금지**한다.

- source/diff
- schema/design/API/internal architecture
- logs/errors
- internal URL/IP/account
- customer/personal/confidential data
- non-public business/security constraints
- 위 정보를 재구성하거나 추론할 수 있는 derived context

현재 개인 Synology AGY bridge는 회사 데이터에 승인되었다고 가정하지 않는다.

### 9.2 Authorization

회사 내용으로 external/AGY review를 하려면 최소 다음이 명시적으로 확인되어야 한다.

- service/endpoint approval
- repository/project scope
- data classification
- applicable contract/security conditions

가능하면 company-approved internal/enterprise AI를 우선한다.

### 9.3 Generic question vs derived information

**독립적으로 작성 가능한 일반 기술 질문**은 회사 confidential derived data로 간주하지 않을 수 있다. 예:

> "Spring transaction self-invocation의 일반 동작은 무엇인가?"

단, 다음 중 하나라도 포함하면 company-derived context로 취급한다.

- 비공개 코드 구조/이름/수치/업무 규칙에서만 나올 수 있는 정보
- 내부 architecture/topology/constraint를 유추할 수 있는 detail
- customer/project-specific failure sequence나 data shape
- scrubbed 형태라도 원 confidential source를 역추론하기 쉬운 조합

불확실하면 외부로 보내지 않고 company-approved source/reviewer를 사용한다.

### 9.4 Not authorized

```text
review: AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA
```

를 기록하고 company-approved human/internal-AI reviewer를 사용한다. 이 경우 `AGY reviewed`라고 기록하지 않는다.

## 10. Company adoption — no shadow governance

기존 artifact에 mapping한다.

```text
requirements/design -> ticket/design doc
verification -> CI/test evidence
independent review -> PR reviewer/security review/approved internal AI
reconciliation -> PR discussion/review record
release -> existing change management
```

팀이 formal adoption하지 않았다면 개인 quality discipline으로 사용하며, 별도 개인 AGY sign-off process를 팀에 강제하지 않는다.

## 11. Substitute independent reviewer

AGY가 사용할 수 없을 때 substitute reviewer는 다음 조건을 만족해야 한다.

- implementation에 직접 참여하지 않았거나 fresh context로 분리됨
- review 대상 source/evidence를 직접 볼 수 있음
- correctness/security/data/operation 관점의 semantic review가 가능함
- 최소권한/read-only가 가능한 경우 그렇게 사용
- review 결과와 identity/capability를 기록

가능한 예:

- human peer/lead/security reviewer
- company-approved internal AI review agent
- 다른 independent AI/model/context

단순 linter, formatter, unit test runner, SAST 결과 자체는 **reviewer가 아니라 evidence**다.

## 12. Tool/runtime failure

### Personal

AGY failure 시:

```text
review: AGY_RUNTIME_UNAVAILABLE
```

정상 Done은 AGY 복구 또는 위 기준을 만족하는 owner-approved substitute independent reviewer가 review를 완료할 때까지 보류한다.

### Company/client

personal/approved AGY failure를 delivery SPOF로 만들지 않는다. 기존 company mandatory human/security/internal-AI review가 완료되면 independent-review 의무를 충족할 수 있다.

## 13. Break-glass — design + final review explicit deferral

Active incident, data-loss expansion, security containment에서는 **pre-implementation design review와 final review 모두 containment를 지연시키면 defer할 수 있다.**

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> DEFERRED DESIGN/FINAL REVIEW
-> RETRO / RECONCILE
```

defer 시:

```text
review: REVIEW_DEFERRED_BREAK_GLASS
review_scope: DESIGN | FINAL | BOTH
review_owner: <responsible person/role>
review_due: <project-policy deadline or explicit concrete deadline>
```

Rules:

- project incident/change policy deadline 우선
- open-ended defer 금지
- 별도 정책이 없으면 concrete due point 지정
- 늦어도 incident/problem record 종료 또는 같은 영역의 다음 non-emergency production change 전에 review/reconciliation 완료
- post-release BLOCKER 발견 시 즉시 exposure/blast radius 재평가 후 project incident/change process에 따라 containment/rollback/disable/forward-fix + human/security escalation

전역 고정 `24h/48h` 숫자는 강제하지 않는다.

## 14. Review record

```text
review_required: yes
reviewer: AGY/Gemini | substitute | company-approved reviewer
review_mode: independent read-only
input_ref: <commit/diff/draft>
result: PASS | FINDINGS | BLOCKED | AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA
blocker: <n>
major: <n>
disposition:
  accepted: ...
  modified: ...
  rejected: ...
  deferred: ...
arbitration:
  type: direct-counter-evidence | second-review | company-governance
  evidence: ...
  reviewer: ...
residual_risk: ...
```

## 15. Review history

- 2026-09-06: v1.0 owner mandatory-review intent established.
- 2026-09-06: AGY/Gemini #353 rejected v1.0; self-arbitration, company default-deny, tool-SPOF, break-glass and shadow-governance findings incorporated into v1.1.
- 2026-09-06: AGY/Gemini #354 returned `NOT_READY`; solo arbitration deadlock and break-glass pre-design-review ambiguity were BLOCKERs.
- 2026-09-06: v1.2 accepted/modified those findings: decisive counter-evidence can resolve directly falsifiable AI findings without mandatory second review; interpretive/risk findings still require independent arbitration; substitute reviewer criteria, internal-AI governance, substantive classification, generic-vs-derived context and explicit design-review break-glass deferral were added.
- AGY suggestion that personal owner disposition should act like institutional separation-of-duties was not adopted; in personal work it is treated as an auditable decision record, not fake organizational separation.
