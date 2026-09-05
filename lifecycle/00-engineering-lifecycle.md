# Engineering Lifecycle Baseline

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- scope: `general software engineering`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

## 목적

문제를 받는 시점부터 운영과 회고까지 반복 적용할 개인 기본 SDLC를 정의한다. 특정 방법론을 강제하거나 12단계 waterfall gate로 해석하지 않는다. 단계는 필요에 따라 반복·병합·축약하며, 실제 적용 깊이는 [`OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 LOW/MEDIUM/HIGH risk tier를 따른다.

## 상위 원칙

1. **문제와 성공 조건을 먼저 정의한다.** 구현 요청을 곧바로 기술 선택으로 번역하지 않는다.
2. **확인된 사실, 추론, 가정, 미확정을 구분한다.**
3. **요구사항은 검증 가능해야 한다.** 구현 방법이 아니라 필요한 결과와 제약을 우선한다.
4. **설계는 중요한 concern과 trade-off를 설명한다.** 다이어그램 개수보다 결정 근거가 중요하다.
5. **보안·데이터 정합성·운영 가능성은 설계 입력이다.**
6. **작고 되돌릴 수 있는 변경을 선호한다.**
7. **작성자의 주장과 검증 증거를 분리한다.** Solo mode에서는 self-approval이 가능하지만 deterministic verification 및 가능한 독립/fresh-eyes review를 활용한다. Team mode에서는 조직 정책을 따른다.
8. **VERIFY / REVIEW / VALIDATE를 구분한다.**
9. **배포 가능성과 복구 가능성을 함께 판단한다.**
10. **실패에서 재사용 가능한 규칙을 추출하되 한 사례를 곧바로 일반 규칙으로 만들지 않는다.**

## 기본 흐름

```text
DISCOVER
  -> DEFINE
  -> REQUIREMENTS
  -> DESIGN
  -> PLAN
  -> IMPLEMENT
  -> VERIFY
  -> REVIEW
  -> VALIDATE
  -> RELEASE
  -> OPERATE
  -> LEARN
```

이 흐름은 순차 승인 체계가 아니라 **확인해야 할 관점의 지도**다. 작은 변경에서는 여러 단계를 하나의 PR 설명과 테스트로 합칠 수 있다.

### DISCOVER

현재 시스템, 이해관계자, 코드·문서·데이터·운영환경·과거 결정과 외부 제약을 확인한다.

**MUST**
- 기존 시스템 수정이라면 실제 repository와 관련 Source of Truth를 먼저 확인한다.
- 현재 동작을 모르면 추측을 사실로 대체하지 않는다.

**SHOULD**
- 보안·인증·버전 의존 동작은 공식 문서와 현재 runtime evidence를 함께 확인한다.

### DEFINE

문제, 목표, 성공 조건, 범위와 제외 범위를 정의한다.

### REQUIREMENTS

기능·보안·데이터·운영·비기능 요구를 필요한 깊이로 정리한다.

상세: [`01-requirements.md`](01-requirements.md)

### DESIGN

stakeholder concern, component/interface/data flow/trust boundary/failure mode를 필요한 깊이로 정리한다.

상세: [`02-architecture-and-design.md`](02-architecture-and-design.md)

### PLAN

변경 범위, 위험, 검증, migration/rollback을 필요한 수준에서 정의한다.

**MUST for HIGH risk**
- 파괴적 데이터 변경, 인증/인가 모델 변경, public exposure, production privilege 확대처럼 blast radius가 큰 변경은 실행 전 영향과 recovery strategy를 확인한다.

### IMPLEMENT

요구와 설계에 필요한 최소 변경을 구현한다.

### VERIFY

기술 specification/contract에 맞게 만들었는지 확인한다.

대표 evidence:
- build/compile
- unit/integration/contract/API/E2E test
- static/security analysis
- migration dry-run
- bounded runtime probe

결과는 `PASS / FAIL / NOT RUN / BLOCKED`로 구분한다.

### REVIEW

사람 또는 독립 AI/fresh-eyes 관점에서 correctness, simplicity, security, data integrity, unintended effects, operability를 의미 수준에서 검토한다.

Solo/Team review 기대치는 [`OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

### VALIDATE

사용자·업무 목적과 acceptance criteria를 실제 대표 시나리오에서 충족하는지 확인한다.

### RELEASE

배포 전 prerequisite, migration, health, rollback/forward-fix 및 관찰 항목을 필요한 수준에서 확인한다.

### OPERATE

운영 시스템은 위험에 맞게 logging/metrics/tracing, health/monitoring, access control, backup/restore, dependency/support 상태와 incident path를 고려한다.

### LEARN

장애·리뷰 finding·사용자 피드백에서 재발 방지 및 handbook candidate rule을 추출한다. deprecation/decommission이나 schema/API sunset이 필요한 변경도 이 lifecycle의 change/release/operate 대상으로 관리한다.

## Tailoring

상세 위험 기준은 [`OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 LOW/MEDIUM/HIGH를 사용한다.

| Risk | 기본 기록 수준 |
|---|---|
| LOW | 목적 + 변경 + 필요한 검증 |
| MEDIUM | Problem/Scope + Auth/Data/Ops impact + 결정 + verification + 필요한 rollback note |
| HIGH | traceability + architecture/security/data decision + migration/recovery + 독립 review + acceptance/release evidence |

MEDIUM이라는 이유만으로 requirement ID, ADR, 모든 quality view, 모든 test level을 강제하지 않는다.

## Break-glass

운영 장애·active security incident·데이터 손상 확대처럼 containment가 더 중요한 경우 일반 흐름을 축약한다.

```text
TRIAGE -> CONTAIN / MINIMAL HOTFIX -> BOUNDED VERIFY -> EXPEDITED RELEASE -> MONITOR -> RETRO / RECONCILE
```

상세 규칙은 [`OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

## 근거

- ISO/IEC/IEEE 12207:2026: https://www.iso.org/standard/90219.html
- ISO/IEC/IEEE 29148:2018: https://www.iso.org/standard/72089.html
- ISO/IEC/IEEE 42010:2022: https://www.iso.org/standard/74393.html
- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: Revised after AGY/Gemini independent review #350: removed solo-review deadlock, clarified stage semantics, simplified risk tailoring, added observability/deprecation and break-glass path.
