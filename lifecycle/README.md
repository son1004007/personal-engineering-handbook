# Engineering Lifecycle

- Status: **draft**
- Baseline date: **2026-09-06**

이 디렉터리는 코딩 이전과 이후를 포함한 전체 software delivery lifecycle 기준을 정의합니다.

초기 구조:

1. `00-intake-and-scope.md` — 문제 정의, 이해관계자, scope/out-of-scope, source
2. `01-planning.md` — 일정, 위험, 비용, dependency, readiness
3. `02-requirements.md` — functional/security/data/ops/NFR, acceptance, traceability
4. `03-design-and-architecture.md` — context, component, interface, data, trust boundary, failure mode, ADR
5. `04-implementation.md` — code change discipline, error handling, observability, testability
6. `05-verification-and-testing.md` — verification, test strategy, evidence
7. `06-review-and-security.md` — correctness, security, maintainability, independent review
8. `07-validation-and-acceptance.md` — 사용자/업무 목적 충족 여부와 검수
9. `08-release-and-operation.md` — deployment, rollback, health, monitoring
10. `09-retrospective-and-improvement.md` — defect/incident learning, reusable rule extraction

## Core distinction

- **Verification:** 설계·요구사항대로 제대로 만들었는가?
- **Validation:** 실제 필요한 것을 만든 것이 맞는가?

두 개를 동일한 의미로 사용하지 않습니다.

각 상세 문서는 공식 표준과 기존 개인 workflow를 검토한 뒤 `draft -> reviewed -> approved` 순서로 작성합니다.
