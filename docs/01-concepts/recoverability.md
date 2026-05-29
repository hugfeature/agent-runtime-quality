# Recoverability

## Definition

Recoverability is the ability to restore interrupted execution to a state where the agent can safely continue working toward its original goal.

Recovery is successful only when **execution continuity is preserved** — not merely when a recovery API returns success.

## The Recovery Illusion

The most dangerous recovery failure is one that reports success.

Observed in the [Engram Recovery Failure Case Study](../04-case-studies/engram-runtime-recovery-failure.md):

- The recovery API returned 200
- The checkpoint loaded without error
- But the agent's runtime behavior after recovery was inconsistent with its pre-interruption state

This pattern — **API-level success with runtime-level failure** — is the core challenge of recoverability. It means you cannot trust recovery status codes alone. You need runtime-behavior verification.

## What Needs to Be Recovered

Recovery is not restoring a conversation. It is restoring **working state**:

| Component | What It Means |
|---|---|
| Active goal | What the agent is trying to achieve |
| Completed steps | What has already been done (and verified) |
| Pending steps | What remains |
| Tool state | File locks, API sessions, external resource handles |
| Constraints | What the agent must not do |
| Known risks | What has already gone wrong or might go wrong |
| Decision context | Why the agent chose its current approach |

Raw conversation history does not contain most of this. This is why checkpoint-based recovery exists — to persist **structured recoverable state** rather than raw dialogue.

## Recovery Quality Levels

Not all recovery is equal:

| Level | Description | Risk |
|---|---|---|
| **No recovery** | Agent starts from scratch | Maximum rework, possible divergence |
| **Prompt-based** | Feed conversation history back to the agent | Context window pressure, goal forgetting |
| **Checkpoint-based** | Restore structured state snapshot | Depends on checkpoint completeness |
| **Verified recovery** | Checkpoint + runtime behavior validation | Highest reliability, highest cost |

Most current agent systems operate at Level 1 or 2. The goal is Level 3+.

## Recovery Drift

A subtle failure pattern: the agent successfully recovers its state but then **drifts away from the original goal** during post-recovery execution.

Causes:

- Checkpoint captures state but not intent
- Recovery context is narrower than original context
- The agent re-plans from recovered state and chooses a different strategy

Recovery drift is hard to detect because the recovery itself "worked." The failure only becomes visible steps later when the trajectory diverges.

## Dry-Run vs Runtime Recovery

A key lesson from real-world recovery testing:

```text
Dry-run success does not guarantee runtime recoverability.
```

Dry-run recovery works in controlled conditions — known state, no concurrent mutations, no external dependencies. Runtime recovery must handle:

- File locks held by crashed processes
- Partially written outputs
- External API state that changed during the interruption
- Race conditions in multi-agent environments

## Related

- [Terminology](../00-overview/terminology.md#recovery)
- [Runtime Continuity](runtime-continuity.md)
- [Checkpoint Schema](../02-architecture/checkpoint-schema.md)
- [SIGTERM Recovery](../02-architecture/sigterm-recovery.md)
- [Engram Recovery Failure Case Study](../04-case-studies/engram-runtime-recovery-failure.md)
