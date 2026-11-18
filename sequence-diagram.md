# Scam report submission sequence diagram

Sequence for a valid `POST /api/reports` request. The optional block runs only when a suspicious phone number is supplied.

```mermaid
sequenceDiagram
  actor Resident
  participant Filter as JwtAuthenticationFilter
  participant Jwt as JwtService
  participant Details as CustomUserDetailsService
  participant Controller as ScamReportController
  participant Service as ScamReportService
  participant Users as UserRepository
  participant Reports as ScamReportRepository
  participant Risk as FraudRiskAnalysisService

  Resident->>Filter: POST /api/reports + bearer token + request
  Filter->>Jwt: extractEmail(token)
  Jwt-->>Filter: email
  Filter->>Details: loadUserByUsername(email)
  Details-->>Filter: UserDetails
  Filter->>Jwt: isTokenValid(token, email)
  Jwt-->>Filter: valid
  Filter->>Controller: continue with authenticated principal
  Controller->>Controller: validate request body
  Controller->>Service: createReport(request, email)
  Service->>Users: findByEmail(email)
  Users-->>Service: User

  opt suspicious phone is present
    Service->>Reports: countActiveReportsByPhone(phone)
    Reports-->>Service: previous report count
    Service->>Reports: countUniqueReportersByPhone(phone)
    Reports-->>Service: distinct reporter count
    Service->>Reports: countVerifiedReportsByPhone(phone)
    Reports-->>Service: verified report count
  end

  Service->>Risk: analyzeReport(report)
  Risk-->>Service: set risk score, level, and reason
  Service->>Reports: save(report)
  Reports-->>Service: saved report
  Service-->>Controller: submission response
  Controller-->>Resident: 201 Created + report and statistics
```

Invalid or missing authentication is rejected by the protected API security chain. Invalid request fields are rejected by controller validation before the service call.
