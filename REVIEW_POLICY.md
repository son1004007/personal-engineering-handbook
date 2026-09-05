# Mandatory Independent Review Policy

- status: `approved`
- version: `1.0`
- approved_date: `2026-09-06`
- owner_decision: `AGY/Gemini independent review is mandatory; findings are evidence to reconcile, not automatic truth`
- applies_to: `personal projects and owner-led engineering practice adopted into company/client projects, subject to higher authority and data-boundary policy`

## Purpose

이 정책은 개인 프로젝트뿐 아니라 회사·고객 프로젝트에서 내가 수행하는 기획, 설계, 구현, 테스트, 보안 검토와 검수에 **독립적인 반대 관점**을 강제로 넣기 위한 기본 quality gate다.

핵심 원칙은 두 가지다.

1. **AGY/Gemini review 자체는 필수다.**
2. **AGY/Gemini의 결론을 무조건 수용하지 않는다.**

리뷰의 목적은 다른 모델의 권위를 추가하는 것이 아니라, 구현자/주요 AI가 놓친 반례·모순·보안·데이터·운영 위험을 독립적으로 발견하고 이를 증거에 따라 다시 판단하게 하는 것이다.

## 1. Authority

이 정책도 다음 우선순위를 넘지 않는다.

```text
법률 / 계약 / 고객 요구
> 회사 정책과 승인된 프로젝트 표준
> 프로젝트의 명시적 AGENTS / 보안 / 데이터 반출 정책
> 이 REVIEW_POLICY.md
> handbook의 다른 approved/reviewed 규칙
> AI preference
```

회사 또는 고객 정책이 외부 AI, source code, 설계, 데이터의 반출을 금지하면 **AGY 사용을 위해 이를 우회하거나 축약해서 반출하지 않는다.**

## 2. 무엇을 반드시 리뷰하는가

### 2.1 Substantive engineering change — AGY/Gemini final review MUST

다음 중 하나라도 바꾸는 작업은 완료/merge/release 판정 전에 AGY/Gemini 독립 review를 받아야 한다.

- application/source code behavior
- configuration 또는 runtime behavior
- database schema/query/migration/data contract
- API/event/file/interface contract
- authentication/authorization/security control
- infrastructure/network/container/CI/CD/deployment behavior
- external dependency 추가·교체·주요 version 변경
- 중요한 test strategy 또는 acceptance behavior
- 운영 절차 중 장애·복구·권한·데이터 안전성에 영향을 주는 변경

단순 오탈자, 링크 수정, formatting처럼 **실행 동작·업무 의미·검증 기준을 전혀 바꾸지 않는 변경**은 `review: NOT_APPLICABLE (non-substantive)`로 기록하고 생략할 수 있다.

### 2.2 Planning / design review — MEDIUM/HIGH MUST

다음과 같은 MEDIUM/HIGH 결정은 구현 전에 AGY/Gemini의 독립 설계 review를 받는다.

- architecture/component responsibility
- authentication/authorization model
- data ownership/invariant/migration
- external API/integration contract
- transaction/concurrency/idempotency strategy
- deployment/recovery strategy
- security policy/control
- 여러 합리적 대안 중 되돌리기 어려운 선택

LOW-risk 국소 변경은 별도 사전 design review를 생략할 수 있지만 **최종 변경 review는 substantive change라면 여전히 필수**다.

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

필요하면 독립 1차 review 후 reconciliation 단계에서 기존 finding과 비교한다.

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

AGY/Gemini finding은 **provisional review input**이다. finding마다 다음 중 하나로 판정한다.

- `ACCEPTED`: 증거상 타당하여 반영
- `MODIFIED`: 핵심 문제는 타당하지만 제안이 과도하거나 범위가 틀려 수정하여 반영
- `REJECTED`: 공식 문서, runtime/test evidence, 프로젝트 요구 또는 실제 context와 맞지 않아 기각
- `DEFERRED`: 유효할 수 있으나 현재 scope가 아니며 별도 issue/risk로 추적

판정 우선순위:

```text
법률/계약/회사·프로젝트 policy
> 현재 runtime/test/reproduction evidence
> current official standard/vendor/framework documentation
> 프로젝트 요구/architecture evidence
> 신뢰 가능한 공개 engineering practice
> AGY/Gemini finding
> implementation agent의 자체 주장
```

**AGY가 BLOCKER라고 썼다는 이유만으로 자동 BLOCKER가 되는 것은 아니다.** 반대로 불편한 finding이라는 이유로 근거 없이 무시해서도 안 된다.

reconciliation 후 `CONFIRMED BLOCKER`와 unresolved `CONFIRMED MAJOR`의 merge/release semantics는 `standards/code-review.md`를 따른다.

## 6. 회사·고객 프로젝트의 data boundary — MUST

회사/고객 프로젝트에서 AGY/Gemini를 사용하기 전에 해당 환경에서 다음 정보 제공이 허용되는지 확인한다.

- source/diff
- 요구사항/설계
- 로그/오류
- schema/API
- 고객명/내부 URL/IP/계정
- 개인정보/기밀정보

### 허용되는 경우

승인된 경계 안에서 exact source/diff 기반 AGY review를 수행한다.

### AGY/Gemini에 제공이 금지된 경우

- 금지 데이터를 개인 Synology, public GitHub, 개인 계정의 외부 AI로 반출하지 않는다.
- review 상태를 `AGY_NOT_PERMITTED_BY_POLICY`로 기록한다.
- 회사/프로젝트가 허용한 **독립 reviewer**(내부 AI, 동료, 보안검토자 등)로 mandatory review gate를 대체한다.
- 이 경우 `AGY reviewed`라고 기록하지 않는다.
- 비식별·요약본조차 정책상 파생정보 반출이 허용된다는 근거가 없으면 임의로 만들어 보내지 않는다.

즉 **review 의무는 유지하지만, AGY 사용 의무가 상위 정책을 위반하게 만들지는 않는다.**

## 7. Tool/runtime failure

정상 작업에서 AGY/Gemini review를 수행하도록 허용된 환경인데 도구/bridge가 실패하면:

```text
review: BLOCKED
reason: AGY_RUNTIME_UNAVAILABLE | AUTH_FAILURE | BRIDGE_FAILURE | ...
```

로 기록한다.

이 handbook 기준으로 substantive change를 `Done` 또는 `review complete`로 표시하지 않는다. 프로젝트 owner가 별도 approved reviewer 경로를 사용하면 그 evidence로 대체할 수 있다.

## 8. Break-glass exception

active incident, 데이터 손상 확대, 즉시 containment가 필요한 security event에서는 AGY review가 **containment/hotfix 자체를 지연시키면 안 된다.**

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> MANDATORY AGY/approved-independent REVIEW
-> RETRO / RECONCILE
```

긴급 release 전에 review를 못 했다면 반드시 `REVIEW_DEFERRED_BREAK_GLASS`로 기록하고 정상화 후 review와 finding reconciliation을 수행한다.

## 9. 기록 형식

최소 review record:

```text
review_required: yes
reviewer: AGY/Gemini
review_mode: independent read-only
input_ref: <commit/diff/draft identifier>
result: PASS | FINDINGS | BLOCKED | AGY_NOT_PERMITTED_BY_POLICY
blocker: <n>
major: <n>
reconciliation:
  accepted: ...
  modified: ...
  rejected: ...
  deferred: ...
confirmed_residual_risk: ...
```

회사 프로젝트에서는 review record 자체도 프로젝트의 보안·문서 정책에 맞는 위치에 저장한다.

## 10. 관계

- `OPERATING_MODEL.md`: risk tier와 SDLC tailoring을 정의
- `standards/code-review.md`: finding severity와 merge semantics 정의
- `standards/ai-assisted-development.md`: AI 활용·검증 원칙 정의
- `checklists/definition-of-done.md`: 완료 gate 정의
- `son1004007/ai-agent-workflow-playbook`: ChatGPT/Codex/AGY orchestration 실행 규칙

이 정책은 **문서량은 risk-based로 유지하면서 독립 review 자체는 기본적으로 생략할 수 없게** 한다.
