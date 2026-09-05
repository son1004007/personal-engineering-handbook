# Requirements Engineering Standard

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- primary_reference: `ISO/IEC/IEEE 29148:2018`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

## 목적

요구사항을 설계·테스트·검수와 연결되는 검증 가능한 계약으로 관리하되, low/medium-risk 작업에 불필요한 ID 문서를 강제하지 않는다.

## 요구사항 분류와 ID

필요할 때 다음 식별자를 사용할 수 있다.

- `FR-###`: 기능
- `SEC-###`: 보안·권한·감사
- `DATA-###`: 데이터 품질·정합성·보존
- `OPS-###`: 배포·운영·복구
- `NFR-###`: 성능·신뢰성·호환성·유지보수성 등

**MAY:** LOW/MEDIUM risk의 단순 변경은 PR/Issue의 명확한 acceptance 항목만으로 충분할 수 있다.

**SHOULD:** HIGH risk, 다수 requirement, 감사/계약 추적 필요, 장기간 변경에서는 stable ID를 사용한다.

## Source와 상태

필요한 경우 source에도 `SRC-###` 식별자를 부여한다. 모든 소스에 번호를 매기는 것이 목적이 아니다.

상태:

- `CONFIRMED`: 명시적 근거나 직접 증거가 있음
- `INFERRED`: 근거에서 도출한 기술적 추론이며 명시 확인은 없음
- `UNKNOWN`: 필요한 정보가 부족함
- `CONFLICT`: 둘 이상의 source가 충돌함

**MUST:** `INFERRED`, `UNKNOWN`, `CONFLICT`를 `CONFIRMED`처럼 표현하지 않는다.

## Inference 경계

상세 공통 기준은 [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

### 진행 가능한 기술적 inference

다음은 근거를 명시하고 `INFERRED`로 둘 수 있다.

- repository 코드/config/test에서 직접 도출되는 현재 동작
- 현재 공식 framework/vendor 문서에서 확인되는 기술 동작
- 기존 contract와 일관된 되돌릴 수 있는 기술적 기본값
- business semantics를 새로 만들지 않는 low-risk defensive behavior

### 임의 확정하면 안 되는 요구

다음은 기술적으로 그럴듯해도 confirmation 없이 승인 requirement로 만들지 않는다.

- business policy / 상태 전이 의미
- role entitlement / object access policy
- retention/deletion 기간
- SLA/SLO/성능·가용성 수치
- 가격/비용/과금 정책
- encryption/key ownership
- destructive migration semantics
- 회사/고객 승인 절차

이 distinction은 AI가 정상 기술 판단까지 모두 `UNKNOWN`으로 만들어 작업을 멈추는 것과, hallucination을 `INFERRED`로 포장하는 것을 둘 다 방지하기 위한 것이다.

## 좋은 요구사항의 최소 조건

중요한 요구사항은 가능한 범위에서 다음에 답한다.

1. 누가 또는 무엇이 대상인가?
2. 어떤 결과/제약이 필요한가?
3. 어떤 조건에서 적용되는가?
4. 성공/실패를 어떻게 판정하는가?
5. 다른 요구와 충돌/의존하는가?

`중요한`의 기본 판정은 [`OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

### 나쁜 예

> 검색이 빨라야 한다.

### 개선 방향

측정 환경, 데이터셋, 부하 조건과 목표값이 실제 source에서 확인된 후 acceptance 기준을 만든다. **AI가 근거 없는 p95 수치 같은 목표를 임의 생성하지 않는다.**

## 구현과 요구사항 분리

- `사용자는 본인 데이터만 조회할 수 있어야 한다` → 요구사항
- `Spring Security @PreAuthorize를 사용한다` → 일반적으로 구현/설계 선택

기술이 법률·계약·플랫폼 제약으로 실제 필수라면 requirement가 될 수 있다.

## Acceptance Criteria

중요한 요구사항은 사람이 판정 가능한 acceptance 또는 verification과 연결한다.

```text
Given 사용자 A와 사용자 B의 서로 다른 소유 데이터
When A가 B의 객체 식별자로 접근을 시도
Then 프로젝트가 정의한 정보노출 정책에 따라 B의 데이터가 반환되지 않는다.
```

구체 HTTP status나 오류 형식은 기존 API contract 또는 확인된 프로젝트 policy가 있을 때 확정한다.

## Traceability

필요할 때 다음 연결을 유지한다.

```text
source -> requirement -> design/decision -> implementation -> test/evidence
```

- LOW: 보통 별도 traceability table 불필요
- MEDIUM: 핵심 auth/data/API/ops 변경만 간단히 연결
- HIGH: 중요한 source/requirement/decision/evidence를 명시적으로 추적

모든 코드 라인을 requirement에 매핑하지 않는다.

## Readiness Gate

다음이 핵심 blocker라면 `NOT READY` 또는 명시된 assumption 아래 제한적으로 진행한다.

- scope/actor가 불명확
- 중요 source 충돌
- 데이터 ownership/authorization이 불명확
- destructive migration의 영향/복구 기준 없음
- 성공 판정 기준 없음

LOW/MEDIUM이며 되돌리기 쉬운 기술적 세부는 `READY WITH ASSUMPTIONS`로 진행할 수 있다. business/security/data ownership 같은 금지 fabrication 영역은 이 경로로 우회하지 않는다.

긴급 장애/보안 containment는 일반 DoR 대신 [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 Break-glass를 따른다.

## 검수 checklist

- [ ] 문제와 성공 조건이 구분된다.
- [ ] 중요한 requirement에 근거가 있다.
- [ ] 사실과 inference/unknown/conflict가 구분된다.
- [ ] business 요구와 implementation choice를 혼동하지 않았다.
- [ ] auth/data/ops/NFR 영향을 필요한 깊이로 확인했다.
- [ ] 중요한 요구는 검증/acceptance와 연결된다.
- [ ] risk tier보다 과도한 ID/문서를 만들지 않았다.

## 근거

- ISO/IEC/IEEE 29148:2018: https://www.iso.org/standard/72089.html
- ISO/IEC/IEEE 12207:2026: https://www.iso.org/standard/90219.html

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: Revised after AGY/Gemini #350. Accepted inference/fabrication boundary and lighter traceability; did not adopt AGY's specific HTTP/idempotency examples as universal rules because those depend on actual protocol/project contracts.
