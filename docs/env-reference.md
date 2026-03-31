# Environment Variables

## Required Environment Variables

These must be set in your n8n instance for the production error handler workflow to function. No real credentials are included in the JSON files.

| Variable | Used By | Description |
|----------|---------|-------------|
| `RETRY_WORKFLOW_ID` | Production Error Handler | n8n workflow ID of the retry-with-backoff sub-workflow |
| `FALLBACK_WORKFLOW_ID` | Production Error Handler | n8n workflow ID of the fallback-path sub-workflow |
| `DEAD_LETTER_WORKFLOW_ID` | Production Error Handler | n8n workflow ID of the dead-letter-queue sub-workflow |
| `AUDIT_WORKFLOW_ID` | Production Error Handler | n8n workflow ID of the audit-logging sub-workflow |

## Setting Up in n8n

1. Import all four sub-workflows (`retry-with-backoff.json`, `fallback-path.json`, `dead-letter-queue.json`, `audit-logging.json`)
2. Note each workflow's ID from the n8n URL (e.g., `http://localhost:5678/workflow/123` → ID is `123`)
3. Go to **Settings → Environment Variables** (n8n 1.x+)
4. Add each variable above with the corresponding workflow ID
5. Import `production-error-handler.json` — it references the sub-workflows via `$env.VARIABLE_NAME`

## Credential Configuration

The following workflows require credentials configured directly in their nodes (not via env vars):

| Workflow | Node | Credential Type |
|----------|------|-----------------|
| `audit-logging.json` | Write Audit Log | Google Sheets OAuth2 |
| `dead-letter-queue.json` | Write to Dead Letter Sheet | Google Sheets OAuth2 |
| `dead-letter-queue.json` | Slack Alert | Slack OAuth2 or Webhook |
