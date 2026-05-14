# SIGTERM Recovery

Minimal viable SIGTERM recovery:

1. Read latest TaskStarted event
2. Read latest ToolCalled event
3. Read latest failure/error
4. Collect modified files from git diff

Generate interruption checkpoint during SIGTERM.

Goal:

Recover working state after runtime interruption.

Not perfect memory.
Not full replay.
Just better than total state loss.
