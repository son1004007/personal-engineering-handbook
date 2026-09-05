# Definition of Done

- status: `draft`
- version: `0.5`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.2`

변경이 `작성됨`을 넘어 현재 risk tier에서 완료 가능한 상태인지 판단한다. 문서/검증 깊이는 risk-based지만 substantive change의 independent review는 생략하지 않는다.

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

## Review applicability

- [ ] `REVIEW_POLICY.md`의 SUBSTANTIVE/NON_SUBSTANTIVE 기준으로 판정했다.
- [ ] 회색 영역은 substantive로 처리하거나 제외 근거를 기록했다.

## Mandatory Independent Review

### Personal / explicitly AGY-authorized

- [ ] substantive change이면 AGY/Gemini final independent review가 있다.
- [ ] MEDIUM/HIGH design이면 구현 전 AGY/Gemini design review가 있다.

### Company/client

- [ ] 외부/개인 AGY 사용은 DEFAULT DENY로 판단했다.
- [ ] 명시적 authorization이 있는 경우에만 승인된 AGY boundary를 사용했다.
- [ ] authorization이 없거나 불명확하면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`를 기록했다.
- [ ] company-approved human/internal-AI independent review evidence가 있다.
- [ ] 개인 Synology/public GitHub/미승인 외부 AI로 company/client context를 반출하지 않았다.

## Finding Arbitration / Reconciliation

- [ ] 모든 finding이 `ACCEPTED / MODIFIED / REJECTED / DEFERRED`로 정리됐다.
- [ ] AI `BLOCKER/MAJOR`는 arbitration 전 `PENDING_BLOCKER/PENDING_MAJOR`로 blocking 처리됐다.

### Directly falsifiable finding

- [ ] 기각/하향했다면 deterministic reproduction/test, exact-version official docs, direct runtime evidence 또는 explicit contract가 finding 핵심을 직접 반증한다.
- [ ] evidence 위치와 재현 방법을 기록했다.

이 조건이면 personal project에서 second reviewer는 필수가 아니다.

### Interpretive / risk / design finding

- [ ] personal: counter-evidence + eligible second independent semantic reviewer + owner decision record가 있다.
- [ ] company: counter-evidence + human peer/lead/security/designated reviewer concurrence + project policy가 있다.

- [ ] pending/confirmed BLOCKER가 없다.
- [ ] unresolved MAJOR는 해결되었거나 상위 policy가 허용한 risk acceptance가 있다.

## Substitute reviewer

AGY unavailable 시 substitute를 사용했다면:

- [ ] implementation과 분리됐거나 fresh context다.
- [ ] review source/evidence를 직접 볼 수 있다.
- [ ] correctness/security/data/operation semantic review가 가능하다.
- [ ] reviewer identity/capability/result가 기록됐다.

linter/test runner/scanner 자체는 reviewer가 아니라 evidence다.

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

- [ ] 동작이 달라졌다면 관련 README/ADR/API/운영문서가 필요한 수준으로 갱신됐다.
- [ ] 중요한 `why` 설명이 구현과 일치한다.
- [ ] verification/review/arbitration evidence 위치를 다시 찾을 수 있다.

## Completion statement

```text
implemented: yes
build/tests: PASS
runtime verification: PASS | NOT RUN | BLOCKED
review applicability: SUBSTANTIVE | NON_SUBSTANTIVE
independent design review: PASS | FINDINGS | NOT_APPLICABLE | DEFERRED_BREAK_GLASS
independent final review: PASS | FINDINGS | BLOCKED | DEFERRED_BREAK_GLASS
reviewer: AGY/Gemini | substitute | company-approved reviewer
agy authorization: AUTHORIZED | NOT_AUTHORIZED_FOR_PROJECT_DATA | NOT_APPLICABLE
arbitration/reconciliation: complete
pending blocker: 0
pending major: 0
human/project approval: PASS | NOT_REQUIRED | PENDING
release/deployment: PASS | NOT RUN
residual risk: ...
```

## Break-glass

긴급 containment에서는 **pre-design review와 final review 모두** 지연이 위험하면 defer할 수 있다.

- `REVIEW_DEFERRED_BREAK_GLASS`
- `review_scope: DESIGN | FINAL | BOTH`
- `review_owner`
- `review_due`
- incident/problem closure 또는 동일 영역 다음 non-emergency production change 전에 independent review/reconciliation
- post-release BLOCKER 발견 시 exposure 재평가 + containment/rollback/disable/forward-fix + escalation

을 수행한다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: Risk-based review depth incorporated after AGY/Gemini #350.
- 2026-09-06: Mandatory independent-review gate added by owner decision.
- 2026-09-06: AGY/Gemini #353/#354 governance findings selectively incorporated through REVIEW_POLICY v1.2.
