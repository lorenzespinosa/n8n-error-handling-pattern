# Architecture Design

## Design Philosophy

Every error handling pattern in this repo follows three principles:

1. **Make failures visible** — configure logging and alerts for the paths you need to monitor
2. **Retry before giving up** — transient failures get exponential backoff
3. **Degrade gracefully** — when all retries fail, fall back to safe defaults

## Pattern Hierarchy

```
Combined Error Handler (example orchestrator)
├── Retry with Backoff (first line of defense)
│   └── On exhaustion → Dead-Letter Queue
├── Fallback Path (when primary service is down)
│   └── Primary → Secondary → Cached Default
└── Audit Logging (runs on every execution)
    └── Example field masking → Structured Google Sheets record
```

## When to Use Each Pattern

| Scenario | Pattern | Why |
|----------|---------|-----|
| API call might timeout | Retry with Backoff | Transient failures resolve themselves |
| Third-party service has outages | Fallback Path | Don't block the whole pipeline |
| Need an example event trail | Audit Logging | Review which sensitive fields are logged and who can access them |
| Critical data must not be lost | Dead-Letter Queue | Failed items preserved for manual recovery |
| All of the above | Combined example | Combines the pattern files for study and testing |
