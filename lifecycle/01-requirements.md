# Requirements Engineering Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- primary_reference: `ISO/IEC/IEEE 29148:2018`

## 목적

요구사항을 구현 전의 메모가 아니라, 설계·테스트·검수와 연결되는 검증 가능한 계약으로 관리한다.

## 요구사항 분류

기본 식별자는 다음을 사용한다.

- `FR-###`: 기능 요구사항
- `SEC-###`: 보안·권한·감사 요구사항
- `DATA-###`: 데이터 품질·정합성·보존 요구사항
- `OPS-###`: 배포·운영·복구 요구사항
- `NFR-###`: 성능·신뢰성·호환성·유지보수성 등 비기능 요구사항

필요하면 프로젝트가 더 구체적인 prefix를 정의할 수 있다.

## Source와 상태

각 중요한 요구사항은 가능한 범위에서 출처를 가진다.

```text
SRC-001 사용자 명시 요청
SRC-002 계약/공식 요구 문서
SRC-003 기존 시스템 동작
SRC-004 운영 로그/장애 증거
SRC-005 법률/표준/공식 vendor 문서
```

요구 또는 source의 상태는 다음 중 하나로 구분한다.

- `CONFIRMED`: 명시적 근거나 직접 증거가 있음
- `INFERRED`: 합리적 추론이지만 명시 확인은 없음
- `UNKNOWN`: 결정에 필요한 정보가 부족함
- `CONFLICT`: 둘 이상의 source가 충돌함

**MUST:** `INFERRED`, `UNKNOWN`, `CONFLICT`를 `CONFIRMED`처럼 표현하지 않는다.

## 좋은 요구사항의 최소 조건

요구사항은 가능한 범위에서 다음 질문에 답해야 한다.

1. 누가 또는 무엇이 대상인가?
2. 어떤 결과/제약이 필요한가?
3. 어떤 조건에서 적용되는가?
4. 성공/실패를 어떻게 판정하는가?
5. 다른 요구사항과 충돌하거나 의존하는가?

### 나쁜 예

> 검색이 빨라야 한다.

### 개선 예

> `NFR-004`: 정상 운영 부하에서 검색 API의 p95 응답시간은 합의된 측정 환경과 데이터셋 기준 1초 이하를 목표로 한다. 측정 환경이 확정되지 않으면 수치를 승인 요구사항으로 사용하지 않는다.

수치가 근거 없이 만들어지면 안 되므로, **측정 조건이 없는 숫자를 AI가 임의 생성하지 않는다.**

## 구현과 요구사항 분리

요구사항에 특정 기술을 넣기 전에 그것이 실제 제약인지 구현 선택인지 구분한다.

예:

- `사용자는 본인 데이터만 조회할 수 있어야 한다` → 요구사항
- `Spring Security @PreAuthorize를 사용한다` → 구현/설계 선택

법률·계약·기존 플랫폼 제약 등으로 기술이 실제 필수라면 요구사항에 포함할 수 있다.

## Acceptance Criteria

각 중요한 요구사항은 사람이 판정 가능한 acceptance criteria 또는 검증 방법과 연결한다.

```text
FR-012
Given 유효한 사용자와 검색 조건
When 검색 요청
Then 권한 범위 안의 결과만 반환한다.

SEC-007
Given 사용자 A와 사용자 B의 서로 다른 소유 데이터
When A가 B의 객체 식별자로 조회를 시도
Then B의 데이터가 노출되지 않는다.
```

## Traceability

중간 이상 위험의 변경은 최소한 다음 연결을 유지한다.

```text
source -> requirement -> design/decision -> implementation -> test/evidence
```

모든 코드 라인을 요구사항에 매핑할 필요는 없다. 업무 규칙, 보안, 데이터, 운영 위험이 큰 변경을 우선한다.

## Readiness Gate

다음 중 하나라도 중대한 경우 구현을 보류하거나 `READY WITH ASSUMPTIONS`로 명시한다.

- scope 또는 actor가 불명확함
- 상충하는 source가 해결되지 않음
- 데이터 소유/권한 규칙이 불명확함
- 파괴적 migration의 영향/복구 기준이 없음
- 성공 판정 기준이 없음

되돌리기 쉬운 low-risk 변경은 가정과 영향을 기록하고 진행할 수 있다.

## 검수 checklist

- [ ] 문제와 성공 조건이 분리되어 있다.
- [ ] 중요한 요구사항에 출처 또는 근거가 있다.
- [ ] 사실과 추론/가정을 구분했다.
- [ ] 구현 기술을 업무 요구와 혼동하지 않았다.
- [ ] 보안·데이터·운영·비기능 요구를 필요한 범위에서 확인했다.
- [ ] 중요한 요구사항은 검증/acceptance 방식과 연결된다.
- [ ] 충돌과 미확정 사항이 숨겨져 있지 않다.

## 근거

- ISO/IEC/IEEE 29148:2018: https://www.iso.org/standard/72089.html
- ISO/IEC/IEEE 12207:2026: https://www.iso.org/standard/90219.html

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
