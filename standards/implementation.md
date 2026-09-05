# Implementation Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- scope: `language/framework neutral`

## 목적

구현 단계의 기본값을 정의한다. 특정 언어 스타일보다 **정확성, 단순성, 변경 가능성, 검증 가능성**을 우선한다.

## IMP-001 — 요구와 무관한 변경을 섞지 않는다 — SHOULD

하나의 변경은 가능한 한 하나의 목적을 중심으로 한다. 대규모 포맷 변경, 이름 변경, dependency 교체와 기능 변경을 한 번에 섞지 않는다.

예외: 안전한 migration을 위해 불가피하거나, 먼저 분리해야 기능 구현이 가능한 경우.

## IMP-002 — 명시적 계약을 우선한다 — MUST

다음은 코드 또는 가까운 문서/테스트에서 판정 가능해야 한다.

- 입력 허용 범위
- 반환/오류 의미
- 상태 변경
- 권한 판정
- transaction/atomicity
- 외부 side effect
- retry/idempotency가 필요한 경우 그 정책

## IMP-003 — 이름과 구조가 `무엇`을 말하고 설명은 `왜`를 보완한다 — SHOULD

코드가 이미 표현하는 동작을 주석으로 반복하지 않는다. 설명은 업무 규칙, 제약, 실패 방식, 변경 영향을 중심으로 한다.

상세 언어별 기준은 별도 문서로 분리한다.

## IMP-004 — 신뢰 경계를 명시한다 — MUST

사용자 입력, 외부 API, file, message, LLM output, client-supplied identifier 등 신뢰하지 않는 데이터는 검증 없이 내부 불변조건에 직접 사용하지 않는다.

## IMP-005 — 핵심 invariant는 가장 강한 적절한 경계에서 보호한다 — SHOULD

예:

- unique business identity → DB unique constraint 검토
- 허용 상태 전이 → domain/application rule
- 객체 소유권 → server-side authorization
- 필수 schema → parser/validation + persistence constraint

중복 보호의 비용과 일관성도 함께 검토한다.

## IMP-006 — 예외를 정상 제어 흐름 대용으로 남용하지 않는다 — SHOULD

예외는 실패 계약을 표현하고 boundary에서 필요한 형태로 변환한다. 내부 예외 메시지/stack trace를 외부 응답으로 직접 노출하지 않는다.

## IMP-007 — 실패 후 상태를 먼저 생각한다 — MUST for state-changing code

상태를 바꾸는 코드는 다음을 검토한다.

- 중간 실패 시 무엇이 남는가?
- transaction으로 묶어야 하는가?
- 외부 side effect와 DB commit 순서는 안전한가?
- 재시도 시 중복이 생기는가?
- partial success를 어떻게 복구하는가?

## IMP-008 — timeout/retry를 무한정 위임하지 않는다 — MUST for external I/O

외부 I/O에는 환경과 라이브러리의 기본값을 확인하고, 서비스 위험에 맞는 timeout을 명시할지 검토한다. retry는 idempotency와 부하 증폭을 함께 고려한다.

## IMP-009 — 불필요한 추상화를 만들지 않는다 — SHOULD

다음 이유가 명확하지 않으면 abstraction을 추가하지 않는다.

- 정책을 한 곳에서 강제
- 구현 교체 가능성이 실제로 존재
- 테스트 경계가 필요
- 변경 이유가 명확히 분리됨
- 외부 기술 세부를 domain에서 격리

`미래에 혹시`만으로 interface/factory/layer를 늘리지 않는다.

## IMP-010 — configuration과 secret을 분리한다 — MUST

비밀정보는 source code/repository에 저장하지 않는다. configuration 값도 환경별 차이와 안전한 기본값을 분명히 한다.

## IMP-011 — 로그는 운영과 감사에 필요한 정보를 남기되 민감정보를 최소화한다 — MUST

로그에 password/token/session/secret 및 불필요한 개인정보를 남기지 않는다. 식별이 필요한 경우 최소한의 stable identifier/correlation id를 사용한다.

## IMP-012 — 변경 가능성보다 이해 가능성을 먼저 최적화한다 — SHOULD

추상화, generic framework, meta-programming, reflection 등의 사용은 실제 복잡도 감소가 명확한 경우에 한한다.

## 완료 전 최소 확인

- [ ] 요구사항과 구현이 연결된다.
- [ ] 불필요한 변경이 섞이지 않았다.
- [ ] 오류/실패 상태가 정의돼 있다.
- [ ] 권한 및 신뢰 경계가 필요한 위치에서 확인된다.
- [ ] 데이터 invariant와 transaction을 검토했다.
- [ ] 외부 I/O timeout/retry/side effect를 검토했다.
- [ ] secret/민감정보 노출이 없다.
- [ ] 테스트 가능한 구조다.

## 근거

- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- Google Engineering Practices: https://google.github.io/eng-practices/review/
- 기존 개인 공개 기준: `engineering-career-portfolio/03_portfolio/code-explanation-standard.md`

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
