# Architecture Design

## Design Philosophy

Every error handling pattern in this repo follows three principles:

1. **Never fail silently** — every error is captured, logged, and alerted
2. **Retry before giving up** — transient failures get exponential backoff
3. **Degrade gracefully** — when all retries fail, fall back to safe defaults

## Pattern Hierarchy

```
Production Error Handler (orchestrator)
├── Retry with Backoff (first line of defense)
│   └── On exhaustion → Dead-Letter Queue
├── Fallback Path (when primary service is down)
│   └── Primary → Secondary → Cached Default
└── Audit Logging (runs on every execution)
    └── PII masking → Structured log → Alert
```

## When to Use Each Pattern

| Scenario | Pattern | Why |
|----------|---------|-----|
| API call might timeout | Retry with Backoff | Transient failures resolve themselves |
| Third-party service has outages | Fallback Path | Don't block the whole pipeline |
| Need compliance audit trail | Audit Logging | PII-safe records for review |
| Critical data must not be lost | Dead-Letter Queue | Failed items preserved for manual recovery |
| All of the above | Production Error Handler | Combines all patterns |
