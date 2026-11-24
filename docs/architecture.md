# Architecture

## Four layers
| Layer | What lives here |
|---|---|
| Business | Member registration and renewal, entrance verification, member communication, management reporting |
| Application | React front end; Spring Boot back end (JWT authentication, role-based access control, audit log); intelligence layer (Greg: system prompt and guardrails) |
| Data | PostgreSQL (members, contracts, attendance, audit records) |
| Infrastructure | Render-hosted services, devices on the gym's premises, OpenAI API, transactional email provider |

Data flows: users reach the React front end over HTTPS; the front end calls the Spring Boot API; the back end talks to PostgreSQL; for reports it computes aggregates itself and sends only the approved, anonymous summary to the AI provider; email notices go out through the email provider.

## Operational vs development value stream
**Operational (before → after):** pay at cashier → paper contract → staff record expiry → QuickTag registration → renewal repeats the cycle, **became** centralised registration with preview/confirmation, searchable contracts, automatic expiry notices and dashboards.

**Development:** requirements and SRS → build (Spring Boot / React) → test (including adversarial and concurrency tests) → deploy → feedback from the client. Streaks, registration preview, controlled corrections, analytics and notification features each map to a specific pain point in the operational stream.

## South African constraints and mitigations
| Constraint | Mitigation |
|---|---|
| Lost or misplaced paper contracts | Searchable member and contract records with contract history |
| Internet or remote-service outage | Minimised local access-control replica and queued offline event synchronisation |
| Loadshedding | Laptops (built-in battery) instead of desktops; proposed UPS-backed workstations and safe transaction recovery |
| High mobile data use | Responsive, optimised pages; controlled image size and bandwidth for member self-service |
| Sensitive membership information | Restricted back-end access, role separation, encrypted transport, limited exposure of personal records; PII stripped before the AI handles anything |
| High registration or attendance load | Centralised workflows, API latency testing, uniform transactional updates, capacity monitoring |
| AI availability, cost and unsafe output | Insights are never critical-path, rate limiting, limited output, backup credentials and standard reports as fallback |
