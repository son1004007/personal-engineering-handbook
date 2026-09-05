# Definition of Done

- status: `draft`
- version: `0.4`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.1`

변경이 `작성됨`을 넘어 현재 risk tier에서 검토 가능한 완료 상태인지 판단한다. 문서/검증 깊이는 risk-based지만 substantive change의 independent review는 생략하지 않는다.

## Requirement / Scope

- [ ] 구현이 합의된 scope를 벗어나지 않는다.
- [ ] 중요한 requirement/acceptance가 구현과 연결된다.
- [ ] assumption/known gap/residual risk가 숨겨져 있지 않다.

## Implementation

- [ ] 코드/설정/스키마 변경이 목적에 필요한 범위다.
- [ ] authorization, transaction, data invariant, external side effect를 검토했다.
- [ ] secret/credential/민감정보가 부적절하게 노출되지 않는다.
- [ ] 불필요한 abstraction/dead code가 없다.

## Verification

- [ ] build/compile/static verification 결과가 기록됐다.
- [ ] risk에 필요한 test 또는 다른 verification이 실행됐다.
- [ ] 계획/작성한 test와 실제 실행한 test를 구분했다.
- [ ] `PASS / FAIL / NOT RUN / BLOCKED`가 명확하다.
- [ ] bug fix라면 가능한 regression evidence가 있다.

## Mandatory Independent Review

### Personal / AGY-authorized environment

- [ ] substantive change이면 AGY/Gemini final independent review가 있다.
- [ ] MEDIUM/HIGH design이면 구현 전 AGY/Gemini design review가 있다.

### Company/client

- [ ] 외부/개인 AGY 사용은 default-deny로 판단했다.
- [ ] 명시적 approval이 있는 경우에만 승인된 AGY/Gemini boundary를 사용했다.
- [ ] approval이 없거나 불명확하면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`를 기록했다.
- [ ] company-approved human/internal-AI independent review evidence가 있다.
- [ ] 개인 Synology/public GitHub/미승인 외부 AI로 회사·고객 정보를 반출하지 않았다.

## Finding Arbitration / Reconciliation

- [ ] 모든 finding이 `ACCEPTED / MODIFIED / REJECTED / DEFERRED`로 정리됐다.
- [ ] AGY BLOCKER/MAJOR는 arbitration 전 `PENDING_BLOCKER/PENDING_MAJOR`로 blocking 처리됐다.
- [ ] personal에서 BLOCKER/MAJOR 기각·하향은 objective evidence + second independent reviewer + owner disposition이 있다.
- [ ] company에서 BLOCKER/MAJOR 기각·하향은 objective evidence + human peer/lead/security/designated reviewer concurrence가 있다.
- [ ] pending/confirmed BLOCKER가 없다.
- [ ] unresolved MAJOR는 해결되었거나 상위 policy가 허용한 risk acceptance가 있다.

## Tool failure

- 개인 프로젝트에서 AGY가 허용됐지만 runtime failure면 `review: BLOCKED`이며 정상 Done 처리하지 않는다. 명시적으로 승인된 substitute reviewer가 있으면 evidence를 남긴다.
- 회사 프로젝트에서 개인/승인 AGY tool failure는 delivery SPOF로 만들지 않는다. `AGY_RUNTIME_UNAVAILABLE`를 기록하고 회사의 기존 mandatory review gate가 완료되면 독립-review 의무를 충족할 수 있다.

## Validation

해당되는 경우:
- [ ] 사용자/업무 acceptance scenario가 충족된다.
- [ ] 대표 data/environment 확인이 수행됐다.

## Release / Operation

해당되는 경우:
- [ ] deployment/config/migration 영향이 확인됐다.
- [ ] HIGH-risk 변경은 rollback/forward-fix/recovery 전략이 있다.
- [ ] health/log/metric/trace/alert 영향 중 필요한 항목이 반영됐다.
- [ ] dependency/support/deprecation 영향이 기록됐다.
- [ ] 배포하지 않았다면 `배포 완료`라고 표현하지 않는다.

## Documentation / Traceability

해당되는 경우:
- [ ] 실제 동작이 달라졌다면 관련 README/ADR/API/운영문서가 갱신됐다.
- [ ] 중요한 `why` 설명이 구현과 일치한다.
- [ ] verification/review/arbitration evidence 위치를 다시 찾을 수 있다.

## Completion statement

```text
implemented: yes
build/tests: PASS
runtime verification: PASS | NOT RUN | BLOCKED
independent design review: PASS | FINDINGS | NOT_APPLICABLE
independent final review: PASS | FINDINGS | BLOCKED
reviewer: AGY/Gemini | company-approved reviewer
agy authorization: AUTHORIZED | NOT_AUTHORIZED_FOR_PROJECT_DATA | NOT_APPLICABLE
arbitration/reconciliation: complete
pending blocker: 0
pending major: 0
human/project approval: PASS | NOT_REQUIRED | PENDING
release/deployment: PASS | NOT RUN
residual risk: ...
```

## Break-glass

긴급 hotfix에서는 사전 review를 defer할 수 있지만:

- `REVIEW_DEFERRED_BREAK_GLASS` 기록
- `review_owner` 지정
- `review_due`를 project policy 또는 concrete deadline으로 지정
- 늦어도 incident/problem closure 또는 동일 영역 다음 non-emergency production change 전에 independent review/reconciliation 완료
- post-release BLOCKER 발견 시 즉시 exposure 재평가, containment/rollback/disable/forward-fix 및 escalation

을 수행한다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: Risk-based review depth incorporated after AGY/Gemini #350.
- 2026-09-06: Mandatory independent-review gate added by owner decision.
- 2026-09-06: AGY/Gemini #353 governance findings incorporated through REVIEW_POLICY v1.1.
