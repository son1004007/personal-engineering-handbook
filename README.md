# Personal Engineering Handbook

> Status: **independently reviewed baseline + approved mandatory review policy**  
> Baseline date: **2026-09-06**  
> Mandatory review policy: **approved by owner 2026-09-06**  
> Prior final independent baseline review: **AGY/Gemini READY_FOR_REVIEWED, BLOCKER 0**

개인적으로 사용하는 소프트웨어 엔지니어링 원칙, SDLC 기준, 개발 표준, 검토 체크리스트를 공개 가능한 형태로 정리하는 저장소입니다.

이 저장소의 목적은 단순한 `coding style` 모음이 아닙니다. **개인 프로젝트뿐 아니라 회사·고객 프로젝트에서 내가 수행하는 작업 방식의 기본 골격으로 사용하되, 법률·계약·회사·고객·프로젝트 정책을 항상 우선합니다.**

```text
문제 정의
-> 기획 / scope
-> 요구사항
-> 설계 / architecture
-> 구현
-> verification / testing
-> mandatory independent review
-> finding reconciliation
-> validation / acceptance
-> release
-> operation / retrospective
```

## Mandatory review — approved

가장 먼저 [`REVIEW_POLICY.md`](REVIEW_POLICY.md)를 읽습니다.

현재 owner-approved 기본값:

- substantive code/config/schema/API/infra/security/deployment behavior change는 **AGY/Gemini independent final review MUST**
- MEDIUM/HIGH architecture/security/data/operation decision은 **구현 전 AGY/Gemini design review MUST**
- AGY/Gemini finding은 자동 정답이 아니라 `ACCEPTED / MODIFIED / REJECTED / DEFERRED`로 evidence-based reconciliation MUST
- 회사/고객 policy가 외부 AGY에 정보 제공을 금지하면 data를 반출하지 않고 `AGY_NOT_PERMITTED_BY_POLICY`를 기록한 뒤 프로젝트가 승인한 독립 reviewer로 대체
- break-glass containment는 review를 지연할 수 있지만 없애지는 못함

즉 **문서량은 risk-based, 독립 review는 mandatory**입니다.

## Current baseline

### Operating / review model

- [`REVIEW_POLICY.md`](REVIEW_POLICY.md) — **approved v1.0**, mandatory AGY/Gemini independent review + reconciliation
- [`OPERATING_MODEL.md`](OPERATING_MODEL.md) — BCP14 vocabulary, Solo/Team, LOW/MEDIUM/HIGH risk tier, inference boundary, break-glass, anti-overengineering

### Lifecycle

- [`lifecycle/00-engineering-lifecycle.md`](lifecycle/00-engineering-lifecycle.md) — 전체 SDLC concern map
- [`lifecycle/01-requirements.md`](lifecycle/01-requirements.md) — requirement/source/acceptance/traceability
- [`lifecycle/02-architecture-and-design.md`](lifecycle/02-architecture-and-design.md) — architecture, data, security, operability, dependency, evolution

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

프로젝트 규칙과 handbook이 충돌하면 프로젝트 규칙을 우선합니다.

## Public boundary

이 저장소는 개인적으로 독립 작성한 일반 원칙만 공개합니다.

- 회사 또는 고객사의 source code를 복제하지 않습니다.
- 회사 내부 개발표준, 템플릿, 회의록, 설계서, 운영 절차를 복제하거나 재작성해 공개하지 않습니다.
- 고객사명, 내부 IP/URL, 계정, schema/table 이름, 실제 운영 데이터와 비공개 architecture를 포함하지 않습니다.
- 예시는 synthetic domain / synthetic data로 독립 작성합니다.
- 이 저장소는 현재 또는 과거 고용주, 고객사의 공식 정책이 아닙니다.
- 회사 프로젝트 review를 위해 AGY를 사용할 때도 **회사/고객의 AI 사용·데이터 반출 정책을 우회하지 않습니다.**

자세한 내용은 [`PUBLICATION_POLICY.md`](PUBLICATION_POLICY.md)를 따릅니다.

## Rule status

- `draft`: 초안
- `independently-reviewed draft` / `reviewed`: 독립 검토를 받았지만 owner 최종 승인 전
- `approved`: owner가 개인 기본 engineering rule로 명시적으로 채택
- `deprecated`: 신규 작업에 적용하지 않음
- `superseded`: 새로운 문서가 대체

현재 **mandatory review policy는 approved**, 나머지 2026-09-06 baseline은 독립 검토를 받은 상태에서 계속 세부 검증·승인을 진행합니다.

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

AGY/Gemini review는 **항상 수행하는 독립 검토 gate**지만, finding 내용은 공식 문서와 실제 환경으로 교차검증해 `accepted / modified / rejected / deferred`를 기록합니다.

## References

현재 기준 버전과 공식 링크는 [`references/README.md`](references/README.md)를 따릅니다.

주요 baseline:

- ISO/IEC/IEEE 12207:2026
- ISO/IEC/IEEE 29148:2018
- ISO/IEC/IEEE 42010:2022
- ISO/IEC 25010:2023
- NIST SP 800-218 SSDF v1.1
- OWASP ASVS 5.0.0
- ISTQB CTFL v4.0.1
- RFC 2119 / RFC 8174
- language/framework-specific current official documentation

## Existing source material

초기 작성에는 다음 기존 개인 자산을 참고했지만 그대로 복제하지 않고 public boundary와 최신 근거를 다시 검토했습니다.

- `son1004007/ai-agent-workflow-playbook/WORKFLOW.md`
- `son1004007/engineering-career-portfolio/03_portfolio/code-explanation-standard.md`
- `son1004007/son1004007.github.io`의 공개 기술 글

## Global control

Cross-repository AI 작업은 다음 전역 control을 먼저 따릅니다.

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

이 handbook은 **engineering practice와 mandatory review policy의 Source of Truth**이고, global control은 **AI workflow / repository routing의 Source of Truth**입니다.
