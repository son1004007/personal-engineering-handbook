# Definition of Ready

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

제품 코드 구현을 시작해도 되는지 판단하는 기본 checklist다. 모든 항목을 항상 요구하지 않고 LOW/MEDIUM/HIGH risk에 따라 tailoring한다.

## LOW

최소한 다음이 분명하면 충분할 수 있다.
- [ ] 해결하려는 문제/변경 목적이 설명된다.
- [ ] 변경 범위가 국소적이고 되돌릴 수 있다.
- [ ] 완료 여부를 확인할 방법이 있다.

## MEDIUM / HIGH Core

- [ ] 해결하려는 문제와 기대 결과가 설명된다.
- [ ] scope와 out-of-scope가 필요한 수준에서 구분된다.
- [ ] 주요 actor/stakeholder가 확인됐다.
- [ ] 중요한 요구사항에 source/evidence가 있다.
- [ ] `CONFIRMED / INFERRED / UNKNOWN / CONFLICT`가 필요한 곳에서 구분된다.
- [ ] 중요한 acceptance criteria 또는 verification 방법이 있다.

## Data / Security

해당되는 경우:
- [ ] data source of truth와 ownership이 확인됐다.
- [ ] protected data/object라면 authorization 기준이 명확하다.
- [ ] secret/개인정보/민감정보 영향이 확인됐다.
- [ ] destructive migration/irreversible action이면 영향과 recovery strategy가 있다.

## Design / Operation

해당되는 경우:
- [ ] 주요 component/interface 변경이 파악됐다.
- [ ] 중요한 failure mode가 알려져 있다.
- [ ] external dependency의 timeout/retry/side effect를 검토했다.
- [ ] deployment/config/runtime/observability 제약을 확인했다.

## Testability

- [ ] 무엇을 자동 test/static analysis/runtime/manual acceptance 등으로 확인할지 정해졌다.
- [ ] 필요한 test data/environment가 있는지 확인했다.
- [ ] 실행할 수 없는 검증은 숨기지 않고 `BLOCKED` 또는 known gap으로 표시한다.

## Decision

- `READY`: 현재 risk tier에서 필요한 핵심 정보와 검증 경로가 확보됨
- `READY WITH ASSUMPTIONS`: 되돌릴 수 있는 기술적 assumption이 명시돼 있고 그 위험을 수용 가능
- `NOT READY`: business/security/data ownership, destructive impact, scope, acceptance 등 핵심 불확실성이 blocker

AI는 `NOT READY`를 숨기기 위해 business/security/data requirement를 임의 생성하지 않는다. Inference 경계는 [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

## Break-glass

운영 장애, active security incident, 데이터 손상 확대처럼 즉시 containment가 더 중요한 경우 이 checklist 전체를 선행조건으로 강제하지 않는다. [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 Break-glass 절차로 최소 안전정보와 bounded verification을 우선하고, 정상화 후 누락된 기록을 reconciliation한다.
