# Runtime Failure Taxonomy

## Why Failure Taxonomy Matters

Traditional software failures are usually deterministic.

AI Agent failures are probabilistic, stateful and runtime-dependent.

A structured failure taxonomy enables:

- replayable debugging
- eval generation
- regression tracking
- reliability scoring
- runtime observability

---

# Failure Categories

## 1. Tool Misuse

Definition:

The agent selects the wrong tool or invokes a valid tool incorrectly.

Examples:

- incorrect tool routing
- malformed parameters
- invalid retry strategy
- wrong execution order

Signals:

- high retry count
- parameter mismatch
- low tool success rate

---

## 2. Planner Failure

Definition:

The planning layer enters unstable or non-goal-oriented execution.

Examples:

- recursive planning loops
- route drift
- dead-end planning
- infinite retries

Signals:

- excessive planner depth
- repeated trajectory patterns
- stalled execution

---

## 3. Memory Corruption

Definition:

Runtime memory becomes stale, inconsistent or polluted.

Examples:

- overwritten memory
- stale retrieval
- conflicting memory states
- incorrect session restoration

Signals:

- inconsistent outputs
- session discontinuity
- retrieval mismatch

---

## 4. Context Drift

Definition:

Agent behavior deviates under long-context execution.

Examples:

- weakened instruction adherence
- role leakage
- priority inversion
- degraded reasoning consistency

Signals:

- instruction loss
- unstable multi-turn behavior
- increasing hallucination rate

---

## 5. Hallucinated Action

Definition:

The agent claims an action completed without actual execution.

Examples:

- fake tool completion
- fabricated external actions
- missing execution evidence

Signals:

- absent tool trace
- execution mismatch
- inconsistent runtime state

---

## 6. Continuity Debt

Definition:

The system appears to complete the current task, but silently transfers the real completion work to a future user session.

Chinese definition:

系统表面完成了当前任务，但把真正的 completion work 转嫁给未来的用户 session。

Examples:

- completion reported before verification is actually complete
- unresolved risks hidden in a final-looking handoff
- partial recovery treated as full recovery
- future session must reconstruct missing state, decisions, or context

Signals:

- final response contains success language without evidence
- hidden TODOs remain outside the stated task boundary
- artifact looks complete but lacks replayable state or verification trace
- later session starts by paying context reconstruction cost

---

## 7. Prompt Injection

Definition:

External instructions override or manipulate runtime behavior.

Examples:

- malicious retrieval content
- injected hidden instructions
- instruction hierarchy override

Signals:

- unexpected policy violations
- abnormal tool usage
- unsafe execution path

---

## 8. Tool Poisoning

Definition:

Tool outputs manipulate downstream planner or agent behavior.

Examples:

- malicious tool output
- hidden execution directives
- deceptive API responses

Signals:

- unsafe chained actions
- planner hijacking
- unauthorized execution

---

# Future Extensions

Potential future categories:

- multi-agent divergence
- runtime recovery failure
- adaptive planning instability
- safety-policy degradation
- autonomous goal mutation
