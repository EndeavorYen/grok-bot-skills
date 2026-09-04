---
name: su-architecture-first
description: >-
  Use when the request contains 架构优先 (including 架构优先的方式), or asks for
  first-principles, layered, ownership, or source-of-truth analysis; also for
  recurring failures, patch accumulation, conflicting state, unclear ownership,
  cross-layer or high-risk work, or user-facing leaks of AI or workflow
  complexity. Quick pass for clear local changes; full pass when risk or
  ambiguity warrants it. Decision preflight, not a full-system diagram or
  technology-selection workflow.
---

# Su Architecture First

Upstream: https://github.com/doublesq97-ui/su-architecture-first  
Last-synced: 2026-09-04  
Copyright: Su / doublesq (MIT). See `LICENSE` in this folder.

Run the smallest architecture-first preflight that makes the work safe and checkable. Decide what should change, which layer owns it, and what evidence will prove it.

## Scale the Depth

Use the same reasoning spine for engineering work of any size, but scale the legwork to the risk:

- **Quick pass:** For a local, reversible change with clear ownership, briefly confirm the real goal, owning layer or object, change class, and check, then proceed.
- **Full pass:** For a recurring, cross-layer, ambiguous, high-risk, or costly change—or a user-visible change that materially affects workflow, state, permissions, data, or recovery—run the full sequence and load the matching references. Complete it when each material problem also has a source of truth, an evidence-backed root-cause judgment, a next step, and validation or regression evidence.

This preflight is not a new approval ceremony. Stay in review mode only until the user authorizes implementation for the current scope. When authorization already exists, do not ask again; proceed as soon as the necessary decision and evidence are explicit.

## Choose the Lens

### Engineering and Systems

Use for fixes, features, refactors, recurring failures, patch accumulation, conflicting state, unclear ownership, and implementation planning.

- Trace the symptom or request to the component and authoritative source that can produce it.
- Treat recurrence as structural until evidence localizes it to a one-off fault.
- Before adding behavior, check whether existing logic can be safely removed or consolidated.
- Make invalid states harder or impossible to produce when the system can enforce that rule.

### Customer and Product

Use when engineering choices affect a customer-facing product, service, workbench, or workflow.

- Map the user's value flow before the internal organization or agent topology.
- Keep internal coordination internal; expose only the choices and status the user needs.
- Prefer real execution capability over scripted claims of success.
- Use Chat as the primary surface when conversation is the primary job; use a dashboard when monitoring is the primary job.

## Decision Sequence

### 1. Resolve the real goal

Identify the outcome, affected user or operator, decision boundary, current authorization, and any ambiguity that could change the result.

Infer intent from the available context before asking. Ask only when a material uncertainty could change the result, and ask no more than two questions in one clarification turn. If the user is unsure, do not repeat the same question. Offer concrete options, examples, or tradeoffs and keep guiding until the goal, scope, and task granularity are aligned.

Complete when the desired change and scope are explicit enough to avoid solving a nearby but different problem.

### 2. Inspect the existing system

Read the relevant source, architecture, state model, workflow, or artifact. Identify the current authority for each material fact. Reuse an adequate representation, update only affected branches, and expand the map only when ownership or boundaries remain unclear.

Complete when evidence explains the relevant path and source of truth; label any remaining inference.

### 3. Locate ownership and root cause

Separate the visible symptom from the layer and object that can cause it. Distinguish user-visible objects from internal objects where relevant. For a repeated issue, inspect competing sources, duplicate logic, leaky seams, workarounds, and equivalent entry points before accepting a local explanation.

Complete when each material problem has an owning layer, responsible object, source of truth, and evidence-backed root-cause judgment.

### 4. Classify the change

Choose among delete, refactor, implement, hide, copy change, and UI polish. Deletion is a candidate, not the default. Choose or perform it only when evidence shows that the target is wrong, obsolete, duplicated, or superseded and acceptance or regression evidence covers the behavior that must remain. Simplicity alone is not justification, and this analysis never authorizes deletion outside the user's approved scope.

Preserve meaningful behavior while removing proven residue. Sequence structural work before surface polish.

Complete when every proposed action has one primary class and a reason that matches the owning layer.

### 5. Define implementation and evidence

Stage the work in dependency order. Before mutation, define observable acceptance signals, regression checks, old failure modes, affected entry points, and the rollback or version boundary.

For user-visible deliverables, make clear where the output is, what happens next, who or what executes, where the result returns, and where it is saved.

Complete when each proposed change has a check that distinguishes success from a plausible-looking patch.

## Output

Use the smallest representation that makes the decision checkable. Preserve this reasoning spine:

```text
goal -> relevant structure -> ownership and root cause
     -> change class -> next step -> validation
```

Use prose for a focused decision, a table for repeated mappings, and a tree or diagram only when relationships or ownership are otherwise hard to see.

Do not expand this preflight into a full system map unless the current decision requires one. Do not substitute it for a task-specific technology comparison; for technology selection, use it only to establish the real goal, constraints, ownership, and acceptance evidence before comparing candidates.

## Load References When Their Condition Applies

- Read [references/layer-model.md](references/layer-model.md) when the system needs a stable layer map, architecture tree, or source-of-truth analysis.
- Read [references/structural-diagnosis.md](references/structural-diagnosis.md) when a failure recurs, patches accumulate, state sources compete, or users carry a workaround.
- Read [references/change-types.md](references/change-types.md) when actions are mixed together or their dependency order is unclear.
- Read [references/regression-design.md](references/regression-design.md) for a full pass, when validation or rollback boundaries are unclear, or before declaring a material structural change complete.
- Read [references/ai-workbench.md](references/ai-workbench.md) only for AI workbenches, multi-agent products, file intake, Chat-first design, internal jobs, execution handoffs, or workbench UI.

## Continue the Work

After the preflight, continue with the agent's available tools and methods. This skill chooses direction and evidence; it does not require another skill, a specific agent host, or a fixed downstream workflow.
