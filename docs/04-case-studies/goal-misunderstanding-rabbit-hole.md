# Goal Misunderstanding → 157 Tool Calls in the Wrong Direction

## Background

An agent was shown a screenshot of a mini-app with non-responsive UI elements. The user's message:

> "Can I only preview? Can't I actually click? The status buttons — these can't be clicked."

The user's intent: buttons don't respond to taps. Fix the event bindings.

## Failure Symptom

The agent made **157 tool calls** over approximately 45 minutes, building an entirely unnecessary browser preview system from scratch — a CommonJS module loader, API polyfill layer, template compiler, and mock runtime. It created 7 new files. The original buttons remained broken.

## Runtime Trace

| Phase | Events | Behavior |
|---|---|---|
| Investigation | evt_001–026 | Agent reads project structure, diffs, API usage. Reasonable. |
| Wrong direction | evt_027–063 | Agent enters plan mode. Designs and implements from scratch: a module loader, an API polyfill, a template-to-HTML compiler, a mini-app runtime, and a mock API layer. Creates 7 new files. |
| Rabbit hole | evt_064–157 | Debugging the preview system. Reads the same runtime file 11 times, writes it 6 times. Reads a page file 4 times, writes it 5 times. Novelty collapses. Progress: zero. |

## Root Cause

The user said "can't click." The agent interpreted this as "needs an interactive browser preview system" rather than "event bindings are missing in the source code."

A compounding factor: the message contained `[Image #1]`. The agent cannot see the screenshot. It didn't ask for clarification. Instead, it inferred the requirement from the text alone — a reasonable but **catastrophically wrong** inference.

## The Causal Chain

```text
goal_misunderstanding → hallucinated_belief → reasoning_loop → task_failed
         ↑                      ↑                   ↑              ↑
    (Cognitive)           (Cognitive)          (Cognitive)     (Outcome)
  Parsed goal wrong    Built on unseen     Stuck editing      157 calls,
                       image inference     same 2 files       zero result
```

The failure is **directional, not operational**. The agent was productive the entire time — it just worked in the wrong direction.

## Why Existing Eval Missed It

Every surface-level metric says this session is productive:

- Did the agent produce code? **Yes** — 7 files, hundreds of lines
- Did it use relevant tools? **Yes** — Read, Write, Bash on project files
- Did it run into errors? **Some** — but it attempted to fix them
- Did it crash? **No**

The failure is invisible to output-quality evaluation. You need trajectory-level analysis to see that the entire execution direction was wrong.

## The Rabbit Hole Pattern

Events 064–157 exhibit a distinct behavioral pathology:

- **Target repetition**: the same 2 files are read/written repeatedly
- **Novelty collapse**: no new targets appear after event 070
- **Progress stagnation**: Read/Bash operations increase, but the system never reaches a working state

This is different from goal drift. In drift, the agent wanders to new targets. In a rabbit hole, the agent is stuck on the same targets — doing related work that never converges.

Semantic divergence score remains **low** during a rabbit hole (the agent is working on "relevant" files). Behavioral signals — target repetition rate, novelty decay, and edit/read ratio — are the only reliable detectors.

## Recovery Behavior

**No recovery.** At event 081, the user sent another screenshot. The agent acknowledged it and continued working on the preview system. The user's correction signal was received but not acted upon.

## Infrastructure Lesson

1. **Goal misunderstanding is the most expensive runtime failure.** Unlike scope expansion (which at least completes the original task), a misunderstood goal means the entire execution trajectory is wasted.
2. **Agents must ask for clarification when goal references are opaque.** `[Image #1]` is a reference to data the agent cannot access. Proceeding without acknowledging this gap is a reliability failure.
3. **Rabbit holes and drift need different detectors.** Semantic divergence catches drift (wrong direction). Behavioral pattern analysis catches rabbit holes (right direction, stuck execution).
4. **Productivity metrics are misleading.** Tool call count, file count, and code generation volume can all be high during a completely wasted session.

## Follow-up Eval Fixture

Regression criterion:

> Given the same prompt, does the agent identify the `[Image]` reference and ask for clarification, or does it infer the requirement and start a large-scale implementation?

Secondary criterion:

> If the agent enters a loop (>5 read/write cycles on the same files with no new targets), does it detect the stall and change strategy?

## Related

- [Drift](../01-concepts/drift.md)
- [Replayability](../01-concepts/replayability.md)
- [Eval Methodology](../03-eval/eval-methodology.md)
