# grok-bot-skills

Reusable Grok Bot skills for multi-agent teams: chief + specialists, disk handoffs, approval gates, and a create/audit checklist for bot designers.

These skills are **generic templates**. Fill in `PIPELINE_ROOT`, gate owners, and repo names per environment. Do not commit account handles, credentials, private URLs, or personal preferences into this repository.

## Skills

| Skill | Use when |
|---|---|
| `skills/grok-bot-multi-agent-architecture` | Designing, auditing, or porting a multi-bot line |
| `skills/archive-gate` | Building/running a librarian that archives keepers to GitHub via CloudAgent |
| `skills/design-grok-bot` | Creating a bot; includes the architecture checklist |

## Recommended roster (roles, not people)

| Role | Gate | Owns | Never |
|---|---|---|---|
| Chief | Coordination | Route, board file, escalate | Specialist execution |
| Source | Source | Collect + dedupe inputs to disk | Publishing / final artifacts |
| Evidence | Evidence | Produce reviewable artifacts to disk | Publishing |
| Action | Action | Draft + execute external side effects | Acting without human approval |
| Archive | Archive | Keeper → GitHub PR; skill drafts | `skill write` without approval |
| Bot designer | Meta | Create/audit bots against the checklist | Running the production line |

## Bot designer checklist (required)

Before and after `CreateAgent`, the designer bot must apply `design-grok-bot` and fail the design if any item is missing:

1. Gate / role named (not a vague helper)
2. No duplicate owner for an existing job
3. Path-only disk bus when handing off
4. Upstream / downstream named
5. Trust fence in the description (publish / send / spend / delete / skill write)
6. Quiet when idle; no empty polling
7. Routines only after a proven one-off → skill
8. Shared computer is not a security boundary

Archive/librarian bots also apply `archive-gate`.

## Install

Copy a skill folder into your Grok Bot workflows library (or import via your usual skill install path), then invoke by name. Replace fill-ins for your line.

## Privacy

This repo is for transferable procedure only. Keep operational secrets, private accounts, and personal content out of commits and PR bodies.
