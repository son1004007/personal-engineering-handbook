# Personal Engineering Handbook

> Status: **bootstrap / draft**  
> Baseline date: **2026-09-06**

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

## Authority

이 저장소의 규칙은 개인 기본값입니다. 다음 우선순위를 절대 넘지 않습니다.

```text
법률 / 계약 / 고객 요구사항
> 회사 정책 및 회사가 승인한 개발 표준
> 현재 프로젝트의 명시적 요구사항과 AGENTS.md
> 이 Personal Engineering Handbook의 approved 규칙
> framework / language 일반 관례
> AI의 자체 판단
```

프로젝트 규칙과 이 handbook이 충돌하면 프로젝트 규칙을 우선합니다.

## Public boundary

이 저장소는 개인적으로 독립 작성한 일반 원칙만 공개합니다.

- 회사 또는 고객사의 source code를 복제하지 않습니다.
- 회사 내부 개발표준, 템플릿, 회의록, 설계서, 운영 절차를 복제하거나 재작성해 공개하지 않습니다.
- 고객사명, 내부 IP/URL, 계정, schema/table 이름, 실제 운영 데이터와 비공개 architecture를 포함하지 않습니다.
- 예시는 synthetic domain / synthetic data로 독립 작성합니다.
- 이 저장소는 현재 또는 과거 고용주, 고객사의 공식 정책이 아닙니다.

자세한 내용은 [`PUBLICATION_POLICY.md`](PUBLICATION_POLICY.md)를 따릅니다.

## Rule status

각 규칙 문서는 다음 상태 중 하나를 명시합니다.

- `draft`: 초안. 다른 프로젝트에 기본 규칙으로 강제하지 않음
- `reviewed`: 독립 검토를 받았지만 개인 최종 승인 전
- `approved`: 개인 기본 engineering rule로 사용 가능
- `deprecated`: 더 이상 신규 작업에 적용하지 않음
- `superseded`: 새로운 문서가 대체함

날짜가 다르면 **더 최신의 approved 문서**를 우선합니다.

## Repository structure

```text
lifecycle/     SDLC 단계별 기준
standards/     개발·설계·테스트·보안·문서 표준
checklists/    단계별 점검/검수 checklist
templates/     공개 가능한 일반 template
references/    공식 표준 및 신뢰 가능한 출처 mapping
```

초기 상세 구조는 각 디렉터리의 README를 따릅니다.

## Review principle

AI의 합의만으로 규칙을 승인하지 않습니다.

```text
실제 test / runtime evidence
> 공식 표준·공식 vendor documentation
> 신뢰 가능한 engineering practice
> 독립 human review
> 복수 AI의 독립 검토
> 단일 AI 의견
```

ChatGPT, Codex, Gemini 등을 사용할 경우 가능하면 같은 초안에 대해 독립적으로 검토하고, 한 AI의 결론을 다른 AI에게 정답처럼 전달하지 않습니다.

최종 `approved` 여부는 저장소 소유자가 결정합니다.

## Existing source material

초기 작성 시 다음 기존 개인 자산을 참고할 수 있습니다. 단, 그대로 복제하지 않고 public boundary와 최신 근거를 다시 검토합니다.

- `son1004007/ai-agent-workflow-playbook/WORKFLOW.md`
- `son1004007/engineering-career-portfolio/03_portfolio/code-explanation-standard.md`
- `son1004007/son1004007.github.io`의 공개 기술 글

## Global control

Cross-repository AI 작업은 다음 전역 control을 먼저 따릅니다.

`son1004007/ai-agent-workflow-playbook/CONTROL.md`

이 handbook은 **engineering practice의 Source of Truth**이고, global control은 **AI workflow / repository routing의 Source of Truth**입니다.
