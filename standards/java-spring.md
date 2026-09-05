# Java / Spring Engineering Standard

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- scope: `Java / Spring Framework / Spring Boot business applications`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

## 목적

Java/Spring 애플리케이션의 개인 기본값을 정의한다. 프로젝트 요구와 **현재 사용 중인 Spring/DB 버전의 공식 문서**를 우선하며, layer 개수를 강제하는 style guide가 아니다.

## JS-001 — Boundary와 업무 규칙을 분리한다 — SHOULD

Controller는 HTTP request/response, authentication context 전달, boundary validation에 집중하고 핵심 업무 상태 전이·권한·transaction 규칙은 재사용 가능한 application/domain 경계로 분리하는 것을 기본값으로 한다.

단순 read-only endpoint 등에서는 별도 계층이 실제 가치를 주지 않으면 불필요한 class/interface를 만들지 않는다.

## JS-002 — Public API contract와 persistence model을 기본적으로 분리한다 — SHOULD

외부/public HTTP API에서 JPA entity 또는 persistence schema를 request/response contract로 직접 사용하는 것을 기본값으로 삼지 않는다.

이유:
- 민감·내부 field 과다 노출 방지
- mass assignment/over-posting 위험 감소
- API 변경과 DB 변경 격리
- validation/serialization 계약 명시
- lazy-loading/circular serialization 같은 persistence 부작용 격리

**SHOULD NOT:** role, status, owner, audit field 등 중요한 상태를 가진 persistence entity를 public writable request에 직접 binding한다.

예외는 내부 전용·read-only projection 등 위험이 명확히 낮고 별도 DTO가 실질 가치를 주지 않는 경우에 둘 수 있으며 이유를 이해하고 선택한다.

## JS-003 — Transaction boundary는 업무 원자성을 기준으로 결정한다 — MUST

`@Transactional` 위치를 관례로 정하지 않는다. 함께 성공하거나 함께 실패해야 하는 상태 변경을 기준으로 transaction boundary를 결정한다.

다음을 확인한다.
- DB write 순서와 flush 시점
- 외부 API/file/message side effect
- exception 후 rollback semantics
- retry/idempotency
- concurrency/locking
- connection/lock hold time

### Spring declarative transaction의 실제 적용 경계를 확인한다 — MUST

Spring의 기본 declarative transaction은 AOP proxy 기반이다. **default proxy mode에서는 같은 bean 내부의 self-invocation이 proxy를 통과하지 않으므로 `@Transactional` advice가 적용되지 않을 수 있다.**

따라서 transaction이 필요한 메서드를 같은 객체 안에서 호출했다는 이유만으로 새 transaction boundary가 생긴다고 가정하지 않는다. 필요하면 책임 분리, 외부 proxied call, programmatic transaction 또는 프로젝트가 선택한 다른 공식 지원 방식을 사용한다.

Spring 공식 문서: https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html

### Rollback semantics를 명시적으로 이해한다 — MUST

Spring의 기본 rollback rule은 일반적으로 `RuntimeException`과 `Error`를 rollback 대상으로 보고 checked exception은 기본 rollback 대상이 아니다. 그러나 Spring 버전과 프로젝트 configuration에 따라 global rollback 정책을 바꿀 수도 있다.

따라서:
- checked exception을 사용하는 업무 코드라면 실제 rollback expectation을 확인한다.
- `rollbackFor = Exception.class`를 모든 메서드에 기계적으로 붙이지 않는다.
- 현재 Spring 버전의 공식 rollback 문서와 프로젝트 transaction policy를 기준으로 설정한다.
- rollback을 기대하는 failure path는 integration test로 확인한다.

Spring 공식 문서: https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/rolling-back.html

## JS-004 — DB transaction 안의 외부 network I/O를 기본적으로 피한다 — SHOULD NOT

외부 HTTP/RPC 등 latency와 failure가 독립적인 network I/O 동안 DB transaction, connection, lock을 오래 유지하면 connection-pool 고갈과 lock contention, retry 복잡도가 커질 수 있다.

가능하면 다음을 검토한다.
- transaction 이전에 필요한 외부 조회 수행
- commit 이후 side effect
- outbox/event 기반 decoupling
- compensation/idempotency

단, 업무 원자성·기술 제약상 transaction 내부 호출이 불가피할 수 있으므로 절대 금지로 두지 않는다. 이 경우 timeout, 최대 지연, connection/lock 영향, failure/rollback, retry를 명시적으로 검토하고 중요한 경로는 fault/latency 상황으로 검증한다.

## JS-005 — 애플리케이션 선조회만으로 uniqueness/concurrency를 보장하지 않는다 — MUST when invariant requires uniqueness

경합 가능한 핵심 uniqueness는 DB unique constraint, locking 또는 해당 DB가 제공하는 atomic mechanism을 검토한다.

`find -> if absent -> save`만으로 동시 요청의 중복 생성을 방지했다고 가정하지 않는다.

## JS-006 — Validation과 business invariant를 구분한다 — SHOULD

- DTO/boundary validation: 형식, 길이, 필수값 등
- business invariant: 현재 상태, 소유권, 중복, 허용 전이 등

annotation validation만 통과했다고 업무적으로 유효한 요청이라고 판단하지 않는다.

## JS-007 — Authentication과 Authorization을 분리한다 — MUST

Spring Security 설정 또는 annotation을 사용하더라도 role 판정만으로 object-level authorization이 필요한 경우를 놓치지 않는다.

보안 정책은 controller expression에 흩뜨리기보다 변경과 테스트가 가능한 경계를 둔다.

CSRF 여부는 [`security.md`](security.md)의 client/threat-model 기준과 현재 Spring Security 공식 문서를 따른다.

## JS-008 — Exception은 외부 API contract로 변환한다 — MUST

내부 exception, stack trace, SQL/library 메시지를 API 응답으로 그대로 노출하지 않는다.

외부 오류는 stable error semantics를 제공하고, 내부 log에는 correlation에 필요한 최소 evidence를 남긴다.

## JS-009 — Repository/query 이름이 업무 의미를 숨기지 않게 한다 — SHOULD

단순 CRUD는 framework convention을 활용한다. 복잡한 조회가 업무 의미를 갖는 경우 메서드명, query object 또는 service abstraction에서 의도를 드러낸다.

## JS-010 — Lazy loading / query count를 우연에 맡기지 않는다 — SHOULD

목록/상세 API에서 필요한 data shape와 query behavior를 확인한다. N+1 또는 과도한 fetch가 위험한 경로는 integration/query evidence로 검증한다.

## JS-011 — Configuration에는 안전한 기본값과 환경 경계를 둔다 — MUST

- secret hard-code 금지
- profile별 차이를 명시
- 위험한 debug/dev 설정이 production default가 되지 않도록 함
- 필수 설정 누락 시 조용히 insecure fallback하지 않도록 검토

## JS-012 — 코드 설명은 `무엇`보다 `왜`를 남긴다 — SHOULD

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

## JS-013 — 테스트는 Spring wiring과 persistence 위험을 필요한 수준에서 실제로 검증한다 — SHOULD

모든 bean을 mock한 unit test만으로 다음을 검증했다고 주장하지 않는다.
- transaction/rollback/proxy behavior
- security filter/method authorization
- serialization/validation
- DB constraint/query
- configuration/profile

위험에 맞는 slice/integration/E2E를 선택한다.

## Review checklist

- [ ] Controller가 핵심 업무규칙을 과도하게 소유하지 않는다.
- [ ] public API와 persistence model의 결합 위험을 검토했다.
- [ ] transaction boundary가 업무 원자성과 일치한다.
- [ ] self-invocation/proxy와 rollback semantics를 실제 Spring 버전 기준으로 확인했다.
- [ ] DB transaction과 외부 network I/O의 latency/failure 영향을 검토했다.
- [ ] uniqueness/concurrency를 application check만으로 가정하지 않는다.
- [ ] boundary validation과 business invariant가 구분된다.
- [ ] object-level authorization이 필요한지 확인했다.
- [ ] 내부 exception/민감정보가 외부로 노출되지 않는다.
- [ ] query/loading behavior가 중요한 경로에서 검증됐다.
- [ ] secret/config/profile이 안전하다.
- [ ] 설명과 테스트가 실제 코드 계약에 일치한다.

## 근거 및 source material

- Spring Framework transaction annotations: https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html
- Spring Framework rollback rules: https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/rolling-back.html
- Spring Security reference: https://docs.spring.io/spring-security/reference/
- Hibernate ORM documentation when Hibernate/JPA behavior is relevant: https://hibernate.org/orm/documentation/
- 사용하는 DB의 official documentation을 transaction/locking/constraint 동작의 최종 기준으로 사용
- 기존 개인 공개 기준: `son1004007/engineering-career-portfolio/03_portfolio/code-explanation-standard.md`

프레임워크와 DB 동작은 버전에 따라 달라질 수 있으므로 구현 전 현재 프로젝트 version과 공식 문서를 다시 확인한다.

## Review record

- 2026-09-06: ChatGPT initial draft incorporating existing personal code-explanation standard.
- 2026-09-06: AGY/Gemini #351 review received.
- 2026-09-06: Official Spring docs confirmed proxy self-invocation and default rollback semantics; those findings were adopted with version-aware wording.
- 2026-09-06: AGY suggestions for absolute DTO/entity separation and absolute network-I/O prohibition were intentionally softened to risk-based `SHOULD` / `SHOULD NOT` rules because universal prohibition would overfit some architectures.
