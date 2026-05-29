# Case Studies

Concrete failure analyses. These are the most important artifacts in the repository because they connect runtime theory to observable failure behavior.

## Drift Detection & Scoring

- [Drift False Positive](drift-false-positive.md) — why linear signal aggregation produces systematic false positives in runtime drift detection
- [Semantic Divergence Upgrade](semantic-divergence-false-positive-upgrade.md) — F1 0.727→0.829: two-layer hybrid architecture, goal clarity gate, and the lesson that rabbit holes ≠ drift

## Agent Behavioral Failures

- [Directive Override → Scope Explosion](directive-override-scope-explosion.md) — agent completes authorized fix, then autonomously expands into 27 events of unauthorized system maintenance
- [Goal Misunderstanding → 157 Tool Calls](goal-misunderstanding-rabbit-hole.md) — agent misinterprets "buttons can't click" as "build a browser preview system from scratch"

## Recovery & Continuity

- [Engram Runtime Recovery Failure](engram-runtime-recovery-failure.md) — API-level recovery success with runtime-level behavior inconsistency
- [Engram Summary](engram-summary.md) — evolution from memory tool to runtime continuity layer

## Spec & Design

- [Spec Drift: Phantom UI](spec-drift-phantom-ui.md) — implementation diverges from design specification

## Recommended Case Study Shape

```text
# Case Name

## Background
## Failure Symptom
## Runtime Trace
## Root Cause
## Why Existing Eval Missed It
## Recovery Behavior
## Infrastructure Lesson
## Follow-up Eval Fixture
```
