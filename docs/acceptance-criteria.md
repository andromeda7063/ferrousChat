# ferrousChat acceptance criteria

Acceptance baseline from the canonical [SRS](srs.md), preserving AC IDs and expected results.

## 9. Acceptance Criteria

The following tests constitute the minimum acceptance baseline.

| ID | Acceptance Test | Expected Result |
|---|---|---|
| AC-001 | Register a new user | Registration completes through Cognito |
| AC-002 | Log in with valid credentials | Authenticated chat session established |
| AC-003 | Attempt access without valid authentication | Access rejected |
| AC-004 | Connect five clients | All five establish usable sessions |
| AC-005 | Send message in public room | Authorized connected participants receive it |
| AC-006 | Create and join a private room | Authorized membership enforced |
| AC-007 | Unauthorized private room access | Access denied |
| AC-008 | Send direct message | Only intended participants have access |
| AC-009 | Disconnect a recipient before sending | Message remains available in history |
| AC-010 | Restart a client | Previously stored history can be retrieved |
| AC-011 | Type in a conversation | Other authorized participants see temporary typing status |
| AC-012 | Disconnect a user | Presence eventually changes to offline |
| AC-013 | Upload a file below 2 MiB | Upload succeeds with valid authorization |
| AC-014 | Upload a file above 2 MiB | Upload rejected |
| AC-015 | Request attachment from unauthorized account | Access rejected |
| AC-016 | Upload a demo image below 1 MiB | Image stored and referenced successfully |
| AC-017 | Use slash commands | Supported commands invoke intended operations |
| AC-018 | Interrupt network connection | Client displays failure and attempts recovery |
| AC-019 | Trigger a controlled backend error | Diagnostic evidence appears in CloudWatch |
| AC-020 | Inspect cloud deployment | All five required AWS service integrations are demonstrable |
| AC-021 | Validate client portability | Client operates on Linux and macOS |
| AC-022 | Review service usage and charges | Deployment remains within approved zero-spend conditions |

Acceptance tests involving external cloud infrastructure shall be performed only after eligibility and cost controls have been verified.

---
