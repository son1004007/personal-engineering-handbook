# Personal Engineering Handbook

> Status: **independently reviewed draft / owner approval pending**  
> Baseline date: **2026-09-06**  
> Final independent review: **AGY/Gemini READY_FOR_REVIEWED, BLOCKER 0**

개인적으로 사용하는 소프트웨어 엔지니어링 원칙, SDLC 기준, 개발 표준, 검토 체크리스트를 공개 가능한 형태로 정리하는 저장소입니다.

이 저장소의 목적은 단순한 `coding style` 모음이 아닙니다.

```text
문제 정의
-> 기획 / scope
-> 요구사항
-> 설계 / architecture
-> 구현
-> verification / testing
-> review / security review
-> validation / acceptance
-> release
-> operation / retrospective
```

실제 적용 시에는 [`OPERATING_MODEL.md`](OPERATING_MODEL.md)의 **Solo/Team mode + LOW/MEDIUM/HIGH risk tier**를 먼저 사용합니다. 모든 변경에 동일한 문서·리뷰·테스트를 강제하지 않습니다.

## Current baseline

### Operating model

- [`OPERATING_MODEL.md`](OPERATING_MODEL.md) — BCP14 vocabulary, Solo/Team, risk tier, inference boundary, break-glass, anti-overengineering

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

이 저장소의 규칙은 개인 기본값 후보입니다. 다음 우선순위를 절대 넘지 않습니다.

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

자세한 내용은 [`PUBLICATION_POLICY.md`](PUBLICATION_POLICY.md)를 따릅니다.

## Rule status

각 규칙은 다음 상태를 사용합니다.

- `draft`: 초안. 다른 프로젝트에 기본 규칙으로 강제하지 않음
- `independently-reviewed draft` / `reviewed`: 독립 검토를 받았지만 owner 최종 승인 전
- `approved`: 개인 기본 engineering rule로 사용 가능
- `deprecated`: 더 이상 신규 작업에 적용하지 않음
- `superseded`: 새로운 문서가 대체함

**2026-09-06 baseline은 AGY/Gemini 독립 리뷰에서 `READY_FOR_REVIEWED` 판정을 받았지만 아직 owner-approved로 승격하지 않았습니다.**

## Review principle

AI의 합의만으로 규칙을 승인하지 않습니다.

```text
법률/계약/프로젝트 정책
> 실제 test / runtime evidence
> current official standard / official vendor documentation
> 신뢰 가능한 engineering practice
> 독립 human review
> 복수 AI의 독립 검토
> 단일 AI 의견
```

AI finding은 공식 문서와 실제 환경으로 교차검증하고 `accepted / modified / rejected`를 기록합니다.

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

이 handbook은 **engineering practice의 Source of Truth 후보/approved baseline**이고, global control은 **AI workflow / repository routing의 Source of Truth**입니다.
