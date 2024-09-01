# Error Handling Patterns — Zapier Equivalent

> Conceptual rebuild guide for implementing these patterns in Zapier. Zapier has no public workflow export format — this documents the **logic** so you can rebuild the Zaps manually.

## Retry with Exponential Backoff

**Zapier approach:** Zapier has built-in auto-retry for failed steps (up to 3 times over 24 hours). For custom retry logic with specific backoff timing, you need a workaround.

**Logic (custom approach):**
1. Webhook trigger (Catch Hook)
2. Code by Zapier step: attempt the API call
3. Filter: check if the call succeeded
4. If failed: Webhooks by Zapier → POST back to the same Catch Hook with incremented retry count and a delay parameter
5. Delay by Zapier step (available in some plans) before the webhook POST
6. Filter: if retryCount >= 3, route to Paths for dead-letter handling

**Zapier-specific notes:**
- Zapier's native retry is simpler but doesn't give you control over timing
- Code by Zapier (JavaScript or Python) can implement the retry logic inline
- Zapier Paths can split success/failure flows

## Dead-Letter Queue

**Zapier approach:** A dedicated Zap triggered when items exhaust retries.

**Logic:**
1. Webhook trigger (Catch Hook) receives the failed item
2. Formatter by Zapier: format the dead letter record
3. Google Sheets: add row with timestamp, source, error, original payload
4. Slack: send message to error channel

**Zapier-specific notes:**
- Zapier's built-in task history shows failures, but it's not structured for review queues
- Use Google Sheets or Airtable as your dead-letter store for searchability

## Fallback Path

**Zapier approach:** Use Paths to try primary service first, then fallback.

**Logic:**
1. Webhook trigger
2. Webhooks by Zapier → GET/POST to primary service
3. Path A (success): continue with primary response
4. Path B (failure): Webhooks by Zapier → GET/POST to fallback service
5. Path B sub-path (fallback also fails): Code by Zapier → return cached/default data

**Zapier-specific notes:**
- Zapier Paths are the key feature — they allow conditional branching
- "Filter" steps can check HTTP status codes from webhook responses
- Zapier doesn't have a native "try/catch" — use Paths + Filters as the equivalent

## Audit Logging

**Zapier approach:** A reusable Zap called via webhook that logs execution data with PII masking.

**Logic:**
1. Webhook trigger (Catch Hook) receives execution data
2. Code by Zapier: regex replace for PII masking
   - Emails: `/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g` → `***@***.com`
   - Phones: `/\d{3}[-.]?\d{3}[-.]?\d{4}/g` → keep last 4 digits
3. Google Sheets: add row with masked data + timestamp + source

**Zapier-specific notes:**
- Code by Zapier supports JavaScript and Python — use whichever you're comfortable with
- Formatter by Zapier can do simple text replacements but not regex — use Code for PII masking

---

*Last updated: August 2024*
*These guides describe logic patterns, not UI steps. Zapier updates its interface regularly — consult Zapier's documentation for current step names and configuration.*
