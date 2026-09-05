# Code Review Standard

- status: `draft`
- version: `0.4`
- baseline_date: `2026-09-06`
- primary_practice_reference: `Google Engineering Practices`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.1`

## 목적

코드 리뷰는 style lint의 반복이 아니라 변경이 시스템에 들어가도 되는지 의미 수준에서 판단하는 quality gate다. Substantive engineering change는 independent review를 반드시 거치며, review finding은 증거와 arbitration을 거쳐 최종 severity를 확정한다.

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

리뷰어는 diff만 보지 않고 요구사항, issue 또는 변경 설명을 확인한다.

## CR-002 — Correctness와 design을 style보다 우선한다 — MUST

formatter/linter가 잡을 수 있는 문제보다 업무 규칙, 권한, 데이터, race/transaction, side effect, 테스트 누락을 우선한다.

## CR-003 — 변경은 필요한 만큼만 복잡해야 한다 — SHOULD

더 작은 변경, 불필요한 abstraction, 미래 추측, 중복 정책을 검토한다.

## CR-004 — 테스트가 주장과 일치하는지 확인한다 — MUST

테스트 존재 자체보다 실제 위험과 acceptance를 검증하는지 확인한다.

## CR-005 — 작성자의 설명과 실행 증거를 분리한다 — MUST

PR 본문의 주장과 CI/test/runtime evidence를 구분한다. 실행하지 못한 항목은 `NOT RUN` 또는 `BLOCKED`로 유지한다.

## CR-006 — 보안·상태 변경·외부 I/O는 우선 검토 대상이다 — MUST

특히 authorization, transaction, migration, bulk operation, external API, file, secret/config, concurrency/idempotency, AI output 실행을 높은 우선순위로 본다.

## CR-007 — 설명은 구현과 일치해야 한다 — SHOULD

README/Javadoc/comment/ADR와 실제 코드가 충돌하면 요구사항·테스트·결정 기록으로 의도를 확인한다.

## CR-008 — Mandatory independent review — MUST

- 개인/AGY-authorized 환경의 substantive change: AGY/Gemini final review MUST
- MEDIUM/HIGH 개인/AGY-authorized 환경: AGY/Gemini design review MUST + final review MUST
- 회사/client: independent review MUST. 외부/개인 AGY는 DEFAULT DENY이며 명시적 승인 시에만 사용
- 회사에서 AGY가 승인되지 않으면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`를 기록하고 company-approved reviewer로 대체

## CR-009 — BLOCKER/MAJOR arbitration — MUST

AGY `BLOCKER`와 `MAJOR`는 각각 `PENDING_BLOCKER`, `PENDING_MAJOR`로 취급하며 arbitration 전까지 기본 blocking이다.

### Personal

기각/하향에는:
- objective counter-evidence
- implementation과 독립된 second reviewer
- owner final disposition

이 필요하다.

### Company/client

기각/하향에는:
- objective counter-evidence
- human peer / tech lead / security owner / designated reviewer concurrence
- project review/risk-acceptance policy 준수

가 필요하다.

구현자 단독으로 BLOCKER/MAJOR를 `REJECTED`, 하향 `MODIFIED`, release 가능한 `DEFERRED`로 만들지 않는다.

## Review finding 수준

### `BLOCKER` — merge/release blocking

보안 경계 실패, 데이터 손상/유실, 핵심 요구 위반, 복구 어려운 운영 결함, 검증 자체 무효 등의 문제.

### `MAJOR` — 기본 blocking

correctness, reliability, security, maintainability 또는 test adequacy의 실질적 문제. 상위 policy가 허용하는 명시적 risk acceptance가 있을 때만 예외.

### `MINOR` — non-blocking by default

안전성/정확성을 직접 깨지는 않지만 개선 가치가 있는 문제.

### `NIT` — non-blocking

style, naming, 표현 등 선택적 개선.

### `QUESTION` — non-blocking until evidence changes severity

의도/근거 확인 질문. 답변으로 실제 결함이 드러나면 재분류한다.

## 승인 조건

- 변경 목적과 scope가 맞다.
- 중요한 요구사항을 충족한다.
- deterministic verification evidence가 있다.
- mandatory independent review가 수행됐다.
- review findings가 arbitration/reconciliation됐다.
- pending/confirmed BLOCKER가 없다.
- unresolved MAJOR는 해결되었거나 상위 policy가 허용하는 explicit risk acceptance가 있다.
- 알려진 잔여 위험이 숨겨져 있지 않다.

회사 프로젝트에서는 human approval/separation-of-duties가 별도로 요구되면 independent AI review가 이를 대체하지 않는다.

## Company adoption

handbook review를 기존 PR discussion, team reviewer, security review와 change-management에 mapping한다. 개인 AGY sign-off를 팀의 별도 공식 gate처럼 강제하지 않는다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: AGY/Gemini #351 severity semantics incorporated.
- 2026-09-06: Owner made independent review mandatory.
- 2026-09-06: AGY/Gemini #353 self-arbitration and company-governance findings incorporated into REVIEW_POLICY v1.1 and this standard.
