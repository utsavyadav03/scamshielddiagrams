# Scam report submission activity diagram

This activity follows `POST /api/reports`. The optional phone-statistics lookup occurs before risk analysis and saving.

```mermaid
flowchart TD
  start([Start]) --> request[Submit report with bearer token]
  request --> auth{Authenticated request?}
  auth -- No --> denied[Reject request]
  denied --> finish([End])
  auth -- Yes --> validate{Request fields valid?}
  validate -- No --> invalid[Return validation error]
  invalid --> finish
  validate -- Yes --> user[Find submitting user by email]
  user --> phone{Suspicious phone provided?}
  phone -- Yes --> stats[Count active reports, distinct reporters, and verified reports]
  phone -- No --> build
  stats --> build[Build ScamReport with PENDING status]
  build --> analyze[Analyze title and description for risk indicators]
  analyze --> persist[Save ScamReport]
  persist --> response[Return report, risk assessment, and phone statistics]
  response --> finish
```

If authentication is absent or invalid, the stateless security filter chain does not grant access to this protected API route. The risk assessment is a heuristic based on configured text indicators.
