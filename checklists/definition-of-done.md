# Definition of Done

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

변경이 `작성됨`을 넘어 **현재 risk tier에서 검토 가능한 완료 상태**인지 판단하는 기본 checklist다. LOW/MEDIUM/HIGH에 따라 필요한 깊이만 적용한다.

## Requirement / Scope

- [ ] 구현이 합의된 scope를 벗어나지 않는다.
- [ ] 중요한 requirement/acceptance가 구현과 연결된다.
- [ ] 남은 assumption/known gap/residual risk가 숨겨져 있지 않다.

## Implementation

- [ ] 코드/설정/스키마 변경이 목적에 필요한 범위다.
- [ ] authorization, transaction, data invariant, external side effect를 해당되는 수준에서 검토했다.
- [ ] secret/credential이 source/log/output에 노출되지 않는다.
- [ ] 개인정보/민감정보는 프로젝트 정책에 맞게 최소화·보호된다.
- [ ] 불필요한 abstraction 또는 dead/commented-out code가 없다.

## Verification

- [ ] build/compile 또는 해당 언어의 기본 정적 검증 결과가 기록됐다.
- [ ] risk에 필요한 unit/integration/API/E2E/security negative test 또는 다른 verification이 실행됐다.
- [ ] 계획/작성한 테스트와 실제 실행한 테스트를 구분했다.
- [ ] evidence summary의 `PASS / FAIL / NOT RUN / BLOCKED`가 명확하다.
- [ ] bug fix라면 가능한 경우 regression evidence가 있다.

## Review

- [ ] [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 Solo/Team 및 LOW/MEDIUM/HIGH 기준에 맞는 review를 수행했다.
- [ ] HIGH risk는 가능한 경우 implementation context와 분리된 independent review를 받았다.
- [ ] 보안·권한·민감정보 변경은 위험에 맞는 security review를 받았다.
- [ ] 알려진 BLOCKER가 없다.
- [ ] unresolved MAJOR는 해결되었거나 project owner가 residual risk를 명시적으로 수용했다.

Solo/AI-pair mode에서 human reviewer가 없다는 이유만으로 LOW/MEDIUM 변경을 영구 차단하지 않는다. 대신 deterministic verification, fresh-eyes self-review 또는 독립 AI review를 위험에 맞게 조합한다.

## Validation

해당되는 경우:
- [ ] 사용자/업무 관점 acceptance scenario가 충족된다.
- [ ] 대표 data/environment 확인이 필요한 경우 수행됐다.

## Release / Operation

해당되는 경우:
- [ ] deployment/config/migration 영향이 확인됐다.
- [ ] HIGH-risk 변경은 rollback 또는 현실적인 forward-fix/recovery 전략이 있다.
- [ ] health/log/metric/trace/alert 영향 중 필요한 항목이 반영됐다.
- [ ] dependency/support/deprecation 영향이 있으면 기록됐다.
- [ ] 배포하지 않았다면 `배포 완료`라고 표현하지 않는다.

## Documentation / Traceability

해당되는 경우:
- [ ] 실제 동작이 달라졌다면 관련 README/ADR/API/운영문서가 함께 갱신됐다.
- [ ] 중요한 `why` 설명이 구현과 일치한다.
- [ ] 중요한 verification evidence 위치를 다시 찾을 수 있다.

## Completion statement

최종 보고는 검증 범위를 구분한다.

```text
implemented: yes
build: PASS
tests: PASS (summary; framework raw result preserved separately)
runtime verification: NOT RUN
security review: PASS with 1 MINOR
deployment: NOT RUN
residual risk: external production SSO not verified
```

`완료`라는 한 단어로 verification/review/validation/deployment 수준을 숨기지 않는다.

## Break-glass

긴급 hotfix에서는 일반 DoD의 일부를 축약할 수 있다. 단, 최소 bounded verification, release 관찰, residual risk 기록을 유지하고 정상화 후 생략된 test/doc/review를 합리적인 시점에 reconciliation한다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: Revised after AGY/Gemini #350 to remove mandatory medium-change external-review deadlock and align with risk-based Solo/Team operating modes.
