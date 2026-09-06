# Deliverables and Handover Standard

- status: `independently-reviewed draft`
- version: `0.3`
- baseline_date: `2026-09-06`
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.4.1`
- review_inputs: `AGY/Gemini device-control #359, #360`

## Purpose

프로젝트 산출물은 문서를 많이 만드는 것이 목적이 아니다. 다음 질문에 재현 가능하게 답할 수 있어야 한다.

1. 무엇을 요구받았는가?
2. 무엇을 설계하고 구현했는가?
3. 각 in-scope 요구사항은 어디에 구현됐고 어떤 evidence로 검증됐는가?
4. 무엇을 실제로 검증했고 무엇은 검증하지 못했는가?
5. 다른 사람이 필요한 경우 설치·배포·운영·인수할 수 있는가?

산출물 깊이와 파일 개수는 risk/규모에 비례해 조절한다. **7개 deliverable class는 정보 영역이지 항상 7개 파일을 만들라는 규칙이 아니다.**

## Applicability and tailoring

각 deliverable은 다음 중 하나로 표시한다.

- `APPLICABLE`: 프로젝트/변경에 실제로 필요함
- `N/A BY ARCHITECTURE`: 해당 layer/capability가 존재하지 않아 적용 불가. 한 줄 이상의 근거를 남김
- `MERGED`: 작은 프로젝트에서 다른 living document에 통합. 실제 정보 위치를 가리킴

### 기본 applicability

- **DLV-01 Requirements and Traceability:** substantive software work에는 기본 `APPLICABLE`. 작은 작업은 issue/README/PR 안에 병합 가능.
- **DLV-02 UI Publishing / Screen Guide:** 사용자 UI/interaction이 있을 때 적용. headless service/library/CLI는 `N/A BY ARCHITECTURE` 가능.
- **DLV-03 System / Software Design:** component/interface/data/security/failure decision이 있는 MEDIUM/HIGH 또는 지속 운영 software에 적용. 작은 단일 script/library는 README/코드 계약에 `MERGED` 가능.
- **DLV-04 Database / Data Specification:** persistent data, schema, file/data contract, message/data model이 있을 때 적용. stateless software는 `N/A BY ARCHITECTURE` 가능.
- **DLV-05 Implemented Source and Test Code:** software 구현 작업이면 기본 `APPLICABLE`.
- **DLV-06 Installation / Build / Deployment Guide:** 다른 환경에서 build/install/run/deploy해야 하면 적용. 단순 package/library는 README install section에 `MERGED` 가능.
- **DLV-07 Operation / Acceptance / Handover:** 장기 운영, 인수인계, 고객/사용자 acceptance, 장애 대응이 필요한 경우 적용. 일회성 개인 실험은 README/result record에 `MERGED`하거나 운영 부분을 `N/A BY ARCHITECTURE`로 둘 수 있음.

**적용 가능한 정보를 생략하면 안 되지만, 존재하지 않는 UI/DB/운영 layer를 억지 문서로 만들지 않는다.**

## Canonical deliverable set

### DLV-01 — Requirements and Traceability

**Purpose:** 요구사항, 출처, acceptance criteria와 구현/검증 위치를 연결한다.

Minimum content:
- scope / out-of-scope
- requirement ID 또는 추적 가능한 reference
- source / source-status (`CONFIRMED`, `INFERRED`, `UNKNOWN`, `CONFLICT`)
- acceptance criteria
- implementation target/reference
- verification evidence/reference
- implementation/verification status
- known gap / residual risk
- conditional UI/API/DB/operation mappings only when those layers are involved

권장 lifecycle status:
- `PLANNED`
- `IN_PROGRESS`
- `IMPLEMENTED_NOT_VERIFIED`
- `VERIFIED`
- `BLOCKED`
- `DESCOPED_APPROVED`

Release/acceptance candidate에서는 **정의된 모든 in-scope requirement**가 명시적 상태를 가져야 한다. `VERIFIED`가 아닌 항목은 승인된 descoping/risk decision 또는 release 차단 사유가 추적되어야 한다.

Completion condition:
- 모든 defined in-scope requirement가 implementation reference + evidence/result 또는 approved descoping/blocking decision까지 추적 가능하다.
- 미구현/미검증 항목을 완료처럼 표시하지 않는다.

### DLV-02 — UI Publishing Build and Screen Guide

**Purpose:** 요구사항을 반영한 실행 가능한 UI와 화면별 동작 계약을 제공한다.

Minimum content when applicable:
- target user / scenario
- screen/navigation map
- screen purpose
- input/output/action
- loading / empty / error / disabled / permission-denied state where relevant
- validation / interaction behavior
- responsive behavior where required
- API/backend dependency or synthetic/static boundary
- runnable publishing artifact or equivalent evidence when useful

Completion condition:
- required user flow와 주요 state가 runnable artifact/spec에서 식별 가능하다.
- mock/synthetic state와 실제 구현 상태가 명확히 구분된다.

### DLV-03 — System / Software Design Specification

**Purpose:** 구현에 필요한 시스템 경계와 중요한 설계 결정을 명시한다.

Minimum content when applicable:
- system context / actors
- component responsibility and state ownership
- relevant interface/API/file/message contracts
- relevant data flow
- trust/security boundary
- state/transaction/concurrency behavior where relevant
- failure semantics: timeout/retry/idempotency/partial failure/recovery where relevant
- deployment/runtime topology at appropriate abstraction
- major decision rationale / ADR reference
- observability/recovery expectations for operationally relevant systems

Completion condition:
- relevant component/interface contracts are explicitly recorded.
- relevant failure semantics and recovery responsibility are recorded.
- implementation-changing unresolved architecture questions are either resolved or explicitly tracked as `UNKNOWN/CONFLICT/BLOCKED` with owner/decision path.

### DLV-04 — Database / Data Specification

**Purpose:** persistent data와 데이터 계약을 구현·운영 가능한 수준으로 명세한다.

Minimum table/data-object metadata when applicable:
- logical / physical name or schema/type identifier
- purpose / ownership/domain where useful
- fields/columns with type/null/default/key/constraint/business meaning as relevant
- indexes / constraints / relationships / code values where relevant
- migration / compatibility note
- large-data/query/index considerations where relevant

Data classification declaration — MUST when data is persisted or transferred:
- project/company taxonomy가 있으면 그 taxonomy를 사용
- 없으면 최소 `public/synthetic`, `non-public`, `sensitive/personal`, `UNKNOWN` 중 적절한 상태를 명시
- retention/deletion이 정의되지 않았다면 조용히 생략하지 말고 `N/A`, `UNKNOWN`, 또는 higher-policy reference로 표시

`UNKNOWN`은 discovery/design 중에는 허용하지만, **release candidate에서 실제 persistent/transferred data의 handling, access, retention, external transfer 또는 security control에 영향을 주는 classification이 UNKNOWN이면 이를 해결하거나 해당 requirement/change를 `BLOCKED`/`DESCOPED_APPROVED`로 처리해야 한다.**

Completion condition:
- actual schema/data contract와 문서가 모순되지 않는다.
- 주요 constraint/relationship/classification 상태를 추적할 수 있다.

Migration ownership split:
- DLV-04: target data contract/schema meaning
- DLV-05: executable migration code
- DLV-06: migration execution order, release compatibility, rollback/forward-fix procedure
- DLV-07: operational verification/maintenance result

### DLV-05 — Implemented Source and Test Code

**Purpose:** 승인된 요구사항/설계를 실행 가능한 형태로 구현하고 검증 가능한 code/evidence를 제공한다.

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
- source matches documented contracts
- secret/internal data is not embedded
- build/test evidence is anchored to a commit/tag/build artifact identifier when practical; **HIGH-risk or production release evidence MUST be anchored to an immutable commit SHA, tag or build/artifact identifier**
- substantive change has independent review evidence compliant with `REVIEW_POLICY.md`
- AI review evidence includes severity calibration/reconciliation rather than treating raw AI labels as final authority
- company/client review evidence confirms the reviewer/data path was authorized; personal/external AGY is not used merely to satisfy this deliverable when higher policy prohibits it

### DLV-06 — Installation / Build / Deployment Guide

**Purpose:** 승인된 환경에서 build/install/configure/deploy/verify/recover할 수 있게 한다.

Minimum content when applicable:
- prerequisites and supported/verified versions
- resource/port assumptions at non-sensitive abstraction
- directory/artifact layout
- configuration and secret handling
- DB/data migration execution when applicable
- build/package commands
- install/deploy/start/stop/restart procedure
- health/smoke verification with expected success condition
- upgrade compatibility note where relevant
- rollback / disable / recovery path
- known deployment limitations
- deployment target anchored to commit/tag/build artifact/checksum when practical; **HIGH-risk or production deployment MUST identify the immutable release/build target**

Completion condition:
- 승인된 target/release artifact에 대해 재설치/재배포 절차와 verification 결과를 재현할 수 있거나 정확한 higher-policy procedure를 참조한다.

### DLV-07 — Operation / Acceptance / Handover Guide and Results

**Purpose:** 정상 상태 판단, 장애 대응, acceptance 결과와 인수 경계를 남긴다.

Minimum content when applicable:
- service/process start/stop/status procedure or reference
- health indicators and expected state
- logs/diagnostic entry points
- monitoring/alerting where applicable
- routine operation
- backup/restore where applicable
- incident first-response / rollback-disable-recovery entry point
- access/permission operation where applicable
- maintenance/data cleanup where applicable
- acceptance/test result summary
- `NOT RUN`, `BLOCKED`, known issues, residual risks with release relevance
- handover owner/boundary/open work

Completion condition:
- 정상/비정상 판단 기준과 diagnostic entry point가 명시되어 있다.
- acceptance result와 미검증 범위가 명시되어 있다.
- 운영/인수에 필요한 command/procedure 또는 authoritative reference가 있다.
- **active calibrated BLOCKER가 남아 있는 normal release/handover를 이 문서에 기록했다는 이유만으로 허용하지 않는다.** `REVIEW_POLICY.md`의 release semantics가 항상 우선한다.

`NOT RUN`/verification-`BLOCKED`는 상태를 숨기지 않기 위해 기록할 수 있지만, 해당 미검증이 release eligibility에 미치는 영향은 DoD/review/risk policy로 별도 판정한다.

긴급 incident containment는 normal Done/release 예외가 아니라 `REVIEW_POLICY.md`의 **break-glass temporary deferral**을 따른다. break-glass 상태는 review debt와 due/owner가 닫히기 전 정상 Done/handover로 재분류하지 않는다.

## Traceability model

Universal core chain:

```text
Requirement
-> Implementation reference (code/config/schema/UI/etc.)
-> Test / Evidence
-> Result / Status
```

Conditional layer mappings are added only when touched:

```text
UI | API | DB/Data | Operation | Security | Migration | Deployment
```

Example:

| Requirement | Implementation | Test/Evidence | Result | UI | API | DB/Data |
| --- | --- | --- | --- | --- | --- | --- |
| FR-001 | `ServiceA` | `TC-001` | VERIFIED | SCR-01 | API-01 | N/A |

불필요한 `N/A` 열을 모든 프로젝트에 고정하지 않는다.

## Small-project collapse

LOW-risk personal/small software may use a two-part physical structure:

1. `README.md` or one living document: scope/requirements/basic design/install/run/result/maintenance, plus applicability declarations
2. source + tests + review/evidence

UI/DB/operation deliverable classes that do not exist may be `N/A BY ARCHITECTURE` with rationale.

Medium/high-risk or long-lived company work should separate artifacts where separation improves review/ownership/operations.

## Risk acceptance authority

- personal project with no higher external obligation: the owner may accept a **calibrated MAJOR** according to `REVIEW_POLICY.md`, recording reason/impact/mitigation/revisit trigger.
- company/client project: only a role/process authorized by the company/project may accept risk. This handbook does not grant a developer, AI, delivery team or handbook owner authority to waive company risk.
- calibrated BLOCKER remains non-waivable in normal release.

## Change-to-deliverable mapping

- requirement/scope change -> DLV-01
- UI/interaction change -> DLV-02 when applicable
- architecture/API/data-flow/security-boundary change -> DLV-03
- schema/query/data-contract change -> DLV-04
- implementation/test/migration-code change -> DLV-05
- environment/build/deployment/migration-execution change -> DLV-06
- operation/acceptance/handover change -> DLV-07

**변경된 진실만 갱신한다.** 모든 변경에서 모든 문서를 기계적으로 수정하지 않는다.

## Review and acceptance

- generated documentation is not accepted only because an AI produced it.
- raw AI finding/severity is provisional and follows `REVIEW_POLICY.md` calibration/reconciliation.
- active calibrated BLOCKER prevents normal Done/release/handover regardless of how clearly it is documented.
- company/client artifacts and review inputs follow higher data-boundary/retention/reviewer-authorization policy.
- public handbook examples remain independently created and synthetic.
