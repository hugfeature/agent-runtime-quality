# Replayability

## Definition

Replayability is the ability to reconstruct a prior execution trajectory for debugging, regression testing, or eval generation.

A replayable runtime can answer:

- What happened?
- In what order?
- Why did the agent make that decision at that step?
- Can we reproduce the failure?

## Why Replayability Matters

Without replayability, runtime failures are **one-shot events** — they happen, someone notices (or not), and the evidence disappears with the session.

This creates three problems:

1. **Debugging is guesswork**: without a replayable trace, engineers reconstruct failures from memory and logs, missing subtle state transitions
2. **Eval has no ground truth**: you cannot build regression tests from failures you cannot reproduce
3. **Reliability cannot improve systematically**: each failure is treated as an isolated incident instead of a data point in a failure distribution

## Replayability vs Logging

Logging captures events. Replayability captures **executable trajectories**.

| Capability | Logging | Replayability |
|---|---|---|
| What happened | ✅ | ✅ |
| In what order | Partially | ✅ |
| Tool inputs and outputs | Sometimes | ✅ |
| State transitions | Rarely | ✅ |
| Can reproduce the failure | ❌ | ✅ |
| Can generate eval fixtures | ❌ | ✅ |

The gap matters because agent failures are often **state-dependent** — the same tool call sequence can succeed or fail depending on intermediate state. Logs alone cannot capture this.

## What Makes a Trajectory Replayable

A replayable trajectory needs:

- **Goal state**: what the agent was trying to achieve
- **Step sequence**: ordered list of actions with inputs and outputs
- **State snapshots**: intermediate state at key decision points
- **Tool call records**: exact parameters, responses, and side effects
- **Decision context**: what information the agent had when it chose each action
- **Failure point**: where and how the trajectory diverged from expected behavior

## Replay Mismatch

A common failure pattern: replayed execution diverges from the original trajectory even with the same inputs.

Causes:

- Non-deterministic model outputs
- External state changes between record and replay
- Tool side effects that cannot be safely re-executed
- Missing intermediate state that the replay engine cannot reconstruct

Replay mismatch is itself a failure signal — it indicates that the runtime's state model is incomplete.

## Replayability as Eval Infrastructure

The most valuable use of replayability is **converting production failures into eval fixtures**:

```text
production failure → trace capture → replay verification → eval fixture → regression suite
```

This pipeline turns runtime incidents into permanent quality assets. Without replayability, this pipeline breaks at the first step.

## Related

- [Terminology](../00-overview/terminology.md#replay)
- [Runtime Event Stream](../02-architecture/runtime-event-stream.md)
- [Checkpoint Schema](../02-architecture/checkpoint-schema.md)
- [Engram Recovery Failure Case Study](../04-case-studies/engram-runtime-recovery-failure.md)
