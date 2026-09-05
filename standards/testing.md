# Testing and Verification Standard

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- practical_reference: `ISTQB CTFL v4.0.1`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

## 목적

테스트의 목적은 숫자형 coverage 목표를 채우는 것이 아니라 **요구사항, 설계 가정, 실패 위험을 증거로 확인하는 것**이다.

## TST-001 — 테스트는 위험과 요구사항에서 출발한다 — MUST

중요한 요구사항과 위험은 최소 하나의 검증 방법과 연결한다.

```text
requirement/risk -> test or review -> evidence -> result
```

모든 요구사항이 자동 테스트를 필요로 하는 것은 아니다. 문서 review, static analysis, runtime inspection, manual acceptance가 더 적합할 수 있다.

## TST-002 — 테스트 레벨을 기계적으로 모두 만들지 않는다 — MUST

필요한 레벨을 선택한다.

- unit: 작은 규칙/계산/상태 전이
- integration: DB/framework/external adapter 계약
- contract: 서비스/consumer/provider interface
- API/component: boundary와 대표 업무 흐름
- E2E: 핵심 사용자 흐름과 실제 wiring
- acceptance: 업무/사용자 요구 충족

동일한 동작을 여러 레벨에서 의미 없이 중복하지 않는다.

## TST-003 — Happy path만으로 완료하지 않는다 — MUST for important behavior

해당되는 경우 다음을 포함한다.
- invalid input
- unauthorized / forbidden
- object ownership mismatch
- duplicate/retry
- concurrency/race
- timeout
- partial failure
- rollback
- empty/boundary/large input
- dependency unavailable

## TST-004 — 구현 세부보다 observable behavior를 검증한다 — SHOULD

private method 호출 횟수와 내부 구조에 과도하게 결합한 테스트는 refactoring 비용을 높인다. 업무 규칙과 외부 계약을 우선 검증한다.

예외: 호출 횟수 자체가 중요한 side effect, 비용, idempotency 계약인 경우.

## TST-005 — Mock은 경계를 대체할 때만 사용한다 — SHOULD

모든 dependency를 mock하여 실제 wiring·transaction·serialization 문제를 숨기지 않는다. DB query, mapping, framework configuration 등이 위험의 핵심이면 실제 integration test를 사용한다.

## TST-006 — 데이터 정합성 규칙은 DB까지 확인한다 — SHOULD

unique constraint, foreign key, transaction, isolation, migration 같은 동작은 application unit test만으로 완료 처리하지 않는다.

## TST-007 — Security negative test를 별도로 생각한다 — MUST when access/data is protected

예:
- 인증 없는 접근
- 잘못된 역할
- 다른 사용자 object ID
- tampered parameter
- 민감 field의 과다 응답

## TST-008 — flaky test를 정상으로 받아들이지 않는다 — MUST

재현성이 낮은 테스트는 원인을 추적한다. 단순 retry로 녹색 상태를 만드는 것을 기본 해결책으로 사용하지 않는다.

## TST-009 — Coverage는 보조 지표다 — MUST

line/branch coverage 임계값은 필요하면 사용할 수 있지만, 높은 coverage를 품질 보장으로 해석하지 않는다. 중요한 state transition, failure path, authorization rule이 누락되지 않았는지가 우선이다.

## TST-010 — 실행 evidence 상태를 명확히 구분한다 — MUST

Handbook/PR/test-plan의 **검증 요약**은 다음 상태를 사용한다.

- `PASS`: 계획한 확인을 실제 실행했고 기대를 충족
- `FAIL`: 실행했고 기대를 충족하지 못함
- `NOT RUN`: 계획했거나 필요성을 인식했지만 실행하지 않음
- `BLOCKED`: 환경/권한/자료 때문에 실행할 수 없음

이 4개 상태를 JUnit, pytest 등 test framework의 내부 status vocabulary로 강제하지 않는다. framework의 `SKIPPED`, `ABORTED` 등 원래 결과는 그대로 보존하고, 최종 evidence report에서 필요하면 이유와 함께 요약한다.

**MUST:** 작성한 테스트와 실제 실행한 테스트를 혼동하지 않는다.

## Regression 기준

bug fix에는 가능하면 해당 결함을 재현하는 실패 테스트 또는 동일 수준의 재현 evidence를 확보하고 수정 후 통과를 확인한다.

## Verification vs Validation

### Verification

> 설계·계약대로 만들었는가?

- build
- static analysis
- unit/integration/API test
- security verification
- migration validation

### Validation

> 실제 사용자/업무 요구를 충족했는가?

- acceptance scenario
- 고객/사용자 review
- 대표 데이터로 결과 확인
- 운영 시나리오 확인

둘을 같은 의미로 사용하지 않는다.

## Test Plan 최소 형식

MEDIUM/HIGH risk에서 필요한 범위로 다음을 사용한다.

```text
Scope
Critical risks
Requirements covered
Test levels / techniques
Environment/data
Negative/failure scenarios
Entry/exit criteria
Known gaps
Evidence location
```

LOW risk에는 이 전체 template을 강제하지 않는다.

## 검수 checklist

- [ ] 가장 큰 실패 위험부터 테스트한다.
- [ ] acceptance criteria와 연결된다.
- [ ] 중요한 negative/failure path가 있다.
- [ ] DB/framework가 위험의 핵심인데 전부 mock하지 않았다.
- [ ] 권한/객체 접근 negative test가 필요하면 존재한다.
- [ ] 계획/작성/실행 결과를 구분한다.
- [ ] flaky/skip 테스트를 숨기지 않는다.
- [ ] coverage 수치만으로 완료를 주장하지 않는다.

## 근거

- ISTQB Certified Tester Foundation Level v4.0.1: https://www.istqb.org/certifications/certified-tester-foundation-level-ctfl-v4-0/
- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html
- Google Engineering Practices: https://google.github.io/eng-practices/review/

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #351 terminology finding accepted; PASS/FAIL/NOT RUN/BLOCKED clarified as evidence-report states rather than custom runner states.
