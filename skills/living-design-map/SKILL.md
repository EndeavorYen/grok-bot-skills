---
name: living-design-map
description: >-
  Use when reconciling a living design map, running a design audit / 設計審計 /
  對帳, docs drift, plan vs code, or before the next phase mid-slice — the
  operator report must be plain-language with ranked next steps and reasons.
---
# Living design map

Reconcile the project's **living map** (design docs that stay current) against what is already shipped. Prefer **completed goals** as the authority when design text and implementation disagree.

The operator-facing report must be **plain language** first. Internal labels (Update map / Update implementation / …) stay in the agent notes or a short appendix — never as the only explanation.

Do not open implementation knives in the same turn unless the operator already authorized a specific fix.

## Fill-ins (set once per environment)

- `REPO` — `owner/name`
- map paths — locate the five roles below; name the actual paths once in the report

## When

- the operator asks for 對帳 / living map / 設計審計 / design audit / plan-vs-code drift / how far docs lag reality
- before starting a new phase while a slice is mid-flight
- the same product decision keeps looping, or docs feel stale after several merges
- after a burst of operator locks that have not been written back into the map

Skip for a clear local knife with settled ownership ([su-architecture-first](sand-workflow:su-architecture-first) or [poteto-dispatch](sand-workflow:poteto-dispatch) instead).

## Living map

Five roles. Default paths if the repo uses this shape; otherwise locate the same roles first:

| Doc role | Default path | Holds |
|---|---|---|
| Locked product facts ("is / is not") | `docs/00-north-star.md` | Lasting product locks |
| Borrow / reference boundaries | `docs/01-*.md` | What this line may copy |
| Engine and system locks | `docs/02-architecture.md` | Architecture locks |
| Current slice acceptance | `docs/03-vertical-slice.md` | What this slice must ship |
| Truly undecided items only | `docs/04-open-questions.md` | Open questions |

Briefs and board notes are **evidence**, not a second map — lasting locks fold into the facts / architecture / slice docs; lasting open items into open-questions.

## Steps

### 1. Inventory completed goals

From merges, CI acceptance tests, and the slice doc, list what already ships and can be re-proven.

Done when every completed goal has a one-line proof (test name, PR, or playable path).

### 2. Read the map

Read the five roles (or the located equivalents). Tag each material claim:

- **Locked** — written as decided
- **Open** — in open-questions (note default and whether it blocks the slice)
- **Stale candidate** — still says draft / undecided while a completed goal proves otherwise

Done when every section is tagged.

### 3. Detect drift

Compare map ↔ completed goals ↔ recent operator locks. Record each material disagreement with: what disagrees, what the map says, what shipped says, how costly a wrong call is (user confusion / reverse work / low).

Done when every material disagreement is listed. Do not invent aspirational features as drift.

### 4. Classify each drift (agent-internal)

Pick **one** primary class per item:

1. **Update map** — shipped matches the operator's last lock; docs lag
2. **Update implementation** — docs or an explicit operator lock still win; code lagged
3. **Needs decision** — two authorities conflict, or neither is clearly newer
4. **Out of slice** — real gap, but current slice acceptance does not need it

**Completed-goals rule:** operator-confirmed shipped work (merged acceptance, playtest/operator lock, or explicit "done") → prefer **Update map**. Choose **Update implementation** only when the operator has said the shipped behavior is wrong, or a still-live lock clearly contradicts the code.

Done when every drift has one class and a next owner (docs / Implementer / the operator).

### 5. Write the operator report, then wait

Speak in the operator's language. Lead with human words; put file paths and class labels in parentheses or an appendix.

**Required shape (all five blocks):**

1. **One-line conclusion** — Are we mostly fine? Is the lag in docs, code, or undecided product calls?
2. **Already done** — Short bullets a non-engineer can skim. Proof in parentheses, not as the headline.
3. **Where it disagrees** — For each item, four short lines (no jargon table as the only view):
   - **What happened** — plain sentence
   - **Docs say / product does** — one line each
   - **Suggested move** — update docs / update code / operator decides / later
   - **Why** — one sentence (cost, who gets confused, what breaks if ignored)
4. **Ranked next (with reasons)** — 1–3 actions only. Each line: **what** + **why now** + **cost of inaction**. Default when docs lag and code matches locks: recommend a docs-only sync first. Recommend a code knife only for Update-implementation rows the operator should care about now. Recommend a decision prompt only for Needs-decision rows that block the next phase.
5. **This turn does not implement** — Explicit: no implementation knives unless already authorized. Say what you will do after they reply (e.g. "you say update docs → I open a docs PR").

Optional appendix for agents: class labels, doc paths, PR numbers.

Done when a reader who did not see the audit can answer: what is fine, what is off, what to do next, and why — without decoding internal jargon.

**Fail the report** if block 4 is missing, empty, or only says "look again" without a ranked action + why-now + cost-of-inaction.

Stop. Apply doc updates only after the operator accepts (or picks rows). Implementation knives only after accept + a separate [su-architecture-first](sand-workflow:su-architecture-first) / [poteto-dispatch](sand-workflow:poteto-dispatch) pass.

## Pointers

- [work-item-plan](sand-workflow:work-item-plan) — redirect a multi-day stream
- [gentle-grill-me](sand-workflow:gentle-grill-me) — settle Needs-decision rows
- [su-architecture-first](sand-workflow:su-architecture-first) — before an Update-implementation knife
- [poteto-dispatch](sand-workflow:poteto-dispatch) — after the operator accepts a code row

## Guardrails

- Write lasting locks back into the facts / slice docs; do not leave chat as the only source of truth.
- One reconcile report per run; do not fan out issues or PRs in the same turn.
- Keep slice scope stable; reconciling the map is not a license to grow the slice.
