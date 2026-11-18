# ScamShield diagrams

UML diagrams for the ScamShield Spring Boot application. The diagrams describe the current project structure and representative report-submission flow; they do not include application source code or real user data.

GitHub renders each Mermaid diagram on its page:

- [Class diagram](class-diagram.md) - main presentation, service, security, persistence, and domain relationships.
- [Object diagram](object-diagram.md) - a sample runtime snapshot of a resident, reports, a story, and an audit log.
- [Activity diagram](activity-diagram.md) - scam-report submission, including validation, optional phone checks, and risk analysis.
- [Sequence diagram](sequence-diagram.md) - authenticated report submission across the filter, controller, services, and repositories.
- [Use-case diagram](use-case-diagram.md) - resident, administrator, and guest capabilities.

## Scope notes

- `ScamReport` is the report entity used by the report workflow. `Report` is a separate legacy entity.
- `AdminAuditLog` stores a report ID and admin email as scalar values; it does not have a JPA relationship to `ScamReport`.
- The current security configuration requires authentication for API routes except registration and login, and restricts `/api/admin/**` to the `ADMIN` role. Public page routes are configured separately.
- Risk scores are generated from configured text indicators. They are community signals, not proof that a phone number or person is fraudulent.

These are documentation diagrams, not executable UML or a specification of behavior beyond the current source.
