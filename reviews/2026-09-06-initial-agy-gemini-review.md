# Initial AGY / Gemini Independent Review — 2026-09-06

- handbook_status_at_review: `draft`
- reviewer_capability: `large-context independent review`
- execution_mode: `read-only`
- agent: `AGY / Gemini`
- owner_reconciliation: `ChatGPT + handbook owner policy`

## Execution evidence

### Review A — lifecycle / requirements / architecture / quality / DoR / DoD

- device-control Issue: `#350`
- workflow run: `33995782549`
- AGY version: `1.1.27`
- status: `SUCCEEDED`
- settings_restored: `yes`
- read_only_violation: `no`
- semantic_failure: `no`

The prompt contained the handbook draft text directly and instructed AGY to ignore the unrelated execution workspace files.

### Review B — implementation / testing / security / code review / AI / Java-Spring

- device-control Issue: `#351`
- workflow run: `33995799754`
- AGY version: `1.1.27`
- status: `SUCCEEDED`
- settings_restored: `yes`
- read_only_violation: `no`
- semantic_failure: `no`

## Accepted findings

### 1. Personal handbook self-review deadlock

**Finding:** A personal handbook cannot require an external reviewer for every medium change.

**Decision:** accepted.

Added explicit `Solo / AI-pair` and `Team / organization` operating modes. Solo work may be owner-approved with risk-appropriate deterministic verification, fresh-eyes review, or independent AI review. Organization policy still overrides the handbook.

### 2. Requirement inference vs. fabrication

**Finding:** `INFERRED` and `READY WITH ASSUMPTIONS` needed a deterministic boundary.

**Decision:** accepted with narrower wording.

Allowed inference is limited to evidence-backed technical deductions and reversible technical defaults. Business policy, authorization entitlement, retention/SLA numbers, key ownership, destructive migration semantics and organization approval cannot be invented.

### 3. Break-glass lifecycle

**Finding:** Planned lifecycle did not cover production/security emergency changes.

**Decision:** accepted.

Added `TRIAGE -> CONTAIN/HOTFIX -> BOUNDED VERIFY -> EXPEDITED RELEASE -> MONITOR -> RETRO/RECONCILE`.

### 4. Medium-change bureaucracy

**Finding:** stable IDs, architecture questions and quality views could become excessive ceremony.

**Decision:** accepted.

Introduced LOW/MEDIUM/HIGH risk tiers. Stable requirement/source IDs and full traceability are primarily for HIGH-risk or genuinely traceability-heavy work.

### 5. MUST / SHOULD ambiguity

**Finding:** normative words and `important` needed operational meaning.

**Decision:** accepted.

Added BCP 14-style uppercase vocabulary and an `Important` heuristic based on trust/security boundaries, persistent data, state mutation, public contracts, distributed coordination and blast radius.

### 6. VERIFY / REVIEW / VALIDATE distinction

**Decision:** accepted.

- VERIFY = deterministic technical evidence
- REVIEW = semantic inspection
- VALIDATE = user/business acceptance

### 7. Missing operability / supply-chain / decommissioning concerns

**Decision:** accepted selectively.

Architecture now considers observability, dependency/supply-chain risk and compatibility/deprecation/decommissioning. No universal RED/USE or distributed tracing mandate was added.

### 8. Exposed secret response

**Finding:** `may need rotation` was too weak.

**Decision:** accepted and hardened.

Exposed credentials are treated as compromised and should be revoked/rotated/invalidated as soon as practical, with misuse evidence checked where available.

### 9. CSRF blanket guidance

**Finding:** a blanket prohibition on disabling CSRF was inappropriate for some non-browser services.

**Decision:** accepted after official Spring Security verification, but AGY's simplified bearer-token formulation was not adopted as a universal rule.

The handbook now uses client/threat-model reasoning and official Spring Security guidance. JSON alone is not considered proof that CSRF is impossible.

### 10. Spring transaction proxy and rollback behavior

**Decision:** accepted after official Spring Framework verification.

Added:
- default proxy-mode self-invocation caveat;
- default unchecked-vs-checked rollback semantics;
- version-aware policy rather than universal `rollbackFor = Exception.class`.

### 11. AI package/dependency hallucination

**Decision:** accepted.

AI-suggested third-party dependencies must be verified against official registries/vendor/source before adoption.

### 12. Review severity semantics

**Decision:** accepted.

`BLOCKER` and unresolved `MAJOR` are blocking by default. `MINOR`, `NIT`, and `QUESTION` are non-blocking unless new evidence changes severity or a stricter project policy applies.

### 13. Testing evidence status terminology

**Decision:** accepted.

`PASS / FAIL / NOT RUN / BLOCKED` is a handbook evidence-summary vocabulary, not a replacement for JUnit/pytest native statuses.

## Modified rather than accepted literally

### External network I/O inside DB transactions

AGY proposed an absolute prohibition.

**Decision:** modified to `SHOULD NOT`.

Holding DB connections/locks over external I/O is risky, but there are architectures where this cannot be an absolute universal prohibition. Exceptions require explicit latency/failure/locking/retry analysis and verification.

### DTO / Entity separation

AGY proposed making separation absolute for all public APIs.

**Decision:** retained as a strong default (`SHOULD`) plus `SHOULD NOT` for public writable binding of sensitive/stateful entities.

A universal absolute rule would overfit low-risk/internal/read-only cases.

### Multi-model review

AGY correctly noted excessive cost if used on every change.

**Decision:** HIGH risk = `SHOULD`, MEDIUM/LOW = `MAY` based on ambiguity and value.

## Rejected findings / claims

### Fixed 24–48 hour post-incident backfill deadline

Rejected because no universal policy basis was established. Reconciliation must occur at a project-defined reasonable time after stabilization.

### Universal HTTP/idempotency examples as permissible inference

Not adopted as general requirements because actual API/protocol contracts can differ.

### `Codex is retired` claim

Rejected as a basis for changing this handbook. The current personal environment actively uses a repository-capable Codex toolchain. The useful part of the finding was retained: normative roles are capability-based and product names are only dated implementation examples.

### Universal stateless bearer-token CSRF shortcut

Not adopted as a universal rule. Current official Spring Security guidance is framed around whether browser users can process the request and the actual credential/request model.

## Current conclusion before final review

The initial AGY reviews exposed real process and security weaknesses. Those findings were reconciled against current official documentation and the handbook's anti-overengineering goal.

The revised baseline requires one final independent review before core documents are promoted from `draft` to `reviewed`. `reviewed` will still not mean `approved`; owner approval remains separate under `GOVERNANCE.md`.
