# Architecture and Design Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- primary_reference: `ISO/IEC/IEEE 42010:2022`

## 목적

설계 문서는 예쁜 다이어그램이 아니라, 중요한 이해관계자 concern과 trade-off를 설명하고 변경의 영향을 예측할 수 있게 하는 의사결정 기록이다.

## 최소 설계 질문

중간 이상 규모의 변경은 가능한 범위에서 다음 질문에 답한다.

1. **System of interest**: 무엇을 설계하는가?
2. **Stakeholders**: 누가 사용·운영·보호·변경하는가?
3. **Concerns**: 무엇이 중요한가? 기능, 보안, 데이터, 성능, 운영, 비용 등.
4. **Context**: 외부 시스템과 어떤 계약으로 연결되는가?
5. **Components / responsibilities**: 책임은 어디에 나뉘는가?
6. **Interfaces / contracts**: API, event, DB, file, batch 계약은 무엇인가?
7. **Data flow / ownership**: 데이터의 생성·소유·변경·삭제 책임은 어디에 있는가?
8. **Trust boundaries**: 어느 입력과 주체를 신뢰하지 않는가?
9. **Failure modes**: 무엇이 실패할 수 있고 어떤 상태가 남는가?
10. **Change / rollback**: 변경과 복구는 어떻게 가능한가?

모든 질문에 별도 문서를 만들 필요는 없다. 위험과 복잡도에 비례해 필요한 viewpoint만 남긴다.

## 설계 산출물 기본값

### Small change

- 변경 책임
- 영향 받는 interface/data
- 테스트/rollback 메모

### Medium change

- context 또는 component diagram
- 주요 data flow
- 중요 interface 계약
- security/data/failure concern
- ADR 또는 decision note

### High-risk / large change

- stakeholder/concern 목록
- context/component/deployment 또는 필요한 viewpoint
- trust boundary와 threat/risk 분석
- data lifecycle/migration
- availability/failure/recovery
- alternatives와 trade-off
- rollout/rollback

## 책임 분리 원칙

**SHOULD:** 책임은 변경 이유가 다른 것끼리 분리한다.

예:

- HTTP parsing/response formatting과 업무 규칙은 같은 책임이 아니다.
- 업무 상태 전이와 persistence 기술은 같은 책임이 아니다.
- 외부 API protocol 세부와 내부 domain rule은 같은 책임이 아니다.

단, 작은 서비스에서 계층을 기계적으로 늘리지 않는다. 별도 추상화가 변경 격리, 테스트, 정책 강제에 실제로 도움이 되는 경우에만 도입한다.

## Interface와 Contract

외부 또는 계층 간 중요한 계약은 다음을 필요한 범위에서 명확히 한다.

- input/output schema
- validation rule
- error semantics
- authentication/authorization assumption
- idempotency/retry
- timeout
- ordering/concurrency
- version compatibility
- side effect

**MUST:** 내부 구현 세부를 외부 계약으로 불필요하게 노출하지 않는다.

## Data Design

데이터 설계에서 다음을 우선 확인한다.

- source of truth
- ownership
- identity/key
- invariant
- unique constraint
- transaction boundary
- retention/deletion
- audit need
- migration/backfill
- concurrent update behavior

애플리케이션 validation만으로 보호하기 어려운 핵심 invariant는 DB constraint 등 더 강한 경계에서 중복 보호할지 검토한다.

## Security by Design

보안은 별도 마지막 단계가 아니다.

설계 중 최소한 다음을 확인한다.

- actor와 privilege
- object-level authorization
- sensitive data
- secret/credential boundary
- external/untrusted input
- trust boundary
- audit event
- abuse/failure path

상세 기준: [`../standards/security.md`](../standards/security.md)

## Failure-first Design

정상 흐름만 그리지 않는다. 외부 호출, DB, queue, file, LLM 또는 비동기 작업이 있으면 다음을 검토한다.

- timeout
- partial failure
- duplicate/retry
- out-of-order
- stale data
- crash between steps
- rollback/compensation
- degraded behavior

## ADR 사용 기준

다음 중 하나면 짧은 ADR을 권장한다.

- 되돌리기 어렵다.
- 여러 팀/컴포넌트에 영향을 준다.
- 보안/정합성/운영 trade-off가 있다.
- 합리적인 대안이 둘 이상이다.
- 나중에 `왜 이렇게 했는가`를 다시 물을 가능성이 높다.

ADR은 최소 다음을 포함한다.

```text
Context
Decision
Alternatives
Consequences
Evidence / constraints
Review date if assumptions may change
```

## Design Review checklist

- [ ] 주요 stakeholder와 concern이 빠지지 않았다.
- [ ] component 책임과 interface가 이해 가능하다.
- [ ] 데이터 source of truth와 ownership이 분명하다.
- [ ] trust boundary와 권한 판정 위치가 보인다.
- [ ] 정상 흐름뿐 아니라 실패·재시도·부분 실패를 고려했다.
- [ ] 중요한 trade-off와 대안을 기록했다.
- [ ] 테스트 가능한 설계인가?
- [ ] rollout/rollback이 현실적인가?
- [ ] 불필요한 계층·추상화·분산화를 만들지 않았다.

## 근거

- ISO/IEC/IEEE 42010:2022: https://www.iso.org/standard/74393.html
- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
