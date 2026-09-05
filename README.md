# Personal Engineering Handbook

> Status: **independently reviewed baseline + approved mandatory review policy v1.2**  
> Baseline date: **2026-09-06**

개인적으로 사용하는 소프트웨어 엔지니어링 원칙, SDLC 기준, 개발 표준, 검토 체크리스트를 공개 가능한 형태로 정리하는 저장소입니다.

이 저장소는 단순 coding style 모음이 아니라 **개인 프로젝트와 회사·고객 프로젝트에서 내가 수행하는 작업 방식의 기본 골격**입니다. 단, 법률·계약·회사·고객·프로젝트 정책이 항상 우선합니다.

```text
문제 정의
-> 기획 / scope
-> 요구사항
-> 설계 / architecture
-> 구현
-> verification / testing
-> mandatory independent review
-> arbitration / finding reconciliation
-> validation / acceptance
-> release
-> operation / retrospective
```

## Mandatory review — approved v1.2

가장 먼저 [`REVIEW_POLICY.md`](REVIEW_POLICY.md)를 읽습니다.

### Personal / explicitly AGY-authorized

- substantive change: **AGY/Gemini independent final review MUST**
- MEDIUM/HIGH design: **AGY/Gemini design review MUST**

### Company/client

- independent review: **MUST**
- external/personal AGY: **DEFAULT DENY**
- 명시적 service/repository/data-classification authorization이 확인된 경우에만 AGY 사용
- 미확인/미승인: `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA` + company-approved human/internal-AI reviewer
- current personal Synology AGY는 company data에 승인된 것으로 가정하지 않음

### Findings are rebuttable, not authoritative

AGY/Gemini finding은 자동 정답이 아닙니다.

- `BLOCKER -> PENDING_BLOCKER`
- `MAJOR -> PENDING_MAJOR`

직접 반증 가능한 finding은 deterministic reproduction/test, exact-version official docs, direct runtime evidence 또는 explicit contract가 핵심 전제를 직접 반증하면 evidence 기반으로 기각/하향할 수 있습니다.

반대로 architecture/security/risk/requirement ambiguity처럼 해석이 필요한 BLOCKER/MAJOR는 구현자가 혼자 무효화할 수 없습니다.

- personal: independent second semantic reviewer 필요
- company: human peer/tech lead/security/designated reviewer concurrence + project policy 필요

즉 **AGY 의견을 무조건 수용하지 않지만, 불편하다는 이유로 임의 기각할 수도 없습니다.**

## Current baseline

### Operating / review model

- [`REVIEW_POLICY.md`](REVIEW_POLICY.md) — **approved v1.2**
- [`OPERATING_MODEL.md`](OPERATING_MODEL.md)

### Lifecycle

- [`lifecycle/00-engineering-lifecycle.md`](lifecycle/00-engineering-lifecycle.md)
- [`lifecycle/01-requirements.md`](lifecycle/01-requirements.md)
- [`lifecycle/02-architecture-and-design.md`](lifecycle/02-architecture-and-design.md)

### Engineering standards

- [`standards/quality-model.md`](standards/quality-model.md)
- [`standards/implementation.md`](standards/implementation.md)
- [`standards/testing.md`](standards/testing.md)
- [`standards/security.md`](standards/security.md)
- [`standards/code-review.md`](standards/code-review.md)
- [`standards/ai-assisted-development.md`](standards/ai-assisted-development.md)
- [`standards/java-spring.md`](standards/java-spring.md)

### Gates

- [`checklists/definition-of-ready.md`](checklists/definition-of-ready.md)
- [`checklists/definition-of-done.md`](checklists/definition-of-done.md)

## Company-data boundary

회사·고객 source/diff/schema/design/log/internal context/민감정보 또는 이를 재구성할 수 있는 derived context를 개인 Synology AGY, 개인 외부 AI, public GitHub로 보내는 것은 **명시적 authorization 전에는 금지**합니다.

독립적으로 작성 가능한 일반 기술 질문은 가능할 수 있지만 company-specific non-public detail을 포함하거나 역추론할 수 있으면 외부로 보내지 않습니다.

## Company adoption — no shadow governance

회사 프로젝트에서는 handbook을 별도 병렬 process로 강제하지 않습니다.

```text
requirements/design -> existing ticket/design doc
verification -> existing CI/test
independent review -> PR reviewer/security review/approved internal AI
reconciliation -> PR discussion/review record
release -> existing change management
```

팀이 formal adoption하지 않았다면 **내 작업 품질을 높이는 개인 discipline**으로 사용합니다.

## Break-glass

active incident에서는 containment를 지연시키지 않기 위해 pre-design review와 final review를 모두 defer할 수 있습니다. 대신 `review_scope`, `review_owner`, `review_due`를 기록하고 post-release review/reconciliation을 반드시 완료합니다.

## Authority

```text
latest explicit user instruction
> law / contract / client requirement
> employer/project policy
> approved handbook rules
> reviewed/draft guidance
> framework convention
> AI preference
```

## Public boundary

회사/고객 source code, 내부 표준·템플릿·설계·운영자료, 내부 IP/URL/account/schema, 운영데이터, proprietary rule을 이 public repo에 복제하지 않습니다. 예시는 independently-created synthetic content를 사용합니다.

## Review evidence

2026-09-06에 AGY/Gemini 독립 governance review를 반복 수행했습니다.

- `#353`: v1.0 rejected — self-arbitration, company default-deny, tool SPOF, shadow governance 문제 발견
- `#354`: v1.1 NOT_READY — solo arbitration deadlock, break-glass pre-design review ambiguity 및 기타 보완점 발견
- 해당 finding은 무조건 수용하지 않고 evidence/applicability 기준으로 `ACCEPTED / MODIFIED / REJECTED`하여 v1.2에 반영

최신 최종 review 결과는 `reviews/`에 기록합니다.

## Global control

Cross-repository AI 작업은:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

에서 시작합니다.
