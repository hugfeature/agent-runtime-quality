# Runtime Semantics

## Continuity

Continuity means an agent can resume interrupted execution with awareness of prior progress and intent.

It is not:

- chat memory
- long-term knowledge storage
- simple conversation replay

Current capability:

- single-task continuity: mostly works
- interrupted execution recovery: works
- multi-task continuity: weak
- long-horizon intent persistence: missing

---

## Observed Failure

P0 task recovery works well because execution state is persisted.

P1 tasks are often forgotten because they exist only in session intent, not runtime execution state.

This reveals a semantic gap between:

- execution continuity
- task continuity

---

## Runtime State

Runtime state currently represents:

- current progress
- active plan
- tool execution history
- checkpointed intermediate state

Current limitation:

runtime state does not fully preserve latent user intent outside the active execution branch.
