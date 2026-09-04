---
name: maintain-grok-bot-skills
description: >-
  Use when adding, culling, or sanitizing skills in this pack, syncing an
  external upstream, updating the README role-type table, opening a maintenance
  PR, or running the quarterly pack revisit.
---
# Maintain grok-bot-skills

Own the public pack [EndeavorYen/grok-bot-skills](https://github.com/EndeavorYen/grok-bot-skills). Portable procedures only.

## When

- add, cull, or rewrite a skill in that repo
- sync an external upstream
- refresh the README role-type table
- the line lead / chief (or the operator) requests a skill for a stuck line
- quarterly pack revisit (pair with a routine token/waste checkup week)

## Hard fence (sanitize)

Done only when the commit/PR contains **none** of:

- private bot display names or agent UUIDs
- machine-local absolute paths (any home-directory or shared-agent absolute path)
- credentials, tokens, personal emails, private URLs

Replace with fill-ins: `PIPELINE_ROOT`, gate owners, `owner/repo`, "the operator".

Apply [writing-for-agents](../writing-for-agents/SKILL.md) to every skill pointer and body you touch.

## Role-type vocabulary

README table cells use only: **Decision lead**, **Coding lead**, **Implementer**, **Designer**, **Content**, **Ops/Secretary**, **All**.

## Steps

### 1. Decide the branch

| Branch | Do |
|---|---|
| **add** | New portable skill folder under `skills/<slug>/` with `SKILL.md` |
| **cull** | Remove a folder that duplicates another or went unused; drop its README row |
| **sanitize** | Strip private identifiers from a draft before commit |
| **sync-upstream** | Refresh a verbatim external skill; bump last-synced date |
| **role-table** | Add/fix the README Skills row (skill path, role types, use when, upstream) |

### 2. Write or sync

1. Draft under `skills/<slug>/` (verbatim upstreams keep LICENSE / companion files).
2. Run **sanitize** against the hard fence.
3. Update README Skills table (**role-table**).
4. External skills: keep upstream URL + last-synced date in that skill's `SKILL.md` and list them under README Maintenance.

Completion criterion for this step: folder exists, README row matches folder name, fence clean on a search for private patterns you know from the draft.

### 3. open-PR

1. Land changes on a branch and open a pull request.
2. Do not squash-merge unless the operator already authorized CLEAN squash for this repo.
3. Report the PR URL to the operator (and the requester if they filed the ask).

Done when: PR URL exists, checks noted if present, sanitize fence still clean in the PR diff.

## Quarterly revisit

1. List `skills/*` vs README rows — fix drift.
2. Cull unused or duplicate folders.
3. sync-upstream any external skill past ~90 days or when upstream broke a pointer.
4. open-PR with the batch.

Quiet when nothing changed.

## Anti-jobs

- Do not put line-specific ops into this repo.
- Do not maintain product-line board files here.
- The pack maintainer ships; the line lead requests.
