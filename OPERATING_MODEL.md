# Operating Model

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- review_input: `AGY/Gemini issue #350`

이 문서는 handbook 규칙을 실제 작업에 적용할 때 필요한 공통 해석 기준을 정의한다. 개인 handbook이 불필요한 절차 장벽이 되지 않도록 **운영 모드, 위험 등급, 규범 키워드, inference 경계와 긴급 변경 경로**를 명시한다.

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
- 중간 위험 변경은 가능하면 **독립 AI review, fresh-eyes self-review, deterministic verification** 중 하나 이상을 사용한다.
- high-risk 변경은 가능한 경우 독립 reviewer를 추가하고, 사람 reviewer가 없으면 그 사실과 residual risk를 기록한다.
- 독립 reviewer 부재만으로 low/medium-risk 개인 작업을 영구 차단하지 않는다.

### Team / organization mode

회사·고객·팀 프로젝트.

- 프로젝트의 실제 review/approval policy가 이 handbook보다 우선한다.
- separation of duties, reviewer 지정, change approval이 요구되면 그대로 따른다.
- 개인 handbook을 근거로 조직의 승인 절차를 축소하지 않는다.

## 3. Risk tiers

문서와 review 깊이는 `코드 줄 수`보다 실패 영향으로 결정한다.

### LOW — routine / reversible

예:
- 오탈자/문서
- 작은 read-only 변경
- 국소 refactoring
- 영향이 제한적이고 즉시 되돌릴 수 있는 수정

기본 evidence:
- 목적/변경 내용
- 필요한 기본 검증

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

`SRC-###`, `FR/SEC/DATA/...` ID는 traceability가 실제 가치를 줄 때만 사용한다. medium이라는 이유만으로 모든 ID를 강제하지 않는다.

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
- 독립 review
- acceptance/release evidence

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
- **REVIEW:** 사람이 읽는 의미 수준 또는 독립 AI가 correctness, simplicity, security, unintended effects를 검사. Evidence: findings/approval record.
- **VALIDATE:** 사용자·업무 목적과 acceptance criteria를 실제 시나리오에서 충족하는지 확인. Evidence: acceptance result.

순서는 작업 특성에 따라 반복될 수 있으며 waterfall gate로 해석하지 않는다.

## 7. Break-glass / Fast-track

운영 장애, active security incident, 데이터 손상 확대 등 즉시 containment가 더 중요한 경우 일반 DoR를 축약할 수 있다.

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> RETRO / RECONCILE
```

### MUST

- 변경 목적과 긴급 사유를 기록한다.
- blast radius를 줄이는 최소 변경을 우선한다.
- 가능한 bounded verification을 수행한다.
- rollback/disable 방법을 가능한 경우 확보한다.
- 정상화 후 생략된 requirement/test/doc/review를 **프로젝트가 정한 합리적인 시점에** backfill하고 재발 방지 여부를 판단한다.

임의의 24시간/48시간 같은 고정 시간을 이 handbook이 전역 규칙으로 강제하지 않는다.

## 8. Overengineering 방지

다음은 명시적으로 금지되는 오해다.

- 모든 변경에 ADR 작성
- 모든 요구사항에 ID 부여
- 모든 quality characteristic 측정
- 모든 테스트 레벨 생성
- 모든 개인 작업에 외부 human reviewer 대기
- 모든 endpoint에 별도 계층/interface/factory 생성

handbook의 목적은 **판단 품질과 증거 품질을 높이는 것**이지 산출물 개수를 늘리는 것이 아니다.

## Review record

- 2026-09-06: ChatGPT initial operating model.
- 2026-09-06: AGY/Gemini #350 findings incorporated selectively: solo/team mode, risk-tier simplification, inference boundary, break-glass path, BCP14 vocabulary, VERIFY/REVIEW/VALIDATE separation.
- Rejected/modified AGY suggestion: fixed 24–48 hour incident backfill deadline was not adopted because it lacks a universal policy basis.
