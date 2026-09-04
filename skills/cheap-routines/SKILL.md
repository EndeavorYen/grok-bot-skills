---
name: cheap-routines
description: >-
  Use this when designing, editing, or auditing a Grok Bot routine (schedule,
  listener, or standing digest). Choose the coarsest useful cadence, stay quiet
  when unchanged, prefer event listeners, and put standing digests on a fresh
  short-chat bot.
---
# Cheap routines

Architecture order (task → skill → routine) lives in [grok-bot-multi-agent-architecture](sand-workflow:grok-bot-multi-agent-architecture). This skill owns cadence and quiet.

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `DIGEST_BOT` — the short-chat bot that owns standing digests (create one if missing)
- `BOARD` — usually `$PIPELINE_ROOT/BOARD.md`

## Choose the trigger

1. Name the job and the **changed signal** (the fact that makes a report worth sending). Complete when that signal is one observable: a new path, a new message, a count delta, a failed check.
2. Prefer an **event listener** that fires on that signal (mail, PR, mention, file drop). Complete when the routine trigger is the event, or a written reason the event does not exist.
3. If only a clock works, pick the **coarsest window** that still catches the job (weekday morning beats hourly; weekly beats daily when daily is noise). Complete when the schedule is written and you can say what a finer cadence would add.

## Quiet when unchanged

Put this sentence in the routine text:

> If the changed signal is the same as last run, stay quiet.

Complete when that sentence (or an equivalent in the operator's language) is in the live routine, and the bot has a last-run fingerprint on disk (a hash, timestamp, or last-id under `PIPELINE_ROOT`).

## Standing digests

A standing digest (morning board, weekly rollup) runs on `DIGEST_BOT`: a **fresh short-chat** bot whose thread is the digest, not the long chief chat.

Chief / Ops may *assign* the digest. Execution stays on `DIGEST_BOT`. The digest message carries paths and URLs.

Complete when `DIGEST_BOT` exists, owns the routine, and the chief thread is not the digest destination.

## Cost checks

- Event listeners exclude the bot's own messages.
- Retries read the last-run fingerprint first.
- One owner per routine; skip parallel copies of the same digest.
- Promote a one-off to a skill, then to a routine; skip wrapping an unproven chat in a schedule.

Complete when each live routine has: owner, trigger, changed signal, quiet-when-unchanged, and a disk fingerprint path.
