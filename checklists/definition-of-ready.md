# Definition of Ready

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`

제품 코드 구현을 시작해도 되는지 판단하는 기본 checklist다. 모든 항목을 항상 요구하지 않고 위험도에 따라 tailoring한다.

## Core

- [ ] 해결하려는 문제와 기대 결과가 설명된다.
- [ ] scope와 out-of-scope가 구분된다.
- [ ] 주요 actor/stakeholder가 확인됐다.
- [ ] 중요한 요구사항의 source가 있다.
- [ ] `CONFIRMED / INFERRED / UNKNOWN / CONFLICT`가 구분된다.
- [ ] 중요한 acceptance criteria 또는 검증 방법이 있다.

## Data / Security

- [ ] 데이터 source of truth와 ownership이 필요한 수준에서 확인됐다.
- [ ] protected data/object라면 authorization 기준이 명확하다.
- [ ] 민감정보/secret/개인정보 영향이 확인됐다.
- [ ] destructive migration이나 irreversible action이면 영향과 복구 전략이 있다.

## Design / Operation

- [ ] 주요 component/interface 변경이 파악됐다.
- [ ] 중요한 failure mode가 알려져 있다.
- [ ] external dependency가 있다면 timeout/retry/side effect 고려 여부가 정해졌다.
- [ ] deployment/config/runtime 제약이 필요한 수준에서 확인됐다.

## Testability

- [ ] 무엇을 unit/integration/API/E2E/manual acceptance로 확인할지 대략 정해졌다.
- [ ] 필요한 test data/environment에 접근 가능한지 확인했다.
- [ ] 실행 불가능한 검증이 있다면 `BLOCKED`로 표시했다.

## Decision

- `READY`: 중요한 미확정 사항 없이 구현 가능
- `READY WITH ASSUMPTIONS`: low/reversible risk의 명시된 가정 아래 진행 가능
- `NOT READY`: 권한, 데이터, scope, destructive impact, acceptance 등 핵심 불확실성이 blocker

AI는 `NOT READY`를 숨기기 위해 requirement를 임의 생성하지 않는다.
