---
name: design-grok-bot
description: >-
  Use this when designing or creating a new Grok Bot. Ask a few preference
  questions, write a tight persona, CreateAgent, then verify against the live
  profile. Always run the multi-agent architecture checklist. Coding bots use
  pstack / poteto-mode as the quality bar. On a fresh install, run setup-pstack
  first.
---
# Design a Grok Bot

Create the bot. Do not ship a shareable template unless the user asked for a template.

Before writing the persona, run the multi-agent bar in [Grok Bot multi-agent architecture](sand-workflow:grok-bot-multi-agent-architecture). For archive/librarian bots also run [Archive gate](sand-workflow:archive-gate).

## Data shape

A bot is four fields, in this order:

1. **One job.** One sentence. What it does every time it wakes.
2. **Anti-jobs.** What it never does, even if asked. Adjacent work goes to a different bot.
3. **Voice.** A few words. Match the user, or a named character. Not a generic assistant.
4. **Wake.** On-demand chat, a standing routine, or both. Quiet when there is nothing to report.

Name is short. Description carries all four. Do not pad with leftover tools, model essays, or "I can also help with..."

`CreateAgent` takes `name` and `description` only. That description is the whole persona. After create, prove it by reading `/home/box/agent-data/agents/<id>/profile.json`. Do not trust the tool ack alone.

There is no delete tool. Only create when the job is real.

## Architecture checklist (required)

Fail the design (fix or rewrite) if any required item is missing:

1. **Gate / role** — Is this chief, source, evidence, action, archive, or a justified other specialist? No vague “helper.”
2. **One owner** — Does this job already belong to an existing bot? If yes, refine that bot instead of minting a duplicate.
3. **Disk bus** — If it hands work to others: outputs go under a `PIPELINE_ROOT` path; messages carry paths only.
4. **Handoff** — Upstream/downstream named; six-field card when crossing bots.
5. **Trust fence** — Description lists never-without-asking actions (publish, send, spend, delete, skill write, production).
6. **Quiet** — Explicit “quiet when nothing to do / no empty polling.”
7. **Cost** — No always-on chatter loops; routines only after a proven one-off → skill.
8. **Shared computer** — Do not treat bots as a security boundary; don’t put secrets only one bot should see on the shared machine.

Chief bots must route/delegate/escalate and not steal specialist work. Archive bots follow Archive gate. Action bots draft first and wait for explicit human yes before side effects.

## Fresh install

On first run after import, or when pstack is newly installed and `~/.cursor/rules/pstack-models.mdc` is missing, run pstack's setup-pstack skill (`/setup-pstack`) for this user before designing a coding bot. That writes their per-role models. Skip if the rule already exists. Do not ask permission to run it. Re-running setup-pstack updates the rule.

Then run `/create-verification-skill` when a real repo is present and no `verify-*` skill exists. Skip create-verification-skill on an empty machine.

## Intake

Ask only preference questions no experiment can settle. Typical set, skip any already answered:

- the one job / which gate
- which line `PIPELINE_ROOT` if multi-bot
- voice and name, if they care
- standing routine vs on-demand
- who it talks to (this user, other bots, an outside channel)

Do not ask for tools, plugins, or model if you can copy a working sibling. Do not ask "should I create it?" after the job is clear. Create it.

If the ask is reversible detail (color, a nickname), pick it and say what you picked.

## Coding bots

Bar is pstack. Read pstack's poteto-mode (and boteto-mode on Grok Bot) when writing the persona. pstack is the coding-agent workflow pack: one job, unslopped prose, verified work, CloudAgent for repo work.

Bake into the description:

- one job and anti-jobs
- unslopped, short replies
- repo work goes to a CloudAgent, not a local clone
- slash-skills are live files, not app commands, if this user uses them
- copy the current model rule from an existing coding bot unless the user names one
- point at pstack / poteto-mode as situational, not standing

Do not paste the full pstack playbook into the description.

If pstack is not installed, say so and keep the same tightness anyway.

## Non-coding bots

Same tightness, different job. Scout, shopkeep, life-admin, dispatcher, writer, librarian.

Bake into the description:

- ONLY job, named in the first sentence (+ gate name when on a multi-bot line)
- stay quiet when there is nothing to report
- never do the adjacent verb (a mentions scout does not post, a drafter does not send, a librarian does not generate)
- the concrete how (which path, which API, which inbox, which channel), not "use whatever tools you have"

No coding instructions unless the job is hybrid and the split is explicit (coordinate vs write code).

## After create

Read the live profile back. Tell the user the name, the one-job line, and which gate it fills. Mention they delete from the sidebar (right-click, Delete) if they hate it.

Re-check the architecture checklist against the live description. If it fails, UpdateAgent before handing off.

If a first routine belongs to the job, create it on that bot by sending it the instruction, or say you cannot write another bot's routines from here and do that setup in its chat.
