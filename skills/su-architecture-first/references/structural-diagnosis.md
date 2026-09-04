# Structural Diagnosis

Use this reference when a failure returns or the system is accumulating rules, workarounds, and local fixes.

## Structural Signals

Investigate structure first when any of these is true:

- A previous fix passed but the user-visible failure returned.
- The proposed fix adds another prohibition, special case, or synchronization rule.
- A user or operator must remember a workaround.
- Two entry points or surfaces disagree about the same fact.
- Internal orchestration becomes required user knowledge.
- A prompt is expected to prevent a state that code or policy could make impossible.
- One entry point was fixed while equivalent paths remain unchanged.

A structural signal is a hypothesis, not a verdict. Confirm it against current source, state, and execution evidence.

## Diagnosis Chain

For each material problem, establish:

1. **Symptom:** the exact observable failure.
2. **Owner:** the layer and object capable of producing it.
3. **Authority:** the policy, record, or component that should decide the truth.
4. **Competing path:** duplicate logic, state, entry point, or handoff that can disagree.
5. **Root cause:** the mechanism that permits the bad state.
6. **Prevention:** the smallest structural change that removes or constrains that mechanism.
7. **Evidence:** the check that fails on the old behavior and passes on the corrected one.

Distinguish observed facts from inference. If the available evidence cannot locate a cause, continue the diagnosis until evidence can support the claim instead of presenting a structural guess as fact.

## Delete and Consolidate Check

Before adding behavior, look for:

- obsolete components or flows;
- competing sources of truth;
- copied policy or state transitions;
- scripted success responses without execution;
- debug or transitional objects exposed in the normal user path;
- patch-specific conditions masking a missing invariant;
- separate implementations of one interaction contract.

Deletion is an evidence-backed outcome, not a default. It is correct only when the removed behavior is wrong, obsolete, duplicated, or replaced by an authoritative path, and acceptance or regression evidence covers what must remain. Simplicity alone is not evidence. Preserve behavior that still carries a real user or system obligation, and never delete outside the user's approved scope.

## Stronger Fix Test

Prefer the change that makes the invalid state difficult or impossible:

- Two surfaces independently refresh copied task state -> both project from one authoritative lifecycle record.
- A model is told not to expose a payload -> the normal user view never receives payload fields.
- A UI claims a file was processed after a timer -> a real execution result drives the visible state.

Use prompt or copy constraints for communication behavior they genuinely own, not as substitutes for enforceable state, policy, or execution design.
