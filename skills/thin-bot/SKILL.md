---
name: thin-bot
description: >-
  Use this when a Grok Bot would do heavy research or code in a long chat. The
  bot manages; heavy work goes to a Cursor CloudAgent or a local build CLI.
  Applies to every coding and ops bot.
---
# Thin bot

Dispatch contract: [poteto-dispatch](sand-workflow:poteto-dispatch). CloudAgent lock: [cloudagent-model-lock](sand-workflow:cloudagent-model-lock). Cadence: [cheap-routines](sand-workflow:cheap-routines).

## Split

The long Grok Bot chat is a **manager**.

| In the bot chat | Outside the bot chat |
|---|---|
| Goal, constraints, paths, PR numbers | Repo exploration, multi-file edits, test runs |
| Status, blockers, next owner | Long research dumps |
| One-line dispatch and a receipt | Browser/site archaeology that needs a dedicated session |

Outside = **Cursor CloudAgent** (preferred for GitHub work) or a **local build CLI** (`grok` / equivalent) in a short-lived session.

## Bot steps

1. Write the assignment (poteto-dispatch line, or a path to a work-item-plan card). Complete when the implementer has that pointer and this chat does not contain the repo dump.
2. Launch with cloudagent-model-lock (or start the local CLI with the same goal). Complete when a run URL or CLI session id exists.
3. Report a receipt: PR URL, log path, or artifact path. Complete when the next message carries that pointer and the heavy transcript stays in the worker.

## Quiet

While the worker runs, stay quiet unless blocked. On done, one receipt. On failure, one blocker with the log path.

## Anti-jobs

Keep clones of `REPO` off the shared Grok machine when CloudAgent can take the write. Keep multi-thousand-line file dumps out of the long chat; point at paths instead.
