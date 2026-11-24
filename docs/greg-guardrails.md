# Greg: guardrails and audit

Greg is an admin-only assistant that turns analytics into management reports. The principle: **the model writes, the backend decides.**

## Design rules
1. **Data minimisation.** Only allow-listed, anonymous aggregate statistics are sent to the AI provider. No names, ID numbers, addresses, contact details or individual visit histories.
2. **Server-side authorisation.** Role checks happen in the back end; hiding a button in the UI is not treated as security.
3. **Backend-computed figures.** Counts and calculations are produced before the AI is called. Reports state their period and separate verified facts from the AI's interpretation.
4. **Honest uncertainty.** With too little history, Greg says so rather than predicting. Estimated values (such as hours saved) stay labelled as estimates.
5. **Validation of output.** Malformed responses, unsupported claims and references to metrics that weren't provided are rejected.
6. **Human oversight.** Greg cannot approve access, suspend a member, or modify a contract or membership record. Authorised staff remain accountable.
7. **Secrets stay server-side.** API keys and credentials are never exposed to the front end, logs or presentations.

## Audit results (as reported by the team)
| # | Area | What was tried | Reported outcome |
|---|---|---|---|
| 1 | Access to Greg | Admin, normal member and access-control employee; oversized request | Only admins allowed; others and oversized requests blocked before processing (4/4) |
| 2 | Privacy and calculations | Small groups, empty data, private figures, registrations/renewals, arrival data | Unsafe small-group info hidden; impossible calculations marked unavailable (5/5) |
| 3 | Report accuracy | Missing configuration, malformed AI output, unsupported claims, estimate shown as fact | Unreliable output rejected; estimates stayed labelled (4/4) |
| 4 | Data sent to provider | Inspected the exact outbound request; response citing an unavailable metric | Only allow-listed anonymous aggregates sent; unknown metrics rejected (2/2) |
| 5 | Password reset | Known/unknown emails, locked account, wrong codes, request flooding | No account enumeration; attempts limited; codes single-use; old sessions revoked (5/5) |
| 6 | Simultaneous check-ins | Two near-simultaneous requests for one member | One visit and one streak increase recorded (1/1) |
| 7 | Access decisions | Warning-level and denied decisions | Warning recorded a visit; denial did not add attendance (2/2) |
| 8 | Record integrity | Identifier change, duplicate identifier, existing student-assistant account | Usernames stayed in sync; duplicates rejected before changes; accounts linked (3/3) |
| 9 | Expiry notifications | Valid/missing/invalid addresses, repeats, one delivery failure, disabled setting | Bad addresses skipped; processing continued; disabled sent nothing (5/5) |
| 10 | Audit-system failure | Simulated failure saving an audit record | Failure contained; main operation unaffected (1/1) |

Total reported: 32 of 32 checks passed. These are the team's own results; they show the checks that were designed, not proof the system is unbreakable. Prompt-injection-style attacks on free-text input are a natural next area to test, since Greg takes structured options rather than free text.

## Privacy and ethics
Purpose limitation, role-based access, accountability, human oversight, secret handling, retention per institutional and legal requirements, and fictional or anonymised data for demonstrations and testing. Live demonstrations with the client need the client's permission.
