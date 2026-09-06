# Engineering Lifecycle

- status: `draft index`
- baseline_date: `2026-09-06`

코딩 이전과 이후를 포함한 전체 software delivery lifecycle 기준이다.

## Current baseline

1. [`00-engineering-lifecycle.md`](00-engineering-lifecycle.md) — 전체 lifecycle, tailoring, gate
2. [`01-requirements.md`](01-requirements.md) — source, requirement, acceptance, traceability, readiness
3. [`02-architecture-and-design.md`](02-architecture-and-design.md) — stakeholder/concern, component/interface/data/trust/failure/ADR
4. [`03-deliverables-and-handover.md`](03-deliverables-and-handover.md) — DLV-01~07 산출물, 최소 필수항목, 완료조건, 인수 기준

구현·테스트·보안·리뷰는 [`../standards/`](../standards/)의 reusable standard를 사용한다.

검수 gate는 [`../checklists/definition-of-ready.md`](../checklists/definition-of-ready.md)와 [`../checklists/definition-of-done.md`](../checklists/definition-of-done.md)를 사용한다.

재사용 가능한 산출물 골격은 [`../templates/`](../templates/)를 사용한다.

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
-> DELIVER / HANDOVER
-> RELEASE
-> OPERATE
-> LEARN
```

## Core distinction

- **Verification:** 설계·요구사항대로 제대로 만들었는가?
- **Validation:** 실제 필요한 것을 만든 것이 맞는가?
- **Delivery/Handover:** 다른 사람이 구현·설치·운영·검수 상태를 재현하고 인수할 수 있는가?

세 개를 동일한 의미로 사용하지 않는다.

## Deliverable principle

산출물은 문서 수를 늘리기 위한 것이 아니다. 요구사항, UI/설계, DB/data, 구현 code, 검증 evidence, 설치/배포, 운영/인수 사이의 변경된 진실을 재현 가능하게 유지한다.

작은 프로젝트에서는 여러 deliverable을 합칠 수 있지만 필요한 정보와 검증 상태를 숨기지 않는다.
