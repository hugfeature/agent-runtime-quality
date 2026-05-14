# Structured Checkpoint Schema

Structured checkpoint is the core abstraction for interruption recovery.

Example schema:

```json
{
  "task_id": "",
  "goal": "",
  "status": "",
  "last_success_action": "",
  "last_failure": "",
  "pending_actions": [],
  "modified_files": [],
  "resume_hint": ""
}
```

Important rule:

Deterministic state should come from runtime/code.

LLM should only generate semantic hints.
