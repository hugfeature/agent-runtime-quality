# Reading Path

Use this path if you are reading the repository for the first time.

## 1. Understand The Position

Read:

- [Positioning](positioning.md)
- [Terminology](terminology.md)

Goal:

Understand why the repository treats agents as long-running runtime systems instead of chatbot wrappers.

## 2. Learn The Core Concepts

Read:

- [Runtime Continuity](../01-concepts/runtime-continuity.md)
- [Runtime State Semantics](../01-concepts/runtime-state-semantics.md)
- [Drift](../01-concepts/drift.md)
- [Replayability](../01-concepts/replayability.md)
- [Recoverability](../01-concepts/recoverability.md)

Goal:

Understand the difference between conversation memory, recoverable state, and execution continuity. Understand why drift, replay, and recovery are first-class runtime problems.

## 3. Inspect The Runtime Architecture Layer

Read:

- [Runtime Event Stream](../02-architecture/runtime-event-stream.md)
- [Checkpoint Schema](../02-architecture/checkpoint-schema.md)
- [Runtime Observability](../02-architecture/runtime-observability.md)

Goal:

Understand what signals need to be captured before failures can be replayed or evaluated.

## 4. Study The Eval Layer

Read:

- [Runtime Failure Taxonomy](../03-eval/runtime-failure-taxonomy.md)
- [Eval Methodology](../03-eval/eval-methodology.md)
- [Runtime Reliability Roadmap](../03-eval/runtime-reliability-roadmap.md)

Goal:

Understand how runtime failures can become repeatable evaluation assets. Learn the failure-to-fixture pipeline.

## 5. Read The Case Studies

Read:

- [Drift False Positive](../04-case-studies/drift-false-positive.md)
- [Engram Runtime Recovery Failure](../04-case-studies/engram-runtime-recovery-failure.md)
- [Spec Drift: Phantom UI](../04-case-studies/spec-drift-phantom-ui.md)

Goal:

See how abstract runtime reliability concepts show up in concrete failures.

## 6. Use Research Notes As Background

Read selectively:

- [Agent Runtime Infra Research Map 2026](../05-research-notes/agent-runtime-infra-research-map-2026.md)
- [Runtime Wars 2026](../05-research-notes/runtime-wars-2026.md)
- [Agent Runtime Learning Path](../05-research-notes/agent-runtime-learning-path.md)

Goal:

Understand the broader industry and learning context behind the repository.
