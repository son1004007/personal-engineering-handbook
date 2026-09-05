# Mandatory Independent Review Policy

- status: `approved`
- version: `1.1`
- approved_date: `2026-09-06`
- last_reviewed: `2026-09-06`
- owner_decision: `AGY/Gemini independent review is mandatory where authorized; findings are evidence to reconcile, not automatic truth`
- review_evidence: `device-control issue #353 / workflow run 33996776013`
- applies_to: `personal projects and owner-led engineering practice adopted into company/client projects, subject to higher authority and data-boundary policy`

## Purpose

이 정책은 개인 프로젝트뿐 아니라 회사·고객 프로젝트에서 내가 수행하는 기획, 설계, 구현, 테스트, 보안 검토와 검수에 **독립적인 반대 관점**을 강제로 넣기 위한 기본 quality gate다.

핵심 원칙은 다음과 같다.

1. **허용된 환경에서 AGY/Gemini independent review 자체는 필수다.**
2. **AGY/Gemini의 결론을 무조건 수용하지 않는다.**
3. **BLOCKER/MAJOR finding은 구현자가 혼자 기각하거나 하향할 수 없다.**
4. **회사·고객 데이터는 외부/개인 AGY에 대해 default-deny로 취급한다.**
5. **회사 프로젝트에서는 이 handbook을 별도 shadow process로 만들지 않고 기존 PR/CI/security/change-management 절차에 mapping한다.**

리뷰의 목적은 다른 모델의 권위를 추가하는 것이 아니라, 구현자/주요 AI가 놓친 반례·모순·보안·데이터·운영 위험을 독립적으로 발견하고 이를 증거에 따라 다시 판단하게 하는 것이다.

## 1. Authority

```text
법률 / 계약 / 고객 요구
> 회사 정책과 승인된 프로젝트 표준
> 프로젝트의 명시적 AGENTS / 보안 / AI-use / 데이터 반출 정책
> 이 REVIEW_POLICY.md
> handbook의 다른 approved/reviewed 규칙
> AI preference
```

이 handbook은 조직의 공식 change-management나 code-review 절차를 대체하거나 축소하지 않는다.

## 2. 무엇을 반드시 리뷰하는가

### 2.1 Substantive engineering change — final independent review MUST

다음 중 하나라도 바꾸는 작업은 완료/merge/release 판정 전에 독립 review를 받아야 한다.

- application/source code behavior
- configuration 또는 runtime behavior
- database schema/query/migration/data contract
- API/event/file/interface contract
- authentication/authorization/security control
- infrastructure/network/container/CI/CD/deployment behavior
- external dependency 추가·교체·주요 version 변경
- 중요한 test strategy 또는 acceptance behavior
- 운영 절차 중 장애·복구·권한·데이터 안전성에 영향을 주는 변경

개인 프로젝트와 AGY 사용이 명시적으로 허용된 프로젝트에서는 **AGY/Gemini final review를 MUST 수행한다.**

단순 오탈자, 링크 수정, formatting처럼 **실행 동작·업무 의미·검증 기준을 전혀 바꾸지 않는 변경**은 `review: NOT_APPLICABLE (non-substantive)`로 기록하고 생략할 수 있다.

### 2.2 Planning / design review — MEDIUM/HIGH MUST

다음과 같은 MEDIUM/HIGH 결정은 구현 전에 독립 설계 review를 받는다.

- architecture/component responsibility
- authentication/authorization model
- data ownership/invariant/migration
- external API/integration contract
- transaction/concurrency/idempotency strategy
- deployment/recovery strategy
- security policy/control
- 여러 합리적 대안 중 되돌리기 어려운 선택

AGY 사용이 허용된 환경에서는 AGY/Gemini design review를 사용한다.

LOW-risk 국소 변경은 별도 사전 design review를 생략할 수 있지만 final review는 substantive change라면 여전히 필요하다.

## 3. 독립성 요구 — MUST

첫 AGY/Gemini review는 가능한 한 implementation agent의 결론에 오염되지 않은 상태에서 수행한다.

리뷰 입력의 기본값:

```text
원래 요구 / acceptance / 제약
+ review 대상의 exact commit/diff 또는 정확한 draft
+ 필요한 architecture/context
+ 실행된 test/evidence
```

첫 review prompt에 다음을 정답처럼 주입하지 않는다.

- ChatGPT/Codex가 이미 내린 최종 결론
- 예상 finding 목록
- "문제가 없다고 확인해 달라"는 식의 유도 문구

독립 1차 review 후 reconciliation 단계에서 기존 분석과 비교한다.

## 4. Review output 요구 — MUST

AGY/Gemini에게 최소 다음을 요구한다.

- overall verdict
- `BLOCKER / MAJOR / MINOR / NIT / QUESTION` finding
- finding별 대상 section/file/behavior
- 왜 문제인지와 실패 시 영향
- 가능한 경우 재현/검증 방법
- 제안 수정안
- 확신이 낮거나 공식 문서 확인이 필요한 부분 표시

제품·framework·표준의 구체 동작을 기억만으로 단정하지 않도록 요구한다.

## 5. Finding reconciliation — MUST

AGY/Gemini finding은 **provisional review input**이다.

기본 상태:

- AGY `BLOCKER` -> `PENDING_BLOCKER`
- AGY `MAJOR` -> `PENDING_MAJOR`
- `MINOR / NIT / QUESTION` -> non-blocking review item

`PENDING_BLOCKER`와 `PENDING_MAJOR`는 아래 arbitration이 끝날 때까지 release/merge를 기본적으로 차단한다.

### 5.1 Finding disposition

각 finding은 다음 중 하나로 판정한다.

- `ACCEPTED`: 증거상 타당하여 반영
- `MODIFIED`: 핵심 문제는 타당하지만 제안·범위·severity를 수정하여 반영
- `REJECTED`: finding이 실제 context에 적용되지 않거나 false positive임을 근거로 기각
- `DEFERRED`: 현재 scope 밖이지만 별도 issue/risk로 추적

### 5.2 객관적 arbitration — BLOCKER/MAJOR MUST

**구현자 또는 implementation agent는 AGY의 BLOCKER/MAJOR를 단독으로 `REJECTED`, 하향 `MODIFIED`, release 가능한 `DEFERRED` 상태로 만들 수 없다.**

#### Personal project

AGY BLOCKER/MAJOR를 기각하거나 severity를 낮추려면 최소 다음이 필요하다.

1. finding을 반증하거나 적용 불가임을 보여주는 **객관적 evidence** 중 하나 이상:
   - deterministic reproduction/test
   - current official vendor/framework/standard documentation
   - 직접 확인한 runtime behavior
   - 명시적 requirement/architecture constraint
2. implementation에 관여하지 않은 **second independent reviewer**의 arbitration review
3. owner의 최종 disposition 기록

second reviewer는 human 또는 별도 독립 AI/context일 수 있으나, 동일 implementation agent의 자기 재판정만으로 대체하지 않는다.

#### Company/client project

AGY BLOCKER/MAJOR의 기각, severity 하향, release 가능한 defer에는 최소 다음이 필요하다.

1. 위와 같은 객관적 counter-evidence
2. 프로젝트 정책상 적절한 **human peer / tech lead / security owner / designated reviewer의 명시적 동의**
3. 프로젝트의 공식 review/risk-acceptance 절차 준수

회사 프로젝트에서 개인 AI 또는 구현자 단독 판단으로 AGY BLOCKER/MAJOR를 무효화하지 않는다.

### 5.3 DEFERRED 제한

- `BLOCKER`는 단순 backlog 이동만으로 release 가능 상태가 되지 않는다.
- `MAJOR`도 기본적으로 merge/release 전에 해결한다.
- 예외적 risk acceptance는 해당 프로젝트에서 권한 있는 사람이 승인하고 후속 owner/issue/기한을 기록해야 한다.

### 5.4 판정 우선순위

```text
법률/계약/회사·프로젝트 policy
> 현재 runtime/test/reproduction evidence
> current official standard/vendor/framework documentation
> 프로젝트 요구/architecture evidence
> 신뢰 가능한 공개 engineering practice
> AGY/Gemini finding
> implementation agent의 자체 주장
```

AGY가 높은 severity를 붙였다는 이유만으로 무조건 수용하지 않고, 불편한 finding이라는 이유만으로 단독 기각하지도 않는다.

## 6. 회사·고객 프로젝트의 data boundary — DEFAULT DENY / MUST

### 6.1 보수적 기본값

이 handbook의 기본값은 **외부/개인 AGY에 대한 DEFAULT DENY**다.

회사·고객 프로젝트의 source, diff, schema, design, log, API contract, 내부 URL/IP, 고객정보, 개인정보, 기밀정보 또는 파생정보를 다음으로 보내는 것은 **명시적 허용 근거가 확인되기 전에는 금지**한다.

- 개인 Synology의 AGY bridge
- 개인 계정의 외부 Gemini/LLM endpoint
- public repository/issue
- 회사가 승인하지 않은 외부 AI service

**현재 개인 Synology AGY bridge가 회사 프로젝트 데이터에 대해 승인되었다고 가정하지 않는다.**

### 6.2 AGY 사용 가능 조건

다음이 확인될 때만 회사/고객 내용으로 AGY review를 수행한다.

- 회사/고객의 AI-use 정책상 해당 service/endpoint가 승인됨
- 해당 repository/project가 허용 범위에 포함됨
- data classification상 전송이 허용됨
- 필요한 경우 계약/NDA/보안 정책의 외부 처리 조건 충족

가능하면 회사가 제공하거나 승인한 내부/enterprise AI boundary를 우선한다.

### 6.3 명시적 허용을 확인하지 못한 경우

```text
review: AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA
```

로 기록하고 **회사/프로젝트가 승인한 독립 reviewer**(human peer, tech lead, security reviewer, approved internal AI 등)로 mandatory review gate를 수행한다.

- 금지되었거나 승인 여부가 불명확한 데이터를 개인 환경으로 반출하지 않는다.
- 비식별·요약본도 파생정보 반출이 허용된다는 근거가 없으면 보내지 않는다.
- 이 경우 `AGY reviewed`라고 기록하지 않는다.

즉 **회사에서는 독립 review가 mandatory이며, 개인 AGY 사용은 명시적으로 승인된 경우에만 선택 가능한 구현 수단**이다.

## 7. Company adoption — shadow governance 방지

회사·고객 프로젝트에 이 handbook을 적용할 때 **새로운 조직 프로세스를 병렬로 강제하지 않는다.**

가능한 한 다음처럼 기존 artifact에 mapping한다.

```text
handbook requirement/design check -> existing ticket / design doc
verification -> existing CI / test evidence
independent review -> existing PR reviewer / security review / approved internal AI
reconciliation -> PR discussion / review record
release decision -> existing change-management process
```

내 개인 handbook 때문에 팀이 별도 11단계 workflow, 추가 문서, 개인 AGY sign-off를 공식 gate로 받아들여야 한다고 주장하지 않는다. 팀이 formal adoption하지 않았다면 이는 **내 작업 품질을 높이는 개인 discipline**으로 사용한다.

## 8. Tool/runtime failure

### Personal project

AGY 사용이 승인된 개인 프로젝트에서 AGY bridge/runtime/API가 실패하면:

```text
review: BLOCKED
reason: AGY_RUNTIME_UNAVAILABLE | AUTH_FAILURE | BRIDGE_FAILURE | ...
```

로 기록하고 정상 경로에서는 `Done` 처리하지 않는다. owner가 별도 independent reviewer를 명시적으로 승인해 substitute하는 경우 그 evidence를 남긴다.

### Company/client project

개인 AGY 또는 승인된 AGY tool의 장애가 회사 delivery의 SPOF가 되어서는 안 된다.

AGY가 정책상 허용되었지만 runtime/API가 실패하면:

```text
review: AGY_RUNTIME_UNAVAILABLE
```

를 기록하고 **회사/프로젝트의 기존 mandatory review gate**(예: human peer PR approval, security review, approved internal AI)가 완료되면 handbook의 independent-review 의무를 충족한 것으로 볼 수 있다.

개인 tooling 장애 때문에 조직의 공식 delivery commitment를 임의로 차단하지 않는다.

## 9. Break-glass exception

active incident, 데이터 손상 확대, 즉시 containment가 필요한 security event에서는 review가 **containment/hotfix 자체를 지연시키면 안 된다.**

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> MANDATORY INDEPENDENT REVIEW
-> RETRO / RECONCILE
```

긴급 release 전에 review를 못 했다면 반드시 다음을 기록한다.

```text
review: REVIEW_DEFERRED_BREAK_GLASS
review_owner: <responsible person/role>
review_due: <project-policy deadline or explicit concrete deadline>
```

### Deadline rule

- 프로젝트 incident/change policy가 deadline을 정의하면 그것을 따른다.
- 별도 정책이 없으면 open-ended defer를 허용하지 않고 owner가 concrete due point를 정한다.
- 늦어도 **incident/problem record를 종료하거나 같은 영역의 다음 non-emergency production change를 release하기 전**에는 review/reconciliation을 끝낸다.

전역적으로 임의의 `24h/48h` 숫자를 강제하지 않는다.

### Post-release BLOCKER

deferred review에서 production에 이미 배포된 변경의 BLOCKER가 확인되면 단순 backlog item으로 두지 않는다.

- 즉시 현재 exposure/blast radius를 재평가한다.
- 프로젝트 incident/change-management 정책에 따라 containment, rollback, disable 또는 forward-fix를 결정한다.
- 필요한 human/security owner에게 escalation한다.
- remediation evidence가 확보될 때까지 risk를 명시적으로 추적한다.

## 10. 기록 형식

최소 review record:

```text
review_required: yes
reviewer: AGY/Gemini | company-approved reviewer
review_mode: independent read-only
input_ref: <commit/diff/draft identifier>
result: PASS | FINDINGS | BLOCKED | AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA | AGY_RUNTIME_UNAVAILABLE
blocker: <n>
major: <n>
reconciliation:
  accepted: ...
  modified: ...
  rejected: ...
  deferred: ...
arbitration:
  reviewer: ...
  evidence: ...
confirmed_residual_risk: ...
```

회사 프로젝트에서는 review record 자체도 프로젝트의 보안·문서 정책에 맞는 위치에 저장한다.

## 11. 관계

- `OPERATING_MODEL.md`: risk tier와 SDLC tailoring을 정의
- `standards/code-review.md`: finding severity와 merge semantics 정의
- `standards/ai-assisted-development.md`: AI 활용·검증 원칙 정의
- `checklists/definition-of-done.md`: 완료 gate 정의
- `son1004007/ai-agent-workflow-playbook`: ChatGPT/Codex/AGY orchestration 실행 규칙

이 정책은 **문서량은 risk-based로 유지하면서 독립 review를 기본적으로 생략할 수 없게 하고, 회사 프로젝트에서는 개인 AI가 아니라 회사 승인 경계와 human governance를 우선하게 한다.**

## Review record

- 2026-09-06: Owner approved mandatory independent-review intent.
- 2026-09-06: AGY/Gemini issue #353 rejected v1.0 with governance findings.
- 2026-09-06: Accepted/modified: self-arbitration loophole, default-deny company data boundary, company tool-outage fallback, break-glass remediation and non-open-ended due point, shadow-governance concern.
- 2026-09-06: Modified rather than blindly accepted: universal 24h/next-business-day SLA was not adopted; project-defined/concrete due point plus incident-closure/next-release bound is used instead.
