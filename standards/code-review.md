# Code Review Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- primary_practice_reference: `Google Engineering Practices`

## 목적

코드 리뷰는 style lint의 반복이 아니라 변경이 시스템에 들어가도 되는지 독립적으로 판단하는 quality gate다.

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

자동 formatter/linter가 잡을 수 있는 문제보다 다음을 우선한다.

- 잘못된 업무 규칙
- 권한 우회
- 데이터 손상
- race/transaction 문제
- 숨은 side effect
- 불필요한 복잡도
- 테스트 누락

## CR-003 — 변경은 필요한 만큼만 복잡해야 한다 — SHOULD

리뷰 중 다음을 질문한다.

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
- LLM output execution/use

## CR-007 — 설명은 구현과 일치해야 한다 — SHOULD

README/Javadoc/comment/ADR가 실제 코드와 충돌하면 구현을 무조건 정답으로 간주하지 않는다. 의도된 계약을 요구사항·테스트·결정 기록으로 확인한다.

## Review finding 수준

- `BLOCKER`: 보안, 데이터 손상, 요구 위반, 중대한 운영 위험
- `MAJOR`: correctness/maintainability/test 문제로 merge 전 해결 필요
- `MINOR`: 개선 가치가 있으나 위험 낮음
- `NIT`: 선택적 style/표현 개선
- `QUESTION`: 의도 확인 필요

중요도를 과장하지 않는다.

## 승인 조건

리뷰어는 최소한 다음을 납득해야 한다.

- 변경 목적과 scope가 맞다.
- 중요한 요구사항을 충족한다.
- 치명적인 보안/정합성 문제가 보이지 않는다.
- 필요한 검증 evidence가 있다.
- 알려진 잔여 위험이 숨겨져 있지 않다.

## AI Review 사용

AI 리뷰는 추가 reviewer로 사용할 수 있으나 사람/실행 증거를 대체한다고 가정하지 않는다.

가능하면 동일 diff를 서로 독립된 모델에 제공하고 한 모델의 finding을 다른 모델에게 정답처럼 전달하지 않는다.

AI finding은 다음을 별도로 확인한다.

- 실제 코드 라인에 근거하는가?
- 라이브러리/버전 동작을 공식 문서로 확인했는가?
- 존재하지 않는 requirement를 만들어내지 않았는가?
- theoretical issue와 실제 exploitable/reproducible issue를 구분했는가?

## 근거

- Google Engineering Practices - Code Review: https://google.github.io/eng-practices/review/
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
