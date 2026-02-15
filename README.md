# n8n Workflow (Sanitized) — KPI Extraction & Comms Outputs

This folder contains a **sanitized** export of an n8n workflow for portfolio purposes.

## What the workflow does (high level)

1. Manual trigger starts the workflow.
2. A Set node provides meeting notes (redacted in this public version).
3. An LLM step extracts numeric KPIs into a strict JSON schema.
4. A code step enriches KPIs (variance vs plan, simple checks/flags).
5. A second LLM step drafts three outputs:
   - External LinkedIn post (no raw KPIs)
   - Internal brief (bullets + risks + next actions)
   - Partner email (subject + body)
6. A code step splits the LLM output into fields for downstream use.
7. Optional downstream actions:
   - Create a draft email
   - Create a LinkedIn post
   - Create / update a Google Doc

## What is redacted and why

To keep this safe for a public GitHub repo, the exported JSON has been sanitized:

- Meeting notes text: replaced with `<REDACTED_MEETING_NOTES>`
- Email addresses: replaced with `<REDACTED_EMAIL>`
- Document URLs / folder IDs: replaced with placeholders
- n8n credential references: replaced with `<REDACTED_CREDENTIALS>`
- Webhook IDs / workflow IDs / instance IDs: replaced with placeholders

## How to import

1. In n8n, go to Workflows → Import from File
2. Select `n8n_workflow_sanitized.json`
3. Reconnect credentials in your own n8n instance and replace placeholders with your own values.

## Notes

This export is intended as a portfolio artifact (structure + logic), not a runnable workflow without configuration.
