# Regression Design

Define evidence before mutation. A regression gate should distinguish the intended structural change from a plausible local patch.

## Evidence Layers

Choose the evidence that matches the change:

- **Structure:** ownership, source-of-truth, dependency, or call-path inspection.
- **Behavior:** unit, integration, end-to-end, scenario, or contract tests.
- **State:** lifecycle transitions, projections, persistence, and recovery checks.
- **User path:** interaction, content, accessibility, or rendered visual checks.
- **Execution:** tool results, logs, durable outputs, and failure-path evidence.
- **Version:** diff, commit, release anchor, rollback point, or migration boundary.

Static checks are supporting evidence. They do not substitute for the user-visible behavior that motivated the change.

## Pre-Implementation Gate

Before mutation, record:

1. Desired observable outcome.
2. Old failure mode or counterexample.
3. Owning layer and source of truth.
4. Entry points and projections that must agree.
5. Behavior to remove and behavior to preserve.
6. Checks to run before and after the change.
7. Untested or externally constrained areas.
8. Rollback or version boundary for a material change.

When the user has already authorized implementation, proceed after this gate without requesting duplicate approval.

## Recurring Failure Gate

For a repeated problem:

- reproduce or precisely identify the old failure;
- explain why the previous fix did not remove its cause;
- test every equivalent entry point, not only the reported one;
- verify the authoritative state and its projections;
- run the original scenario after the fix;
- preserve a regression check at the correct seam when one exists.

If the cause cannot yet be established, continue diagnosis before treating the change as a verified fix.

## Completion Gate

A structural change is complete only when:

- relevant syntax, type, or static checks pass;
- applicable behavioral and scenario checks pass;
- the old failure mode no longer reproduces;
- preserved behavior remains intact;
- visible outputs and state transitions close their loop;
- untested areas and residual risks are explicit;
- a recoverable version anchor exists for material behavior changes.
