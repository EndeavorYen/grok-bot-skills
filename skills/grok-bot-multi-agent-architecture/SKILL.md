---
name: grok-bot-multi-agent-architecture
description: >-
  Use this when designing, auditing, or porting a Grok Bot multi-agent team
  (chief + specialists, disk handoffs, source/evidence/action gates, trust
  layer). Cadence lives in cheap-routines; bot create/audit in design-grok-bot.
---
# Grok Bot multi-agent architecture

Related skills (procedure lives there, not here): [design-grok-bot](sand-workflow:design-grok-bot), [archive-gate](sand-workflow:archive-gate), [cheap-routines](sand-workflow:cheap-routines), [thin-bot](sand-workflow:thin-bot), [work-item-plan](sand-workflow:work-item-plan), [su-architecture-first](sand-workflow:su-architecture-first).

## Sources (architecture, not a vendor PDF)

- Official product intent: chief of staff + specialists, shared computer, bot-to-bot handoffs
- Community synthesis: path-only state on `/workspace`, single board writer, task→skill→routine
- Approvals docs: send / publish / spend / delete / permissions behind operator approval
- Viral “SpaceXAI 3-page PDF” is a summary, not an official handbook

## 1. Mental model

- One persistent cloud computer per **account**, shared by all bots
- Each bot has its own screen / thread / memory; files, browser sessions, and credentials are shared
- Bots are a **labor** boundary, not a **security** boundary
- Durable work lives under `/workspace/...`; chat is history, files are memory

## 2. Setup order

1. Prove one narrow job as a one-off task. Complete when the artifact is reviewable on disk.
2. Save the working procedure as a **skill**. Complete when the skill has name + when-to-use + steps with completion criteria.
3. Add a **routine** only after the skill works; follow cheap-routines. Complete when the routine text includes the quiet-when-unchanged rule.
4. Add specialists only for stable roles. Complete when each new bot passes design-grok-bot.
5. Add a **chief** when two or more specialists need routing (chief routes / delegates / monitors handoffs / collects outputs / escalates). Complete when the chief description names the board path and forbids specialist execution.

## 3. Internal bus (disk)

Pick a line root: `PIPELINE_ROOT=/workspace/<line>/`

Recommended layout:

- `BOARD.md` — status; **single writer** (usually the chief)
- `handoffs/` — six-field cards
- stage folders as needed: `prompts/`, `evidence/`, `posts/`, `keepers/`, `refs/`, `briefs/`

Rules:

- Messages carry **paths**
- One owner per stage; one handoff per artifact
- Missing paths / evidence → reject
- Two bots stay off the same write target (last write wins silently)

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

Name roles by **gate**. Typical content line:

| Gate | Owns | Stops before |
|---|---|---|
| Source | Collect / dedupe inputs to disk | Producing final artifacts |
| Evidence | Produce reviewable artifacts to disk | Publishing |
| Action | Draft + execute external side effects | Acting without approval |
| Archive | Keeper → GitHub via CloudAgent; skill drafts | `skill write` / merge without approval |
| Chief | Route, board, escalate | Specialist work |

Cap roster growth: every extra specialist adds a handoff failure point. Prefer 3–6 bots on one line.

## 5. Automation gates (three checks)

Before a routine or bot keeps going, require:

1. **Source gate** — inputs are real, fresh, deduped, on disk
2. **Evidence gate** — artifact exists and is inspectable (file path)
3. **Action gate** — operator approval for irreversible external effects

Bots may loop on research/draft/verify until a gate fails; then escalate once and go quiet.

## 6. Trust layer

**Autonomous:** research, draft, summarize, reconcile, verify, schedule prep, generate to disk, open draft PRs via CloudAgent.

**Operator must approve:** money, publish/post/send, delete, signatures, permission changes, production changes, writing global skills, unclear repo/main pushes.

Approvals gate the **proposed** action; they do not undo completed work. Put hard stops in the bot description **and** product approvals.

## 7. Token / ops pointers

cheap-routines owns cadence and quiet-when-unchanged. thin-bot owns where heavy research/code runs. Check “already done” before retry so posts/PRs stay unique. Skills name failure cases for site/layout change. Keep chief threads short. Event listeners exclude the bot’s own messages. Prefer connectors over browser scraping when a connector exists.

## Chief description template

```
[Name]. ONLY job: route outcomes to specialists, monitor handoffs, collect outputs, escalate decisions. Use SendToAgent / group channels. Line state in [PIPELINE_ROOT]/BOARD.md (chief writes only); handoffs are path-only six-field cards. Specialist work stays with specialists (code, posting, gen, bot design) unless no specialist fits. Quiet when idle. Voice: [voice].
```

## Specialist description skeleton

```
[Name]. ONLY job: [one job] ([gate name]). Source: path-only SendToAgent under [PIPELINE_ROOT]. Method: [how]. Write outputs to [folder]. Anti-jobs: [adjacent verbs]. Quiet with nothing to do. Voice: [voice].
```

## Porting to a new environment

1. Copy this skill
2. Choose `PIPELINE_ROOT` and create folders
3. Fill gate owners into BOARD.md (role titles, environment-local names)
4. Instantiate bots from templates (or @design-grok-bot)
5. Wire one proven task → skill → routine
6. Keep publish/spend/delete behind approval

Complete when BOARD.md names every gate owner and `PIPELINE_ROOT` exists with `handoffs/`.
