# Operating Model

- status: `independently-reviewed draft`
- version: `0.6`
- baseline_date: `2026-09-06`
- review_inputs: `AGY/Gemini issues #350, #352, #353, #354`
- review_policy: [`REVIEW_POLICY.md`](REVIEW_POLICY.md) `approved v1.5`

이 문서는 handbook 규칙을 실제 작업에 적용할 때 필요한 공통 해석 기준을 정의한다. **문서와 필수 검증 깊이는 위험에 비례하며 별도 독립 AI 품질 검토는 `REVIEW_POLICY.md` v1.5에 따라 선택 사항**이다. 실제 회사/계약/프로젝트상 필수 검토는 유지한다.

Review applicability, AGY authorization, finding arbitration, substitute reviewer, break-glass review deferral의 상세 규칙은 `REVIEW_POLICY.md`를 단일 Source of Truth로 사용한다. 이 문서에서 중복 정의하지 않는다.

## 1. Normative keywords

대문자 `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, `MAY`는 BCP 14(RFC 2119 + RFC 8174)의 취지를 적용한다.

- `MUST / MUST NOT`: 기본적으로 위반하면 안 되는 요구
- `SHOULD / SHOULD NOT`: 합리적 예외가 있으면 영향/이유를 이해하고 달리할 수 있음
- `MAY`: 상황별 선택

## 2. Operating modes

### Solo / AI-pair

- owner가 최종 책임을 진다.
- substantive change는 자체 검토 및 기능·보안 검증 MUST; 독립 AI final review MAY.
- MEDIUM/HIGH design은 위험 기반 분석 MUST; 독립 AI pre-design review MAY.
- AI finding은 자동 정답이 아니며 v1.2 evidence/arbitration rule을 따른다.

### Team / organization

- 회사/고객/project policy가 handbook보다 우선한다.
- 실제 회사/계약상 요구된 검토는 mandatory; 개인 독립 AI review는 선택이며 외부/개인 AGY는 DEFAULT DENY다.
- 실제 검토 의무가 있으면 company-approved human/internal-AI reviewer를 사용한다.
- handbook을 별도 shadow governance로 만들지 않고 기존 ticket/PR/CI/security/change-management에 mapping한다.

## 3. Risk tiers

위험등급은 **문서·자체 검토·verification 깊이와 선택적 AI review 필요성**을 결정한다.

### LOW

routine/reversible/localized change.

기본:
- purpose/scope
- 필요한 verification
- substantive라면 자체 검토, 독립 AI review 선택

### MEDIUM

meaningful behavior change이나 blast radius가 제한적이고 복구 가능.

예:
- 상태 변경
- API contract
- DB query/schema 영향
- authenticated user flow
- external dependency
- runtime config

기본:
- Problem / Scope
- Auth/Data/Operation impact
- decision
- verification evidence
- 필요한 rollback note
- 위험 기반 자체 설계/최종 검토, 독립 AI review 선택

### HIGH

architectural/security/irreversible/wide-blast-radius change.

예:
- destructive migration
- authn/authz model
- public exposure/network security
- credential/key ownership
- large data backfill
- multi-service contract
- hard-to-reverse architecture
- production privilege expansion

기본:
- source/requirement traceability
- architecture/security/data decision
- threat/risk analysis
- migration/recovery strategy
- 위험 기반 설계·보안·최종 자체 검토, 독립 AI review 선택
- project-required human/SoD approval
- acceptance/release evidence

## 4. `Important` 기본 판정

다음 중 하나 이상이면 중요 대상으로 본다.

- trust/security boundary
- authn/authz 또는 sensitive data
- persistent data integrity/ownership/retention
- state mutation 또는 irreversible side effect
- public/external contract
- async/distributed coordination
- wide blast radius

## 5. Inference vs fabrication

### `INFERRED`로 진행 가능한 것

- repo/config/test에서 직접 도출되는 current behavior
- current official framework/vendor docs로 확인된 technical behavior
- existing contract와 일관된 reversible technical default
- business semantics를 새로 만들지 않는 low-risk defensive behavior

### confirmation이 필요한 것

- business policy/state meaning
- role entitlement/object access policy
- legal/contractual retention
- SLA/SLO 수치
- pricing/billing
- key ownership
- destructive migration semantics
- company/client approval procedure

핵심 blocker면 `UNKNOWN/CONFLICT`로 유지한다.

## 6. Verification / Review / Validation

- **VERIFY:** technical contract/spec을 test/static/runtime evidence로 확인
- **REVIEW:** 독립 관점에서 correctness/security/data/operation/unintended effect를 검사
- **VALIDATE:** user/business acceptance를 확인

서로 대체하지 않으며 필요하면 반복한다.

## 7. Break-glass

active incident/data-loss/security containment에서는 **pre-design review와 final review 모두 지연이 더 위험하면 명시적으로 defer 가능**하다.

```text
TRIAGE
-> CONTAIN / MINIMAL HOTFIX
-> BOUNDED VERIFY
-> EXPEDITED RELEASE
-> MONITOR
-> DEFERRED DESIGN/FINAL REVIEW
-> RETRO / RECONCILE
```

반드시 `review_scope`, `review_owner`, `review_due`를 남기고 `REVIEW_POLICY.md`의 post-release remediation 규칙을 따른다.

## 8. Anti-overengineering

금지되는 오해:

- 모든 변경에 ADR
- 모든 requirement에 ID
- 모든 quality characteristic 측정
- 모든 test level 생성
- 모든 endpoint에 layer/interface/factory
- HIGH risk라고 위험과 무관한 검증 모두 실행
- AI가 제안했다는 이유로 모든 finding 구현
- 회사에 개인 workflow/AGY sign-off를 별도 조직 gate로 강제

**필수 검증과 실제 governance 의무는 유지하며 독립 AI review는 선택 사항이다.**

## Review record

- 2026-09-06: #350 기반 risk tailoring/inference/break-glass/verification-review-validation 구분 도입.
- 2026-09-06: #352 prior baseline `READY_FOR_REVIEWED`.
- 2026-09-06: owner가 mandatory independent-review 의도를 명시적으로 승인.
- 2026-09-06: #353/#354 governance review를 선별 반영하여 `REVIEW_POLICY.md v1.2`로 통합. 상세 arbitration은 해당 policy에서만 유지.

- 2026-11-20: owner changes independent AI quality review to optional; REVIEW_POLICY.md v1.5 supersedes historical mandatory rules.
