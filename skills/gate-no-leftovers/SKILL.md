---
name: gate-no-leftovers
description: >-
  Use when a lead or merge gate is about to mark work done — ban leftover
  tails, P2 deferrals, PASS-with-notes, and "handle it later"; clear or name a
  real blocker in the same window.
---
# Gate no leftovers

When a shift hole is in scope, **finish it**. Ban "does not block pass", "open later", "mark P2 and leave", and "note it for tomorrow".

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `BOARD` — usually `$PIPELINE_ROOT/BOARD.md`
- **gate owner** — Decision lead or merge-gate seat who may say pass
- **shift lead** — Decision lead or Coding lead who reports pass / done

## When

- the gate owner is about to pass / merge / 對上
- a shift lead reports pass / PASS / done
- WATCH, PASS-with-notes, P2, or "handle later" appears on `BOARD`, a PR, or a receipt
- the operator says no leftover tails / finish it / no later

## Done looks like

1. Every in-scope hole is **fixed**, or is a **named blocker** (who, what is missing, which tool already went out). There is no "later".
2. Notes / WATCH items are cleared or upgraded to a named blocker before anyone reports pass.
3. P2 / "does not block pass" / a new issue opened as closing talk **does not** close the gate.
4. The receipt carries an evidence path (PR, board row, artifact). A verbal "handle later" is fail.

Complete when the gate owner's pass message lists each in-scope hole as fixed or named-blocked, with paths, and `BOARD` matches.

## Anti-patterns

- PASS with uncleared notes
- `#N = P2` treated as leave-for-the-day
- "wait for the next refresh" as a WATCH close
- squash-merge while a known hole is still open

## Pointers

- Merge after coding work: [poteto-dispatch](sand-workflow:poteto-dispatch)
- Delivery spine: [verifiable-delivery](sand-workflow:verifiable-delivery)
