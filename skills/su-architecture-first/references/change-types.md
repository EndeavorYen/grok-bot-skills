# Change Types

Use one primary type for each action. A project can need several types, but each should have a clear owner and dependency order.

| Type | Use when | Evidence |
|---|---|---|
| Delete | Logic, state, UI, or terminology is wrong, obsolete, duplicated, or superseded | Callers and required behavior remain covered after removal |
| Refactor | Correct behavior exists but ownership, state, or policy is fragmented | One authority or seam serves every relevant entry point |
| Implement | A promised capability or enforceable behavior does not exist | A real end-to-end path produces and persists the expected result |
| Hide | Internal detail remains useful for advanced operation but is not part of normal user work | Normal flow succeeds without understanding the internal object |
| Copy change | Structure and behavior are correct but language misstates meaning or next action | Users can interpret the state and act correctly |
| UI polish | Structure, behavior, and language are correct; hierarchy or visual consistency remains | Visual and interaction checks pass without masking a structural defect |

## Classification Tests

- If the current path should not exist, **delete** it.
- If multiple paths should become one authority, **refactor** them.
- If the system only simulates or promises the capability, **implement** it.
- If operators still need the detail but ordinary users do not, **hide** it behind an advanced boundary.
- If the behavior is correct and only its meaning is unclear, make a **copy change**.
- If meaning and behavior are correct and only presentation remains, apply **UI polish**.

## Dependency Order

Use this default order when several classes interact:

```text
delete obsolete paths
  -> refactor ownership and sources of truth
  -> implement missing capability
  -> hide internal complexity
  -> correct copy
  -> polish UI
```

Change the order when evidence requires it, and state the dependency that justifies the exception.

## Mixed-Symptom Example

An obsolete upload panel is a deletion. Two composers with separate drafts are a refactor. A claimed but unwired PDF reader is an implementation. Raw job payload in the normal flow should be hidden. An internal term on a user button needs a copy change. Inconsistent spacing is UI polish after the flow is structurally sound.

