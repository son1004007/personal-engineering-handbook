# AI-assisted Development Standard

- status: `draft`
- version: `0.2`
- baseline_date: `2026-09-06`
- scope: `AI assistants / coding agents / review agents`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)

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

## AI-005 — 독립 다중 모델 review는 risk-based로 사용한다

- **HIGH risk: SHOULD** — architecture, security policy, auth/crypto, destructive migration, wide-blast-radius change는 가능하면 implementation context와 독립된 model/agent로 second opinion을 받는다.
- **MEDIUM risk: MAY** — ambiguity나 복잡도가 높을 때 사용한다.
- **LOW risk: MAY / normally unnecessary** — deterministic test와 fresh-eyes review가 더 효율적일 수 있다.

독립 review를 할 때는 가능한 한 같은 source/diff를 각 reviewer에게 먼저 제공하고 첫 reviewer의 결론으로 다음 reviewer를 유도하지 않는다.

## AI-006 — 제품명보다 capability profile을 먼저 정의한다 — SHOULD

기본 capability profile 예:

- **requirements / architecture / reconciliation**: source를 비교하고 요구·설계·trade-off를 구조화
- **repository implementation / verification**: 실제 repo를 읽고 수정·빌드·테스트·diff 검증
- **large-context independent review**: 큰 문서/코드 묶음에서 누락·반례·모순 탐색

2026-09-06 현재 개인 workflow의 구현 예는 ChatGPT, Codex, AGY/Gemini지만 이 제품명이 영구 표준은 아니다. 실제 도구 기능·version·접근권한을 현재 시점에 확인한다.

## AI-007 — 실행 권한을 최소화한다 — MUST

읽기만 필요한 review agent에는 write/shell/admin 권한을 주지 않는다. write가 필요한 경우에도 workspace, repository, command, network, secret 범위를 최소화한다.

## AI-008 — AI-generated test도 검토한다 — MUST

테스트 코드가 존재한다는 사실만으로 검증 완료로 판단하지 않는다.
- assertion이 실제 요구를 확인하는가?
- mock 때문에 결함을 숨기지 않는가?
- 테스트가 실행됐는가?
- 실패 canary가 필요한가?

## AI-009 — AI가 제안한 dependency/package는 실제 존재와 출처를 확인한다 — MUST before adoption

새로운 Maven/npm/PyPI 등 third-party package를 AI가 제안한 경우 다음을 확인한다.
- 공식 registry 또는 vendor/source에 실제 존재하는가?
- package 이름과 group/owner가 맞는가?
- 공식 project/documentation에서 연결되는가?
- 유지보수/지원 상태와 license/security risk가 허용 가능한가?

AI가 만든 그럴듯한 package 이름을 검색 없이 dependency에 추가하지 않는다. 이는 package hallucination과 dependency-squatting 위험을 줄이기 위한 기본 방어다.

## AI-010 — 외부 사실은 최신 공식 source를 우선한다 — SHOULD

version-dependent CLI/framework/security behavior는 모델 기억보다 현재 공식 documentation, release notes, installed version/runtime evidence를 우선한다.

## AI-011 — 민감정보를 prompt/context에 불필요하게 넣지 않는다 — MUST

고객 기밀, credential, 개인정보, 내부망 정보, 비공개 source는 필요한 권한/환경에서 최소 범위로만 사용한다. public agent/repository에 복제하지 않는다.

## AI-012 — 생성 속도보다 변경 규모를 통제한다 — SHOULD

AI가 빠르게 많은 파일을 만들 수 있다는 이유로 큰 변경을 한 번에 만들지 않는다. 변경 목적과 검증 단위를 작게 유지한다.

## AI-013 — 완료 보고는 evidence 기반이어야 한다 — MUST

다음을 구분한다.
- 작성함
- 정적 검토함
- 테스트를 만들었음
- 테스트를 실행함
- runtime에서 확인함
- 배포함

AI는 실행하지 않은 일을 실행했다고 표현하지 않는다.

## 추천 workflow

```text
1. repository/source discovery
2. requirements + assumptions
3. plan + verification/validation strategy
4. implementation
5. deterministic verification
6. risk-appropriate independent review
7. reconcile findings
8. owner/human decision when required
9. document evidence and residual risk
```

## AI Review 기록 예

```text
reviewer: <agent/model/tool>
capability: large-context-independent-review
input_ref: <commit SHA or exact draft>
mode: read-only
focus: correctness, overengineering, missing risks, ambiguity
result: findings with severity and evidence
accepted: ...
modified: ...
rejected: ... + reason
```

## 근거

- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SP 800-218A: https://csrc.nist.gov/pubs/sp/800/218/a/final
- 개인 AI workflow Source of Truth: `son1004007/ai-agent-workflow-playbook`

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #351 review received.
- 2026-09-06: Accepted capability-first wording, risk-tiered multi-model review and package-hallucination defense.
- 2026-09-06: Rejected AGY statement that `Codex` is retired as an applicable reason to remove it; the current personal environment actively uses a repository-capable Codex toolchain. Product names are retained only as dated implementation examples, not normative roles.
