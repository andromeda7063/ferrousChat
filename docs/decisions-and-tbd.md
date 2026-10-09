# ferrousChat confirmed constraints and TBD register

This register separates explicit constraints and assumptions from unresolved source questions. Preserve TBD identifiers exactly.

## 2.4 Design and Implementation Constraints
### 2.4 Design and Implementation Constraints

**CON-01:** The terminal client shall be implemented in Rust.

**CON-02:** The terminal UI shall use Ratatui.

**CON-03:** Application backend logic shall be implemented in Rust and deployed using AWS Lambda.

**CON-04:** WebSocket connectivity shall be provided through Amazon API Gateway.

**CON-05:** Persistent application data shall be stored in DynamoDB.

**CON-06:** User authentication shall be provided through Amazon Cognito.

**CON-07:** Operational logging and monitoring shall use Amazon CloudWatch.

**CON-08:** File and image objects shall be stored in Cloudflare R2 rather than Amazon S3.

**CON-09:** At least five distinct AWS services shall have demonstrable architectural responsibilities.

**CON-10:** The project shall be designed for zero out-of-pocket expenditure, subject to account eligibility and configured usage restrictions.

**CON-11:** The project shall prioritize a working academic demonstration over enterprise-scale production capabilities.


### 2.5 Assumptions and Dependencies

- Developers have access to a personal AWS account supporting the selected services.
- AWS account eligibility, service availability, and free-account restrictions must be verified before deployment.
- A Cloudflare R2 account and bucket can be configured.
- Users possess a working network connection.
- All clients use a compatible version of the application protocol.
- The expected demonstration workload is small.
- Persistent messages are not automatically deleted by application retention policies.
- The system does not guarantee uninterrupted operation when AWS or Cloudflare services are unavailable.
- The application is intended for authorized educational use.

---


## 14. Open Questions and Items to Be Determined

The following matters require further design decisions.

| ID | Pending Decision | Current Status |
|---|---|---|
| TBD-01 | Account sign-in identifier: email or username | Open |
| TBD-02 | Account verification and password recovery workflow | Open |
| TBD-03 | Minimum terminal size | Open |
| TBD-04 | Maximum text message length | Open |
| TBD-05 | Exact WebSocket authorization mechanism | Open |
| TBD-06 | Exact JSON protocol schema and error codes | Open |
| TBD-07 | DynamoDB table design and partition keys | Open |
| TBD-08 | Private room invitation and removal rules | Open |
| TBD-09 | Message and attachment deletion permissions | Open |
| TBD-10 | R2 bucket region/jurisdiction and object naming policy | Open |
| TBD-11 | Supported attachment MIME types | Open |
| TBD-12 | Terminal keyboard shortcuts and final visual layout | Open |
| TBD-13 | Infrastructure deployment framework | Open |
| TBD-14 | AWS account eligibility and service-specific free usage | Open |
| TBD-15 | Presence expiry and typing timeout thresholds | Open |
| TBD-16 | Client local token storage policy | Open |
| TBD-17 | Concurrency behavior, duplicate delivery, and reconciliation details | Open |
| TBD-18 | Private room creator administration permissions | Open |
| TBD-19 | Log retention and monitoring configuration | Open |
| TBD-20 | Privacy and retention treatment of data at project teardown | Open |

These items must be resolved before the affected components are considered implementation-ready.

---
