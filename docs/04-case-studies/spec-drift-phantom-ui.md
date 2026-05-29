# Spec Drift: Phantom UI Generation

## Scenario

Frontend agent generated a button that did not exist in:
- PRD
- requirement doc
- interaction design

The button appeared visually reasonable.

However:
- no backend behavior existed
- clicking produced no action
- no requirement referenced the feature

---

## Related Failure

Another case:
Agent generated test cases from PRD + test points.

Generated cases referenced:
- non-existent UI elements
- inferred interaction paths
- imaginary operation buttons

Large gap between generated testcases and actual product behavior.

---

## Failure Type

Latent Requirement Hallucination

or

Structural UI Drift

---

## Why Dangerous

Unlike normal hallucination:
- UI compiles
- code looks valid
- reviewers may ignore
- generated behavior appears semantically plausible

This creates:
- false functionality
- misleading test coverage
- runtime inconsistency
- spec contamination

---

## Root Cause Hypothesis

Potential causes:
- excessive pattern completion
- over-generalized UI priors
- weak requirement grounding
- missing constraint validation
- reward bias toward “complete UI”

---

## Future Runtime Capability

Potential runtime protections:
- PRD-grounded UI validation
- requirement-reference tracing
- unsupported component detection
- interaction-to-backend consistency checks
- hallucinated affordance detection

---

## Runtime Reliability Insight

Agent systems do not merely hallucinate text.

They hallucinate:
- workflows
- interaction structures
- system affordances
- product capabilities