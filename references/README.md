# References

- status: `draft`
- baseline_date: `2026-09-06`

이 디렉터리는 handbook 규칙의 근거가 되는 공개·신뢰 가능한 출처와 현재 사용 버전을 기록한다.

## Evidence hierarchy

```text
법률 / 계약 / 회사·고객 정책
> 실제 runtime/test evidence
> current final official standard / official vendor documentation
> 신뢰 가능한 공개 engineering practice
> 독립 human review
> 복수 AI 독립 review
> 단일 AI 의견
```

프로젝트별 더 엄격한 policy가 있으면 그 정책을 우선한다.

## Current baseline map — 2026-09-06

| Area | Baseline | Status / use |
|---|---|---|
| Software lifecycle | ISO/IEC/IEEE 12207:2026 | current published lifecycle framework |
| Requirements | ISO/IEC/IEEE 29148:2018 | current; ISO page notes confirmation in 2024 |
| Architecture description | ISO/IEC/IEEE 42010:2022 | current architecture-description baseline |
| Product quality | ISO/IEC 25010:2023 | current 9-characteristic product quality model |
| Secure development | NIST SP 800-218 SSDF v1.1 | final normative baseline |
| SSDF next revision | NIST SP 800-218 Rev.1 / SSDF v1.2 | draft; informative only until final |
| AI/model secure development | NIST SP 800-218A | final; informative where AI model development is relevant |
| Web application security verification | OWASP ASVS 5.0.0 | latest stable baseline |
| Testing | ISTQB CTFL syllabus v4.0.1 | practical public testing vocabulary/concepts |
| Code review | Google Engineering Practices | public practical reference, not mandatory standard |

## Official links

- ISO/IEC/IEEE 12207:2026: https://www.iso.org/standard/90219.html
- ISO/IEC/IEEE 29148:2018: https://www.iso.org/standard/72089.html
- ISO/IEC/IEEE 42010:2022: https://www.iso.org/standard/74393.html
- ISO/IEC 25010:2023: https://www.iso.org/standard/78176.html
- NIST SP 800-218 SSDF v1.1: https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SSDF publication status: https://csrc.nist.gov/projects/ssdf/publications
- NIST SP 800-218A: https://csrc.nist.gov/pubs/sp/800/218/a/final
- OWASP ASVS: https://owasp.org/www-project-application-security-verification-standard/
- ISTQB CTFL: https://www.istqb.org/certifications/certified-tester-foundation-level-ctfl-v4-0/
- Google Engineering Practices: https://google.github.io/eng-practices/review/

## Framework / language official-source rule

구체적인 코딩 규칙은 해당 프로젝트의 실제 version에 맞는 공식 문서를 우선한다.

예:

- Java: https://docs.oracle.com/en/java/
- Spring Framework: https://docs.spring.io/spring-framework/reference/
- Spring Security: https://docs.spring.io/spring-security/reference/
- Python: https://docs.python.org/
- FastAPI: https://fastapi.tiangolo.com/
- PostgreSQL: https://www.postgresql.org/docs/
- Hibernate ORM: https://hibernate.org/orm/documentation/

## Citation / copyright rules

- 유료 ISO 표준의 본문을 복제하지 않는다.
- 공개 ISO abstract/meta information과 독립적으로 정리한 적용 원칙만 기록한다.
- 공개 standard/framework 문서도 긴 원문 복사 대신 링크와 독립 요약을 사용한다.
- draft 표준을 final처럼 표현하지 않는다.
- version-sensitive rule은 `last_verified` 또는 review date를 남기고 구현 시 다시 공식 문서를 확인한다.

## Review record

- 2026-09-06: ChatGPT verified current public status/versions against official sources.
- Independent AGY/Gemini review: pending.
