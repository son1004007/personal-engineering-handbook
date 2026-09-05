# Personal Engineering Handbook

> Status: **independently reviewed baseline + approved mandatory review policy v1.1**  
> Baseline date: **2026-09-06**  
> Mandatory review policy: **approved by owner 2026-09-06**

개인적으로 사용하는 소프트웨어 엔지니어링 원칙, SDLC 기준, 개발 표준, 검토 체크리스트를 공개 가능한 형태로 정리하는 저장소입니다.

이 저장소의 목적은 단순한 coding style 모음이 아닙니다. **개인 프로젝트뿐 아니라 회사·고객 프로젝트에서 내가 수행하는 작업 방식의 기본 골격으로 사용하되, 법률·계약·회사·고객·프로젝트 정책을 항상 우선합니다.**

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

## Mandatory review — approved v1.1

가장 먼저 [`REVIEW_POLICY.md`](REVIEW_POLICY.md)를 읽습니다.

현재 owner-approved 기본값:

- **Personal / explicitly AGY-authorized environment**
  - substantive change: AGY/Gemini independent final review MUST
  - MEDIUM/HIGH design: AGY/Gemini independent design review MUST
- **Company/client environment**
  - independent review MUST
  - external/personal AGY is **DEFAULT DENY** until explicit authorization for service/repository/data classification is confirmed
  - authorization이 없거나 불명확하면 `AGY_NOT_AUTHORIZED_FOR_PROJECT_DATA` + company-approved human/internal-AI reviewer
- AGY/Gemini finding은 자동 정답이 아니라 evidence-based reconciliation 대상
- AGY BLOCKER/MAJOR는 구현자가 혼자 기각/하향할 수 없음
  - personal: objective evidence + second independent reviewer + owner disposition
  - company: objective evidence + human peer/tech lead/security owner/designated reviewer concurrence
- break-glass는 review를 defer할 수 있지만 `review_owner`, `review_due`, post-release review/remediation을 반드시 남김

즉 **문서량은 risk-based, independent review는 mandatory, 회사 데이터는 default-deny, 고위험 finding은 독립 arbitration**입니다.

## Current baseline

### Operating / review model

- [`REVIEW_POLICY.md`](REVIEW_POLICY.md) — **approved v1.1**, mandatory independent review, arbitration, company default-deny boundary
- [`OPERATING_MODEL.md`](OPERATING_MODEL.md) — BCP14 vocabulary, Solo/Team, LOW/MEDIUM/HIGH risk tier, inference boundary, break-glass, anti-overengineering

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

### Review evidence

- [`reviews/2026-09-06-initial-agy-gemini-review.md`](reviews/2026-09-06-initial-agy-gemini-review.md)
- [`reviews/2026-09-06-final-agy-gemini-review.md`](reviews/2026-09-06-final-agy-gemini-review.md)
- policy governance review: device-control Issue `#353`, run `33996776013`

## Authority

```text
사용자의 현재 명시적 지시
> 법률 / 계약 / 고객 요구사항
> 회사 정책 및 회사가 승인한 개발 표준
> 현재 프로젝트의 명시적 요구사항과 AGENTS.md
> 이 Personal Engineering Handbook의 approved 규칙
> reviewed/draft handbook guidance
> framework / language 일반 관례
> AI의 자체 판단
```

## Public / company-data boundary

이 저장소는 개인적으로 독립 작성한 일반 원칙만 공개합니다.

- 회사 또는 고객사의 source code를 복제하지 않습니다.
- 회사 내부 개발표준, 템플릿, 회의록, 설계서, 운영 절차를 복제하거나 재작성해 공개하지 않습니다.
- 고객사명, 내부 IP/URL, 계정, schema/table 이름, 실제 운영 데이터와 비공개 architecture를 포함하지 않습니다.
- 예시는 synthetic domain / synthetic data로 독립 작성합니다.
- 이 저장소는 현재 또는 과거 고용주, 고객사의 공식 정책이 아닙니다.

회사 프로젝트 내용을 개인 Synology AGY, 개인 외부 AI 또는 public GitHub로 보내는 것은 **명시적 organizational/contractual authorization이 확인되기 전에는 금지**합니다. 회사가 승인한 내부/enterprise AI가 있다면 그 승인 범위 안에서 사용합니다.

## Rule status

- `draft`: 초안
- `independently-reviewed draft` / `reviewed`: 독립 검토를 받았지만 owner 최종 승인 전
- `approved`: owner가 개인 기본 engineering rule로 명시적으로 채택
- `deprecated`: 신규 작업에 적용하지 않음
- `superseded`: 새로운 문서가 대체

현재 mandatory review policy는 approved v1.1이고, 나머지 baseline은 세부 검증·승인을 계속 진행합니다.

## Review principle

```text
법률/계약/회사·프로젝트 정책
> 실제 test / runtime evidence
> current official standard / official vendor documentation
> 프로젝트 요구/architecture evidence
> 신뢰 가능한 engineering practice
> AGY/Gemini 및 기타 독립 review
> implementation agent의 자체 주장
```

AGY/Gemini review는 독립 검토 도구이며 절대 권위가 아닙니다. 그러나 BLOCKER/MAJOR를 구현자 혼자 무시할 수도 없습니다.

## Company adoption

회사 프로젝트에서는 이 handbook을 **별도 shadow governance로 만들지 않습니다.** 가능한 한 기존 ticket, design doc, PR, CI, security review, change-management에 이 기준을 mapping합니다. 팀이 formal adoption하지 않았다면 내 작업 품질을 높이는 개인 discipline으로 사용합니다.

## References

현재 기준 버전과 공식 링크는 [`references/README.md`](references/README.md)를 따릅니다.

## Global control

Cross-repository AI 작업은 다음 전역 control을 먼저 따릅니다.

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

이 handbook은 engineering practice와 mandatory review policy의 Source of Truth이고, global control은 AI workflow / repository routing의 Source of Truth입니다.
