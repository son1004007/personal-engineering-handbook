# Personal Engineering Handbook

> Status: **independently reviewed baseline + approved mandatory review policy v1.4.1**  
> Baseline date: **2026-09-06**

개인적으로 사용하는 소프트웨어 엔지니어링 원칙, SDLC 기준, 개발 표준, 산출물, 검토 체크리스트와 재사용 템플릿을 공개 가능한 형태로 관리한다.

이 저장소는 coding style 모음이 아니라 **기획 → 요구사항 → 설계 → 구현 → 검증 → 독립 리뷰 → 검수 → 산출/인수 → 배포 → 운영**의 기본 작업 방식이다. 법률·계약·회사·고객·프로젝트 정책이 항상 우선한다.

## Mandatory review

가장 먼저 [`REVIEW_POLICY.md`](REVIEW_POLICY.md) **approved v1.4.1**을 읽는다.

- personal / explicitly AGY-authorized: substantive final change는 AGY/Gemini independent review MUST
- MEDIUM/HIGH design: pre-implementation independent design review MUST
- company/client: independent review MUST, external/personal AGY는 explicit authorization 전까지 DEFAULT DENY
- raw AI finding/severity는 provisional이며 objective severity calibration + evidence/arbitration을 거친다
- calibrated BLOCKER는 normal release에서 waiver 불가
- calibrated MAJOR는 authorized risk acceptance 가능
- review 없이 test PASS만으로 Done 처리하지 않는다

## Engineering flow

```text
Problem / Scope
-> Requirements + Traceability
-> Design
-> Implementation
-> Verification
-> Independent Review
-> Severity Calibration / Reconciliation
-> Validation / Acceptance
-> Deliverables / Handover
-> Release / Operation
```

## Canonical deliverables

[`lifecycle/03-deliverables-and-handover.md`](lifecycle/03-deliverables-and-handover.md)에서 다음 7개 deliverable class를 정의한다.

1. **DLV-01 Requirements and Traceability**
2. **DLV-02 UI Publishing Build and Screen Guide**
3. **DLV-03 System / Software Design Specification**
4. **DLV-04 Database / Data Specification**
5. **DLV-05 Implemented Source and Test Code**
6. **DLV-06 Installation / Build / Deployment Guide**
7. **DLV-07 Operation / Acceptance / Handover Guide and Results**

작은 프로젝트에서는 파일을 합칠 수 있다. 중요한 것은 문서 개수가 아니라 **요구사항이 어디에 구현됐고 무엇으로 검증됐으며 다른 사람이 설치·운영·인수할 수 있는가**이다.

Reusable skeletons: [`templates/`](templates/)

## Current baseline

### Operating / review
- [`REVIEW_POLICY.md`](REVIEW_POLICY.md) — approved v1.4.1
- [`OPERATING_MODEL.md`](OPERATING_MODEL.md)

### Lifecycle
- [`lifecycle/00-engineering-lifecycle.md`](lifecycle/00-engineering-lifecycle.md)
- [`lifecycle/01-requirements.md`](lifecycle/01-requirements.md)
- [`lifecycle/02-architecture-and-design.md`](lifecycle/02-architecture-and-design.md)
- [`lifecycle/03-deliverables-and-handover.md`](lifecycle/03-deliverables-and-handover.md)

### Standards
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

## Authority

```text
latest explicit user instruction
> law / contract / client requirement
> employer/project policy
> project AGENTS / requirements / approved decisions
> approved handbook rules
> reviewed/draft handbook guidance
> framework convention
> AI preference
```

## Company / public boundary

회사·고객 source/diff/schema/design/log/internal context/민감정보 또는 reconstructable non-public derived context를 personal Synology AGY, public GitHub, personal external AI로 보내지 않는다 unless explicitly authorized by higher policy.

이 public handbook에는 employer/client source code, 내부 표준·템플릿·설계·운영자료, 내부 IP/URL/account/schema, 운영데이터, proprietary business rule을 복제하지 않는다. 예시는 independently-created synthetic content를 사용한다.

## Company adoption

이 handbook은 회사 프로젝트에 별도 shadow governance를 만드는 것이 아니라 기존 ticket/design doc/PR/CI/security review/change-management에 mapping하는 개인 quality discipline으로 사용한다 unless formally adopted.

## Review evidence

Mandatory review governance는 2026-09-06 AGY/Gemini 반복 검토를 통해 v1.4.1까지 보완됐다. 최신 policy history와 provenance는 `REVIEW_POLICY.md`와 `reviews/`를 따른다.

## Global control

Cross-repository AI work starts from:

`son1004007/ai-agent-workflow-playbook/CONTROL.md`
