# Layer Model

Use this reference to locate responsibility and sources of truth. Adapt the names to the actual system; the purpose is ownership, not compliance with a fixed diagram.

## Stable Layers

| Layer | Typical objects | Ownership question |
|---|---|---|
| User-visible | Conversation, input, navigation, output, confirmation, status | What must the user understand or operate? |
| Dialogue and orchestration | Intent, routing, sequencing, approvals, model responses | Who decides what happens next? |
| Domain and policy | Business rules, permissions, eligibility, invariants | Where is the rule defined and enforced? |
| Data and state | Messages, tasks, jobs, files, results, lifecycle state | Which record is authoritative? |
| Execution and integration | Tool calls, workers, APIs, file access, browser actions | What performs the real external work? |
| Memory and lifecycle | Context summaries, archives, resume points, retention | How can work be resumed or audited? |
| Validation and version | Tests, contracts, observability, release anchors, rollback | What proves the behavior and preserves recovery? |

Not every system needs every layer. Add a layer only when it owns a distinct decision or source of truth.

## Ownership Trace

Trace a problem through this chain:

```text
visible symptom
  -> interaction or entry point
  -> owning layer
  -> responsible component or policy
  -> authoritative state or source
  -> execution path
  -> validation signal
```

The visible surface is often where a problem appears, not where it is owned. A label can reveal a state error; a sidebar can reveal duplicated lifecycle state; a manual handoff can reveal a missing execution integration.

## Source-of-Truth Test

For every material fact, ask:

1. Which object is authoritative?
2. Which objects are projections or caches?
3. Who may write the authority?
4. How do projections receive updates?
5. What happens when an update is delayed, duplicated, or reordered?
6. Can two components independently claim authority?

When two surfaces disagree, correct the authority and projection path before changing either display.

## Representation Depth

- Reuse an existing architecture when it already locates ownership.
- Update the affected branch when the rest of the structure is stable.
- Use a compact table when several problems need the same fields.
- Draw a tree or flow when relationships, lifecycle, or handoffs remain ambiguous.
- Stop expanding when the current decision and regression boundary are checkable.

