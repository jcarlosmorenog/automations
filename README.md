# n8n Automation Portfolio

Real, importable [n8n](https://n8n.io) workflows that automate repetitive business operations — built to show, not just tell. Each workflow is self-contained: a clear README, the importable JSON (with all credentials and secrets stripped), example payloads, and supporting media.

## Why this exists

Most small teams lose hours every week moving data between forms, email, spreadsheets, and CRM/ATS tools by hand. These workflows replace that manual copy-paste with reliable, AI-assisted automation. This repo is a working portfolio of those builds — every one can be imported into a fresh n8n instance and run.

## Workflows

| Workflow | What it does | Stack |
|---|---|---|
| [Candidate Intake → AI Follow-up](./workflows/candidate-intake-ai-followup) | Receives a candidate intake submission, uses AI to score and summarize the candidate, logs the record to Google Sheets, and sends a personalized follow-up email. | Webhook · OpenAI · Google Sheets · SMTP |

_More workflows are added as folders under [`workflows/`](./workflows)._

## Repository structure

```
workflows/
  <workflow-name>/
    README.md                     # what it does, how to import, how to test
    <workflow-name>.n8n.json      # importable workflow (no secrets)
    payloads/                     # example request bodies for testing
    schema/                       # supporting schemas (e.g. sheet headers)
    media/                        # screenshots and walkthrough videos
```

## How to use any workflow

1. Open the workflow's folder and read its `README.md`.
2. In n8n, create a new workflow → **Import from File** → select the `*.n8n.json`.
3. Wire up your own credentials where the JSON shows `REPLACE_WITH_*` placeholders.
4. Send the example payload from `payloads/` to the webhook to test.

## A note on security

Every shared workflow JSON has its credentials, API keys, document IDs, and personal email addresses removed and replaced with clearly named `REPLACE_WITH_*` placeholders. No secrets are committed to this public repository. Demo data is anonymized.

## Contact

Have a manual workflow you'd like automated? Send it over and I'll suggest one automation to start with.

- 📧 morenocarlos9973@gmail.com
