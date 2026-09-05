# Secure Development Standard

- status: `draft`
- version: `0.1`
- baseline_date: `2026-09-06`
- primary_reference: `NIST SP 800-218 SSDF v1.1`
- web_verification_reference: `OWASP ASVS 5.0.0`

## 목적

보안을 구현 후 체크리스트가 아니라 요구사항·설계·구현·검증·운영 전 과정의 품질 속성으로 다룬다.

2026-09-06 기준 NIST SP 800-218 SSDF v1.1은 final이며, SSDF v1.2는 draft이므로 이 문서의 normative baseline은 v1.1로 둔다.

## SEC-001 — 신뢰하지 않는 입력을 명시한다 — MUST

다음을 기본적으로 untrusted로 취급한다.

- 사용자 입력
- client-supplied identifier/role/state
- 외부 API response
- uploaded file
- message/event
- URL/query/header/cookie
- LLM/AI output
- 다른 trust domain에서 넘어온 data

검증은 syntax뿐 아니라 업무 의미, 허용 범위, 권한과 연결한다.

## SEC-002 — 인증과 인가를 분리한다 — MUST

`로그인되어 있다`와 `해당 객체/행위를 허용한다`는 다른 판정이다.

서버에서 최소한 다음을 필요한 범위에서 확인한다.

- actor identity
- role/function privilege
- object ownership/scope
- state-dependent permission

client가 전달한 user ID/role만으로 권한을 결정하지 않는다.

## SEC-003 — 최소권한을 기본으로 한다 — MUST

사용자, service account, DB account, container, CI token, filesystem permission 등은 필요한 최소 범위로 제한한다.

## SEC-004 — Secret은 repository와 로그에 남기지 않는다 — MUST

금지 예:

- password
- API token/key
- private key
- session/OAuth token
- recovery/OTP secret

secret이 이미 노출되면 파일 삭제만으로 해결됐다고 보지 않고 회전/revocation 필요성을 검토한다.

## SEC-005 — 민감정보를 최소 수집·최소 노출한다 — MUST

필요한 데이터만 수집·저장·응답·로그한다. DTO/API response에서 entity 전체를 그대로 직렬화해 불필요한 field가 노출되지 않도록 한다.

## SEC-006 — 객체 수준 권한을 negative test로 검증한다 — MUST when user-owned/protected objects exist

사용자 A가 사용자 B의 식별자를 알고 있어도 B의 데이터에 접근하지 못함을 검증한다.

OWASP ASVS의 관련 requirement를 프로젝트 보안 요구에 mapping할 수 있다.

## SEC-007 — 안전한 실패를 설계한다 — MUST

오류 시 다음을 피한다.

- stack trace/internal path 노출
- secret/SQL/raw exception 노출
- 일부 권한 검사만 우회된 fallback
- 실패 후 잘못된 상태 확정

외부에는 필요한 오류 의미만 제공하고 내부에는 상관관계 추적에 필요한 evidence를 남긴다.

## SEC-008 — Dependency와 supply chain을 관리한다 — SHOULD

- dependency source와 version을 관리한다.
- 알려진 취약점과 지원 종료 여부를 확인할 수 있는 경로를 둔다.
- lockfile/SBOM/signature 등은 프로젝트 위험과 배포 환경에 맞게 사용한다.
- 자동 update는 테스트/호환성 검증 없이 곧바로 운영 반영하지 않는다.

## SEC-009 — 보안 요구·위험·설계 결정을 추적한다 — SHOULD

중요한 security requirement와 design decision은 구현/테스트 evidence와 연결한다. 이는 NIST SSDF가 요구사항, 위험, 설계 결정의 추적을 강조하는 방향과 일치한다.

## SEC-010 — Logging/Audit 목적을 구분한다 — SHOULD

운영 로그와 감사 로그는 목적이 다를 수 있다.

감사 가치가 높은 event 예:

- 로그인/인증 실패
- 권한/role 변경
- 중요 데이터 생성/변경/삭제
- 승인/거부
- 관리자 행위
- 보안 설정 변경

민감정보 자체를 감사 증거라는 이유로 과도하게 저장하지 않는다.

## SEC-011 — 보안 완화를 troubleshooting 기본값으로 사용하지 않는다 — MUST

문제 해결을 위해 다음을 무조건 끄는 방식은 금지한다.

- TLS verification
- host key checking
- authentication/authorization
- SELinux/AppArmor/seccomp
- CSRF/CORS protection
- certificate validation

원인을 확인하고 최소·국소·되돌릴 수 있는 변경을 우선한다.

## SEC-012 — AI output은 신뢰 경계를 넘은 입력이다 — MUST

AI가 생성한 code, SQL, shell, config, security advice는 실행/merge 전에 검증한다. 특히 권한, 삭제, migration, credential, network/security setting은 독립 검토 또는 실행 evidence를 요구한다.

상세: [`ai-assisted-development.md`](ai-assisted-development.md)

## Security Review 최소 질문

- 누구를 인증하고 무엇을 인가하는가?
- object-level authorization이 필요한가?
- 어떤 입력/응답이 신뢰되지 않는가?
- 민감정보/secret은 어디에 존재하는가?
- 로그/오류에 무엇이 노출되는가?
- 데이터 변경은 원자적이고 추적 가능한가?
- dependency/supply chain 위험이 있는가?
- 실패 시 보안 경계가 약해지는가?
- 배포/운영 권한이 과도하지 않은가?

## ASVS 사용 방법

OWASP ASVS 5.0.0을 전체 checklist로 무조건 적용하지 않는다. 웹 애플리케이션의 위험과 필요한 assurance 수준에 맞게 relevant requirement를 선택하고 프로젝트 requirement ID와 mapping한다.

예:

```text
SEC-007 -> ASVS <relevant requirement ID> -> integration negative test -> evidence
```

정확한 ASVS 번호는 사용 시점의 공식 5.0.0 문서에서 확인하고 기억으로 작성하지 않는다.

## 근거

- NIST SP 800-218 SSDF v1.1 (Final): https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SSDF publications status: https://csrc.nist.gov/projects/ssdf/publications
- OWASP ASVS 5.0.0: https://owasp.org/www-project-application-security-verification-standard/

## Review record

- 2026-09-06: ChatGPT initial draft.
- Independent AGY/Gemini review: pending.
