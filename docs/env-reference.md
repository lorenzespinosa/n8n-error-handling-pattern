# Environment Variables

## Required Environment Variables

These must be set in your n8n instance for the workflows to function. No real credentials are included in the JSON files.

| Variable | Used By | Description |
|----------|---------|-------------|
| `AIRTABLE_API_KEY` | Dead-Letter Queue, Audit Logging | Airtable PAT for writing records |
| `AIRTABLE_BASE_ID` | Dead-Letter Queue, Audit Logging | Base ID for your error tracking base |
| `SLACK_WEBHOOK_URL` | Dead-Letter Queue, Fallback Path | Slack incoming webhook for alerts |
| `FALLBACK_API_URL` | Fallback Path | URL of your secondary/fallback service |
| `PRIMARY_API_URL` | Retry with Backoff, Fallback Path | URL of your primary service |

## Setting Up in n8n

1. Go to **Settings → Environment Variables** (n8n 1.x+)
2. Add each variable above with your actual values
3. In workflow JSON, references use `$env.VARIABLE_NAME` syntax
4. Never hardcode credentials in workflow JSON — always use env vars

## Optional Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `MAX_RETRIES` | `3` | Maximum retry attempts before dead-letter |
| `BACKOFF_MULTIPLIER` | `2` | Exponential backoff base (seconds) |
| `DLQ_TABLE_NAME` | `Dead Letter Queue` | Airtable table name for failed items |
| `AUDIT_TABLE_NAME` | `Audit Log` | Airtable table name for audit records |
