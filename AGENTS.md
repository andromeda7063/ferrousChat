# ferrousChat agent instructions (TEMPORARY: reading/analysis session only)

This file replaces the full `AGENTS.md` for one session. The original is backed up as `AGENTS.full.md`. Do not merge the two. This file will be rewritten later.

## Session purpose

Read the specification pack and analyze it. Nothing else. No code, no scaffolding, no cloud work.

## Where things are

- All specification files are in `docs/`. The canonical source is `docs/srs.md`.
- `docs/README.md` indexes the focused files. If a focused file disagrees with `srs.md`, `srs.md` wins. Report the disagreement; do not fix it.

## Allowed actions

- Read any file in this repository.
- Create or overwrite exactly one file: `docs/tbd-analysis.md`.

## Forbidden actions

- Editing, renaming, or deleting any other file, including everything in `docs/` and this file.
- Writing source code, manifests, CI config, or infrastructure config.
- Running any command that touches AWS, Cloudflare, or any network service.
- Installing packages or running builds.
- Using the Plane MCP server. No Plane issue is required for this session.
- Making git commits.

If a task seems to need a forbidden action, stop and say so in your reply.

## Rules for analysis

- Treat every `TBD-01` through `TBD-20` as open. Do not decide any of them. Offer options and a recommendation only, and label the recommendation clearly as a recommendation.
- Preserve requirement IDs (`FR`, `NFR`, `IR`, `DR`, `AC`, `CON`, `TBD`) exactly. Cite them when you reference a requirement.
- Do not invent requirements. If something is missing from the spec, report it as a gap.
- Do not claim any AWS or Cloudflare service, price, or free-tier allowance is available or free. Mark such claims as unverified and tied to `TBD-14`.
- Quote the spec sparingly. Point to IDs and sections instead.
- State assumptions explicitly. If something is ambiguous, list the interpretations rather than picking one silently.

## Standing project constraints (context only, for judging the design)

- Stack: Rust client with Ratatui; Rust backend on AWS Lambda; API Gateway WebSocket; DynamoDB; Cognito; CloudWatch; IAM; Cloudflare R2 for attachments (max 2 MiB).
- Zero out-of-pocket spend. Free-tier limits need 20% unused headroom and a fail-closed enforcement mechanism. Alerts and budgets are not hard caps.
- Message history persists until authorized deletion or teardown. No automatic expiry.
- Typing indicators are ephemeral. Presence is advisory.

## Required output: `docs/tbd-analysis.md`

Include these sections:

1. **Contradictions and gaps** between files, with file and ID references.
2. **TBD analysis**: for each of `TBD-01` to `TBD-20`, give 2-3 options, tradeoffs, a recommendation, and which components or ACs it blocks.
3. **Start-now work**: what can begin before any TBD is resolved.
4. **Free-tier tension**: places where the zero-spend policy conflicts with or complicates the design (for example, whether a budget ledger itself consumes billable requests).
5. **Suggested resolution order** for the TBDs.

When finished, reply with a short summary and the list of files you wrote. It should contain only `docs/tbd-analysis.md`.
