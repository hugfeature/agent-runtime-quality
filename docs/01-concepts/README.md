# Concepts

Core concepts for Agent runtime reliability.

These are the foundational abstractions that the rest of the repository builds on. Read [Terminology](../00-overview/terminology.md) first for canonical definitions.

- [Runtime Continuity](runtime-continuity.md) — preserving task intent and working state across sessions and interruptions
- [Runtime State Semantics](runtime-state-semantics.md) — what runtime state actually represents vs conversation history
- [Drift](drift.md) — goal-state divergence during execution, the central failure mode
- [Replayability](replayability.md) — reconstructing execution trajectories for debugging and eval
- [Recoverability](recoverability.md) — restoring interrupted execution to a safe, goal-aligned state
