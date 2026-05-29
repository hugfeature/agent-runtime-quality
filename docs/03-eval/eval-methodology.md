# Eval Methodology

## Core Principle

Agent eval is not prompt eval.

Prompt eval measures single-turn answer quality. Runtime eval measures **behavior across stateful, long-running, recoverable execution**.

The question is not "did the agent produce a correct answer?" but:

- Did the agent stay on goal across 50+ steps?
- Did recovery actually restore working state?
- Did drift go undetected?
- Did the agent claim completion while leaving hidden work?

## The Failure-to-Fixture Pipeline

The most valuable eval assets come from production failures, not synthetic benchmarks.

```text
Production Failure
    ↓
Signal Capture — what runtime signals were visible before/during/after?
    ↓
Trace Extraction — isolate the minimal replayable trajectory
    ↓
Fixture Construction — define input state, expected behavior, failure criteria
    ↓
Metric Definition — what specifically should be measured?
    ↓
Regression Suite — run against future versions to prevent recurrence
```

Each step adds structure. The raw failure becomes a permanent quality asset.

### Step 1: Signal Capture

Before a failure can become a fixture, you need to know **what signals were visible**.

Runtime signals include:

| Signal Type | Examples |
|---|---|
| Semantic | goal-output divergence, instruction adherence decay |
| Behavioral | tool call pattern anomalies, retry storms, loop detection |
| State | checkpoint inconsistency, state graph divergence |
| Temporal | step count anomalies, time-to-completion drift |
| Structural | scope expansion, unauthorized file mutations |

Not all failures produce all signal types. The signal profile of a failure determines what kind of eval fixture it becomes.

### Step 2: Trace Extraction

A full production session may be thousands of steps. The eval fixture needs the **minimal trajectory that reproduces the failure**.

Extraction criteria:

- Include the goal state and initial context
- Include the decision point where the trajectory diverged
- Include enough preceding steps to establish the expected path
- Exclude unrelated tool calls and side conversations

### Step 3: Fixture Construction

A fixture is a structured test case:

```json
{
  "fixture_id": "drift-false-positive-001",
  "failure_type": "drift",
  "source": "production session 2026-04-15",
  "input_state": {
    "goal": "...",
    "context": "...",
    "tools_available": ["..."]
  },
  "trajectory": [
    {"step": 1, "action": "...", "result": "..."},
    {"step": 2, "action": "...", "result": "..."}
  ],
  "expected_behavior": "agent should have ...",
  "actual_behavior": "agent instead ...",
  "failure_criteria": ["goal_forgotten", "scope_expanded"],
  "eval_signals": ["semantic_divergence > 0.7", "scope_mutation_detected"]
}
```

### Step 4: Metric Definition

Each failure type needs specific metrics:

| Failure Type | Primary Metric | What It Measures |
|---|---|---|
| Drift | Goal alignment score over time | Does the agent stay on track? |
| Continuity debt | Hidden work ratio | How much undisclosed work remains? |
| Recovery failure | Post-recovery behavior consistency | Does the agent behave correctly after recovery? |
| Replay mismatch | Trajectory divergence rate | Does replay reproduce the original path? |
| Context collapse | Instruction retention rate | Does the agent remember constraints over long sessions? |

### Step 5: Regression Suite

Fixtures accumulate into a regression suite that runs against each new version:

- New model version → run drift fixtures → compare goal alignment scores
- New recovery mechanism → run recovery fixtures → verify behavior consistency
- New checkpoint format → run replay fixtures → measure trajectory divergence

## What Makes Agent Eval Different

| Dimension | Traditional Eval | Agent Runtime Eval |
|---|---|---|
| Unit of evaluation | Single response | Multi-step trajectory |
| State | Stateless | Stateful, mutable |
| Time horizon | Instant | Minutes to hours |
| Failure visibility | Immediate | Delayed, sometimes invisible |
| Ground truth | Known correct answer | Expected behavioral trajectory |
| Reproducibility | High | Low without replay infrastructure |

## Eval Anti-Patterns

- **Synthetic-only fixtures**: easy to construct, but miss the failure distributions that matter in production
- **Single-metric eval**: drift and recovery quality cannot be captured in one number
- **Offline-only eval**: runtime failures are state-dependent — they need runtime conditions to reproduce
- **Pass/fail scoring**: most runtime behaviors exist on a spectrum — binary scoring hides degradation trends

## Current Gaps

Honest assessment of what is not yet solved:

- **Eval for multi-agent coordination failures**: no established methodology
- **Automated fixture generation**: still manual extraction from production traces
- **Calibration**: what score threshold means "acceptable reliability"?
- **Cost**: runtime eval is expensive — each fixture requires actual agent execution

## Related

- [Runtime Failure Taxonomy](runtime-failure-taxonomy.md)
- [Drift](../01-concepts/drift.md)
- [Replayability](../01-concepts/replayability.md)
- [Recoverability](../01-concepts/recoverability.md)
- [Drift False Positive Case Study](../04-case-studies/drift-false-positive.md)
