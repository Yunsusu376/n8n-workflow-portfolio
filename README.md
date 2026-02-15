# n8n Workflow (Sanitized) — KPI Extraction & Comms Outputs

This repository contains a **sanitized** export of an n8n workflow for portfolio purposes.  
It demonstrates an automation pipeline that extracts structured KPIs from meeting notes and generates communication-ready outputs.

## What this workflow does (high level)

1. **Manual Trigger** starts the workflow.
2. **Set node** provides meeting notes (redacted in this public version).
3. **LLM step (Extract Data / JSON)** extracts numeric KPIs into a strict JSON structure.
4. **Code step** enriches the extracted KPIs (e.g., simple checks/flags and derived fields).
5. **LLM step (Outputs)** generates three written deliverables:
   - External LinkedIn post (no raw KPI numbers)
   - Internal brief (bullets + risks/open questions + next actions)
   - Partner email (subject + body + highlights + next steps)
6. **Code step** splits the LLM output into fields for downstream use.
7. Optional downstream actions (depending on your own n8n setup):
   - Create a draft email
   - Create a LinkedIn post draft
   - Create / update a Google Doc

## Model configuration (important)

This workflow uses OpenAI chat models through the n8n node `@n8n/n8n-nodes-langchain.openAi`.

- Node: **Extract Data (JSON)** → Model: **gpt-4.1-nano**
- Node: **OutputS** → Model: **gpt-4.1-nano**

Notes:
- You can swap the model in each OpenAI node if needed (e.g., for higher accuracy or longer outputs).
- Keep the extraction step on a model that follows structured output reliably.

## Inputs and outputs

### Input (redacted in public version)
- `meeting_notes`: internal notes text used for KPI extraction  
  In this repo it is replaced with: `<REDACTED_MEETING_NOTES>`

### Output (produced by the workflow)
- `kpi_json`: a structured JSON object extracted from notes
- `linkedin_post`: external-facing summary (no confidential numbers)
- `internal_brief`: internal update bullets + risks + next actions
- `partner_email`: subject + email body (with highlights + next steps)

## What is redacted and why

To keep this safe for a public GitHub repo, the exported JSON has been sanitized:

- Meeting notes text: replaced with `<REDACTED_MEETING_NOTES>`
- Email addresses: replaced with `<REDACTED_EMAIL>`
- Document URLs / folder IDs: replaced with placeholders
- n8n credential references: replaced with `<REDACTED_CREDENTIALS>`
- Webhook IDs / workflow IDs / instance IDs: replaced with placeholders

## How to import into n8n

1. In n8n, go to **Workflows → Import from File**
2. Select `n8n_workflow_sanitized.json`
3. Reconnect credentials in your own n8n instance and replace placeholders with your own values.
4. Run the workflow from the top (Manual Trigger).

## How to make it runnable in your environment

You will need to configure:
- OpenAI credentials in n8n (for the LLM nodes)
- Any optional integrations you want to activate (email / Google Docs / LinkedIn)
- Replace placeholder fields:
  - `<REDACTED_EMAIL>` / `<REDACTED_URL>` / `<REDACTED_FOLDER_ID>` / `<REDACTED_MEETING_NOTES>`

## Notes

This export is intended as a **portfolio artifact** (structure + logic).  
It will not run end-to-end without configuring credentials and replacing placeholders.
