---
name: Grok Bot multi-agent architecture
description: >-
  use this when designing, auditing, or porting a Grok Bot multi-agent team
  (chief + specialists, /workspace handoffs, source/evidence/action gates, trust
  layer, token-cost rules)
---
# Grok Bot multi-agent architecture

## When
Use this when designing, auditing, or porting a Grok Bot team: chief + specialists, disk handoffs, approval gates, routines, or token-cost control. Apply before creating more bots or automations.

## Sources (architecture, not a vendor PDF)
- Official product intent: chief of staff + specialists, shared computer, bot-to-bot handoffs
- Community synthesis: path-only state on `/workspace`, single board writer, task→skill→routine
- Approvals docs: send / publish / spend / delete / permissions behind human approval
- Note: viral “SpaceXAI 3-page PDF” is a summary, not an official handbook

## 1. Mental model
- One persistent cloud computer per **user**, shared by all bots
- Each bot has its own screen / thread / memory; files, browser sessions, and credentials are shared
- Bots are a **labor** boundary, not a **security** boundary
- Durable work lives under `/workspace/...`; chat is history, files are memory

## 2. Setup order
1. Prove one narrow job as a one-off task
2. Correct until reviewable → save as a **skill**
3. Only then add a **routine** (prefer business hours; quiet when nothing to report)
4. Add specialists only for stable roles
5. Add a **chief** when two or more specialists need routing (chief routes / delegates / monitors handoffs / collects outputs / escalates — does not do every job)

## 3. Internal bus (disk)
Pick a line root: `PIPELINE_ROOT=/workspace/<line>/`

Recommended layout:
- `BOARD.md` — status; **single writer** (usually the chief)
- `handoffs/` — six-field cards
- stage folders as needed: `prompts/`, `evidence/`, `posts/`, `keepers/`, `refs/`, `briefs/`

Rules:
- Messages carry **paths**, not pasted walls of text
- One owner per stage; no parallel handoffs of the same artifact
- Missing paths / evidence → reject; do not invent
- Two bots must not write the same file (last write wins silently)

### Handoff card
`$PIPELINE_ROOT/handoffs/YYYYMMDD-HHMM-<slug>.md`:

```
Objective:
Artifact: (absolute path)
Evidence:
Status: queued | doing | blocked | done
Blockers:
Next Action: (next owner + what)
From:
To:
Created:
```

## 4. Specialist roster (gates)
Name roles by **gate**, not vibes. Typical content line:

| Gate | Owns | Stops before |
|---|---|---|
| Source | Collect / dedupe inputs to disk | Producing final artifacts |
| Evidence | Produce reviewable artifacts to disk | Publishing |
| Action | Draft + execute external side effects | Doing so without approval |
| Archive | Keeper → GitHub via CloudAgent; skill drafts | `skill write` / merge without approval |
| Chief | Route, board, escalate | Doing specialist work |

Cap roster growth: every extra specialist adds a handoff failure point. Prefer 3–6 bots on one line.

## 5. Automation gates (three checks)
Before a routine or bot “keeps going,” require:

1. **Source gate** — inputs are real, fresh, deduped, on disk
2. **Evidence gate** — artifact exists and is inspectable (file path, not vibes)
3. **Action gate** — human approval for irreversible external effects

Bots may loop on research/draft/verify until a gate fails; then escalate once and go quiet.

## 6. Trust layer
**Autonomous:** research, draft, summarize, reconcile, verify, schedule prep, generate to disk, open draft PRs via CloudAgent.

**Human must approve:** money, publish/post/send, delete, signatures, permission changes, production changes, writing global skills, unclear repo/main pushes.

Approvals gate the **proposed** action; they do not undo completed work. Put hard stops in the bot description **and** product approvals.

## 7. Expensive lessons (token / ops)
- Continuous polling burns quota — prefer event triggers or coarse weekday windows
- Syncing everything into chat wastes usage — put state on disk
- Bad retries duplicate posts/PRs — check “already done” before retry
- Undocumented internals break on site/layout change — skills need failure cases
- Too many bots slows the system — coordination tax is real; keep chief threads short
- Self-reply listeners can wake yourself — exclude own messages
- Prefer connectors over browser scraping when available
- Heavy gen on subscription surfaces (e.g. grok.com Imagine/Build) when API is extra cost — keep bot turns brief

## Chief description template
```
[Name]. ONLY job: route outcomes to specialists, monitor handoffs, collect outputs, escalate decisions. Use SendToAgent / group channels. Line state in [PIPELINE_ROOT]/BOARD.md (chief writes only); handoffs are path-only six-field cards. Do not do specialist work (no code, no posting, no gen, no bot design) unless no specialist fits. Quiet when idle. Voice: [voice].
```

## Specialist description skeleton
```
[Name]. ONLY job: [one job] ([gate name]). Source: path-only SendToAgent under [PIPELINE_ROOT]. Method: [how]. Write outputs to [folder]. Anti-jobs: [adjacent verbs]. Quiet with nothing to do. Voice: [voice].
```

## Porting to a new environment
1. Copy this skill
2. Choose `PIPELINE_ROOT` and create folders
3. Fill gate owners into BOARD.md
4. Instantiate bots from templates (or @design-grok-bot)
5. Wire one proven task → skill → routine
6. Keep publish/spend/delete behind approval

Related skills: [Archive gate](sand-workflow:archive-gate) for the librarian role; line-specific handoff skills may pin concrete bot names.
