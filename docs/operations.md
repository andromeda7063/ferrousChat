# ferrousChat operations, security, and cost

Operational requirements drawn directly from the canonical [SRS](srs.md). These are source requirements, not additional commitments.

## 3.9 Logging and Monitoring
### 3.9 Logging and Monitoring

**FR-071 — Structured Logging [MUST]**

Backend functions shall generate structured operational logs.

**FR-072 — Error Monitoring [MUST]**

The system shall expose sufficient information through CloudWatch to identify Lambda failures and other application errors.

**FR-073 — Operational Metrics [MUST]**

The deployment shall make invocation counts, execution errors, and execution duration observable through CloudWatch.

**FR-074 — Sensitive Data Redaction [MUST]**

Application logs shall not intentionally include passwords, raw authentication tokens, R2 access keys, or presigned URLs.

**FR-075 — Cost Visibility [MUST]**

Developers shall be able to inspect AWS resource usage and billing information through available account management facilities.

**FR-076 — Monitoring Dashboard [SHOULD]**

The project should provide a basic CloudWatch monitoring dashboard where supported by account permissions and cost constraints.

---


### 4.3 Security

**NFR-011 — Transport Encryption [MUST]**

Client-to-backend communication shall use encrypted HTTPS and WSS connections.

**NFR-012 — Authenticated Access [MUST]**

Messaging operations shall be available only to authenticated users.

**NFR-013 — Server-Side Authorization [MUST]**

Authorization decisions for rooms, DMs, and attachments shall be enforced by trusted backend code.

**NFR-014 — Least Privilege [MUST]**

AWS IAM roles shall provide only permissions necessary for assigned operations.

**NFR-015 — Secret Protection [MUST]**

AWS credentials and Cloudflare R2 credentials shall not be embedded in distributed client binaries or committed to source control.

**NFR-016 — Identity Integrity [MUST]**

Backend processing shall derive the authenticated sender identity from trusted authentication context rather than trusting a user ID supplied in a message payload.

**NFR-017 — Private Conversation Isolation [MUST]**

Unauthorized users shall not be able to retrieve private room or direct message data.

**NFR-018 — Attachment Access Control [MUST]**

Attachment operations shall require authorization checks before upload or download credentials are issued.

**NFR-019 — Credential Exposure Prevention [MUST]**

Authentication tokens and temporary signed URLs shall not be exposed through routine logs or debugging output.

**NFR-020 — Stored Data Security [MUST]**

The system shall use the encryption-at-rest capabilities provided by its configured managed storage services.


### 4.6 Cost Constraints

**NFR-030 — Zero-Spend Objective [MUST]**

The deployment shall be designed to avoid monetary expenditure.

**NFR-031 — Account Eligibility Verification [MUST]**

The project team shall verify that each deployed AWS service is usable under the account's applicable free plan, credits, or free-tier allowances before provisioning it.

**NFR-032 — Minimal Resource Provisioning [MUST]**

The deployment shall avoid unnecessary always-on compute resources, paid networking resources, and avoidable managed service charges.

**NFR-033 — Storage Usage Control [MUST]**

Application file size restrictions and test-data volumes shall be controlled to remain within the intended R2 free allowance.

**NFR-034 — Resource Cleanup [MUST]**

The project shall provide documented steps to remove unused infrastructure and test resources after evaluation.

**NFR-035 — Billing Safeguards [MUST]**

Where supported, account-level spending restrictions or notifications shall be configured before deployment. Notifications alone shall not be represented as hard spending limits.

---
