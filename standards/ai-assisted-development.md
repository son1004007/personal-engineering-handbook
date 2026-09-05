# AI-assisted Development Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- scope: `ChatGPT / Codex / Gemini / coding agents`

## 목적

AI를 생산성 도구로 사용하되, AI 출력이 요구사항·설계·보안·검증의 책임을 대체하지 않도록 한다.

## AI-001 — AI output은 candidate다 — MUST

AI가 생성한 code, SQL, shell, config, architecture, test, security advice를 검증 전 사실 또는 승인된 구현으로 취급하지 않는다.

## AI-002 — Source of Truth를 먼저 읽는다 — MUST

기존 프로젝트에서는 가능한 경우 다음 순서를 따른다.

```text
latest user instruction
-> project requirements / AGENTS / current state
-> approved personal engineering rules
-> code/runtime evidence
-> official docs when behavior is uncertain
-> AI proposal
```

과거 대화 기억보다 repository와 현재 실행 증거를 우선한다.

## AI-003 — AI가 requirement를 발명하지 않는다 — MUST

부족한 요구를 합리적으로 추론할 수는 있지만 `INFERRED` 또는 assumption으로 표시한다. 숫자 성능 목표, 보존기간, 권한, 법적 의무 등을 근거 없이 만들어 확정하지 않는다.

## AI-004 — 위험한 작업은 evidence threshold를 높인다 — MUST

다음은 단일 AI 판단만으로 실행/merge하지 않는다.

- destructive DB migration
- production delete/write
- privilege/security relaxation
- credential/secret handling
- authentication/authorization change
- public exposure/network policy change
- irreversible external side effect

프로젝트 정책에 따라 human approval, dry-run, backup/rollback, independent review 또는 runtime verification을 요구한다.

## AI-005 — 독립 리뷰는 독립적으로 수행한다 — SHOULD

복수 AI를 사용할 때 가능하면 동일한 source/diff를 각 모델에 제공한다. 첫 모델의 결론을 두 번째 모델 프롬프트에 정답처럼 넣지 않는다.

이후 orchestration 단계에서 finding을 비교한다.

## AI-006 — 모델별 강점을 역할로 사용하되 권위를 부여하지 않는다 — SHOULD

예시 기본값:

- ChatGPT: 요구/설계/전체 구조, source reconciliation
- Codex: repository-aware implementation/testing, diff-oriented inspection
- Gemini/AGY: large-context independent review, 반례/누락 탐색

실제 성능과 도구 기능이 바뀔 수 있으므로 제품명 자체를 품질 보장으로 사용하지 않는다.

## AI-007 — 실행 권한을 최소화한다 — MUST

읽기만 필요한 review agent에는 write/shell/admin 권한을 주지 않는다. write가 필요한 경우에도 workspace, repository, command, secret 범위를 최소화한다.

## AI-008 — AI-generated test도 검토한다 — MUST

테스트 코드가 존재한다는 사실만으로 검증 완료로 판단하지 않는다.

- assertion이 실제 요구를 확인하는가?
- mock 때문에 결함을 숨기지 않는가?
- 테스트가 실행됐는가?
- 실패하도록 만들어 canary 검증할 필요가 있는가?

## AI-009 — 외부 사실은 최신 공식 source를 우선한다 — SHOULD

version-dependent CLI/framework/security behavior는 모델 기억보다 현재 공식 documentation, release notes, installed version/runtime evidence를 우선한다.

## AI-010 — 민감정보를 prompt/context에 불필요하게 넣지 않는다 — MUST

고객 기밀, credential, 개인정보, 내부망 정보, 비공개 source는 필요한 권한/환경에서 최소 범위로만 사용한다. public agent/repository에 복제하지 않는다.

## AI-011 — 생성 속도보다 변경 규모를 통제한다 — SHOULD

AI가 빠르게 많은 파일을 만들 수 있다는 이유로 큰 변경을 한 번에 만들지 않는다. 변경 목적과 검증 단위를 작게 유지한다.

## AI-012 — 완료 보고는 evidence 기반이어야 한다 — MUST

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
3. plan + validation strategy
4. implementation
5. deterministic verification
6. independent review
7. reconcile findings
8. owner/human decision when required
9. document evidence and residual risk
```

## AI Review 기록 예

```text
reviewer: AGY / Gemini
input_ref: <commit SHA>
mode: read-only independent review
focus: correctness, overengineering, missing risks, ambiguity
result: findings with severity and file/section evidence
accepted: ...
rejected: ... + reason
```

## 근거

- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SP 800-218A (AI model development community profile, informative for AI-specific secure development): https://csrc.nist.gov/pubs/sp/800/218/a/final
- 개인 AI workflow Source of Truth: `son1004007/ai-agent-workflow-playbook`

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
