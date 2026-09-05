# Architecture and Design Standard

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- primary_reference: `ISO/IEC/IEEE 42010:2022`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

## 목적

설계 문서는 예쁜 다이어그램이 아니라, 중요한 이해관계자 concern과 trade-off를 설명하고 변경의 영향을 예측할 수 있게 하는 의사결정 기록이다.

## 최소 설계 질문

MEDIUM/HIGH risk 변경은 필요한 범위에서 다음을 확인한다.

1. **System of interest**: 무엇을 설계하는가?
2. **Stakeholders**: 누가 사용·운영·보호·변경하는가?
3. **Concerns**: 기능, 보안, 데이터, 성능, 운영, 비용 중 무엇이 중요한가?
4. **Context**: 외부 시스템과 어떤 계약으로 연결되는가?
5. **Components / responsibilities**: 책임은 어디에 나뉘는가?
6. **Interfaces / contracts**: API, event, DB, file, batch 계약은 무엇인가?
7. **Data flow / ownership**: 생성·소유·변경·삭제 책임은 어디에 있는가?
8. **Trust boundaries**: 어느 입력과 주체를 신뢰하지 않는가?
9. **Failure modes**: 무엇이 실패할 수 있고 어떤 상태가 남는가?
10. **Operability**: 실패와 성능/상태를 어떻게 관찰하고 진단할 것인가?
11. **Dependencies / supply chain**: 새 dependency가 필요한가, source/support/security risk는 무엇인가?
12. **Evolution / retirement**: versioning, compatibility, deprecation, schema/API/data sunset이 필요한가?
13. **Change / rollback**: 변경과 복구는 어떻게 가능한가?

모든 질문에 별도 문서를 만들 필요는 없다. [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)의 risk tier와 `Important` 기준에 따라 필요한 viewpoint만 남긴다.

## 설계 산출물 기본값

### LOW
- 변경 책임
- 필요한 interface/data 영향
- verification 메모

### MEDIUM
- Problem/Scope
- 필요한 context/component/data flow
- Auth/Data/Ops 영향
- 중요한 interface/failure decision
- verification/rollback note

### HIGH
- stakeholder/concern
- 필요한 architecture viewpoints
- trust boundary와 threat/risk
- data lifecycle/migration
- availability/failure/recovery
- observability/operability
- dependency/supply-chain impact
- compatibility/deprecation strategy
- alternatives/trade-off
- rollout/rollback

MEDIUM이라는 이유만으로 diagram/ADR를 모두 강제하지 않는다.

## 책임 분리 원칙 — SHOULD

책임은 변경 이유가 다른 것끼리 분리하는 것을 기본값으로 한다.

예:
- HTTP parsing/response formatting과 업무 규칙
- 업무 상태 전이와 persistence 기술
- 외부 protocol 세부와 내부 domain rule

단, 별도 abstraction이 변경 격리·정책 강제·테스트에 실제 가치를 주지 않으면 계층을 기계적으로 늘리지 않는다.

## Interface와 Contract

중요한 계약은 해당되는 항목만 명확히 한다.
- input/output schema
- validation/error semantics
- authentication/authorization assumption
- idempotency/retry
- timeout
- ordering/concurrency
- version compatibility/deprecation
- side effect

**MUST:** 내부 구현 세부를 외부 계약으로 불필요하게 노출하지 않는다.

## Data Design

필요한 범위에서 다음을 확인한다.
- source of truth / ownership
- identity/key / invariant / constraint
- transaction/concurrency
- retention/deletion
- audit need
- migration/backfill
- archival/decommission/sunset

핵심 invariant는 application validation만으로 충분한지, DB constraint 같은 더 강한 경계가 필요한지 검토한다.

## Security by Design

설계 중 actor/privilege, object-level authorization, sensitive data, secret boundary, untrusted input, trust boundary, audit event, abuse/failure path를 필요한 수준에서 확인한다.

상세: [`../standards/security.md`](../standards/security.md)

## Operability / Observability by Design

운영 가능한 시스템은 오류가 난 뒤 로그를 추가하는 방식만으로 끝내지 않는다. 중요한 경로에서 다음 중 필요한 것을 설계한다.

- health/readiness signal
- structured logs와 correlation identifier
- metrics
- distributed trace/context propagation
- audit event
- alert 기준 및 runbook 연결

RED/USE나 distributed tracing을 모든 서비스에 강제하지 않는다. 장애 탐지·원인 분석·용량 판단에 실제 필요한 telemetry만 선택하고 민감정보 노출을 피한다.

## Dependency / Supply-chain Design

새 dependency가 architecture에 의미 있는 영향을 줄 경우 다음을 확인한다.
- 실제 source/official registry 존재
- ownership/maintenance/support 상태
- license
- known security risk 확인 경로
- transitive dependency 및 upgrade/exit cost

세부 보안 기준은 [`../standards/security.md`](../standards/security.md)를 따른다.

## Failure-first Design

외부 호출, DB, queue, file, AI/LLM 또는 비동기 작업이 있으면 해당되는 실패를 검토한다.
- timeout
- partial failure
- duplicate/retry
- out-of-order
- stale data
- crash between steps
- rollback/compensation
- degraded behavior

## ADR 사용 기준 — SHOULD when decision cost is high

다음 중 하나면 짧은 ADR을 권장한다.
- 되돌리기 어렵다.
- 여러 component/team에 영향을 준다.
- 보안/정합성/운영 trade-off가 있다.
- 합리적인 대안이 둘 이상이다.
- 나중에 `왜 이렇게 했는가`를 다시 물을 가능성이 높다.

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
- [ ] data source of truth/ownership/invariant가 필요한 수준에서 분명하다.
- [ ] trust boundary와 authorization 위치가 보인다.
- [ ] 정상 흐름뿐 아니라 중요한 failure/retry/partial failure를 고려했다.
- [ ] 운영 시 필요한 관찰 가능성을 검토했다.
- [ ] 새 dependency의 공급망/지원 위험을 필요한 수준에서 확인했다.
- [ ] compatibility/deprecation/decommission 영향이 있으면 계획이 있다.
- [ ] 중요한 trade-off와 대안을 기록했다.
- [ ] verification과 rollout/rollback이 현실적이다.
- [ ] 불필요한 계층·추상화·분산화를 만들지 않았다.

## 근거

- ISO/IEC/IEEE 42010:2022: https://www.iso.org/standard/74393.html
- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #350 findings selectively incorporated: observability, dependency/supply-chain, deprecation/decommission concerns added; no universal RED/USE/tracing mandate adopted.
