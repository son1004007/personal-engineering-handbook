# Code Review Standard

- status: `draft`
- version: `0.5`
- baseline_date: `2026-09-06`
- primary_practice_reference: `Google Engineering Practices`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.2`

## 목적

코드 리뷰는 style lint의 반복이 아니라 변경이 시스템에 들어가도 되는지 의미 수준에서 판단하는 quality gate다. Substantive engineering change는 independent review를 반드시 거치며, finding은 evidence/arbitration을 거쳐 최종 disposition을 정한다.

## 리뷰 순서

1. 목적과 scope
2. 설계 적합성
3. 기능/업무 규칙
4. 보안·데이터 정합성
5. 실패/예외/동시성
6. 테스트와 evidence
7. 복잡도·이해 가능성
8. 운영/배포 영향
9. 문서/주석/traceability
10. style/format은 가능한 한 자동화

## Core rules

### CR-001 — 변경 목적을 먼저 이해한다 — MUST
리뷰어는 diff만 보지 않고 requirement/issue/change description을 확인한다.

### CR-002 — Correctness와 design을 style보다 우선한다 — MUST
업무 규칙, 권한, 데이터, transaction/concurrency, side effect, failure/recovery, 테스트 누락을 우선한다.

### CR-003 — 변경은 필요한 만큼만 복잡해야 한다 — SHOULD
더 작은 변경, 불필요한 abstraction, 미래 추측, 중복 정책을 검토한다.

### CR-004 — 테스트가 주장과 일치하는지 확인한다 — MUST
테스트 존재 자체가 아니라 실제 위험과 acceptance를 검증하는지 확인한다.

### CR-005 — 작성자의 설명과 실행 증거를 분리한다 — MUST
PR의 주장과 CI/test/runtime evidence를 구분한다. 실행하지 못한 것은 `NOT RUN/BLOCKED`로 유지한다.

### CR-006 — 보안·상태 변경·외부 I/O는 우선 검토 대상이다 — MUST
authorization, transaction, migration, bulk operation, external I/O, secret/config, concurrency/idempotency, AI output 실행을 높은 우선순위로 본다.

### CR-007 — 설명은 구현과 일치해야 한다 — SHOULD
README/Javadoc/comment/ADR와 실제 코드가 충돌하면 requirement/test/decision evidence로 의도를 확인한다.

## CR-008 — Mandatory independent review — MUST

- personal / explicitly AGY-authorized substantive change: AGY/Gemini final review MUST
- MEDIUM/HIGH personal / AGY-authorized design: AGY/Gemini design review MUST + final review MUST
- company/client: independent review MUST; external/personal AGY is DEFAULT DENY until explicit authorization
- AGY not authorized: `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA` + company-approved reviewer

## CR-009 — BLOCKER/MAJOR arbitration — MUST

AGY/equivalent independent AI:

```text
BLOCKER -> PENDING_BLOCKER
MAJOR   -> PENDING_MAJOR
```

arbitration 전까지 blocking이다.

### Directly falsifiable finding

같은 조건의 deterministic test/reproduction, exact-version official docs, direct runtime evidence 또는 explicit contract가 finding의 핵심 전제를 **직접 반증**하면 owner가 evidence를 남기고 `REJECTED` 또는 severity-lowered `MODIFIED`로 판정할 수 있다.

이때 추가 reviewer는 필수가 아니다. 단순 경험/선호/일반론은 direct counter-evidence가 아니다.

### Interpretive / risk / design finding

architecture/security/exploitability/concurrency/operability/requirement ambiguity처럼 evidence가 하나의 결론을 강제하지 않는 BLOCKER/MAJOR는 implementer-alone disposition을 금지한다.

- personal: counter-evidence + fresh-context independent semantic reviewer + owner decision record
- company/client: counter-evidence + human peer/tech lead/security owner/designated reviewer concurrence + official project policy

자동 test/linter/SAST는 evidence이지 semantic reviewer 자체가 아니다.

### Company-approved internal AI

회사 정책이 internal AI finding arbitration을 별도로 정의하면 그 정책을 따른다. 별도 규칙이 없으면 이 handbook의 pending/disposition 원칙을 fallback으로 사용하되 BLOCKER/MAJOR 기각·하향은 human concurrence를 요구한다.

## Review finding 수준

### `BLOCKER` — merge/release blocking
보안 경계 실패, 데이터 손상/유실, 핵심 요구 위반, 복구 어려운 운영 결함, 검증 자체 무효 등.

### `MAJOR` — blocking by default
correctness, reliability, security, maintainability 또는 test adequacy의 실질적 문제. 상위 policy가 허용하는 명시적 risk acceptance가 있을 때만 예외.

### `MINOR` — non-blocking by default
현재 안전성/정확성을 깨지 않지만 개선 가치가 있는 문제.

### `NIT` — non-blocking
style/naming/표현 등 선택적 개선.

### `QUESTION` — non-blocking until evidence changes severity
의도/근거 질문. 답변으로 결함이 확인되면 재분류한다.

## 승인 조건

- 목적/scope와 requirement가 맞다.
- deterministic verification evidence가 있다.
- mandatory independent review가 수행됐다.
- pending findings가 arbitration/reconciliation됐다.
- `PENDING/CONFIRMED BLOCKER`가 없다.
- unresolved MAJOR는 해결되었거나 policy-compliant risk acceptance가 있다.
- residual risk가 숨겨져 있지 않다.

회사 프로젝트에서는 AI review가 조직이 요구하는 human approval/SoD를 대체하지 않는다.

## Break-glass

`REVIEW_POLICY.md`에 따라 active incident에서는 **pre-design review와 final review 모두** containment를 지연시키면 명시적으로 defer할 수 있다. `review_scope`, `review_owner`, `review_due`와 post-release arbitration을 남긴다.

## Company adoption

handbook review를 기존 PR discussion, team reviewer, security review, CI/change-management에 mapping한다. 개인 AGY sign-off를 별도 조직 gate로 강제하지 않는다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: AGY/Gemini #351 severity semantics incorporated.
- 2026-09-06: Owner made independent review mandatory.
- 2026-09-06: AGY/Gemini #353/#354 governance findings selectively incorporated into REVIEW_POLICY v1.2 and this standard.
