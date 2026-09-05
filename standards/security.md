# Secure Development Standard

- status: `independently-reviewed draft`
- version: `0.3`
- baseline_date: `2026-09-06`
- primary_reference: `NIST SP 800-218 SSDF v1.1`
- web_verification_reference: `OWASP ASVS 5.0.0`

## 목적

보안을 구현 후 체크리스트가 아니라 요구사항·설계·구현·검증·운영 전 과정의 품질 속성으로 다룬다.

2026-09-06 기준 NIST SP 800-218 SSDF v1.1은 final이며, SSDF v1.2는 draft이므로 normative baseline은 v1.1이다.

## SEC-001 — 신뢰하지 않는 입력을 명시한다 — MUST

사용자 입력, client-supplied identifier/role/state, 외부 API response, uploaded file, message/event, URL/query/header/cookie, LLM/AI output, 다른 trust domain의 data를 기본적으로 untrusted로 본다.

검증은 syntax뿐 아니라 업무 의미, 허용 범위, 권한과 연결한다.

## SEC-002 — 인증과 인가를 분리한다 — MUST

`로그인되어 있다`와 `해당 객체/행위를 허용한다`는 다른 판정이다. 필요한 범위에서 actor identity, function privilege, object ownership/scope, state-dependent permission을 server-side에서 확인한다.

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

이미 외부에 노출된 credential/secret은 단순 파일 삭제로 해결됐다고 보지 않는다.

**MUST:** 해당 값을 `compromised`로 취급하고 secret 종류와 운영 제약에 맞게 가능한 한 신속히 **revoke / rotate / invalidate**하며, 접근 로그나 audit evidence가 있으면 오용 여부를 확인한다.

### Incident evidence preservation

credential invalidation 때문에 관련 session/telemetry/audit evidence가 사라질 가능성이 있고, **노출 지속 시간을 의미 있게 늘리지 않고** 보존할 수 있다면 revoke/rotation 직전 또는 병행하여 필요한 audit/access evidence를 snapshot/export/preserve한다.

**MUST NOT:** 포렌식 보존을 이유로 compromised credential의 containment를 불필요하게 지연한다.

직접 회전할 수 없는 종류라면 project/security incident 절차에 따라 대체 containment를 기록한다.

## SEC-005 — 민감정보를 최소 수집·최소 노출한다 — MUST

필요한 데이터만 수집·저장·응답·로그한다. API response에서 persistence object 전체를 무심코 직렬화해 불필요한 field가 노출되지 않도록 계약을 확인한다.

## SEC-006 — 객체 수준 권한을 negative test로 검증한다 — MUST when user-owned/protected objects exist

다른 사용자의 식별자를 알고 있어도 권한 밖 데이터가 반환되지 않음을 검증한다.

## SEC-007 — 안전한 실패를 설계한다 — MUST

오류 시 다음을 피한다.
- stack trace/internal path 노출
- secret/SQL/raw exception 노출
- 권한 검사를 약화하는 fallback
- 실패 후 잘못된 상태 확정

외부에는 필요한 오류 의미만 제공하고 내부에는 correlation에 필요한 최소 evidence를 남긴다.

## SEC-008 — Dependency와 supply chain을 관리한다 — SHOULD

- dependency source/version을 관리한다.
- 알려진 취약점과 지원 종료 여부를 확인할 경로를 둔다.
- lockfile, SBOM, signature/provenance 등은 risk tier와 배포 환경에 맞게 사용한다.
- 자동 update는 test/compatibility verification 없이 곧바로 운영 반영하지 않는다.
- AI가 제안한 신규 package/library는 실제 공식 registry/vendor/source에 존재하는지 확인한 뒤 채택한다.

## SEC-009 — 보안 요구·위험·설계 결정을 추적한다 — SHOULD

중요 security requirement와 design decision은 구현/검증 evidence와 연결한다. HIGH risk에서 우선 적용한다.

## SEC-010 — Logging/Audit 목적을 구분한다 — SHOULD

운영 로그와 감사 로그는 목적이 다를 수 있다.

감사 가치가 높은 event 예:
- 로그인/인증 실패
- 권한/role 변경
- 중요 데이터 생성/변경/삭제
- 승인/거부
- 관리자 행위
- 보안 설정 변경

credential/secret과 불필요한 개인정보를 감사 증거라는 이유로 저장하지 않는다.

## SEC-011 — 보안 통제를 troubleshooting 목적으로 맹목적으로 완화하지 않는다 — MUST

TLS/certificate/host-key verification, authentication/authorization, OS sandbox/MAC, CSRF/CORS 등 보안 관련 설정을 `에러가 사라지는지 보려고` 전역에서 무조건 끄는 것을 기본 해결책으로 사용하지 않는다.

먼저 다음을 확인한다.
- 해당 통제가 현재 threat model에서 필요한가?
- 공식 framework/vendor guidance는 무엇인가?
- endpoint/client 종류에 따라 적용 범위를 좁힐 수 있는가?
- 더 작은 reversible change가 가능한가?

### CSRF는 client/threat model에 따라 판단한다

CSRF 보호를 무조건 켜거나 끄는 개인 규칙을 만들지 않는다.

Spring Security 공식 guidance처럼 **일반 사용자가 browser로 처리할 수 있는 요청**은 CSRF protection 필요성을 우선 검토한다. 반대로 **non-browser client 전용 service**는 CSRF를 disable하는 것이 적절할 수 있다.

JSON이라는 이유만으로 CSRF가 불가능하다고 가정하지 않는다. cookie/session 등 browser가 자동으로 전송하는 credential을 사용하는지와 실제 request flow를 확인한다.

Spring Security reference: https://docs.spring.io/spring-security/reference/features/exploits/csrf.html

## SEC-012 — AI output은 신뢰 경계를 넘은 입력이다 — MUST

AI가 생성한 code, SQL, shell, config, dependency suggestion, security advice는 실행/merge 전에 검증한다. 특히 권한, 삭제, migration, credential, network/security setting은 risk tier에 맞는 독립 검토 또는 실행 evidence를 요구한다.

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

OWASP ASVS 5.0.0을 모든 웹 프로젝트에 통째로 강제하지 않는다. risk와 필요한 assurance 수준에 맞게 relevant requirement를 선택하고 프로젝트 requirement/evidence와 mapping한다.

정확한 ASVS requirement ID는 사용 시점의 공식 문서에서 확인하고 기억으로 작성하지 않는다.

## 근거

- NIST SP 800-218 SSDF v1.1 (Final): https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SSDF publications status: https://csrc.nist.gov/projects/ssdf/publications
- OWASP ASVS 5.0.0: https://owasp.org/www-project-application-security-verification-standard/
- Spring Security CSRF guidance: https://docs.spring.io/spring-security/reference/features/exploits/csrf.html

## Review record

- 2026-09-06: ChatGPT initial draft.
- 2026-09-06: AGY/Gemini #351 review received.
- 2026-09-06: Accepted secret-compromise hardening and CSRF-context clarification after checking current official Spring Security docs. Rejected an unconditional stateless-bearer shortcut; the rule follows browser/client threat context instead.
- 2026-09-06: Final AGY/Gemini #352 verdict `READY_FOR_REVIEWED`; forensic-evidence preservation was added with an explicit non-delay containment rule.
