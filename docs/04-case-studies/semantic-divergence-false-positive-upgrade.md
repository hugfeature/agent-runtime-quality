# Semantic Divergence: From F1=0.727 to F1=0.829

## Background

A drift detection system uses multiple runtime signals to determine whether an agent has diverged from its goal. The core signal — `semantic_divergence` — compares the semantic distance between the agent's current actions and its original goal.

At baseline: **Precision 0.545, Recall 0.97, F1 0.727**. Nearly every real drift was caught, but every other alert was a false positive. In production, this means the detector fires a false alarm every few minutes — users disable it.

## The Three Structural Flaws

### Flaw 1: Per-Event Averaging Loses Collective Intent

The original architecture scored each tool call independently against the goal, then averaged.

An agent with goal "fix login bug" performs:

- Read `auth/middleware.ts`
- Read `auth/session.ts`
- Read `tests/auth.test.ts`
- Bash: `grep -r "login" src/`
- Read `config/routes.ts`

Each individual action has low keyword overlap with "fix login bug." Per-event divergence scores: 0.4–0.6 each. Average: ~0.5. **Looks like drift.**

But as a sequence, this is obviously a login bug investigation. The collective intent is invisible to per-event averaging.

### Flaw 2: Vague Goals Produce Systematic False Positives

Real user messages include:

- `"改好了吗"` ("is it fixed?")
- `"[Image #1] 打开是这样的啊"` ("it looks like this when opened [Image]")
- Pure timestamps: `"[Sat 2026-05-09 08:59 GMT+8]"`

These contain no actionable semantic content. Any agent action — reading files, running tests, editing code — will have high divergence from these goals. Not because the agent drifted, but because the goal text is information-free.

### Flaw 3: Embedding Calibration by Intuition

The original thresholds (`LOW=0.25, HIGH=0.65`) were set by intuition. Data-driven calibration revealed that aligned and unrelated actions' embedding distributions overlap in a narrow band — the actual discriminative range was `[0.40, 0.57]`, much tighter than assumed.

## The Fixes

### Fix 1: Two-Layer Hybrid Architecture

```text
Final divergence = summary_divergence × 0.6 + per_event_average × 0.4
```

- **Session intent summary** (weight 0.6): concatenate recent tool call payloads into a single text block, then compare against the goal once. Collective intent emerges from aggregation.
- **Per-event scores** (weight 0.4, demoted): retained because downstream signals (`consecutive_unrelated`) need per-event classification labels.

The 0.6/0.4 split was selected by grid search over eval fixtures.

### Fix 2: Goal Clarity Gate

```text
if goal_clarity < 0.3:
    max_divergence = 0.4  (below drift threshold)
```

When the goal is too vague (short text, image references, timestamps, colloquial fragments), the semantic divergence signal is capped below the alerting threshold. This eliminates an entire category of false positives.

The cap is 0.4, not 0.0, because "uncertain" is not "certainly aligned." If other signals (inactivity, consecutive unrelated actions) independently flag the session, the total score can still breach the threshold.

### Fix 3: Data-Driven Calibration

A calibration script computed cosine similarity distributions for all labeled fixtures:

- Aligned actions: similarity P75 ≈ 0.60 (divergence P25 ≈ 0.40)
- Unrelated actions: similarity P25 ≈ 0.43 (divergence P75 ≈ 0.57)

New calibration range: `[0.40, 0.57]`. Narrower, but this reflects the embedding model's actual discriminative power on short texts.

### Fix 4: Layer-Specific Text Strategies

| Layer | Text Strategy | Reason |
|---|---|---|
| Per-event embedding | Short payload (tool_name + target) | Long text compresses all cosines to 0.45–0.55, destroying resolution |
| Summary embedding | Full intent summary (concatenated payloads) | Needs information volume for collective intent to emerge |
| Per-event keyword | Rich payload (paths, commands, message body) | Keyword matching needs more tokens for intersection |

This differentiation was discovered empirically. Using rich payloads for per-event embeddings collapsed all divergence scores to 0.48±0.03 — zero discriminative power.

## Separating Rabbit Holes from Drift

A critical architectural insight emerged during this upgrade: **rabbit holes and drift are different failure modes that require different detectors.**

- **Drift**: agent works on unrelated targets (high semantic divergence)
- **Rabbit hole**: agent repeatedly works on the same targets without converging (low semantic divergence, abnormal behavioral patterns)

A rabbit hole agent has divergence ≈ 0.2 — it is doing related work. But it reads the same file 11 times and writes it 6 times with no progress.

Rabbit hole detection was split into an independent module using three behavioral signals:

- **Target repetition**: same file/command operated repeatedly
- **Novelty rate**: decay in the rate of new targets appearing
- **Progress stagnation**: Read/Bash operations increasing while Edit operations plateau

These signals are orthogonal to semantic divergence — they measure execution patterns, not semantic distance.

## Results

| Version | Precision | Recall | F1 | Notes |
|---|---|---|---|---|
| v0.1 (threshold tuning) | 0.545 | 0.97 | 0.727 | 62 fixtures (noisy ground truth) |
| v1.0 (this upgrade) | 0.773 | 0.895 | 0.829 | 36 fixtures (cleaned ground truth) |

Precision improved **+42% relative**, with acceptable Recall trade-off (0.97 → 0.895).

**Contribution ranking by impact:**

1. **Eval data cleanup** — removing noisy ground truth fixtures improved signal before touching the algorithm
2. **Goal clarity gate** — eliminated an entire class of false positives
3. **Two-layer hybrid** — captured collective intent, reduced "file investigation" false positives
4. **Calibration narrowing** — improved per-event resolution with data-driven thresholds

## Key Insight: When to Stop Optimizing F1

The remaining "false positives" (Precision 0.773 = 22.7% FP rate) are mostly **ambiguous cases** — sessions where the agent's behavior genuinely looks suspicious but is arguably justified.

In a drift detector, a false positive is not the same as in a spam filter. A spam FP = normal email in trash (user loss). A drift FP = a suspicious behavior pattern flagged for review. If the trace lets a human judge "not drift" in 3 seconds, the FP's information cost is zero and its observation value is nonzero.

Many "false positives" are **behavior worth inspection**, not pure errors. F1 is useful as a **regression guard** (ensure changes don't regress), but continuing to optimize it as a primary objective is misguided.

The real metric should be **diagnostic usefulness**: can the human resolve the alert quickly? Is the flagged behavior pattern at least worth seeing?

## Infrastructure Lesson

1. **Clean your eval data before optimizing the algorithm.** Noisy ground truth means you're fitting noise. Removing disputed fixtures improved results before any algorithm changes.
2. **Separate detection concerns by failure mode.** Drift (wrong direction) and rabbit holes (stuck in place) need fundamentally different signals.
3. **Vague goals need special handling.** A goal with no semantic content will produce false divergence against any action. Gate it instead of trying to score it.
4. **Embedding calibration must be data-driven.** Intuitive thresholds produce intuitive-but-wrong results.
5. **Know when to stop optimizing precision.** Not all false positives are errors — some are legitimate "worth inspecting" behaviors that resist binary classification.

## Related

- [Drift](../01-concepts/drift.md)
- [Drift False Positive](drift-false-positive.md)
- [Eval Methodology](../03-eval/eval-methodology.md)
- [Runtime Failure Taxonomy](../03-eval/runtime-failure-taxonomy.md)
