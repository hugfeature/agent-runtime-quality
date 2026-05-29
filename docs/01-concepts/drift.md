# Drift

## Definition

Drift is a divergence between the agent's original goal state and its current execution state.

It is not simply "the output looks different from the input." Drift is a **runtime state transition failure** — the agent's execution trajectory silently deviates from its intended direction without explicit decision or user authorization.

## Why Drift Is Hard

Drift is difficult because it often looks like productive work.

The agent continues calling tools, generating outputs, and reporting progress. But the underlying goal has shifted. Common causes:

- **Scope expansion**: the agent absorbs adjacent tasks without authorization
- **Goal forgetting**: the original objective fades from the active execution context
- **Tool path deviation**: correct tools are called for the wrong purpose
- **Planner instability**: the planning layer oscillates between competing strategies
- **Context pressure**: long sessions push earlier goals out of the effective context window

## Drift vs Error

Errors are visible. Drift is not.

An error stops execution or produces an obviously wrong result. Drift allows execution to continue — sometimes for many steps — before anyone notices the trajectory has changed.

This makes drift harder to detect, harder to debug, and harder to evaluate than explicit failures.

## Drift Detection Is Not Semantic Similarity

Early approaches treated drift as a text similarity problem:

```text
query → embedding → cosine similarity → drift score
```

This fails because agent drift often manifests as:

- tool call path deviation (semantically similar, behaviorally different)
- state transition anomalies (the text looks fine, but the state graph diverged)
- gradual scope creep (each step is locally reasonable, but the trajectory is wrong)

Effective drift detection requires **multi-signal inference** across semantic, behavioral, and state-level signals. See [Drift False Positive Case Study](../04-case-studies/drift-false-positive.md) for a concrete example.

## Drift in the Failure Taxonomy

Drift overlaps with several failure patterns in the [Runtime Failure Taxonomy](../03-eval/runtime-failure-taxonomy.md):

| Failure Pattern | Relationship to Drift |
|---|---|
| scope_expansion | A form of goal drift |
| goal_forgetting | Direct cause of drift |
| cleanup_spiral | Drift into maintenance instead of goal pursuit |
| exploratory_loop | Drift into exploration without convergence |
| autonomous_refactor | Drift into unauthorized structural changes |

## Key Insight

Drift is not a model capability problem. It is a **runtime governance problem**.

The model may be perfectly capable. But without runtime-level goal tracking, state comparison, and trajectory monitoring, drift is invisible until it has already caused damage.

## Related

- [Terminology](../00-overview/terminology.md#drift)
- [Runtime Continuity](runtime-continuity.md)
- [Runtime Failure Taxonomy](../03-eval/runtime-failure-taxonomy.md)
- [Drift False Positive Case Study](../04-case-studies/drift-false-positive.md)
