# Software Quality Model

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- primary_reference: `ISO/IEC 25010:2023`

## 목적

`기능이 동작한다`만으로 완료를 판단하지 않고, 프로젝트 위험에 맞는 품질 특성을 요구사항·설계·테스트·검수에 연결한다.

ISO/IEC 25010:2023의 product quality model은 9개 품질 특성을 제공한다. 이 handbook은 이를 모든 프로젝트에 동일하게 강제하지 않고, **해당 변경의 실패 비용과 사용자 영향에 따라 필요한 특성을 선택하는 checklist**로 사용한다.

## 9개 품질 관점

1. Functional suitability
2. Performance efficiency
3. Compatibility
4. Interaction capability
5. Reliability
6. Security
7. Maintainability
8. Flexibility
9. Safety

표준의 명칭과 세부 정의를 임의로 재정의하지 않는다. 프로젝트 문서에서는 필요한 품질 목표를 구체적인 요구사항/검증으로 번역한다.

## 개인 기본 규칙

### QLT-001 — 품질 목표는 측정/판정 가능하게 만든다 — SHOULD

`빠르게`, `안정적으로`, `사용하기 쉽게` 같은 표현만으로 승인하지 않는다. 가능한 경우 조건·측정 방법·acceptance를 붙인다.

### QLT-002 — 모든 품질 특성을 기계적으로 적용하지 않는다 — MUST

저위험 내부 도구에 대규모 HA 설계를 강제하거나, 단순 CRUD에 불필요한 추상화를 추가하는 식의 overengineering을 피한다.

### QLT-003 — 품질 trade-off는 숨기지 않는다 — SHOULD

성능을 위해 일관성을 완화하거나, 보안을 위해 UX가 복잡해지는 등 중요한 trade-off가 있으면 설계 결정에 남긴다.

### QLT-004 — 신뢰성과 복구 가능성을 함께 본다 — SHOULD

실패를 완전히 제거한다고 가정하지 않고, 실패 감지·격리·재시도·복구·rollback 가능성을 시스템 중요도에 맞게 검토한다.

### QLT-005 — 유지보수성은 코드 미관이 아니라 변경 비용으로 본다 — SHOULD

다음이 유지보수성을 해치는지 검토한다.

- 책임이 뒤섞인 component
- 숨은 side effect
- 테스트하기 어려운 global state
- 문서와 실제 동작 불일치
- 불필요한 abstraction
- 변경 영향이 추적되지 않는 contract

## 요구사항 연결 예

```text
NFR-PERF-001
대상: 검색 API
조건: 합의된 데이터셋/부하
목표: p95 latency <= 합의 수치
검증: load test script + 환경 기록

NFR-REL-002
대상: 외부 API 연동
조건: upstream timeout
목표: 로컬 transaction이 중간 상태로 확정되지 않음
검증: failure injection integration test
```

측정 환경이 정해지지 않았다면 AI가 임의의 수치를 만들어 승인 기준으로 넣지 않는다.

## 품질 검토 질문

- 기능이 요구한 시나리오를 실제로 충족하는가?
- 병목 또는 자원 사용 위험이 있는가?
- 기존 시스템·클라이언트·데이터와 호환되는가?
- 사용자가 오류와 상태를 이해할 수 있는가?
- 일부 component 실패 시 전체 상태가 어떻게 되는가?
- 인증/권한/민감정보/감사 요구가 보호되는가?
- 변경과 테스트가 과도하게 어려운 구조인가?
- 환경·규모·배포 방식이 바뀌면 과도한 재작성이 필요한가?
- 실패가 사람·재산·중대한 업무 피해로 이어질 경우 별도 safety requirement가 필요한가?

## Done criteria와 연결

중간 이상 위험의 기능은 `Definition of Done`에서 선택한 품질 특성의 evidence를 명시한다.

예:

```text
functional: acceptance test PASS
security: object authorization negative test PASS
reliability: timeout failure test PASS
maintainability: independent code review PASS
```

## 근거

- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
