# Engineering Standards

- status: `draft index`
- baseline_date: `2026-09-06`

반복적으로 재사용할 개인 engineering 기본값이다. 프로젝트별 요구·정책이 항상 우선한다.

## Current drafts

- [`quality-model.md`](quality-model.md) — 품질 특성을 요구/검증에 연결하는 기준
- [`implementation.md`](implementation.md) — 언어 중립 구현 원칙
- [`testing.md`](testing.md) — 위험·요구 기반 테스트/verification
- [`security.md`](security.md) — NIST SSDF + OWASP ASVS 기반 secure development
- [`code-review.md`](code-review.md) — 독립 review 및 evidence 기준
- [`ai-assisted-development.md`](ai-assisted-development.md) — ChatGPT/Codex/Gemini 등 AI 활용 개발 기준
- [`java-spring.md`](java-spring.md) — Java/Spring 첫 기술별 기본값

## Next candidates

우선순위에 따라 다음을 확장한다.

- `rest-api.md`
- `sql-database.md`
- `python-fastapi.md`
- `logging-observability.md`
- `configuration-and-secrets.md`
- `dependency-management.md`
- `linux-container.md`
- `ci-cd.md`

## Rule authoring policy

- 개인 취향과 기술적 필수사항을 구분한다.
- `MUST / SHOULD / MAY`를 사용한다.
- rule에는 적용 범위, 이유, 예외, 검증 방법을 가능한 범위에서 포함한다.
- version-sensitive behavior는 현재 프로젝트의 공식 framework/vendor 문서를 다시 확인한다.
- 회사/고객 내부 표준을 이 public handbook의 source로 복제하지 않는다.
- AI 합의만으로 `approved`하지 않는다. [`../GOVERNANCE.md`](../GOVERNANCE.md)를 따른다.
