# ScamShield use-case diagram

Actors and capabilities shown at a high level. Ellipses represent use cases; the system boundary contains ScamShield features.

```mermaid
flowchart LR
  Guest[Guest]
  Resident[Resident]
  Admin[Administrator]

  subgraph ScamShield[ScamShield system]
    Register((Register account))
    Login((Log in))
    Pages((View public pages))
    SubmitReport((Submit scam report))
    ManageOwnReports((View, edit, or delete own reports))
    CheckPhone((Check phone intelligence))
    ViewAlerts((View city alerts and scam patterns))
    SubmitStory((Submit scam story))
    BrowseStories((Browse approved stories))
    Dashboard((View admin dashboard))
    ReviewReports((Review and verify/reject reports))
    ReviewStories((Approve or reject stories))
    ManageUsers((Manage users))
    ViewAudit((View audit logs))
  end

  Guest --- Register
  Guest --- Login
  Guest --- Pages
  Resident --- Login
  Resident --- SubmitReport
  Resident --- ManageOwnReports
  Resident --- CheckPhone
  Resident --- ViewAlerts
  Resident --- SubmitStory
  Resident --- BrowseStories
  Admin --- Login
  Admin --- Dashboard
  Admin --- ReviewReports
  Admin --- ReviewStories
  Admin --- ManageUsers
  Admin --- ViewAudit

  classDef actor fill:#eef2ff,stroke:#334155,color:#111827
  classDef usecase fill:#ffffff,stroke:#2563eb,color:#111827
  class Guest,Resident,Admin actor
  class Register,Login,Pages,SubmitReport,ManageOwnReports,CheckPhone,ViewAlerts,SubmitStory,BrowseStories,Dashboard,ReviewReports,ReviewStories,ManageUsers,ViewAudit usecase
```

`/api/auth/register` and `/api/auth/login` are unauthenticated routes. The current configuration requires authentication for other API routes and the `ADMIN` role for `/api/admin/**`. "View public pages" refers to public web pages; the `/api/stories` API itself is protected by the current catch-all API rule.
