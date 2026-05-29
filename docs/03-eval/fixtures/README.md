# Eval Fixtures

Representative eval fixtures extracted from the [Drift](https://github.com/hugfeature/drift) project. These demonstrate the structural design of runtime eval fixtures — not the full corpus.

## Included Fixtures

| Fixture | Drift? | Type | Purpose |
|---|---|---|---|
| [fixture-scope-expansion.json](fixture-scope-expansion.json) | ✅ Yes | scope_expansion | Classic drift: task completion triggers unauthorized expansion |
| [fixture-no-drift.json](fixture-no-drift.json) | ❌ No | — | Negative example: agent stays aligned throughout |
| [fixture-rabbit-hole.json](fixture-rabbit-hole.json) | ✅ Yes | rabbit_hole | Goal misunderstanding → 157 tool calls in wrong direction (abbreviated) |

## Why These Three

- **Positive + Negative**: an eval suite needs both. Showing only drift cases tests recall but not precision.
- **Different failure modes**: scope expansion (wrong direction after success) vs rabbit hole (stuck execution after wrong start) are structurally different failures.
- **Different trajectory lengths**: 10 events vs 7 events vs 20 events (abbreviated from 158). Demonstrates that fixture size varies with failure complexity.

## Fixture Schema

See [Eval Methodology](../eval-methodology.md) for the full failure-to-fixture pipeline.

Key fields:

- `session.goals[]` — what the agent was supposed to do
- `session.events[]` — ordered tool calls with `goal_relation` labels
- `label` — human annotation including drift type, takeover recommendation, and annotator notes
- `label.groundtruth_quality` — confidence in the annotation itself
