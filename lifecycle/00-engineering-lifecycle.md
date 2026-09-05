# Engineering Lifecycle Baseline

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- scope: `general software engineering`
- normative_keywords: `MUST / SHOULD / MAY`

## 목적

이 문서는 문제를 받는 시점부터 운영과 회고까지 반복 적용할 개인 기본 SDLC를 정의한다. 특정 방법론(애자일, 워터폴, 스크럼)을 강제하지 않고, 규모에 따라 산출물 깊이는 줄이되 핵심 판단과 검증은 생략하지 않는다.

## 상위 원칙

1. **문제와 성공 조건을 먼저 정의한다.** 구현 요청을 곧바로 기술 선택으로 번역하지 않는다.
2. **확인된 사실, 추론, 가정, 미확정을 구분한다.** 미확정 내용을 사실처럼 구현하지 않는다.
3. **요구사항은 검증 가능해야 한다.** 구현 방법이 아니라 필요한 결과와 제약을 우선 기록한다.
4. **설계는 중요한 concern과 trade-off를 설명해야 한다.** 다이어그램의 개수보다 결정 근거가 중요하다.
5. **보안·데이터 정합성·운영 가능성은 구현 후 부가 점검이 아니라 설계 입력이다.**
6. **작은 변경을 선호한다.** 되돌릴 수 있고 검증 가능한 단위로 분해한다.
7. **구현자가 자신의 설명만으로 완료를 승인하지 않는다.** 테스트, 정적 분석, 독립 review 또는 실행 증거를 분리한다.
8. **Verification과 Validation을 분리한다.** 제대로 만들었는지와 필요한 것을 만들었는지를 각각 확인한다.
9. **배포 가능성과 복구 가능성을 함께 판단한다.** 변경에 운영 영향이 있으면 rollback 또는 forward-fix 전략을 정의한다.
10. **실패에서 재사용 가능한 규칙을 추출한다.** 단, 한 번의 사례를 곧바로 일반 규칙으로 승격하지 않는다.

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

### DISCOVER

현재 시스템, 이해관계자, 코드·문서·데이터·운영환경·과거 결정과 외부 제약을 확인한다.

**MUST**
- 기존 시스템 수정이라면 실제 repository와 관련 문서를 먼저 확인한다.
- 현재 동작을 모르면 추측으로 대체하지 않는다.
- 보안·인증·버전 의존 동작은 가능한 경우 공식 문서와 현재 runtime evidence를 함께 확인한다.

### DEFINE

문제, 목표, 성공 조건, 범위와 제외 범위를 정의한다.

**SHOULD**
- `왜 필요한가`, `누가 영향을 받는가`, `완료 후 무엇이 달라져야 하는가`를 한 문단 안에서 설명할 수 있어야 한다.

### REQUIREMENTS

기능·보안·데이터·운영·비기능 요구사항을 검증 가능한 형태로 정리한다.

상세 기준: [`01-requirements.md`](01-requirements.md)

### DESIGN

stakeholder concern, component/interface/data flow/trust boundary/failure mode를 필요한 깊이로 정리한다.

상세 기준: [`02-architecture-and-design.md`](02-architecture-and-design.md)

### PLAN

변경 파일, migration, 테스트, 위험, rollback 및 완료 기준을 정의한다.

**MUST**
- 위험한 데이터 변경, 권한 변경, 외부 공개, 운영 배포는 구현 전에 영향과 되돌림 전략을 확인한다.

### IMPLEMENT

요구와 설계에 필요한 최소 변경을 구현한다.

**SHOULD**
- 명확한 이름과 작은 책임을 우선한다.
- 복잡성을 문서로 덮기 전에 구조를 단순화할 수 있는지 검토한다.
- 신뢰하지 않는 입력은 boundary에서 검증하고 내부 불변조건은 별도로 보호한다.

### VERIFY

`우리가 설계한 것을 제대로 만들었는가?`를 확인한다.

예: build, unit/integration/contract/E2E/security negative test, static analysis, migration dry-run, runtime probe.

### REVIEW

작성자와 독립된 관점에서 correctness, simplicity, security, data integrity, operability, maintainability를 검토한다.

### VALIDATE

`사용자 또는 업무가 실제로 필요로 한 것을 만들었는가?`를 acceptance criteria 및 대표 시나리오로 확인한다.

### RELEASE

배포 전 prerequisite, migration, health check, rollback/forward-fix, 관찰 항목을 확인한다.

### OPERATE

운영이 필요한 시스템은 health, logging, monitoring, backup/restore, access control, incident response를 시스템 위험에 맞게 갖춘다.

### LEARN

장애·리뷰 finding·사용자 피드백에서 재발 방지 항목을 추출한다. 반복성이 있거나 여러 프로젝트에 재사용할 수 있을 때 handbook candidate rule로 승격한다.

## 규모에 따른 Tailoring

| 변경 규모 | 최소 기록 |
|---|---|
| trivial | 목적 + 변경 + 필요한 자동 검증 |
| small | scope + acceptance + 구현/테스트 |
| medium | requirements + design decision + test plan + review |
| large/high-risk | source/requirements traceability + architecture + threat/risk + migration/rollback + 독립 review + acceptance evidence |

문서량을 늘리는 것이 목적이 아니다. **결정 실패 비용이 커질수록 기록과 독립 검증의 깊이를 높인다.**

## 근거

- ISO/IEC/IEEE 12207:2026, Software life cycle processes: https://www.iso.org/standard/90219.html
- ISO/IEC/IEEE 29148:2018, Requirements engineering: https://www.iso.org/standard/72089.html
- ISO/IEC/IEEE 42010:2022, Architecture description: https://www.iso.org/standard/74393.html
- ISO/IEC 25010:2023, Product quality model: https://www.iso.org/standard/78176.html
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final

## Review record

- 2026-09-06: ChatGPT initial draft based on current official sources and existing personal workflow assets.
- Independent AGY/Gemini review: pending.
