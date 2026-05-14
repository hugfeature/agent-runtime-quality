# Runtime Event Stream

Agent continuity depends on runtime events.

Minimal implementation:

```text
.engram/events.jsonl
```

Example:

```json
{"ts": 1, "type": "TaskStarted"}
{"ts": 2, "type": "ToolCalled"}
{"ts": 3, "type": "PatchApplied"}
```

Recommended events:

- TaskStarted
- ToolCalled
- PatchGenerated
- PatchApplied
- RetryTriggered
- ContextPressureHigh
- InterruptDetected
- CheckpointSaved
- RestoreStarted

Do not over-engineer early.

Start with append-only JSONL logs.
