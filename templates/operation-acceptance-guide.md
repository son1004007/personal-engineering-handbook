# Operation / Acceptance / Handover Guide

## Applicability
- `APPLICABLE | MERGED | N/A BY ARCHITECTURE`
- Rationale:

## Release / Handover Identity
- Commit SHA / tag / build-artifact ID:
- Environment:
- Review status:
- Active calibrated BLOCKER: `0 | <count>`

Normal release/handover requires active calibrated BLOCKER = 0. Emergency containment follows `REVIEW_POLICY.md` break-glass and is not normal Done until review debt closes.

## Runtime Operations
- Start:
- Stop:
- Restart:
- Status:

## Health / Diagnostics
- Health endpoint/check:
- Expected healthy result:
- Expected unhealthy/degraded result:
- Logs:
- Metrics/traces:

## Routine Operations
- 

## Backup / Restore
- Backup:
- Restore:
- Verification:

## Incident First Response
1. Detect / confirm impact
2. Contain
3. Collect evidence
4. Rollback/disable/forward-fix as appropriate
5. Escalate / record
6. Track deferred review debt when break-glass is used

## Access / Permission Operations
- 

## Acceptance Results
| Check | Evidence | Result | Release Impact | Notes |
| --- | --- | --- | --- | --- |

Result: `PASS | FAIL | NOT RUN | BLOCKED | N/A`

`NOT RUN` or verification-`BLOCKED` is recorded transparently; it does not itself authorize release.

## Known Issues / Residual Risk
- Finding/risk:
- Calibrated severity:
- Decision: `FIXED | RISK_ACCEPTED | BLOCKED | N/A`
- Authorized approver (for MAJOR risk acceptance):
- Mitigation/revisit trigger:

## Handover
- Runtime owner:
- Code owner:
- Data owner:
- Required credentials/secrets location reference (never secret value):
- Open work / next action:
