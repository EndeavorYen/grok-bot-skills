---
name: work-item-plan
description: >-
  Use this when scoping a multi-day work stream before implementation. Propose
  north-star, freeze, main bet, issue policy, stuck path, and deadline, then
  STOP for operator review.
---
# Work-item plan

Decision lead / Chief owns this card. Implementation waits for an explicit yes. Architecture preflight for engineering changes: [su-architecture-first](sand-workflow:su-architecture-first). Vague asks first: [gentle-grill-me](sand-workflow:gentle-grill-me).

## Fill-ins

- `PIPELINE_ROOT` — shared disk bus
- `PLAN_DIR` — usually `$PIPELINE_ROOT/briefs/`

## Propose the card

Write one markdown file `$PLAN_DIR/YYYYMMDD-<slug>.md` with exactly these six fields:

1. **North-star** — observable done (command, URL, merged PR, shipped artifact).
2. **Freeze** — what stays out of scope until an explicit thaw.
3. **Main bet** — the approach this stream tries first, and the cheapest disproof.
4. **Issue policy** — when to file a new issue vs continue on the current PR; who may open it.
5. **Stuck path** — after what wait, with what evidence, who to ping, and what "unblocked" looks like.
6. **Deadline** — when this stream is done or cut, in a date the operator can reject.

Keep environment-local names as fill-ins. This pack uses role titles (Decision lead, Coding lead, Implementer).

Complete when the file exists, all six headings have a concrete value (not "TBD"), and this turn asks the operator to accept, change, or reject.

## STOP

After the card is written, **stop**. Stay off CloudAgent launches, local builds, and specialist SendToAgent in the same turn.

Resume only after the operator accepts the card (or returns a marked-up file). Then dispatch via [poteto-dispatch](sand-workflow:poteto-dispatch) / [thin-bot](sand-workflow:thin-bot).

Complete when the accepted card path is the only assignment the implementer receives for this stream, or the operator has rejected and this stream is idle.
