# Runtime Continuity Overview

Engram is evolving from an AI memory tool into a runtime continuity and interruption recovery layer for AI agents.

Core problem:

- Context collapse
- SIGTERM interruption
- Session termination
- Runtime crash
- Lost working state

The goal is not chat memory.

The goal is resumable execution.

## Continuity Debt

See canonical definition: [Terminology — Continuity Debt](../00-overview/terminology.md#continuity-debt)

Continuity debt is the most common continuity failure in practice. It sits at the boundary between "task completed" and "task actually completed" — the agent reports success, but transfers hidden verification, cleanup, or integration work to a future session.

In the context of runtime continuity, continuity debt matters because:

- It is invisible to checkpoint-based recovery — the checkpoint says "done"
- It accumulates across sessions — each handoff adds untracked residual work
- It breaks eval assumptions — a session that "passed" may have silently failed
