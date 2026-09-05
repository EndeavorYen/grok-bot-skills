---
name: design-grok-bot
description: >-
  Use this when designing or creating a new Grok Bot (including PM／領班 seats).
  Ask a few preference questions, write a tight persona, CreateAgent, then
  CJK-check the live profile and wire the team bus. PM bots chase continuously;
  routines are gates only. Coding bots use pstack / poteto-mode.
---
# Design a Grok Bot

Create the bot. Ship a shareable template only when the user asked for one.

## Data shape

A bot is four fields, in this order:

1. **One job.** One sentence. What it does every time it wakes.
2. **Anti-jobs.** Hard guardrails: what it never does, even if asked. Adjacent work goes to a different bot.
3. **Voice.** A few words. Match the user, or a named character.
4. **Wake.** On-demand chat, a standing routine, or both. Quiet when idle.

Name is short. Description carries all four.

`CreateAgent` takes `name` and `description` only. That description is the whole persona.

## Architecture checklist

Fail the design if any item is missing:

1. Gate / role named (chief / source / evidence / action / archive)
2. No duplicate owner for an existing job
3. Path-only disk bus when handing off
4. Upstream / downstream named
5. Trust fence in the description (publish / send / spend / delete / skill write)
6. Quiet when idle; no empty polling
7. Routines only after a proven one-off → skill; for PM seats, routines are gates not the chase loop
8. Shared computer is not a security boundary

Coding bots: pstack / poteto-mode bar. Fresh install: run setup-pstack when `~/.cursor/rules/pstack-models.mdc` is missing. `/create-verification-skill` only when a real repo is present and no `verify-*` skill exists.

## Intake

Ask only preference questions no experiment can settle. Skip any already answered:

- the one job
- voice and name, if they care
- standing routine vs on-demand (if PM: routine = gate only; chase is continuous)
- who it talks to

Copy tools / plugins / model from a working sibling when you can. Create when the job is clear.

Reversible detail (color, nickname): pick it and say what you picked.

**Lean default.** Start with the smallest roster that can ship the near-term slice (often 領班＋動手). Expand seats only when a real knife is blocked without that role. Skip naming / seat widgets while the roster shape is still soft.

## PM / 領班 bots

Use when the seat owns a line's progress (Team PM, CoS, 領班), not the specialist craft.

Bake into the description:

1. **One job:** chase in-flight work, name knives, 對上／merge gate (or escalate), report to the operator — drive progress without asking the operator to PM.
2. **Anti-jobs:** do not do the specialist craft (big implement PR, thesis, deck, /aal-go, …); do not wait for a clock to chase.
3. **Continuous chase:** during development, track and re-dispatch on wake / event / handoff. Escalate only true decisions (spend, delete, ship, unresolved product choice).
4. **Routines = checkpoint gates only** — quiet-when-unchanged catch for missed escalations; never the primary progress loop. First routine prompt must say this explicitly.
5. **Downstream named** (動手 / copy / research) + disk bus single board writer.
6. Coding bar still pstack / poteto-mode for knives the PM names; implementer runs CloudAgent.

Do not create a PM bot whose only wake is a daily digest. That trains clock-wait.

## Coding bots

Bake into the description: one job and anti-jobs; unslopped short replies; repo work → CloudAgent (not a local clone); pstack / poteto-mode as situational. Do not paste the full pstack playbook.

## Non-coding bots

Bake into the description: ONLY job first; quiet when idle; never the adjacent verb; the concrete how.

## After create

### CJK-check (completion criterion)

1. Read `the agent profile.json on disk (agents/<id>/)`.
2. Done when `name` and every required CJK token in `description` match the intended strings exactly (no near-homophone swaps from CreateAgent / UpdateAgent).
3. Mismatch → `UpdateAgent` with the disk-proven strings, then re-read until match.
4. Announce to the user only after CJK-check passes. Ack alone is not proof.

### Wire the line (same turn)

For a team line, set the bus yourself. Do this without waiting on the chief bot unless the operator says to.

1. Ensure `PIPELINE_ROOT` (e.g. `$PIPELINE_ROOT` or `PIPELINE_ROOT=/…/teamN/`).
2. Write or refresh `handoff.md` (live ids, anti-jobs, who merges).
3. Write `board.md` — single writer named; gate table; path-only; quiet when idle.
4. Create `handoffs/` (six-field cards) plus stage folders as needed (`briefs/`, `keepers/`).
5. Bake bus + upstream/downstream into each new bot's description; prove with a disk read.
6. First routine: SendToAgent that bot the concrete `update_state` create instruction.
7. Sibling personas: change only the live-id sentence.

Apply [grok-bot-multi-agent-architecture](sand-workflow:grok-bot-multi-agent-architecture). Cap roster growth; add a fifth bot only when the user asks.

Tell the user the name and one-job line. They delete from the sidebar (right-click → Delete) if they hate it.
