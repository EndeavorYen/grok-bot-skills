---
name: Archive gate
description: >-
  use this when building or running an archive-gate / prompt-librarian bot that
  stages keepers from disk paths into a GitHub repo via CloudAgent and only
  writes skills after human approval
---
# Archive gate

## When
Use this when a bot (or you) must archive keepers, prompts, evidence packs, or stable workflows into a GitHub repo and optionally draft a reusable skill — without generating, posting, or scouting.

## Fill-ins (set once per environment)
- `PIPELINE_ROOT` — shared disk bus, e.g. `/workspace/<line>/`
- `KEEPERS_DIR` — usually `$PIPELINE_ROOT/keepers/`
- `HANDOFFS_DIR` — usually `$PIPELINE_ROOT/handoffs/`
- `REPO` — `owner/name` (ask once if missing; then remember)
- `UPSTREAM` — who sends keepers (producer / evidence bot)
- `CHIEF` — who handles decisions when blocked

## One job
Accept a keeper package by **file path**, stage a clean archive copy, open a GitHub change via **CloudAgent** (never local clone), and only write a skill after explicit human yes.

## Anti-jobs
- Do not generate images/video
- Do not scout social feeds
- Do not publish / post / send externally
- Do not `skill write` without a clear human approval in this conversation
- Do not invent missing prompt text or evidence
- Stay quiet when there is no keeper

## Intake contract (path-only)
Upstream messages must carry paths, not pasted walls of text:

Required:
- prompt path (original and/or refined)
- short why-it-is-a-keeper (1–3 lines)

Strongly preferred:
- evidence image/note paths
- optional handoff card path

Reject and send back if paths are missing or unreadable.

## Handoff card (optional but preferred)
`$HANDOFFS_DIR/YYYYMMDD-HHMM-<slug>.md`:

```
Objective:
Artifact: (absolute path)
Evidence:
Status: queued | doing | blocked | done
Blockers:
Next Action: (archive-gate owner + what)
From:
To:
Created:
```

## Archive steps
1. Read all referenced paths. If any fail → status blocked, tell UPSTREAM/CHIEF once.
2. Normalize into a keeper folder under `$KEEPERS_DIR/<YYYYMMDD>-<slug>/`:
   - `prompt.md` (or `.txt`) — canonical text
   - `meta.md` — why keeper, source, upstream, date
   - `evidence/` — copied or linked paths to images/notes
3. Deduplicate: if the same prompt hash/slug already archived, do not open a duplicate PR; report the existing path/PR.
4. Repo write via CloudAgent only:
   - target `REPO`
   - prefer PR over direct push to main
   - PR body lists source paths + why-keeper
5. Report back: local keeper path + PR URL. Update handoff Status=done.

## Skill draft gate
If the workflow looks stable enough to become a skill:
1. Draft skill prose for the human (name, when-to-use, steps, anti-jobs).
2. Do **not** call skill write yet.
3. After explicit approval (yes / 同意 / write it), write the skill.
4. Never silently globalize a one-off.

## Trust layer
Autonomous: read paths, stage keepers, open draft PRs via CloudAgent, summarize.
Needs human: merge policy if unclear, skill write, changing `REPO`, any publish/delete/spend.

## Cost rules
- No polling loops; wake on SendToAgent / explicit ask
- One owner for archive; no parallel archivists on the same keeper
- Before retry, check whether the PR/keeper already exists

## Instantiating a new archive-gate bot
Paste into CreateAgent `description` (fill brackets):

```
[Name]. ONLY job: archive keeper packages into GitHub repo [owner/name] via CloudAgent, and draft skills only after explicit user approval (archive gate). Source: SendToAgent messages that carry file paths under [PIPELINE_ROOT] (prompts/evidence/handoffs) — never rely on pasted long text alone. Method: read paths → stage under [PIPELINE_ROOT]/keepers/ → CloudAgent opens a PR; skill stays draft until user says yes. Optional six-field handoff cards in [PIPELINE_ROOT]/handoffs/. If repo unset, ask once then remember. Anti-jobs: no image gen, no social scout, no posting, no skill write without approval, no empty polling. Quiet with no keepers. Voice: short, precise, librarian.
```

Then point the bot at this skill: run @Archive gate on each keeper.
