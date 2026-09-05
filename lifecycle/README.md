# Engineering Lifecycle

- status: `draft index`
- baseline_date: `2026-09-06`

코딩 이전과 이후를 포함한 전체 software delivery lifecycle 기준이다.

## Current baseline

1. [`00-engineering-lifecycle.md`](00-engineering-lifecycle.md) — 전체 lifecycle, tailoring, gate
2. [`01-requirements.md`](01-requirements.md) — source, requirement, acceptance, traceability, readiness
3. [`02-architecture-and-design.md`](02-architecture-and-design.md) — stakeholder/concern, component/interface/data/trust/failure/ADR

구현·테스트·보안·리뷰는 [`../standards/`](../standards/)의 reusable standard를 사용한다.

검수 gate는 [`../checklists/definition-of-ready.md`](../checklists/definition-of-ready.md)와 [`../checklists/definition-of-done.md`](../checklists/definition-of-done.md)를 사용한다.

## Lifecycle

```text
DISCOVER
-> DEFINE
-> REQUIREMENTS
-> DESIGN
-> PLAN
-> IMPLEMENT
-> VERIFY
-> REVIEW
-> VALIDATE
-> RELEASE
-> OPERATE
-> LEARN
```

## Core distinction

- **Verification:** 설계·요구사항대로 제대로 만들었는가?
- **Validation:** 실제 필요한 것을 만든 것이 맞는가?

두 개를 동일한 의미로 사용하지 않는다.

## Future expansion

필요가 확인되면 release/operation/retrospective를 별도 상세 문서로 분리한다. 문서 구조를 먼저 늘리지 않고 실제 반복 규칙이 생길 때 확장한다.
