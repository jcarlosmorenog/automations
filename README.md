# n8n Workflows for Day-to-Day Stuff

A growing collection of practical, importable [n8n](https://n8n.io) workflows that automate the small, repetitive tasks that quietly eat up a workday. Each one is self-contained: a clear README, the importable JSON (with all credentials and secrets stripped), and example payloads — ready to drop into your own n8n instance and run.

## Why this exists

So much daily work is just moving the same data between a form, an email, a spreadsheet, and the next tool — by hand, over and over. These workflows take those chores off your plate: capture something once, let automation (and a bit of AI where it helps) do the routing, summarizing, logging, and follow-up. Nothing here is a toy demo — every workflow is a real, working build you can import and use today.

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
```

## How to use any workflow

1. Open the workflow's folder and read its `README.md`.
2. In n8n, create a new workflow → **Import from File** → select the `*.n8n.json`.
3. Wire up your own credentials where the JSON shows `REPLACE_WITH_*` placeholders.
4. Send the example payload from `payloads/` to the webhook to test.

## A note on security

Every shared workflow JSON has its credentials, API keys, document IDs, and personal email addresses removed and replaced with clearly named `REPLACE_WITH_*` placeholders. No secrets are committed to this public repository. Demo data is anonymized.

## Contact

Got a repetitive task you'd love to stop doing by hand? Tell me about it and I'll suggest one automation to start with.

- 📧 morenocarlos9973@gmail.com
