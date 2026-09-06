# Definition of Done

- status: `independently-reviewed draft`
- version: `0.8`
- baseline_date: `2026-09-06`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.4.1`
- deliverable_model: [`../lifecycle/03-deliverables-and-handover.md`](../lifecycle/03-deliverables-and-handover.md) `v0.3`

변경이 단순히 `작성됨`을 넘어 현재 risk tier에서 완료 가능한 상태인지 판단한다. 문서/검증 깊이는 risk-based지만 substantive change의 independent review는 생략하지 않는다.

## Requirement / Scope
- [ ] 구현이 합의된 scope를 벗어나지 않는다.
- [ ] **모든 defined in-scope requirement**가 implementation/evidence 또는 approved descoping/blocking decision까지 추적된다.
- [ ] release/acceptance candidate에서 각 in-scope requirement의 상태가 명시되어 있다.
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

`NOT RUN` 또는 verification-`BLOCKED`를 기록하는 것은 허용되지만 **기록 자체가 release 승인을 의미하지 않는다.** 해당 미검증이 acceptance/review severity에 미치는 영향을 별도로 판정한다.

## Mandatory Independent Review
- [ ] `REVIEW_POLICY.md` v1.4.1의 SUBSTANTIVE/NON_SUBSTANTIVE 기준으로 review applicability를 판정했다.
- [ ] personal / AGY-authorized substantive change이면 AGY/Gemini final independent review가 있다.
- [ ] MEDIUM/HIGH design이면 구현 전 independent design review가 있다.
- [ ] raw AI severity를 그대로 release gate로 사용하지 않고 severity calibration을 수행했다.
- [ ] **normal Done/release candidate에는 active calibrated BLOCKER가 0이다.** 문서에 known issue로 기록했다는 이유로 예외를 만들지 않는다.
- [ ] calibrated MAJOR는 해결되었거나 `REVIEW_POLICY.md`가 허용하는 권한자가 policy-compliant risk acceptance를 승인했다.
- [ ] personal project의 MAJOR risk acceptance는 owner decision record가 있고, company/client risk acceptance는 company/project가 지정한 authorized role/process evidence가 있다.
- [ ] reviewer provenance와 arbitration/reconciliation evidence를 다시 찾을 수 있다.
- [ ] company/client에서는 external/personal AGY default-deny와 approved reviewer boundary를 지켰다.

긴급 incident containment는 normal Done 예외가 아니다. 사전/최종 review를 미룰 필요가 있으면 `REVIEW_POLICY.md`의 `REVIEW_DEFERRED_BREAK_GLASS` 절차를 사용하고, review debt가 닫히기 전에는 정상 Done으로 재분류하지 않는다.

## Validation
해당되는 경우:
- [ ] 사용자/업무 acceptance scenario가 충족된다.
- [ ] 대표 data/environment 확인이 수행됐다.

## Deliverables / Traceability
변경된 진실에 해당하는 applicable 산출물만 갱신한다.

- [ ] DLV-01에서 모든 defined in-scope requirement의 implementation/evidence/result 상태를 찾을 수 있다.
- [ ] 각 DLV class가 `APPLICABLE`, `MERGED`, `N/A BY ARCHITECTURE` 중 적절한 상태/근거를 가진다.
- [ ] UI/interaction이 applicable이면 DLV-02가 실제 UI와 일치한다.
- [ ] architecture/API/data-flow/security-boundary가 applicable이면 DLV-03가 일치한다.
- [ ] persistent/transferred data가 applicable이면 DLV-04가 실제 contract와 일치하고 release-impacting data classification `UNKNOWN`이 해결되었거나 해당 requirement/change가 `BLOCKED`/`DESCOPED_APPROVED` 처리됐다.
- [ ] DLV-05 source/config/migration/test code와 문서 계약이 충돌하지 않는다.
- [ ] 설치/build/deploy가 applicable하고 달라졌다면 DLV-06을 갱신했다.
- [ ] 운영/검수/인수가 applicable하고 달라졌다면 DLV-07을 갱신했다.
- [ ] known issue/residual risk/미검증 상태가 숨겨져 있지 않다.

## Release / Operation
해당되는 경우:
- [ ] deployment/config/migration 영향이 확인됐다.
- [ ] HIGH-risk 변경은 rollback/forward-fix/recovery 전략이 있다.
- [ ] health/log/metric/trace/alert 영향 중 필요한 항목이 반영됐다.
- [ ] overdue break-glass review debt가 다음 non-emergency release를 부당하게 통과하지 않는다.
- [ ] LOW/MEDIUM 개인 작업은 필요한 수준으로 commit/tag/build ID에 evidence를 anchor했다.
- [ ] **HIGH-risk 또는 production release는 immutable commit SHA, tag 또는 build/artifact ID에 release/build/test/review evidence가 anchor되어 있다.**
- [ ] 배포하지 않았다면 `배포 완료`라고 표현하지 않는다.

## Handover
해당되는 경우:
- [ ] 설치/배포/운영 command 또는 authoritative procedure reference가 있다.
- [ ] 정상/비정상 상태 판단 기준과 diagnostic entry point가 있다.
- [ ] 운영 owner/code owner/data owner 경계가 필요한 수준으로 명확하다.
- [ ] 다음 작업과 미해결 사항이 남아 있으면 인수자가 찾을 수 있다.
- [ ] active calibrated BLOCKER가 남아 있는 normal handover/release를 허용하지 않는다.

## Completion statement

```text
implemented: yes
build/tests: PASS | FAIL | NOT RUN | BLOCKED
runtime verification: PASS | NOT RUN | BLOCKED
review applicability: SUBSTANTIVE | NON_SUBSTANTIVE
independent design review: PASS | FINDINGS | NOT_APPLICABLE | DEFERRED_BREAK_GLASS
independent final review: PASS | FINDINGS | BLOCKED | DEFERRED_BREAK_GLASS
severity calibration: complete
arbitration/reconciliation: complete
calibrated blocker: 0
calibrated major: 0 | RISK_ACCEPTED
risk acceptance authority: owner | <company-authorized role> | NOT_APPLICABLE
requirements terminal-status check: PASS
data classification release check: PASS | N/A
deliverables: DLV-01=APPLICABLE, DLV-02=N/A BY ARCHITECTURE, ...
release artifact ref: <commit/tag/build-id>
human/project approval: PASS | NOT_REQUIRED | PENDING
release/deployment: PASS | NOT RUN
residual risk: ...
```

Test PASS alone is not Done.
