# grok-bot-skills

Reusable Grok Bot skills for multi-agent teams: chief + specialists, disk handoffs, approval gates, and a create/audit checklist for bot designers.

These skills are **generic templates**. Fill in `PIPELINE_ROOT`, gate owners, and repo names per environment. Do not commit account handles, credentials, private URLs, or personal preferences into this repository.

Role types in the table use only: **Decision lead**, **Coding lead**, **Implementer**, **Designer**, **Content**, **Ops/Secretary**, **All**.

## Skills

| Skill | Role types | Use when | Upstream (if external) |
|---|---|---|---|
| `skills/grok-bot-multi-agent-architecture` | Designer, Decision lead | Designing, auditing, or porting a multi-bot line | — |
| `skills/maintain-grok-bot-skills` | Designer, Ops/Secretary | Add/cull/sanitize pack skills, sync upstreams, keep README role-type table, open maintenance PRs | — |
| `skills/archive-gate` | Ops/Secretary, Content | Building or running a librarian that archives keepers to GitHub via CloudAgent | — |
| `skills/design-grok-bot` | Designer | Creating or auditing a bot (incl. PM／領班: continuous chase; routines=gates) | — |
| `skills/show-me` | All | Explaining a live discussion point visually (pseudocode, call tree, file tree, Mermaid, diff, small HTML) | [humanlayer/skills](https://github.com/humanlayer/skills/tree/main/plugins/show-me/skills/show-me) |
| `skills/writing-for-agents` | Designer | Creating or editing skills, `AGENTS.md`, `CLAUDE.md`, or other agent-consumed docs | [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents) |
| `skills/su-architecture-first` | Decision lead, Coding lead, Implementer | Architecture-first preflight (goal, owning layer, source of truth, change class, validation) | [doublesq97-ui/su-architecture-first](https://github.com/doublesq97-ui/su-architecture-first) |
| `skills/gentle-grill-me` | Decision lead | Vague asks → stress-test the plan, not the person; one decision card per round | [EndeavorYen/gentle-grill-me](https://github.com/EndeavorYen/gentle-grill-me) (portable adaptation) |
| `skills/cheap-routines` | Decision lead, Ops/Secretary | Designing routine cadence: coarsest useful window, quiet when unchanged, event listeners, short-chat digests | — |
| `skills/poteto-dispatch` | Coding lead, Implementer | One-line coding dispatch: `/poteto-mode <goal>. Same PR #<n>. Done when: <runnable proof>.` | — |
| `skills/cloudagent-model-lock` | Coding lead, Implementer | Launching a Cursor CloudAgent with explicit `model` + `model_params` | — |
| `skills/work-item-plan` | Decision lead | Multi-day stream: north-star, freeze, main bet, issue policy, stuck path, deadline, then STOP | — |
| `skills/thin-bot` | All | Bot manages; heavy research/code goes to CloudAgent or local build CLI | — |

## Recommended roster (roles, not people)

| Role | Gate | Owns | Never |
|---|---|---|---|
| Chief | Coordination | Route, board file, escalate | Specialist execution |
| Source | Source | Collect + dedupe inputs to disk | Publishing / final artifacts |
| Evidence | Evidence | Produce reviewable artifacts to disk | Publishing |
| Action | Action | Draft + execute external side effects | Acting without operator approval |
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
7. Routines only after a proven one-off → skill; for PM／領班 seats, routines are checkpoint gates (quiet when unchanged), not the primary progress loop
8. Shared computer is not a security boundary

Archive/librarian bots also apply `archive-gate`.

## Install

Copy a skill folder into your Grok Bot workflows library (or import via your usual skill install path), then invoke by name. Replace fill-ins for your line.

## Maintenance

Pack maintainer owns add / cull / sanitize / sync-upstream / role-table / open-PR.

Procedure: [`skills/maintain-grok-bot-skills`](skills/maintain-grok-bot-skills/SKILL.md).

**sanitize fence:** no private bot names, no agent UUIDs, no machine-local absolute paths, no credentials or personal URLs. Use fill-ins (`PIPELINE_ROOT`, gate owners, `owner/repo`).

**role-table:** every skill folder has one README Skills row; role types use only Decision lead / Coding lead / Implementer / Designer / Content / Ops/Secretary / All.

**sync-upstream:** verbatim external packages keep upstream URL + last-synced date in their `SKILL.md` and in the list below. Revisit quarterly (~90 days) or when a pointer breaks.

**open-PR:** changes land via pull request. Squash to main only when the operator has authorized CLEAN merge for this repo.

Revisit this pack quarterly. Remove skills that duplicate another folder or went unused. Quiet when nothing changed.

External skills (verbatim packages) — last-synced for this revision: **2026-09-04**.

- `show-me` — https://github.com/humanlayer/skills/tree/main/plugins/show-me/skills/show-me
- `writing-for-agents` — https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents
- `su-architecture-first` — https://github.com/doublesq97-ui/su-architecture-first

`gentle-grill-me` is a portable adaptation of https://github.com/EndeavorYen/gentle-grill-me, not a verbatim vendor copy.

## Privacy

This repo is for transferable procedure only. Keep operational secrets, private accounts, and personal content out of commits and PR bodies.
