---
name: verifiable-delivery
description: >-
  Use when starting a project or a delivery knife that must stay verifiable
  across models and hosts — triggers on verifiable delivery, portable
  workflow, first-principles delivery, knife with 完成條件, adversarial verify,
  soft pass, independent oracle, fail-closed, quiz-mirroring, LGTM without
  oracle, missing-case, claims table, just-ten-more hunt; scales open-line vs
  knife; not a second religion beside poteto / architecture-first.
---
# Verifiable delivery

Run the smallest delivery loop that keeps work **spec'd, contracted, verified, and tracked on disk**. Model and host may change; the loop stays.

Compose skills already in this pack ([su-architecture-first](sand-workflow:su-architecture-first), [poteto-dispatch](sand-workflow:poteto-dispatch), [gentle-grill-me](sand-workflow:gentle-grill-me)). This skill is the spine, not a parallel church.

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `PROGRESS` — living map path, usually `$PIPELINE_ROOT/docs/progress.md` or the repo's `docs/progress.md`
- **gate owner** — Decision lead (or the operator) who may merge

## Scale

- **Open-line** (new project / hard turn): default **five-doc pack**, then prune fat. Living progress map is required (`line:` identity **and** `knife:` named or `idle`; **forbidden** to use `idle` on `line:` to mean "no knife").
- **Knife** (same project, next cut): goal + 完成條件 + 不做什麼 + where artifacts live. Skip the five-doc pack. Set `knife:` to this named cut; keep `line:` as identity.

## Spine (order is load-bearing)

1. **Spec** — real goal, 完成條件 (checkable), 不做什麼.
2. **Contract** — what is delivered, what it looks like, where it lives (path / as-of / owner).
3. **Implement** — only after spec + contract exist for this scale, **and** after the undecided gate: no open options / unclear scope / unclear 不做什麼, and no design-review items still awaiting a decision. Work on / reuse **one PR branch**; that branch is the mid-stream artifact. Do not open a second branch for the same knife. Coding dispatch: [poteto-dispatch](sand-workflow:poteto-dispatch).
4. **Verify** — against 完成條件. Runnable checks or visible proof named there. Producer cannot self-declare pass.
5. **Debug** — **only if Verify is red**. Then back to Verify; same knife. If Verify never went red, living-map status is `n/a` (stage exists, does not apply this run — **not** `unplanned`).
6. **Adversarial** — attack the claim that it works (behavior / contracts). See below.
7. **Ablation / Tighten** (optional, only after green) — remove **one** rule / tool / prompt, re-run, **compare**; and/or delete redundancy with evidence. No comparison → not useful.
8. **Ship prep** — clean PR + evidence. **Not merge.**
9. **Merge gate** — gate owner or the operator checks 完成條件, then squash-merge. Do not auto-merge. After merge, update `PROGRESS`: may set **`knife: idle`**; must **not** wipe line design gaps (`missing` / `unplanned`); must **not** write **`line: idle`**. Or set the **next named knife**.

Progress report (any scale): open with the **Status block** — `line:` / `knife:` / `current:` (one line) / `missing:` (true list) — then the full spine table for the current delivery line. Statuses: **done | current | missing | n/a | unplanned**. `missing` = planned, not done. `n/a` = stage exists but does not apply this run. `unplanned` = design has not committed yet. If Verify never went red, Debug is `n/a`. **Ship prep** and **Merge gate** are separate rows. Name **current** (one cell) and true **missing**. `knife: idle` = no named knife; it does **not** mean the line is finished. If `knife: idle` but **missing** / **unplanned** design gaps remain, **surface those gaps**.

## Doc pack (open-line only, prune hard)

1. requirements
2. design
3. implementation plan
4. test plan
5. thin roadmap/milestones

Delete any file that does not change a decision. Living progress map is required (not one of the five). Knife scale skips this pack.

## Debug (only if Verify is red)

The system is wrong until evidence says otherwise. Same knife. After a fix, re-verify the **same** 完成條件.

Adversarial red while Verify is green is **not** Debug. Next is implement the listed holes on the same knife and the same PR branch, then re-run Verify + Adversarial.

## Adversarial

**Coding knives — required:** independent oracle + fail-closed; missing-case / matrix claims table; **one** host-skill `just-ten-more` hunt (list holes, do not fix). Soft pass is red.

**Docs-only:** run **one** `just-ten-more` hunt. That hunt **is** thin Adversarial. Claims / matrix may stay `n/a`. **Forbidden:** skip the hunt and mark Adversarial `n/a`.

Fail-closed: the stage is **red** unless an independent oracle says the contracts / observable behavior hold. The producer cannot self-declare pass.

### Three bans (soft pass = fail)

| Ban | Fail when |
|---|---|
| **self-declared pass** | Producer says it works. No independent oracle. |
| **quiz-mirroring** | Tests restated from the implementation. |
| **LGTM-without-oracle** | Review approval or "looks good" with no independent oracle. |

### Claims table (coding knives)

Each observable from 完成條件 is `covered` | `missing` | `n/a`. In-scope **missing** is fail. A **covered** row needs a mapped oracle or path. Duplicates of the same observable are not completeness. Coverage percent and file count are not the bar.

### just-ten-more hunt (every knife)

Host skill `just-ten-more` by name if present; otherwise list evidenced holes the same way. Do not copy that skill's body here. In-scope evidenced hole = red. Out-of-knife `n/a` does not block. Listing ten vibes, or waving in-scope evidence as noise, is not green. After red: fill listed holes on the same knife and branch, then re-check — not Debug unless Verify was red.

## First principles

- Open-line default is the five-doc pack; prune fat from there. Write a doc only if skipping it causes wrong work.
- Red stops the line. No inventing the next knife while red.
- Chat is not the source of truth.
- Undecided options / scope / 不做什麼: run [gentle-grill-me](sand-workflow:gentle-grill-me) before Implement. Stay off poteto / implement in that turn.

## Anti-jobs

- Do not run optimize or ablation before verify is green.
- Do not treat a coding-knife soft pass as Adversarial green.
- Do not treat Ship prep as merge. Do not auto-merge.
- Do not expand scope when red.
- Do not mark Debug `unplanned` when Verify never went red — that is `n/a`.
- Do not omit Debug on the map to jump to Merge.
- Do not omit `line:` or `knife:` from the living map.
- Do not use `idle` on `line:` to mean "no knife".
- Do not treat `knife: idle` as line-complete.
- Do not mix knives on the map: Prior knife and the spine rows name the same knife.
- Do not print a host grill skill as the operator-facing next when the knife gate is still open — next is settle the knife, then continue.
