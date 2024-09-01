# Error Handling Patterns — Make Equivalent

> Conceptual rebuild guide for implementing these patterns in Make (formerly Integromat). Not step-by-step UI instructions — Make's interface changes frequently. This documents the **logic** so you can build it yourself.

## Retry with Exponential Backoff

**Make concept:** Use a Router with a fallback route. The fallback route contains a Sleep module followed by a loop back to the original HTTP module.

**Logic:**
1. HTTP module attempts the request
2. On error → Router sends to fallback route
3. Sleep module: `pow(2, retryCount - 1)` seconds
4. Increment retry counter (Set Variable)
5. If retryCount < 3 → loop back to HTTP module
6. If retryCount >= 3 → route to Dead Letter scenario

**Make-specific notes:**
- Make has built-in retry on HTTP modules (Settings → Number of retries) — use this for simple cases
- For custom backoff logic, you need the Router + Sleep + Variable approach
- Make's "Break" error handler can catch specific HTTP status codes

## Dead-Letter Queue

**Make concept:** A separate scenario triggered by webhook when items exhaust retries.

**Logic:**
1. Webhook trigger receives failed item payload
2. Set Variables module formats the dead letter record (timestamp, source, error, original payload)
3. Google Sheets / Airtable module writes the record
4. Slack module sends alert notification

**Make-specific notes:**
- Use Make's Data Store as an alternative to Google Sheets for structured queue data
- Data Store supports filtering and searching — better for reviewing failed items

## Fallback Path

**Make concept:** Router with multiple routes, each trying a different service.

**Logic:**
1. HTTP module → primary service
2. Router with error filter → sends to Route 2 on failure
3. Route 2: HTTP module → fallback service
4. Route 3 (final): Set Variables → return cached/default data
5. Aggregator combines whichever route succeeded

**Make-specific notes:**
- Make's Router can filter by error type — route 503s differently from 401s
- Use "Resume" error handler to continue execution after a failed module

## Audit Logging

**Make concept:** A reusable scenario called via webhook at the end of any workflow.

**Logic:**
1. Webhook receives execution data
2. Text Parser / Set Variables masks PII (regex replace on emails, phones)
3. Google Sheets / Airtable writes the audit record
4. Optional: Slack notification for critical events

**Make-specific notes:**
- Make's built-in execution history provides some audit trail, but it's not PII-masked
- For compliance, you need the explicit audit logging scenario with PII masking

---

*Last updated: August 2024*
*These guides describe logic patterns, not UI steps. Make updates its interface regularly — consult Make's documentation for current module names and configuration.*
