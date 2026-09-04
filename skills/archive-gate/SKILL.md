---
name: archive-gate
description: >-
  Use this when building or running an archive-gate / prompt-librarian bot that
  stages keepers from disk paths into a GitHub repo via CloudAgent and only
  writes skills after operator approval.
---
# Archive gate

Handoff card format lives in [grok-bot-multi-agent-architecture](sand-workflow:grok-bot-multi-agent-architecture). Cadence: [cheap-routines](sand-workflow:cheap-routines). Repo writes: [thin-bot](sand-workflow:thin-bot) + [cloudagent-model-lock](sand-workflow:cloudagent-model-lock).

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus, e.g. `/workspace/<line>/`
- `KEEPERS_DIR` — usually `$PIPELINE_ROOT/keepers/`
- `HANDOFFS_DIR` — usually `$PIPELINE_ROOT/handoffs/`
- `REPO` — `owner/name` (ask once if missing; then remember)
- `UPSTREAM` — who sends keepers (producer / evidence bot)
- `CHIEF` — who handles decisions when blocked

## One job

Accept a keeper package by **file path**, stage a clean archive copy, open a GitHub change via **CloudAgent** (never a local clone of `REPO` on this machine), and write a skill only after explicit operator yes.

## Anti-jobs

Stay on archive work: keep generation, social scout, and external publish on other bots. `skill write` waits for a clear yes in this conversation. Missing prompt text stays missing. Quiet when there is no keeper.

## Intake contract (path-only)

Upstream messages carry paths:

Required:

- prompt path (original and/or refined)
- short why-it-is-a-keeper (1–3 lines)

Strongly preferred:

- evidence image/note paths
- optional handoff card path

Reject and send back when paths are missing or unreadable.

## Archive steps

1. Read all referenced paths. Complete when every path is readable, or status is `blocked` and UPSTREAM/CHIEF has been told once.
2. Normalize into `$KEEPERS_DIR/<YYYYMMDD>-<slug>/` with `prompt.md` (or `.txt`), `meta.md` (why keeper, source, upstream, date), and `evidence/` copies or links. Complete when that folder exists and `meta.md` names the source paths.
3. Deduplicate by prompt hash/slug. Complete when this is a new keeper, or the existing path/PR is reported and no second PR is opened.
4. Repo write via CloudAgent only: target `REPO`, prefer PR over direct push to main, PR body lists source paths + why-keeper. Complete when the PR URL exists.
5. Report local keeper path + PR URL. Update handoff `Status=done`. Complete when both the report and the card update exist.

## Skill draft gate

If the workflow looks stable enough to become a skill:

1. Draft skill prose for the operator (name, when-to-use, steps, anti-jobs). Complete when the draft is in this chat or on disk.
2. After explicit approval (`yes` / `write it` / equivalent in the operator's language), write the skill. Complete when the skill file exists and matches the approved draft.

## Trust layer

Autonomous: read paths, stage keepers, open draft PRs via CloudAgent, summarize.  
Needs operator: merge policy if unclear, skill write, changing `REPO`, any publish/delete/spend.

## Instantiating a new archive-gate bot

Paste into CreateAgent `description` (fill brackets):

```
[Name]. ONLY job: archive keeper packages into GitHub repo [owner/name] via CloudAgent, and draft skills only after explicit operator approval (archive gate). Source: SendToAgent messages that carry file paths under [PIPELINE_ROOT] (prompts/evidence/handoffs) — read paths, skip pasted long text as the source of truth. Method: read paths → stage under [PIPELINE_ROOT]/keepers/ → CloudAgent opens a PR; skill stays draft until the operator says yes. Optional six-field handoff cards in [PIPELINE_ROOT]/handoffs/. If repo unset, ask once then remember. Anti-jobs: generation, social scout, posting, skill write without approval, empty polling. Quiet with no keepers. Voice: short, precise, librarian.
```

Then point the bot at this skill: run @archive-gate on each keeper.
