# Definition of Done

- status: `draft`
- version: `0.3`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.0`

변경이 `작성됨`을 넘어 **현재 risk tier에서 검토 가능한 완료 상태**인지 판단하는 기본 checklist다. 문서와 검증 깊이는 LOW/MEDIUM/HIGH에 따라 조절하지만, substantive engineering change의 independent review는 생략하지 않는다.

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

## Mandatory Independent Review

### Substantive change

- [ ] `REVIEW_POLICY.md` 기준으로 review 대상인지 판정했다.
- [ ] 대상이면 exact final diff/commit/source 또는 정확한 draft를 AGY/Gemini에 independent context로 제공했다.
- [ ] MEDIUM/HIGH의 중요 설계라면 구현 전 AGY/Gemini design review evidence가 있다.
- [ ] 최종 변경에 대한 AGY/Gemini review result가 있다.
- [ ] 모든 finding을 `ACCEPTED / MODIFIED / REJECTED / DEFERRED`로 reconciliation했다.
- [ ] `REJECTED/MODIFIED`에는 공식 문서, test/runtime, 프로젝트 요구 등 근거가 있다.
- [ ] confirmed BLOCKER가 없다.
- [ ] unresolved confirmed MAJOR는 해결되었거나 상위 policy가 허용하는 explicit risk acceptance가 있다.

### Company/client policy exception

AGY/Gemini에 source/설계를 제공하는 것이 상위 정책상 금지된 경우에만:

- [ ] `AGY_NOT_PERMITTED_BY_POLICY`를 명시했다.
- [ ] 금지 데이터를 외부/개인 AGY 환경으로 반출하지 않았다.
- [ ] 회사/프로젝트가 허용한 independent reviewer evidence가 있다.
- [ ] 이 경우 `AGY reviewed`라고 허위 표시하지 않는다.

### Runtime/tool failure

AGY 사용이 허용된 환경에서 단순 bridge/runtime failure로 review를 못 한 경우:

- [ ] `review: BLOCKED`로 표시한다.

**정상 경로에서는 이 상태로 Done 처리하지 않는다.**

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
- [ ] 중요한 verification/review evidence 위치를 다시 찾을 수 있다.

## Completion statement

최종 보고는 검증 범위를 구분한다.

```text
implemented: yes
build: PASS
tests: PASS
runtime verification: PASS | NOT RUN | BLOCKED
agy/gemini design review: PASS | FINDINGS | NOT_APPLICABLE | AGY_NOT_PERMITTED_BY_POLICY
agy/gemini final review: PASS | FINDINGS | BLOCKED | AGY_NOT_PERMITTED_BY_POLICY
review reconciliation: complete
confirmed blocker: 0
confirmed major: 0
human/project approval: PASS | NOT_REQUIRED | PENDING
release/deployment: PASS | NOT RUN
residual risk: ...
```

`완료`라는 한 단어로 verification/review/validation/deployment 수준을 숨기지 않는다.

## Break-glass

긴급 hotfix에서는 사전 review를 축약할 수 있다. 단:

- bounded verification을 수행한다.
- `REVIEW_DEFERRED_BREAK_GLASS`를 기록한다.
- release/containment 후 AGY/Gemini 또는 상위 policy가 허용한 independent review를 반드시 수행한다.
- finding reconciliation과 residual-risk 기록을 완료한다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: Revised after AGY/Gemini #350 for risk-based documentation depth.
- 2026-09-06: Owner decision restored mandatory AGY/Gemini independent review as a Done gate for substantive engineering work.
