# Deliverables and Handover Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.4.1`

## Purpose

프로젝트 산출물은 문서를 많이 만드는 것이 목적이 아니다. 다음 질문에 재현 가능하게 답할 수 있어야 한다.

1. 무엇을 요구받았는가?
2. 무엇을 설계하고 구현했는가?
3. 요구사항이 UI/API/DB/code/test 어디에 반영됐는가?
4. 무엇을 실제로 검증했고 무엇은 검증하지 못했는가?
5. 다른 사람이 설치·배포·운영·인수할 수 있는가?

산출물 깊이는 risk/규모에 비례해 조절하고, 작은 프로젝트에서는 여러 산출물을 하나의 파일로 합칠 수 있다. 다만 필요한 정보 자체를 생략해서는 안 된다.

## Canonical deliverable set

### DLV-01 — Requirements and Traceability

**Purpose:** 요구사항, 출처, acceptance criteria와 구현/검증 위치를 연결한다.

Minimum content:
- scope / out-of-scope
- requirement ID 또는 추적 가능한 reference
- source / status (`CONFIRMED`, `INFERRED`, `UNKNOWN`, `CONFLICT`)
- acceptance criteria
- UI / API / DB / code / test / operation mapping where applicable
- implementation status
- verification result (`PASS`, `FAIL`, `NOT RUN`, `BLOCKED`)
- known gap / residual risk

Completion condition:
- 중요한 요구사항이 구현 위치와 검증 evidence까지 추적 가능하다.
- 미구현/미검증 항목이 완료처럼 표시되지 않는다.

### DLV-02 — UI Publishing Build and Screen Guide

**Purpose:** 요구사항을 반영한 실행 가능한 UI와 화면별 동작 계약을 제공한다.

Minimum content:
- target user / scenario
- screen/navigation map
- screen purpose
- input/output
- state: loading / empty / error / disabled / permission-denied where relevant
- validation and interaction behavior
- responsive behavior where required
- API/backend dependency or synthetic/static boundary
- screenshots or runnable publishing artifact when useful

Completion condition:
- 주요 사용자 흐름을 UI에서 확인할 수 있다.
- mock/synthetic state와 실제 구현 상태가 명확히 구분된다.

### DLV-03 — System / Software Design Specification

**Purpose:** 구현 전에 시스템 경계와 중요한 설계 결정을 설명한다.

Minimum content:
- system context / actors
- component responsibilities
- major interfaces / API contracts
- data flow
- trust/security boundary
- state/transaction/concurrency behavior where relevant
- failure / retry / timeout / idempotency behavior
- deployment/runtime topology at an appropriate abstraction level
- major ADR / rationale
- observability and recovery considerations for operationally relevant systems

Completion condition:
- 구현자가 핵심 구조와 failure semantics를 추측하지 않아도 된다.
- 중요한 설계 선택의 이유와 trade-off가 추적 가능하다.

### DLV-04 — Database / Data Specification

**Purpose:** persistent data와 데이터 계약을 구현·운영 가능한 수준으로 명세한다.

Minimum table metadata:
- logical / physical name
- purpose
- owner/domain where useful

Minimum column metadata:
- logical / physical name
- data type / length/precision
- nullability
- default
- PK/FK/unique
- description / business meaning

Additional content where applicable:
- indexes
- constraints
- relationships
- code/reference values
- data classification / sensitive-data note
- retention/deletion rule when actually defined
- migration / compatibility note
- large-data/query/index considerations

Completion condition:
- schema를 재구성하고 주요 데이터 의미/제약을 이해할 수 있다.
- 코드와 실제 schema가 문서와 모순되지 않는다.

### DLV-05 — Implemented Source and Test Code

**Purpose:** 승인된 요구사항/설계를 실행 가능한 형태로 구현하고 검증 가능한 코드를 제공한다.

Includes as applicable:
- application source
- configuration templates excluding secrets
- schema/migration scripts
- tests
- build/packaging files
- CI quality gates

Completion condition:
- build/compile/static checks are reproducible where applicable
- risk-relevant tests have execution evidence
- secret/internal data is not embedded
- source matches documented contracts
- substantive change has mandatory independent review evidence

### DLV-06 — Installation / Build / Deployment Guide

**Purpose:** 새 환경에서 설치·구성·배포·검증·복구할 수 있게 한다.

Minimum content:
- prerequisites and supported versions
- required resources/ports at a non-sensitive abstraction level
- directory/artifact layout
- configuration and secret handling
- DB initialization/migration
- build/package commands
- install/deploy procedure
- start/stop/restart
- health/smoke verification
- upgrade procedure where relevant
- rollback / disable / recovery path
- known deployment limitations

Completion condition:
- 문서만으로 승인된 환경에 재설치/재배포할 수 있거나, 외부 필수 절차를 정확히 참조한다.

### DLV-07 — Operation / Acceptance / Handover Guide and Results

**Purpose:** 운영자가 정상 상태를 판단하고 장애에 대응하며, 인수자가 무엇이 검증됐는지 알 수 있게 한다.

Minimum content:
- service/process start/stop/status
- health indicators
- logs and diagnostic entry points
- monitoring/alerting where applicable
- routine operation
- backup/restore when applicable
- failure/incident first-response procedure
- access/permission operation where applicable
- maintenance / data cleanup where applicable
- acceptance/test result summary
- known issues / NOT RUN / BLOCKED items
- handover checklist / owner boundary

Completion condition:
- 정상/비정상 상태를 구분할 수 있다.
- 검수 결과와 미검증 범위가 명확하다.
- 다음 운영자가 필수 작업을 추측하지 않는다.

## Traceability matrix

권장 최소 형태:

| Requirement | UI | API | DB/Data | Code | Test/Evidence | Result |
| --- | --- | --- | --- | --- | --- | --- |
| FR-001 | SCR-01 | API-01 | TABLE-A | ServiceA | TC-001 | PASS |

모든 요구사항에 모든 열이 필요한 것은 아니다. 관련 없는 항목은 `N/A`로 명시할 수 있다.

## Tailoring

### Small project

다음처럼 통합 가능하다.

```text
requirements-and-traceability.md
ui-and-design.md
database-spec.md
src/
deployment-guide.md
operation-and-acceptance.md
```

### Medium/high-risk project

DLV-01~07을 명확히 분리하고 requirement/design/review/evidence ownership을 분명히 한다.

## Change-to-deliverable mapping

- requirement/scope change -> DLV-01
- UI/interaction change -> DLV-02
- architecture/API/data-flow/security-boundary change -> DLV-03
- schema/query/data-contract change -> DLV-04
- implementation/test change -> DLV-05
- environment/build/deployment change -> DLV-06
- operation/acceptance/handover change -> DLV-07

**변경된 진실만 갱신한다.** 모든 변경에서 모든 문서를 기계적으로 수정하지 않는다.

## Review and acceptance

- deliverable structure itself is subject to the project's review policy.
- generated documentation is not accepted only because an AI produced it.
- sensitive company/client artifacts follow higher data-boundary and retention policy.
- public handbook examples remain independently created and synthetic.
