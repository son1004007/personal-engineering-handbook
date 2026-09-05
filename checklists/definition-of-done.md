# Definition of Done

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`

변경이 `작성됨`을 넘어 `검토 가능한 완료 상태`인지 판단하는 기본 checklist다.

## Requirement / Scope

- [ ] 구현이 합의된 scope를 벗어나지 않는다.
- [ ] 중요한 requirement/acceptance가 구현과 연결된다.
- [ ] 남은 assumption/known gap/residual risk가 숨겨져 있지 않다.

## Implementation

- [ ] 코드/설정/스키마 변경이 목적에 필요한 최소 범위다.
- [ ] 권한, transaction, data invariant, external side effect를 필요한 수준에서 검토했다.
- [ ] secret/민감정보가 source/log/output에 노출되지 않는다.
- [ ] 불필요한 abstraction 또는 dead/commented-out code가 없다.

## Verification

- [ ] build/compile 또는 해당 언어의 기본 정적 검증 결과가 기록됐다.
- [ ] 필요한 unit/integration/API/E2E/security negative test가 실행됐다.
- [ ] 계획한 테스트와 실제 실행한 테스트를 구분했다.
- [ ] `PASS / FAIL / NOT RUN / BLOCKED`가 명확하다.
- [ ] bug fix라면 가능한 경우 regression evidence가 있다.

## Review

- [ ] 중간 이상 변경은 독립 correctness review를 받았다.
- [ ] 보안·권한·민감정보 변경은 필요한 수준의 security review를 받았다.
- [ ] reviewer finding의 BLOCKER/MAJOR가 해결되거나 명시적으로 위험 수용됐다.

## Validation

- [ ] 사용자/업무 관점 acceptance scenario가 충족된다.
- [ ] 실제 대표 데이터/환경 확인이 필요한 경우 수행됐다.

## Release / Operation

- [ ] deployment/config/migration 영향이 확인됐다.
- [ ] 위험한 변경은 rollback 또는 forward-fix 전략이 있다.
- [ ] health/log/monitoring/alert 영향이 필요한 수준에서 반영됐다.
- [ ] 배포하지 않았다면 `배포 완료`라고 표현하지 않는다.

## Documentation / Traceability

- [ ] 실제 동작이 달라졌다면 관련 README/ADR/API/운영문서가 함께 갱신됐다.
- [ ] 코드의 중요한 `why` 설명이 구현과 일치한다.
- [ ] 검증 evidence 위치를 다시 찾을 수 있다.

## Completion statement

최종 보고는 예를 들어 다음처럼 구분한다.

```text
implemented: yes
build: PASS
tests: 42 PASS, 0 FAIL
runtime verification: NOT RUN
security review: PASS with 1 MINOR
deployment: NOT RUN
residual risk: external production SSO not verified
```

`완료`라는 한 단어로 검증 수준을 숨기지 않는다.
