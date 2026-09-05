# AI-assisted Development Standard

- status: `draft`
- version: `0.3`
- baseline_date: `2026-09-06`
- scope: `AI assistants / coding agents / review agents`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.0`

## 목적

AI를 생산성 도구로 사용하되, AI 출력이 요구사항·설계·보안·검증의 책임을 대체하지 않도록 한다. 특정 제품명을 영구적인 역할 정의로 고정하지 않고 **capability profile**을 우선한다.

## AI-001 — AI output은 candidate다 — MUST

AI가 생성한 code, SQL, shell, config, architecture, test, dependency, security advice를 검증 전 사실 또는 승인된 구현으로 취급하지 않는다.

## AI-002 — Source of Truth를 먼저 읽는다 — MUST

기존 프로젝트에서는 가능한 경우 다음 순서를 따른다.

```text
latest user instruction
-> law/contract/company/project policy
-> project requirements / AGENTS / current state
-> approved personal engineering rules
-> code/runtime evidence
-> current official docs when behavior is uncertain
-> AI proposal
```

과거 대화 기억보다 repository와 현재 실행 증거를 우선한다.

## AI-003 — AI가 requirement를 발명하지 않는다 — MUST

부족한 요구를 근거에서 추론할 수는 있지만 `INFERRED` 또는 assumption으로 표시한다. business policy, authorization entitlement, retention, SLA/SLO 수치, destructive migration semantics 등을 근거 없이 확정하지 않는다.

상세 inference 경계는 [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)를 따른다.

## AI-004 — 위험한 작업은 evidence threshold를 높인다 — MUST

다음과 같은 HIGH-risk 작업은 단일 AI 판단만으로 실행/merge하지 않는다.
- destructive DB migration
- production delete/write with wide blast radius
- privilege/security relaxation
- credential/secret handling
- authentication/authorization model change
- public exposure/network policy change
- irreversible external side effect

프로젝트 policy와 위험에 따라 human approval, dry-run, backup/rollback, independent review, runtime verification을 요구한다.

## AI-005 — AGY/Gemini independent review는 mandatory gate다 — MUST

[`../REVIEW_POLICY.md`](../REVIEW_POLICY.md)에 정의된 substantive engineering change는 완료/merge/release 판정 전에 **AGY/Gemini independent final review를 반드시 수행한다.**

또한 MEDIUM/HIGH의 architecture/security/data/operation 결정은 구현 전에 별도 AGY/Gemini design review를 수행한다.

risk tier는 리뷰 여부가 아니라 리뷰 깊이를 조절한다.

- **LOW substantive change:** final diff/change review MUST
- **MEDIUM:** design review MUST + final diff/change review MUST
- **HIGH:** focused design/security/risk review MUST + final change review MUST + 조직이 요구하는 human/SoD review

non-substantive typo/format/link-only 변경만 `NOT_APPLICABLE`로 생략할 수 있다.

회사/고객 정책이 외부 AGY/Gemini에 source/설계 제공을 금지하면 이를 우회하지 않는다. `AGY_NOT_PERMITTED_BY_POLICY`를 기록하고 프로젝트가 승인한 independent reviewer를 사용한다.

## AI-006 — 독립 review는 실제로 독립적이어야 한다 — MUST

첫 AGY/Gemini review에는 가능한 한 다음을 제공한다.

```text
original requirement / acceptance / constraints
+ exact draft, commit or diff
+ 필요한 architecture/context
+ deterministic verification evidence
```

implementation agent의 결론이나 예상 finding을 정답처럼 먼저 주입하지 않는다. 첫 independent review가 끝난 뒤 reconciliation 단계에서 비교한다.

## AI-007 — AGY/Gemini finding은 자동 정답이 아니다 — MUST

모든 finding은 다음 중 하나로 판정하고 근거를 기록한다.

- `ACCEPTED`
- `MODIFIED`
- `REJECTED`
- `DEFERRED`

우선순위는 다음과 같다.

```text
law/contract/company/project policy
> runtime/test/reproduction evidence
> current official docs/standards
> project requirement/architecture evidence
> reputable engineering practice
> AGY/Gemini finding
> implementation agent self-claim
```

AGY가 `BLOCKER`라고 적었다는 이유만으로 자동 blocker로 확정하지 않는다. 반대로 기각할 때도 공식 문서·실행 증거·프로젝트 요구 등 이유를 남긴다.

## AI-008 — 제품명보다 capability profile을 먼저 정의한다 — SHOULD

기본 capability profile 예:

- **requirements / architecture / reconciliation**: source를 비교하고 요구·설계·trade-off를 구조화
- **repository implementation / verification**: 실제 repo를 읽고 수정·빌드·테스트·diff 검증
- **large-context independent review**: 큰 문서/코드 묶음에서 누락·반례·모순 탐색

2026-09-06 현재 개인 workflow의 구현 예는 ChatGPT, Codex, AGY/Gemini지만 이 제품명이 영구 표준은 아니다. 현재 owner policy는 large-context independent review 구현으로 AGY/Gemini를 필수 사용한다. 실제 도구 기능·version·접근권한을 현재 시점에 확인한다.

## AI-009 — 실행 권한을 최소화한다 — MUST

읽기만 필요한 review agent에는 write/shell/admin 권한을 주지 않는다. write가 필요한 경우에도 workspace, repository, command, network, secret 범위를 최소화한다.

AGY/Gemini review는 기본적으로 read-only execution boundary를 사용한다.

## AI-010 — AI-generated test도 검토한다 — MUST

테스트 코드가 존재한다는 사실만으로 검증 완료로 판단하지 않는다.
- assertion이 실제 요구를 확인하는가?
- mock 때문에 결함을 숨기지 않는가?
- 테스트가 실행됐는가?
- 실패 canary가 필요한가?

## AI-011 — AI가 제안한 dependency/package는 실제 존재와 출처를 확인한다 — MUST before adoption

새로운 Maven/npm/PyPI 등 third-party package를 AI가 제안한 경우 다음을 확인한다.
- 공식 registry 또는 vendor/source에 실제 존재하는가?
- package 이름과 group/owner가 맞는가?
- 공식 project/documentation에서 연결되는가?
- 유지보수/지원 상태와 license/security risk가 허용 가능한가?

AI가 만든 그럴듯한 package 이름을 검색 없이 dependency에 추가하지 않는다.

## AI-012 — 외부 사실은 최신 공식 source를 우선한다 — SHOULD

version-dependent CLI/framework/security behavior는 모델 기억보다 현재 공식 documentation, release notes, installed version/runtime evidence를 우선한다.

## AI-013 — 민감정보를 prompt/context에 불필요하게 넣지 않는다 — MUST

고객 기밀, credential, 개인정보, 내부망 정보, 비공개 source는 필요한 권한/환경에서 최소 범위로만 사용한다. public agent/repository에 복제하지 않는다.

외부 AI 사용 여부 자체가 회사/고객 정책의 적용 대상이면 해당 정책을 먼저 확인한다.

## AI-014 — 생성 속도보다 변경 규모를 통제한다 — SHOULD

AI가 빠르게 많은 파일을 만들 수 있다는 이유로 큰 변경을 한 번에 만들지 않는다. 변경 목적과 검증 단위를 작게 유지한다.

## AI-015 — 완료 보고는 evidence 기반이어야 한다 — MUST

다음을 구분한다.
- 작성함
- 정적 검토함
- 테스트를 만들었음
- 테스트를 실행함
- runtime에서 확인함
- AGY/Gemini review를 실행함
- AGY finding을 reconciliation함
- 배포함

AI는 실행하지 않은 일을 실행했다고 표현하지 않는다.

## 기본 workflow

```text
1. repository/source discovery
2. requirements + assumptions
3. plan + verification/validation strategy
4. MEDIUM/HIGH: mandatory AGY/Gemini design review
5. reconcile design findings
6. implementation
7. deterministic verification
8. mandatory AGY/Gemini final independent review
9. reconcile findings against evidence
10. confirmed BLOCKER/MAJOR resolution or explicit policy-compliant risk decision
11. owner/human/project approval when required
12. document evidence and residual risk
```

## AI Review 기록 예

```text
reviewer: AGY/Gemini
capability: large-context-independent-review
input_ref: <commit SHA or exact draft>
mode: read-only independent
result: FINDINGS
reconciliation:
  accepted: ...
  modified: ...
  rejected: ... + evidence
  deferred: ...
```

## 근거

- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SP 800-218A: https://csrc.nist.gov/pubs/sp/800/218/a/final
- 개인 AI workflow Source of Truth: `son1004007/ai-agent-workflow-playbook`
- Mandatory owner review decision: `../REVIEW_POLICY.md`

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #351 review received and selectively reconciled.
- 2026-09-06: Owner decision changed independent AGY/Gemini review from risk-optional to mandatory for substantive engineering work. Findings remain evidence inputs requiring reconciliation, not automatic truth.
