# ferrousChat project specification

Markdown specification pack for the BCSE355L Cloud Architecture Design project. **Start with `srs.md`**, the complete source SRS. Focused files below reproduce relevant source sections for coding-agent workflows.

## Files

- [`srs.md`](srs.md) — complete SRS v0.1, including sections, requirement IDs, tables, use cases, test strategy, risks, academic alignment, and TBDs.
- [`requirements.md`](requirements.md) — functional and non-functional requirements (FR/NFR).
- [`architecture.md`](architecture.md) — proposed AWS serverless architecture and workflows.
- [`interfaces.md`](interfaces.md) — terminal, WebSocket, Cognito, DynamoDB, R2, and observability interfaces (IR).
- [`data-model.md`](data-model.md) — data requirements and proposed data fields (DR).
- [`acceptance-criteria.md`](acceptance-criteria.md) — acceptance tests (AC).
- [`operations.md`](operations.md) — logging, monitoring, security, and zero-spend requirements.
- [`decisions-and-tbd.md`](decisions-and-tbd.md) — implementation constraints, assumptions, and open questions.
- [`course-context.md`](course-context.md) — BCSE355L academic alignment from the SRS.

## Agent guidance

The full SRS is canonical. Focused files reproduce its relevant sections; if they diverge, update them from `srs.md`. Keep all IDs stable. `TBD-01` through `TBD-20` remain open as written. Respect the zero out-of-pocket objective and verify account eligibility, service limits, and billing safeguards before deployment. The architecture specifies AWS API Gateway WebSocket API, Rust Lambda, DynamoDB, Cognito, CloudWatch, IAM, and Cloudflare R2; do not silently substitute services.

**Document baseline:** Version 0.1 — Initial Draft; Draft for Review.
