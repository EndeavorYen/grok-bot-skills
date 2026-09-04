---
name: cloudagent-model-lock
description: >-
  Use this when launching a Cursor CloudAgent. Always pass an explicit model
  plus model_params. Omitting model selects the current UI default, which is
  not this pack's lock.
---
# CloudAgent model lock

Use with [poteto-dispatch](sand-workflow:poteto-dispatch) and [thin-bot](sand-workflow:thin-bot). Roles: Coding lead sets the lock; Implementer passes it on every launch.

## Fill-ins (set once per environment)

This pack's recommended lock is filled in below. Other environments swap the three values and leave the procedure.

| Fill-in | This pack | Meaning |
|---|---|---|
| `MODEL_ID` | `grok-4.6` | Model identifier the CloudAgent run must echo |
| `EFFORT` | `high` | Reasoning effort / thinking level |
| `FAST` | `false` | Fast lane off |

## The hole

If `model` is omitted, the run uses whatever the **current UI / account default** is. That default changes. A coding line that "always uses grok" silently drifts. Treat a launch receipt that does not echo `MODEL_ID` as a failed launch: stop and relaunch with the lock.

## Launch rule

On every CloudAgent create (dashboard, API, MCP, or another agent spawning one), pass **both**:

1. `model` = `MODEL_ID`
2. `model_params` with effort = `EFFORT` and fast = `FAST`

Field names vary by client (`modelParams`, `reasoning_effort`, `reasoningEffort`). The values must be explicit. Complete when the launch payload contains model + params **and** the run receipt echoes `MODEL_ID` (and effort/fast when the receipt includes them).

## Copy-paste lock (this pack)

```
model: grok-4.6
model_params:
  effort: high
  fast: false
```

Swap `MODEL_ID` / `EFFORT` / `FAST` for another environment; keep this block as the filled-in example.

## Anti-jobs

Stay off "use whatever the UI picked" and off inheriting the parent chat model for CloudAgent runs. Local subagents may use a per-role pstack rule; CloudAgent still gets this lock unless the operator names a different `MODEL_ID` for that run.
