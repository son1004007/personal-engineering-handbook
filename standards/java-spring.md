# Java / Spring Engineering Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- scope: `Java / Spring Framework / Spring Boot business applications`

## 목적

Java/Spring 애플리케이션의 개인 기본값을 정의한다. 프레임워크 관례보다 프로젝트 요구와 공식 문서를 우선하며, 이 문서는 layer 개수를 강제하는 style guide가 아니다.

## JS-001 — Boundary와 업무 규칙을 분리한다 — SHOULD

Controller는 HTTP request/response, authentication context 전달, boundary validation에 집중하고 핵심 업무 상태 전이·권한·transaction 규칙은 재사용 가능한 application/domain 경계로 분리하는 것을 기본값으로 한다.

단순 read-only endpoint 등에서는 별도 계층이 실제 가치를 주지 않으면 불필요한 class/interface를 만들지 않는다.

## JS-002 — DTO와 persistence model의 외부 노출을 분리한다 — SHOULD

API contract를 JPA entity 또는 persistence schema와 직접 결합하지 않는 것을 기본값으로 한다.

이유:

- 민감 field 과다 노출 방지
- API 변경과 DB 변경 격리
- validation/serialization 계약 명시

작은 내부 도구에서 위험이 낮고 계약이 단순한 경우 예외를 허용할 수 있다.

## JS-003 — Transaction boundary는 업무 원자성을 기준으로 결정한다 — MUST

`@Transactional` 위치를 관례로 정하지 않는다. 함께 성공하거나 함께 실패해야 하는 상태 변경을 기준으로 transaction boundary를 결정한다.

다음을 확인한다.

- DB write 순서
- flush 시점
- 외부 API/file/message side effect
- exception 후 rollback 가능성
- retry/idempotency
- concurrency

외부 network call을 장시간 DB transaction 안에 두는 경우 lock/time impact와 실패 전략을 명시적으로 검토한다.

## JS-004 — 애플리케이션 선조회만으로 uniqueness/concurrency를 보장하지 않는다 — MUST when invariant requires uniqueness

경합 가능한 핵심 uniqueness는 DB unique constraint, locking 또는 해당 DB가 제공하는 atomic mechanism을 검토한다.

`find -> if absent -> save`만으로 동시 요청의 중복 생성을 방지했다고 가정하지 않는다.

## JS-005 — Validation과 business invariant를 구분한다 — SHOULD

- DTO/boundary validation: 형식, 길이, 필수값 등
- business invariant: 현재 상태, 소유권, 중복, 허용 전이 등

annotation validation만 통과했다고 업무적으로 유효한 요청이라고 판단하지 않는다.

## JS-006 — Authentication과 Authorization을 분리한다 — MUST

Spring Security 설정 또는 annotation을 사용하더라도 role 판정만으로 object-level authorization이 필요한 경우를 놓치지 않는다.

보안 정책은 controller 표현식에 흩뜨리기보다 변경과 테스트가 가능한 경계를 둔다.

## JS-007 — Exception은 외부 API contract로 변환한다 — MUST

내부 exception, stack trace, SQL/library 메시지를 API 응답으로 그대로 노출하지 않는다.

외부 오류는 stable error semantics를 제공하고, 내부 log에는 correlation에 필요한 최소 evidence를 남긴다.

## JS-008 — Repository/query method 이름이 업무 의미를 숨기지 않게 한다 — SHOULD

단순 CRUD는 framework convention을 활용한다. 복잡한 조회가 업무 의미를 갖는 경우 메서드명, query object 또는 service abstraction에서 의도를 드러낸다.

## JS-009 — Lazy loading / query count를 우연에 맡기지 않는다 — SHOULD

목록/상세 API에서 필요한 data shape와 query behavior를 확인한다. N+1 또는 과도한 fetch가 위험한 경로는 integration/query evidence로 검증한다.

## JS-010 — Configuration에는 안전한 기본값과 환경 경계를 둔다 — MUST

- secret hard-code 금지
- profile별 차이를 명시
- 위험한 debug/dev 설정이 production default가 되지 않도록 함
- 필수 설정 누락 시 조용히 insecure fallback하지 않도록 검토

## JS-011 — 코드 설명은 `무엇`보다 `왜`를 남긴다 — SHOULD

기존 개인 승인 기준을 초기 source로 사용한다.

우선 설명 대상:

- 업무 책임이 있는 type
- 공개 application/service entrypoint
- transaction/idempotency/concurrency
- permission/data exposure
- 외부 I/O
- 변경 시 함께 확인해야 할 test/contract

생략 대상:

- 자명한 getter/setter
- 단순 필드 대입
- 코드를 그대로 번역한 comment
- 오래된 TODO/주석 처리 코드

## JS-012 — 테스트는 Spring wiring과 persistence 위험을 필요한 수준에서 실제로 검증한다 — SHOULD

모든 bean을 mock한 unit test만으로 다음을 검증했다고 주장하지 않는다.

- transaction rollback
- security filter/method authorization
- serialization/validation
- DB constraint/query
- configuration/profile

위험에 맞는 slice/integration/E2E를 선택한다.

## Review checklist

- [ ] Controller가 핵심 업무규칙을 과도하게 소유하지 않는다.
- [ ] API contract와 persistence model 결합 위험을 검토했다.
- [ ] transaction boundary가 업무 원자성과 일치한다.
- [ ] uniqueness/concurrency를 application check만으로 가정하지 않는다.
- [ ] boundary validation과 business invariant가 구분된다.
- [ ] object-level authorization이 필요한지 확인했다.
- [ ] 내부 exception/민감정보가 외부로 노출되지 않는다.
- [ ] query/loading behavior가 중요한 경로에서 검증됐다.
- [ ] secret/config/profile이 안전하다.
- [ ] 설명과 테스트가 실제 코드 계약에 일치한다.

## 근거 및 source material

- Spring Framework reference documentation: https://docs.spring.io/spring-framework/reference/
- Spring Security reference: https://docs.spring.io/spring-security/reference/
- Hibernate ORM documentation when Hibernate/JPA behavior is relevant: https://hibernate.org/orm/documentation/
- 사용하는 DB의 official documentation을 transaction/locking/constraint 동작의 최종 기준으로 사용
- 기존 개인 공개 기준: `son1004007/engineering-career-portfolio/03_portfolio/code-explanation-standard.md`

프레임워크와 DB 동작은 버전에 따라 달라질 수 있으므로 구체적 구현 전 현재 프로젝트 version과 공식 문서를 다시 확인한다.

## Review record

- 2026-09-06: ChatGPT initial draft incorporating existing personal code-explanation standard.
- Independent AGY/Gemini review: pending.
