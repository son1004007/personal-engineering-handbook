# Operating Model

- status: `independently-reviewed draft`
- version: `0.5`
- baseline_date: `2026-09-06`
- review_inputs: `AGY/Gemini issues #350, #352, #353`
- mandatory_review_policy: [`REVIEW_POLICY.md`](REVIEW_POLICY.md) `approved v1.1`

이 문서는 handbook 규칙을 실제 작업에 적용할 때 필요한 공통 해석 기준을 정의한다. **문서·산출물의 깊이는 위험에 비례해 조절하지만, substantive engineering change의 독립 review 자체는 `REVIEW_POLICY.md`에 따라 필수**다.

## 1. Normative keywords

대문자로 쓰인 `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, `MAY`는 BCP 14(RFC 2119 + RFC 8174)의 취지를 적용한다.

- `MUST / MUST NOT`: 안전성·정합성·계약·재현성을 위해 기본적으로 위반하면 안 되는 요구
- `SHOULD / SHOULD NOT`: 일반적으로 따르되, 합리적인 예외가 있으면 영향과 이유를 이해하고 달리할 수 있음
- `MAY`: 상황에 따라 선택 가능

소문자 `must/should/may`는 위 규범 의미를 갖지 않는다.

Official references:
- https://www.rfc-editor.org/info/rfc2119/
- https://www.rfc-editor.org/info/rfc8174/

## 2. Operating modes

### Solo / AI-pair mode

개인 프로젝트, 독립 작업 또는 본인이 최종 책임자인 작업.

- 작성자 본인이 최종 승인할 수 있다.
- **Substantive engineering change는 LOW/MEDIUM/HIGH와 무관하게 최종 AGY/Gemini independent review를 MUST 수행한다.**
- MEDIUM/HIGH의 architecture/security/data/operation 결정은 구현 전에 AGY/Gemini design review를 MUST 수행한다.
- AGY/Gemini finding은 자동 정답이 아니며 evidence-based reconciliation을 수행한다.
- AGY BLOCKER/MAJOR의 기각·하향에는 `REVIEW_POLICY.md`의 객관적 arbitration 규칙을 따른다.
- tool/runtime failure로 review가 끝나지 않으면 정상 경로에서는 `Done`으로 표시하지 않는다. 단, owner가 별도 independent reviewer를 명시적으로 승인한 경우 substitute evidence를 남길 수 있다.

### Team / organization mode

회사·고객·팀 프로젝트.

- 프로젝트의 실제 review/approval policy가 이 handbook보다 우선한다.
- separation of duties, reviewer 지정, change approval이 요구되면 그대로 따른다.
- **외부/개인 AGY는 DEFAULT DENY**다. 회사·고객이 해당 service/endpoint, repository와 data classification을 명시적으로 허용한 경우에만 사용한다.
- 명시적 허용이 없으면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`로 기록하고 회사가 승인한 human peer / tech lead / security reviewer / internal AI로 독립 review를 수행한다.
- 개인 Synology AGY bridge가 회사 프로젝트 데이터에 대해 승인되었다고 가정하지 않는다.
- 회사 프로젝트에서 AGY BLOCKER/MAJOR를 기각·하향하려면 객관적 counter-evidence와 human peer/lead/security owner의 명시적 동의가 필요하다.
- 이 handbook을 별도 shadow governance로 만들지 않고 기존 ticket/PR/CI/security/change-management 절차에 mapping한다.
- 개인 handbook을 근거로 조직의 승인 절차를 축소하지 않는다.

## 3. Risk tiers

위험등급은 **review 실행 여부가 아니라 review·문서·검증의 깊이**를 결정한다.

### LOW — routine / reversible

예:
- 작은 read-only behavior 변경
- 국소 refactoring
- 영향이 제한적이고 즉시 되돌릴 수 있는 수정

기본 evidence:
- 목적/변경 내용
- 필요한 기본 검증
- 개인/AGY-authorized 환경: final AGY/Gemini review
- 회사 환경: 회사 승인 독립 reviewer 또는 명시적으로 승인된 AGY/Gemini review

실행 동작·업무 의미·검증 기준을 전혀 바꾸지 않는 오탈자/format/link-only 변경은 `REVIEW_POLICY.md`에 따라 non-substantive로 review를 생략할 수 있다.

### MEDIUM — meaningful behavior change

다음 중 하나가 있으나 blast radius가 제한적이고 복구 가능함.

- 상태 변경
- API contract 변경
- DB query/schema 영향
- 인증된 사용자 흐름 변경
- 외부 dependency 연동
- 운영 configuration 영향

기본 evidence:
- Problem / Scope
- Auth/Data/Operation impact
- 변경 결정
- Verification evidence
- 필요한 rollback note
- 사전 independent design review
- 최종 independent diff/change review

개인/AGY-authorized 환경에서는 AGY/Gemini를 사용한다. 회사 환경에서는 승인된 reviewer 경계를 따른다.

`SRC-###`, `FR/SEC/DATA/...` ID는 traceability가 실제 가치를 줄 때만 사용한다. MEDIUM이라는 이유만으로 모든 ID를 강제하지 않는다.

### HIGH — architectural / security / irreversible / wide blast radius

예:
- destructive migration
- 인증/인가 모델 변경
- public exposure / network security 변경
- credential/key ownership 변경
- 대규모 데이터 backfill
- 여러 서비스에 걸친 contract 변경
- 되돌리기 어려운 architecture 선택
- production privilege 확대

기본 evidence:
- source/requirement traceability
- architecture/security/data decision
- threat/risk review
- migration/rollback 또는 forward-fix 전략
- 사전 independent design/security review
- 최종 independent change review
- 프로젝트가 요구하는 human/SoD review
- acceptance/release evidence

개인/AGY-authorized 환경에서는 AGY/Gemini를 mandatory reviewer로 사용한다. 회사 환경에서는 승인된 reviewer와 공식 risk-acceptance 절차를 우선한다.

## 4. `Important`의 기본 판정

문서에서 `important requirement/interface/decision`이라 할 때 다음 중 하나 이상이면 기본적으로 중요하다고 본다.

- trust/security boundary를 넘는다.
- 인증/인가 또는 개인정보/민감정보에 영향을 준다.
- persistent data integrity/ownership/retention에 영향을 준다.
- state mutation 또는 irreversible side effect가 있다.
- public/external API contract를 바꾼다.
- asynchronous/distributed coordination이 있다.
- 장애 blast radius가 둘 이상의 주요 component/user group으로 확대될 수 있다.

프로젝트 위험이 더 낮거나 높다면 조정한다.

## 5. Inference vs. fabrication

### 허용 가능한 inference

AI/engineer가 다음을 **근거와 함께 `INFERRED`로 표시**하여 진행할 수 있다.

- repository 코드/config/test에서 직접 도출되는 현재 동작
- 사용 중인 framework/vendor의 현재 공식 문서로 확인되는 기술 동작
- 기존 API/schema/contract와 일관된 되돌릴 수 있는 기술적 기본값
- 안전한 실패를 위한 low-risk defensive behavior로, 외부 business semantics를 새로 만들지 않는 것

### confirmation이 필요한 영역

다음은 기술적으로 그럴듯해도 AI가 새 요구사항으로 확정하지 않는다.

- business policy / 허용 상태 전이의 의미
- role entitlement / object access policy
- 법적·계약상 retention/deletion 기간
- SLA/SLO/latency/availability 수치
- 가격/비용/과금 정책
- encryption/key ownership
- destructive migration semantics
- 고객/회사 승인 절차

이 영역이 핵심 blocker라면 `UNKNOWN` 또는 `CONFLICT`로 남긴다.

## 6. Verification / Review / Validation 경계

- **VERIFY:** 기술 specification/contract에 맞게 구현됐는지 테스트·정적 분석·runtime probe 등으로 확인. Evidence: 실행 결과.
- **REVIEW:** implementation과 독립된 관점에서 correctness, simplicity, security, data/operation risk, unintended effects를 검사. Evidence: findings + arbitration/reconciliation record.
- **VALIDATE:** 사용자·업무 목적과 acceptance criteria를 실제 시나리오에서 충족하는지 확인. Evidence: acceptance result.

순서는 작업 특성에 따라 반복될 수 있으며 waterfall gate로 해석하지 않는다.

## 7. Break-glass / Fast-track

운영 장애, active security incident, 데이터 손상 확대 등 즉시 containment가 더 중요한 경우 일반 DoR와 사전 review를 축약할 수 있다.

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> MANDATORY INDEPENDENT REVIEW
-> RETRO / RECONCILE
```

### MUST

- 변경 목적과 긴급 사유를 기록한다.
- blast radius를 줄이는 최소 변경을 우선한다.
- 가능한 bounded verification을 수행한다.
- rollback/disable 방법을 가능한 경우 확보한다.
- 사전 review를 생략했다면 `REVIEW_DEFERRED_BREAK_GLASS`로 명시한다.
- `review_owner`와 `review_due`를 기록한다.
- 프로젝트 incident/change policy가 deadline을 정의하면 그것을 따른다.
- 별도 정책이 없으면 open-ended defer를 허용하지 않고 concrete due point를 정한다.
- 늦어도 incident/problem record 종료 또는 같은 영역의 다음 non-emergency production change 전에는 review/reconciliation을 끝낸다.
- deferred review에서 production BLOCKER가 확인되면 exposure를 즉시 재평가하고 프로젝트 정책에 따라 containment/rollback/disable/forward-fix를 수행하며 human/security owner에게 escalation한다.

전역적으로 임의의 24시간/48시간 같은 숫자를 강제하지 않는다.

## 8. Overengineering 방지

다음은 명시적으로 금지되는 오해다.

- 모든 변경에 ADR 작성
- 모든 요구사항에 ID 부여
- 모든 quality characteristic 측정
- 모든 테스트 레벨 생성
- 모든 endpoint에 별도 계층/interface/factory 생성
- HIGH-risk라는 이유만으로 위험과 무관한 모든 검증 도구를 실행
- AGY가 제안했다는 이유만으로 모든 finding을 구현
- 회사 프로젝트에 개인 11단계 workflow와 개인 AGY sign-off를 별도 조직 gate처럼 강제

**리뷰는 필수지만 산출물과 수정은 위험·근거에 비례하며, 회사에서는 기존 governance에 mapping한다.**

## Review record

- 2026-09-06: ChatGPT initial operating model.
- 2026-09-06: AGY/Gemini #350 findings incorporated selectively: solo/team mode, risk-tier simplification, inference boundary, break-glass path, BCP14 vocabulary, VERIFY/REVIEW/VALIDATE separation.
- 2026-09-06: Fixed 24–48 hour incident backfill suggestion rejected because it lacks a universal policy basis.
- 2026-09-06: AGY/Gemini #352 verdict `READY_FOR_REVIEWED`, no BLOCKER for the prior risk-based baseline.
- 2026-09-06: Owner restored mandatory independent-review intent.
- 2026-09-06: AGY/Gemini #353 governance review rejected review-policy v1.0. Accepted/modified self-arbitration, default-deny company boundary, tool-outage fallback, break-glass remediation and shadow-governance findings into REVIEW_POLICY v1.1.
