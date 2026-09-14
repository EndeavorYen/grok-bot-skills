---
name: routine-healthcheck
description: >-
  Use when auditing Grok Bot routines for token waste, applying an accepted
  healthcheck report (apply-as-a-set), deciding cadence, or running the
  standing routine-healthcheck wake.
---
# Routine healthcheck

Scan the fleet's scheduled and event routines for token waste. Report what to change.

Cadence rules: [cheap-routines](sand-workflow:cheap-routines).

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `AGENT_DATA` — Grok Bot agent-data root on the shared computer (never commit the absolute path)

## When to run

- the standing `routine-healthcheck` routine fires
- the operator asks for a routine checkup, token audit, or cheaper wakes
- before creating a new standing routine
- first import: offer once "want a checkup of every bot's routines?" — run only on yes

## How to scan

1. List every agent folder under `$AGENT_DATA/agents/`.
2. For each agent, read `profile.json` (name) and every `automations/*/automation.json`.
3. Note schedule vs event trigger, enabled vs paused, and the quiet-when-idle rule.

Complete when every agent folder was listed and every enabled/paused routine has a flag or a clean mark.

## Flags

Each hit: bot, routine, flag, one fix line.

- **Too frequent.** Cron denser than hourly. Prefer coarser weekday waking hours, or an event listener when a connector fits.
- **Recurring on a long chat.** Move the standing job to a fresh bot with a short chat.
- **Noisy empty runs.** Prompt lacks quiet-when-idle. Add it.
- **Dead or duplicate.** Paused forever, same job on two bots, or a finite watch past its end.

## Report then apply-as-a-set

1. One short list. Bot, routine, flag, suggested fix. No essay.
2. **apply-as-a-set:** when the operator accepts the report as a whole ("do your list", "apply all", equivalent in the operator's language), apply every listed fix in that turn.
3. Ask which items only when two fixes are mutually exclusive (same routine cannot take both) or one fix is destructive/irreversible and not implied by their accept.
4. Edit another bot's routines only after that accept (or an explicit per-bot ask). SendToAgent the owner with the concrete `update_state` patch when you cannot write their automations from here.

Complete when the report exists, and either the operator has not accepted yet (stop) or every accepted fix is applied.

## Standing routine

After a checkup, offer once to create standing `routine-healthcheck` if missing. Default: Monday morning. Ask only for a different time.

## When you create a bot

Standing sweep → its own bot + one coarsest useful schedule. Keep the long chat free of that timer.
