# Terminology

This repository uses a small set of terms consistently.

## Runtime Continuity

The agent's ability to preserve task intent, working state, and execution direction across a long-running session or after interruption.

## Continuity Debt

English:

Continuity debt is a runtime failure pattern where the system appears to complete the current task, but silently transfers the real completion work to a future user session.

Chinese:

Continuity debt 指的是：系统表面完成了当前任务，但把真正的 completion work 转嫁给未来的用户 session。

This is different from ordinary incomplete work. The dangerous part is that the current session reports completion while leaving hidden verification, cleanup, recovery, or decision work for a later session.

## Runtime State

The state required to continue work safely. It includes active goal, completed steps, pending steps, tool outputs, constraints, and known risks.

Runtime state is not the same as raw conversation history.

## Checkpoint

A structured snapshot of recoverable runtime state. A useful checkpoint should support recovery, debugging, and replay-oriented analysis.

## Recovery

The process of restoring interrupted execution from checkpointed state. Recovery is successful only when execution continuity is preserved, not merely when a recovery API returns success.

## Replay

The reconstruction of a prior execution trajectory for debugging, regression testing, or eval generation.

## Drift

A divergence between the original goal state and the agent's current execution state.

Drift is not just semantic dissimilarity. It can appear as tool path deviation, planner instability, scope expansion, or state transition errors.

## Runtime Observability

The capture and interpretation of runtime events, tool calls, state transitions, metrics, and anomalies.

## Runtime Eval

Evaluation that measures behavior across stateful, long-running, recoverable, and replayable execution rather than single-turn answer quality.
