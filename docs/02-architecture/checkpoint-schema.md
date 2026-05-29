# Structured Checkpoint Schema

## Purpose

A checkpoint is a structured snapshot of recoverable runtime state. It exists so that interrupted execution can resume without losing progress, intent, or context.

A checkpoint is **not** a conversation dump. It is a structured contract between the interruption point and the recovery point.

## Design Principle

```text
Deterministic state comes from runtime/code.
LLM only generates semantic hints.
```

This separation matters because:

- Deterministic fields (modified files, tool call history, step counts) can be verified
- Semantic fields (goal summary, resume hint) cannot — they are the LLM's best interpretation
- Mixing them makes it impossible to distinguish fact from inference during recovery

## Schema

```json
{
  "checkpoint_version": "1.0",
  "task_id": "task-uuid",
  "created_at": "2026-05-15T14:30:00Z",

  "goal": {
    "original": "user's original task description",
    "current": "agent's current interpretation of the goal",
    "drift_flag": false
  },

  "execution_state": {
    "status": "in_progress | completed | failed | interrupted",
    "total_steps": 12,
    "completed_steps": 8,
    "last_success_action": "applied patch to src/recovery.ts",
    "last_failure": null
  },

  "pending_actions": [
    "run integration tests",
    "update changelog",
    "verify build passes"
  ],

  "artifacts": {
    "modified_files": ["src/recovery.ts", "tests/recovery.test.ts"],
    "created_files": [],
    "deleted_files": []
  },

  "context": {
    "constraints": ["do not modify public API surface"],
    "known_risks": ["test database connection may timeout"],
    "decisions_made": [
      "chose retry-with-backoff over circuit-breaker pattern"
    ]
  },

  "resume_hint": "Tests not yet run. Start by running integration test suite, then update changelog if tests pass."
}
```

## Field Categories

| Category | Fields | Source | Verifiable? |
|---|---|---|---|
| **Identity** | task_id, checkpoint_version, created_at | Runtime | ✅ |
| **Goal** | original, current, drift_flag | Mixed — original from user, current from LLM | Partially |
| **Execution** | status, total_steps, completed_steps, last_success_action, last_failure | Runtime | ✅ |
| **Pending** | pending_actions | LLM-generated | ❌ — must verify at recovery time |
| **Artifacts** | modified_files, created_files, deleted_files | Git diff / filesystem | ✅ |
| **Context** | constraints, known_risks, decisions_made | Mixed | Partially |
| **Semantic** | resume_hint | LLM-generated | ❌ |

## Design Tradeoffs

### Completeness vs Size

A checkpoint that captures everything is too large to be useful. A checkpoint that captures too little fails at recovery. The current schema optimizes for:

- Enough to resume without re-reading the entire conversation
- Small enough to fit in a single context window alongside the recovery prompt

### Goal Tracking

The `goal.original` vs `goal.current` split enables drift detection at checkpoint time. If `current` has diverged significantly from `original`, the `drift_flag` can be raised before recovery even begins.

### Why `resume_hint` Is Separate

The resume hint is explicitly marked as LLM-generated because:

- It may be wrong — the LLM might misunderstand what remains
- It should be treated as a suggestion, not a command
- The recovery agent should cross-check it against `pending_actions` and `artifacts`

## What This Schema Does Not Capture

- Tool session state (API tokens, file locks, open connections)
- External system state (database records, deployed artifacts)
- Multi-agent coordination state (other agents' progress)

These require separate mechanisms. The checkpoint schema is scoped to **single-agent execution state**.

## Related

- [Recoverability](../01-concepts/recoverability.md)
- [SIGTERM Recovery](sigterm-recovery.md)
- [Runtime Event Stream](runtime-event-stream.md)
