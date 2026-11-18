# ScamShield object diagram

A representative example snapshot. The names, addresses, IDs, and report text below are fictional; no real user data is shown.

```mermaid
classDiagram
direction LR

class resident1["residentA : User"] {
  id = 12
  name = Alex Example
  email = alex@example.test
  role = RESIDENT
}

class report1["report42 : ScamReport"] {
  id = 42
  title = Urgent payment request
  scamType = Impersonation
  status = PENDING
  riskScore = 70
  riskLevel = HIGH
  suspiciousPhone = +1-555-0100
  incidentCity = Sample City
}

class story1["story7 : ScamStory"] {
  id = 7
  title = A suspicious account message
  scamType = Account theft
  status = APPROVED
  incidentCity = Sample City
}

class admin1["admin1 : AdminAuditLog"] {
  id = 5
  reportId = 39
  adminEmail = moderator@example.test
  oldStatus = PENDING
  newStatus = VERIFIED
}

resident1 --> report1 : reportedBy
resident1 --> story1 : reportedBy
note for admin1 "This log references report 39 by scalar ID; it has no object link to a ScamReport instance."
```

The `ScamReport` and `ScamStory` links represent their `ManyToOne` relationship to `User`. The audit-log example deliberately has no association line to a report because the entity stores only `reportId`.
