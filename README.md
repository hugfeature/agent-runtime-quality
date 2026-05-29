# Agent Runtime Quality

Research notes on Agent runtime evaluation, reliability, recovery, replay, and observability.

The core assumption of this repository:

```text
AI agents behave less like chatbots and more like long-running distributed systems.
```

The focus is not model capability, prompt tricks, or generic agent demos. The focus is:

- Can an agent preserve task continuity across long sessions?
- Can interrupted execution be recovered without hidden state corruption?
- Can runtime behavior be replayed, inspected, and evaluated?
- Can drift, false positives, and recovery failures be turned into reusable eval assets?

## Positioning

This repository is a public research portfolio for Agent evaluation and reliability engineering.

It is meant to demonstrate:

- runtime failure analysis
- eval-oriented thinking
- reliability taxonomy design
- observability and replay awareness
- judgment about where production agents actually fail

It is not intended to be a general article archive. Personal career notes, training logs, and older scratch notes are kept under `docs/archive/`.

## Core Thesis

Production agents fail at runtime.

Typical failure modes include:

- goal drift
- context collapse
- continuity debt
- recovery drift
- hallucinated state
- replay mismatch
- state corruption
- tool misuse
- false positive runtime alarms
- long-session instability

These failures are not fully visible in short demos or offline prompt evals. They need runtime traces, structured checkpoints, replayable trajectories, and failure-oriented eval fixtures.

## Repository Structure

```text
agent-runtime-quality/
├── docs/
│   ├── 00-overview/          # positioning, terminology, reading path
│   ├── 01-concepts/          # core runtime concepts
│   ├── 02-architecture/      # event stream, checkpoint, recovery, observability
│   ├── 03-eval/              # failure taxonomy and eval roadmap
│   ├── 04-case-studies/      # concrete runtime failure analyses
│   ├── 05-research-notes/    # research maps and longer industry notes
│   └── archive/              # personal notes and old scratch entries
└── README.md
```

## Suggested Reading Path

Start here:

1. [Reading Path](docs/00-overview/reading-path.md)
2. [Positioning](docs/00-overview/positioning.md)
3. [Terminology](docs/00-overview/terminology.md)
4. [Runtime Failure Taxonomy](docs/03-eval/runtime-failure-taxonomy.md)
5. [Drift False Positive Case Study](docs/04-case-studies/drift-false-positive.md)
6. [Engram Runtime Recovery Failure](docs/04-case-studies/engram-runtime-recovery-failure.md)

## Core Areas

### Runtime Continuity

How an agent preserves its working state, active goal, intermediate decisions, and execution intent across long-running sessions.

### Runtime State

The recoverable state required to continue work. This is not the same as raw conversation history.

### Checkpoint and Recovery

How interrupted execution is captured, restored, and validated after recovery.

### Replay

How failed trajectories can be reconstructed for debugging, regression testing, and eval generation.

### Observability

How runtime events, tool calls, state transitions, and anomalies become inspectable signals.

### Runtime Eval

How real failures become repeatable benchmarks instead of isolated anecdotes.

## Current Research Questions

- What should be captured in a runtime checkpoint?
- What makes agent recovery appear successful while execution continuity is still broken?
- How should drift detection distinguish behavior divergence from harmless semantic variation?
- How can runtime traces become eval fixtures?
- What does a replayable failure trajectory need to include?
- Which metrics actually reflect agent reliability in long-running work?

## Case Study Direction

The most important artifacts in this repository are case studies. Each useful case study should answer:

- What failed?
- What runtime signal exposed the failure?
- Why did existing eval or dry-run testing miss it?
- What should be captured next time?
- How can this become a reusable eval fixture?

## Status

Active research repository.

Current emphasis:

- runtime failure taxonomy
- recovery and continuity semantics
- drift false positives
- replay-oriented debugging
- eval fixture design
- runtime observability signals
