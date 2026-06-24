# Candidate Intake → AI Summary → Sheet Update → Follow-up Email

A practical n8n proof artifact for staffing and recruiting teams.

> One of my [n8n workflows for day-to-day stuff](../../README.md).

## Demo

<!-- Add a screenshot of the workflow canvas and/or a short walkthrough video to ./media,
     then reference them here. Example:
![Workflow canvas](./media/workflow-canvas.png)
[▶ Watch the 60-second walkthrough](./media/walkthrough.mp4) -->

_Screenshots and a walkthrough video live in [`./media`](./media)._

## Files in this folder

| Path | What it is |
|---|---|
| `candidate-intake-ai-followup.n8n.json` | The importable n8n workflow (credentials and IDs removed). |
| `payloads/sample-candidate-payload.json` | Example webhook request body for testing. |
| `schema/google-sheets-schema.csv` | Header row for the destination Google Sheet. |
| `media/` | Screenshots and walkthrough video. |

## What this workflow proves

This workflow automates a common recruiting operations process:

1. Receive a candidate intake submission over a webhook.
2. Normalize the intake fields and build the AI request.
3. Use AI to score the candidate and generate a recruiter-ready summary.
4. Parse the AI response into clean, flat fields.
5. Append the candidate record to Google Sheets.
6. Send a personalized follow-up email to the candidate.
7. Return a confirmation response.

The business value is simple: less manual admin, faster response time, fewer missed candidates, and cleaner records.

## Workflow nodes

| Step | Node | Type | Purpose |
|---|---|---|---|
| 1 | Webhook | `webhook` | Receives candidate intake JSON (`POST /candidate-intake`) from a form, website, ATS, or test request. |
| 2 | Parse & Context | `code` | Normalizes incoming fields into a consistent candidate object and builds the OpenAI request payload. |
| 3 | LLM Call | `httpRequest` | Calls the OpenAI Chat Completions API (`gpt-4o-mini`, JSON mode) to score and summarize the candidate. |
| 4 | Humanize | `code` | Parses the AI JSON response and merges it with the candidate data into flat downstream fields. |
| 5 | Append row in sheet | `googleSheets` | Appends a structured candidate row to Google Sheets. |
| 6 | Send an Email | `emailSend` (SMTP) | Sends a personalized follow-up email to the candidate. |
| 7 | Respond to Webhook | `respondToWebhook` | Returns a concise JSON success response. |

## What the AI returns

The model is asked to return a single JSON object with exactly these fields:

- `fit_score` — integer 1–10 (10 = perfect match)
- `fit_reason` — one sentence explaining the score
- `summary` — 2–3 sentence professional candidate summary
- `email_subject` — subject line for the follow-up email
- `email_body` — 3–4 sentence warm, professional follow-up (does not mention the fit score)

## Required credentials

- **OpenAI API key** — provided to the HTTP Request node via an n8n Header Auth credential (`Authorization: Bearer ...`).
- **Google Sheets credential** — service account with edit access to the target spreadsheet.
- **SMTP credential** — used by the Send an Email node.

Do not hardcode real credentials in the shared workflow JSON. Use n8n credentials or environment variables.

## Google Sheet columns

Use the included [`schema/google-sheets-schema.csv`](./schema/google-sheets-schema.csv) as the first (header) row in your spreadsheet. The columns are:

`Submitted At`, `Name`, `Email`, `Phone`, `Role Applied`, `Experience (Yrs)`, `Skills`, `LinkedIn`, `Cover Note`, `Fit Score`, `Fit Reason`, `AI Summary`, `Status`, `AI Processed At`

## Test payload

Use the included [`payloads/sample-candidate-payload.json`](./payloads/sample-candidate-payload.json) when testing the webhook.

## How to import

1. Open n8n.
2. Create a new workflow.
3. Use **Import from File** and select `candidate-intake-ai-followup.n8n.json`.
4. Open the Webhook node and copy the test URL.
5. Run the workflow in test mode.
6. Send the sample payload as a POST request.
7. Connect your real OpenAI, Google Sheets, and SMTP credentials.
8. Replace the placeholder spreadsheet document ID and the `fromEmail` / `toEmail` addresses.
9. Publish the workflow and use the production webhook URL.

## Example test request

```bash
curl -X POST "YOUR_N8N_TEST_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  -d @payloads/sample-candidate-payload.json
```

## Safety notes

- The Send an Email node **sends email automatically**. While testing, point `toEmail` at your own inbox (or swap the node for a Gmail "create draft" action) so you don't email real candidates.
- Use header authentication on the webhook before production use.
- Avoid storing sensitive candidate data in public logs or public workflow exports.
- Anonymize any demo data before publishing screenshots or videos.
