# n8n-workflow-portfolio

## Overview
This repository contains a **sanitized n8n workflow** for portfolio purposes.
The workflow turns structured meeting notes into three deliverables and routes them to different channels:
1) an external LinkedIn post
2) an internal brief
3) a partner email draft

Goal: reduce repetitive manual writing and improve delivery speed and consistency, while keeping outputs audience-appropriate.

## Repository contents
- `n8n_workflow_sanitized.json` — workflow export (sanitized for GitHub)
- `README.md`

## What the workflow does (Inputs → Outputs)

### Input
- **Meeting notes** are provided as a single text field (`meeting_notes`) to ensure repeatable runs without relying on an external data source.

### Outputs
- **LinkedIn post** (public-facing, no sensitive details / no raw KPI numbers)
- **Internal brief** (bullets: updates, risks, next actions)
- **Partner email draft** (subject + body, with highlights and a short next-steps paragraph)

## Workflow logic (high-level)
The workflow is organized as a clear pipeline:

1. **Manual Trigger** to start the workflow.
2. **Edit/Set Fields** to store `meeting_notes` as a single standardized text input.
3. **Extract Data (JSON)**: use an OpenAI model via n8n to extract **ONLY numeric KPI information** from notes into **strict JSON** (no prose, no markdown).
4. **Build KPI Dataset**: compute simple checks / derived fields (e.g., variance vs plan if available).
5. **Generate Outputs**: use an OpenAI model to generate three channel-specific texts with strict formatting markers:
   - `[LINKEDIN]`
   - `[INTERNAL_BRIEF]`
   - `[PARTNER_EMAIL]`
6. **Split outputs (code)**: parse the model response into separate fields (linkedin text, internal brief, email subject/body).
7. **Delivery nodes** (optional, depending on your setup):
   - Post to **LinkedIn**
   - Create an **email draft** (Outlook)
   - Create + update an **internal brief** doc (Google Docs)

## Model & prompting (important)
- The workflow uses **n8n’s OpenAI node** (LangChain OpenAI integration).
- **Model name is configurable in the n8n node settings** (this repo does not hardcode a specific model).
- Prompts are designed to prevent hallucinations and sensitive disclosure:
  - “Do not invent facts or numbers.”
  - “All numbers must come from the provided JSON. If missing, write ‘Not provided’.”
  - LinkedIn output explicitly disallows raw KPI numbers and confidential details.

## How to import and run in n8n
1. Open n8n → **Workflows** → **Import from File**
2. Select `n8n_workflow_sanitized.json`
3. Configure credentials in n8n (OpenAI / LinkedIn / Outlook / Google Docs as needed)
4. Replace any placeholder fields (e.g., `<REDACTED_...>`) with your own values
5. Run the workflow from top to bottom (Manual Trigger)

## Data sharing & privacy
- This repository includes **workflow logic only** (no datasets / no meeting notes).
- The workflow JSON is sanitized: emails, URLs, folder IDs, credentials, and internal notes are replaced with placeholders.
- Before posting publicly (e.g., LinkedIn), a **human review step** is recommended to ensure:
  - no sensitive internal info is disclosed
  - wording does not over-claim
  - metric definitions/method notes are appropriate for external audiences

## Limitations / future improvements
- Add a formal “human approval” gate before any external posting.
- Add a reusable metric glossary / method notes component.
- Add automated checks for missing fields and inconsistent definitions across sources.
- Add logging/monitoring and versioning for prompt changes.
