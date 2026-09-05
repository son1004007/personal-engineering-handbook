# AI-assisted Development Standard

- status: `draft`
- version: `0.4`
- baseline_date: `2026-09-06`
- scope: `AI assistants / coding agents / review agents`
- operating_model: [`../OPERATING_MODEL.md`](../OPERATING_MODEL.md)
- mandatory_review_policy: [`../REVIEW_POLICY.md`](../REVIEW_POLICY.md) `approved v1.1`

## 목적

AI를 생산성 도구로 사용하되, AI 출력이 요구사항·설계·보안·검증의 책임을 대체하지 않도록 한다. 특정 제품명을 영구적인 역할 정의로 고정하지 않고 capability profile을 우선한다.

## AI-001 — AI output은 candidate다 — MUST

AI가 생성한 code, SQL, shell, config, architecture, test, dependency, security advice를 검증 전 사실 또는 승인된 구현으로 취급하지 않는다.

## AI-002 — Source of Truth를 먼저 읽는다 — MUST

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

## AI-004 — 위험한 작업은 evidence threshold를 높인다 — MUST

HIGH-risk 작업은 단일 AI 판단만으로 실행/merge하지 않는다. 프로젝트 policy와 위험에 따라 human approval, dry-run, backup/rollback, independent review, runtime verification을 요구한다.

## AI-005 — Independent review는 mandatory gate다 — MUST

`REVIEW_POLICY.md`가 정의한 substantive engineering change는 완료/merge/release 전에 independent final review가 필요하다.

- **Personal / explicitly AGY-authorized environment:** AGY/Gemini final review MUST
- **MEDIUM/HIGH personal / explicitly AGY-authorized environment:** AGY/Gemini design review MUST + final review MUST
- **Company/client project:** independent review MUST. 외부/개인 AGY는 DEFAULT DENY이며, 명시적으로 승인된 service/repository/data classification 범위에서만 사용한다.
- 회사에서 AGY가 승인되지 않았으면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`를 기록하고 company-approved human/internal-AI reviewer를 사용한다.

## AI-006 — 독립 review는 실제로 독립적이어야 한다 — MUST

첫 review에는 가능한 한 다음을 제공한다.

```text
original requirement / acceptance / constraints
+ exact draft, commit or diff
+ 필요한 architecture/context
+ deterministic verification evidence
```

implementation agent의 결론이나 예상 finding을 정답처럼 먼저 주입하지 않는다.

## AI-007 — AGY/Gemini finding은 자동 정답이 아니다 — MUST

finding은 `ACCEPTED / MODIFIED / REJECTED / DEFERRED`로 reconciliation한다.

단, AGY가 `BLOCKER` 또는 `MAJOR`로 분류한 finding은 구현자가 혼자 기각하거나 하향할 수 없다.

- personal: objective counter-evidence + implementation과 독립된 second reviewer + owner disposition
- company/client: objective counter-evidence + human peer/tech lead/security owner/designated reviewer concurrence + project policy

AGY BLOCKER/MAJOR는 arbitration 전까지 pending blocking item으로 취급한다.

판정 우선순위:

```text
law/contract/company/project policy
> runtime/test/reproduction evidence
> current official docs/standards
> project requirement/architecture evidence
> reputable engineering practice
> AGY/Gemini finding
> implementation agent self-claim
```

## AI-008 — 제품명보다 capability profile을 먼저 정의한다 — SHOULD

- requirements / architecture / reconciliation
- repository implementation / verification
- large-context independent review

2026-09-06 현재 개인 workflow의 구현 예는 ChatGPT, Codex, AGY/Gemini지만 제품명 자체가 품질을 보장하지 않는다.

## AI-009 — 실행 권한을 최소화한다 — MUST

읽기만 필요한 review agent에는 write/shell/admin 권한을 주지 않는다. AGY/Gemini review는 기본적으로 read-only execution boundary를 사용한다.

## AI-010 — AI-generated test도 검토한다 — MUST

테스트 코드가 존재한다는 사실만으로 검증 완료로 판단하지 않는다. assertion, mock, 실제 실행 여부, failure canary 필요성을 확인한다.

## AI-011 — AI가 제안한 dependency/package는 실제 존재와 출처를 확인한다 — MUST before adoption

Maven/npm/PyPI 등 third-party package는 공식 registry/vendor/source의 실제 존재, package owner, 유지보수/지원 상태, license/security risk를 확인한다.

## AI-012 — 외부 사실은 최신 공식 source를 우선한다 — SHOULD

version-dependent CLI/framework/security behavior는 모델 기억보다 현재 공식 documentation, release notes, installed version/runtime evidence를 우선한다.

## AI-013 — 회사/고객 데이터의 외부 AI 전송은 DEFAULT DENY — MUST

회사·고객 source, diff, schema, design, log, API contract, 내부 정보, 개인정보, 기밀정보 또는 파생정보를 개인 Synology AGY나 승인되지 않은 외부 AI에 보내지 않는다.

명시적 organizational/contractual authorization이 확인된 경우에만 승인 범위 안에서 사용한다. 승인 여부가 불명확하면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA`로 처리한다.

## AI-014 — 민감정보를 prompt/context에 불필요하게 넣지 않는다 — MUST

credential, 개인정보, 기밀정보는 승인된 환경에서도 최소 범위로 사용한다.

## AI-015 — 생성 속도보다 변경 규모를 통제한다 — SHOULD

AI가 빠르게 많은 파일을 만들 수 있다는 이유로 큰 변경을 한 번에 만들지 않는다.

## AI-016 — 완료 보고는 evidence 기반이어야 한다 — MUST

다음을 구분한다.

- 작성함
- 정적 검토함
- 테스트를 만들었음
- 테스트를 실행함
- runtime에서 확인함
- independent review를 실행함
- review findings를 arbitration/reconciliation함
- 배포함

## 기본 workflow

```text
1. repository/source discovery
2. requirements + assumptions
3. plan + verification/validation strategy
4. MEDIUM/HIGH: mandatory independent design review
5. reconcile design findings
6. implementation
7. deterministic verification
8. mandatory independent final review
9. arbitrate/reconcile findings
10. resolve pending BLOCKER/MAJOR or obtain policy-compliant risk decision
11. owner/human/project approval when required
12. document evidence and residual risk
```

회사 프로젝트에서는 개인 workflow를 별도 shadow process로 만들지 않고 기존 ticket/PR/CI/security/change-management artifact에 mapping한다.

## Review record

- 2026-09-06: Initial draft.
- 2026-09-06: AGY/Gemini #351 selectively reconciled.
- 2026-09-06: Owner made independent review mandatory.
- 2026-09-06: AGY/Gemini #353 governance findings incorporated into REVIEW_POLICY v1.1: self-arbitration guard, default-deny company data boundary, company tool-failure fallback and shadow-governance constraint.
