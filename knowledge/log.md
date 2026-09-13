# Knowledge Action Log

This file records knowledge-base actions for audit and linting. It is not a
startup memory file and does not need to be read for every run.

## Entry format

Append one concise entry when durable knowledge is created, changed, reconciled,
or linted:

```yaml
- timestamp: YYYY-MM-DDTHH:MM:SSZ
  run_id: run-identifier
  action: create | update | reconcile | lint
  paths:
    - knowledge/domain/self/topic.md
  summary: "What durable knowledge action occurred"
  evidence: "Command, source, or artifact supporting the action"
```

Keep entries factual and concise. Do not copy the conversation or routine task
progress into this log.
