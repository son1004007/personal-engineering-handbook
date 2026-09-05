# Code Review Standard

- status: `draft`
- version: `0.3`
- baseline_date: `2026-09-06`
- primary_practice_reference: `Google Engineering Practices`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.0`

## 목적

코드 리뷰는 style lint의 반복이 아니라 변경이 시스템에 들어가도 되는지 의미 수준에서 판단하는 quality gate다. **Substantive engineering change는 AGY/Gemini independent review를 반드시 거치며**, AGY finding은 증거에 따라 reconciliation한 뒤 최종 severity를 확정한다.

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
10. style/format은 자동화 가능한 만큼 자동화

## CR-001 — 변경 목적을 먼저 이해한다 — MUST

리뷰어는 diff만 보지 않고 요구사항, issue 또는 변경 설명을 확인한다. 목적을 모르면 코드가 맞는지 판단할 수 없다.

## CR-002 — Correctness와 design을 style보다 우선한다 — MUST

formatter/linter가 잡을 수 있는 문제보다 다음을 우선한다.
- 잘못된 업무 규칙
- 권한 우회
- 데이터 손상
- race/transaction 문제
- 숨은 side effect
- 불필요한 복잡도
- 테스트 누락

## CR-003 — 변경은 필요한 만큼만 복잡해야 한다 — SHOULD

- 더 작은 변경으로 가능한가?
- abstraction이 실제 문제를 해결하는가?
- 미래 추측 때문에 현재 복잡도를 늘리지 않았는가?
- 같은 정책이 여러 곳에 흩어졌는가?

## CR-004 — 테스트가 주장과 일치하는지 확인한다 — MUST

`테스트 추가` 자체로 충분하지 않다. 테스트가 실제 위험과 acceptance를 검증하는지 본다.

## CR-005 — 작성자의 설명과 실행 증거를 분리한다 — MUST

PR 본문에 `PASS`라고 적혀 있어도 가능한 경우 CI/test/runtime evidence를 확인한다. 실행하지 못한 항목은 `NOT RUN` 또는 `BLOCKED`로 유지한다.

## CR-006 — 보안·상태 변경·외부 I/O는 우선 검토 대상이다 — MUST

특히 다음은 높은 주의를 둔다.
- authorization
- transaction
- migration
- delete/update bulk operation
- external API
- file handling
- secret/config
- concurrency/idempotency
- AI/LLM output의 실행 또는 중요한 상태 반영

## CR-007 — 설명은 구현과 일치해야 한다 — SHOULD

README/Javadoc/comment/ADR가 실제 코드와 충돌하면 구현을 무조건 정답으로 간주하지 않는다. 의도된 계약을 요구사항·테스트·결정 기록으로 확인한다.

## CR-008 — AGY/Gemini independent review를 생략하지 않는다 — MUST

[`../REVIEW_POLICY.md`](../REVIEW_POLICY.md)에 해당하는 substantive change는 final merge/release 전에 AGY/Gemini review evidence가 있어야 한다.

MEDIUM/HIGH 설계 결정은 구현 전 design review evidence도 있어야 한다.

리뷰가 정상적으로 허용된 환경인데 AGY bridge/runtime가 실패했다면 `review: BLOCKED`로 기록하고 완료로 처리하지 않는다.

회사/고객 policy가 AGY에 source 제공을 금지하면 `AGY_NOT_PERMITTED_BY_POLICY`를 기록하고 그 프로젝트가 승인한 independent reviewer로 대체한다. 이 예외는 data-boundary 준수를 위한 것이며 review 자체를 생략하는 예외가 아니다.

## CR-009 — AGY/Gemini finding을 evidence와 대조한다 — MUST

AGY/Gemini가 제시한 finding은 provisional이다. finding별로 다음을 기록한다.

- `ACCEPTED`
- `MODIFIED`
- `REJECTED`
- `DEFERRED`

특히 version-dependent framework/security 주장, 회사/고객 정책 관련 주장, performance 수치, 특정 library 동작은 현재 공식 문서와 실제 runtime/test evidence를 확인한다.

AGY finding의 wording이나 severity 자체를 권위로 사용하지 않는다.

## Review finding 수준

severity는 **reconciliation 후 확인된 위험과 수정 필요성**을 나타내며, 취향 차이를 높게 분류하지 않는다.

### `BLOCKER` — merge/release blocking

다음처럼 즉시 해결하지 않으면 합리적으로 merge/release할 수 없는 문제.

- exploitable/critical security boundary failure
- 데이터 손상·유실 가능성
- 핵심 요구사항의 명백한 위반
- 복구 어려운 migration/운영 결함
- build/test 자체가 유효하지 않아 변경을 검증할 수 없음

### `MAJOR` — 기본적으로 blocking

correctness, reliability, security, maintainability 또는 test adequacy의 실질적인 문제로, merge 전 해결하는 것이 기본값이다.

프로젝트 owner가 명시적으로 residual risk를 수용하고 상위 policy가 이를 허용하며 별도 후속조치를 기록하는 경우 예외가 가능하다.

### `MINOR` — non-blocking by default

현재 변경의 안전성/정확성을 깨지는 않지만 개선 가치가 있는 문제.

### `NIT` — non-blocking

style, naming, 표현 등 선택적 개선. formatter/linter로 자동화 가능한 경우 사람이 반복 지적하지 않는다.

### `QUESTION` — non-blocking until evidence changes severity

의도나 근거를 확인하기 위한 질문. 답변을 통해 실제 결함이 드러나면 BLOCKER/MAJOR/MINOR로 다시 분류한다.

**MUST:** MINOR/NIT/QUESTION을 단지 reviewer preference라는 이유로 merge blocker로 사용하지 않는다. 단, 조직/프로젝트 policy가 더 엄격하면 그 정책을 따른다.

## 승인 조건

리뷰어/owner는 최소한 다음을 납득해야 한다.
- 변경 목적과 scope가 맞다.
- 중요한 요구사항을 충족한다.
- deterministic verification evidence가 있다.
- mandatory AGY/Gemini review 또는 policy-approved replacement review가 수행됐다.
- AGY/independent-review findings가 reconciliation됐다.
- confirmed BLOCKER가 없다.
- unresolved confirmed MAJOR는 해결되었거나 상위 policy가 허용하는 방식으로 명시적 위험 수용됐다.
- 알려진 잔여 위험이 숨겨져 있지 않다.

회사 프로젝트의 human approval/separation-of-duties가 별도로 요구되면 AGY review는 이를 대체하지 않는다.

## AGY/Gemini Review prompt 기본값

첫 독립 리뷰는 가능한 한 neutral하게 요청한다.

```text
Review the supplied requirement/context and exact final diff/source independently.
Find correctness, security, data-integrity, concurrency, failure/recovery,
operability, maintainability, testing and unintended-scope issues.
Classify findings as BLOCKER/MAJOR/MINOR/NIT/QUESTION.
For version-sensitive claims, identify what must be checked against official docs.
Do not assume the implementation author's conclusion is correct.
```

첫 independent pass 뒤에 ChatGPT/Codex/owner가 finding을 공식자료·실행증거와 reconciliation한다.

## 근거

- Google Engineering Practices - Code Review: https://google.github.io/eng-practices/review/
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- Owner mandatory review policy: `../REVIEW_POLICY.md`

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #351 finding accepted; severity and blocking semantics made explicit.
- 2026-09-06: Owner explicitly made AGY/Gemini independent review mandatory for substantive engineering changes while requiring evidence-based reconciliation instead of automatic acceptance.
