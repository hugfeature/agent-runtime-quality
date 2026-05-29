# Runtime Event Stream

## Purpose

The event stream is the runtime's **observable nervous system**. Every significant state transition emits an event. These events serve three downstream consumers:

1. **Observability** — what is happening right now?
2. **Replay** — what happened, and can we reproduce it?
3. **Eval** — did the runtime behave correctly?

Without a structured event stream, all three are guesswork.

## Storage

Append-only JSONL, one file per session:

```text
.engram/events/<session-id>.jsonl
```

JSONL is chosen because:

- Append-only writes survive crashes (no partial JSON corruption)
- Each line is independently parseable
- Streaming-friendly — tail -f works for live monitoring
- No schema migration needed — new event types are additive

## Event Schema

Every event shares a common envelope:

```json
{
  "ts": "2026-05-15T14:30:00.123Z",
  "seq": 1,
  "session_id": "session-uuid",
  "type": "ToolCalled",
  "payload": { }
}
```

| Field | Description |
|---|---|
| **ts** | ISO 8601 timestamp with millisecond precision |
| **seq** | Monotonically increasing sequence number within session |
| **session_id** | Links events to a specific execution session |
| **type** | Event type identifier |
| **payload** | Type-specific data |

## Event Types

### Lifecycle Events

| Type | When | Key Payload Fields |
|---|---|---|
| `TaskStarted` | Agent receives a new goal | `goal`, `constraints` |
| `TaskCompleted` | Agent reports completion | `summary`, `artifacts` |
| `TaskFailed` | Agent gives up or hits unrecoverable error | `error`, `last_action` |
| `InterruptDetected` | SIGTERM, timeout, or manual stop | `signal`, `graceful` |

### Execution Events

| Type | When | Key Payload Fields |
|---|---|---|
| `ToolCalled` | Agent invokes a tool | `tool_name`, `parameters`, `result_status` |
| `ToolRetried` | A tool call is retried | `tool_name`, `attempt`, `reason` |
| `PlanGenerated` | Planner produces a new action plan | `steps`, `reasoning` |
| `PlanRevised` | Planner changes an existing plan | `old_steps`, `new_steps`, `trigger` |
| `PatchApplied` | Code change applied to filesystem | `file`, `diff_summary` |

### State Events

| Type | When | Key Payload Fields |
|---|---|---|
| `CheckpointSaved` | Structured state snapshot persisted | `checkpoint_id` |
| `RestoreStarted` | Recovery from checkpoint begins | `checkpoint_id`, `source` |
| `RestoreCompleted` | Recovery finished | `checkpoint_id`, `success`, `drift_detected` |
| `ContextPressureHigh` | Context window utilization exceeds threshold | `usage_pct`, `strategy` |

### Anomaly Events

| Type | When | Key Payload Fields |
|---|---|---|
| `RetryStorm` | Retry count exceeds threshold in time window | `tool_name`, `count`, `window_sec` |
| `DriftSignal` | Drift detection fires | `signal_type`, `score`, `threshold` |
| `ScopeExpansion` | Agent modifies files outside declared scope | `files`, `original_scope` |
| `GoalDivergence` | Current goal diverges from original | `original`, `current`, `similarity` |

## Design Decisions

### Why Sequence Numbers?

Timestamps alone are insufficient because:

- Multiple events can share the same millisecond
- Clock drift in distributed environments can reorder events
- Sequence numbers provide a total order within a session

### What Not to Log

- Raw LLM prompts and completions (too large, privacy concerns)
- File contents (use git diff references instead)
- Intermediate reasoning chains (log decisions, not thought process)

The event stream captures **what happened**, not **everything the model said**.

### Granularity Tradeoff

Too few events → blind spots in replay and eval.
Too many events → noise drowns signal, storage grows unbounded.

Rule of thumb: log events at **state transition boundaries**, not at every internal step. If the agent's externally visible state didn't change, it probably shouldn't be an event.

## Downstream Usage

```text
Event Stream
    ├── → Live Dashboard (observability)
    ├── → Trace Extraction (replay)
    ├── → Anomaly Detection (drift, retry storms)
    └── → Fixture Generation (eval)
```

## Related

- [Runtime Observability](runtime-observability.md)
- [Checkpoint Schema](checkpoint-schema.md)
- [Replayability](../01-concepts/replayability.md)
- [Eval Methodology](../03-eval/eval-methodology.md)
