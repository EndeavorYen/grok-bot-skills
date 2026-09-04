---
name: design-grok-bot
description: >-
  Use this when designing, creating, or auditing a Grok Bot. Ask preference
  questions the environment cannot settle, write a tight persona, CreateAgent,
  then verify the live profile. Always run grok-bot-multi-agent-architecture.
  Coding bots: poteto-dispatch and thin-bot. Fresh pstack install: run
  setup-pstack first.
---
# Design a Grok Bot

Create the bot. Ship a shareable template only when the operator asked for one.

Before writing the persona, run [grok-bot-multi-agent-architecture](sand-workflow:grok-bot-multi-agent-architecture). For archive/librarian bots also run [archive-gate](sand-workflow:archive-gate). Apply [writing-for-agents](sand-workflow:writing-for-agents) to the description.

## Data shape

A bot is four fields, in this order:

1. **One job.** One sentence. What it does every time it wakes.
2. **Anti-jobs.** Adjacent work that belongs to a different bot.
3. **Voice.** A few words. Match the operator, or a named character they chose.
4. **Wake.** On-demand chat, a standing routine, or both. Quiet when there is nothing to report.

Name is short. Description carries all four.

`CreateAgent` takes `name` and `description` only. That description is the whole persona. After create, read the live profile the product stores for that agent id. Trust the stored description, not the tool ack.

There is no delete tool. Create only when the job is real.

## Architecture checklist (required)

Fail the design (fix or rewrite) until every item is present:

1. **Gate / role** — chief, source, evidence, action, archive, or a justified other specialist.
2. **One owner** — if this job already belongs to an existing bot, refine that bot.
3. **Disk bus** — outputs under a `PIPELINE_ROOT` path; messages carry paths only.
4. **Handoff** — upstream/downstream named; six-field card from grok-bot-multi-agent-architecture when crossing bots.
5. **Trust fence** — description lists never-without-asking actions (publish, send, spend, delete, skill write, production).
6. **Quiet** — explicit quiet-when-idle; cadence via [cheap-routines](sand-workflow:cheap-routines).
7. **Cost** — routines only after a proven one-off → skill.
8. **Shared computer** — bots are a labor boundary; keep secrets off the shared machine when only one role should see them.

Chief bots route, delegate, and escalate. Archive bots follow archive-gate. Action bots draft first and wait for explicit operator yes before side effects.

Complete when the live profile passes all eight items.

## Fresh install

On first run after import, or when pstack is newly installed and `~/.cursor/rules/pstack-models.mdc` is missing, run pstack's setup-pstack skill (`/setup-pstack`) before designing a coding bot. Skip if the rule already exists. Re-running setup-pstack updates the rule.

Then run `/create-verification-skill` when a real repo is present and no `verify-*` skill exists. Skip create-verification-skill on an empty machine.

Complete when the model rule exists (coding bots) and a verify skill exists or was skipped for a documented reason.

## Intake

Ask only preference questions no experiment can settle. Typical set, skip any already answered:

- the one job / which gate
- which line `PIPELINE_ROOT` if multi-bot
- voice and name, if they care
- standing routine vs on-demand
- who it talks to (this operator, other bots, an outside channel)

Copy tools, plugins, and model from a working sibling when one exists. After the job is clear, create it.

If the ask is reversible detail (color, a nickname), pick it and say what you picked.

## Coding bots

Bar is pstack. Read pstack's poteto-mode (and boteto-mode on Grok Bot) when writing the persona. Dispatch shape: [poteto-dispatch](sand-workflow:poteto-dispatch). Heavy work leaves the chat: [thin-bot](sand-workflow:thin-bot). CloudAgent launches: [cloudagent-model-lock](sand-workflow:cloudagent-model-lock).

Bake into the description:

- one job and anti-jobs
- unslopped, short replies
- repo work goes to a CloudAgent (or local build CLI), not a long-chat clone
- slash-skills are live files, not app commands, if this operator uses them
- copy the current model rule from an existing coding bot unless the operator names one
- point at pstack / poteto-mode as situational

Keep the pstack playbook out of the description; the skill is the source of truth.

If pstack is not installed, keep the same tightness and say it is missing.

## Non-coding bots

Same tightness, different job. Scout, shopkeep, life-admin, dispatcher, writer, librarian.

Bake into the description:

- ONLY job, named in the first sentence (+ gate name when on a multi-bot line)
- stay quiet when there is nothing to report
- the adjacent verb belongs to another bot (a mentions scout stays off posting, a drafter stays off sending, a librarian stays off generating)
- the concrete how (which path, which API, which inbox, which channel)

Add coding instructions only when the job is hybrid and the split is explicit (coordinate vs write code).

## After create

Read the live profile back. Tell the operator the name, the one-job line, and which gate it fills. Deletion is from the sidebar (right-click, Delete).

Re-check the architecture checklist against the live description. If it fails, UpdateAgent before handing off.

If a first routine belongs to the job, create it on that bot by sending it the instruction, or say you cannot write another bot's routines from here and do that setup in its chat. Routine cadence follows cheap-routines.
