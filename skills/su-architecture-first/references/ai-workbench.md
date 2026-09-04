# AI Workbench Patterns

Use this reference only for AI workbenches, multi-agent products, file intake, workbench UI, execution handoffs, or Chat-first decisions.

## Start With the User Value Flow

A common workbench flow is:

```text
express a goal
  -> provide material
  -> clarify or confirm material actions
  -> receive progress proportionate to the wait
  -> receive a usable result
  -> continue, save, archive, or resume
```

Internal roles, routes, jobs, payloads, prompts, tool calls, logs, and state transitions may implement this flow. They are not normal-use prerequisites.

## Chat-First Is Conditional

Use Chat as the primary surface when the user's main job is expressing needs, supplying context, deciding, and receiving results. Use a dashboard as the primary surface when the main job is monitoring many stable metrics or operations.

Supporting navigation answers where the user is working. Supporting panels surface the current task, result, and light context. Advanced or debug views may expose internal detail without turning it into a second required workflow.

## User-Visible and Internal Objects

Typical user-visible objects:

- conversation or request;
- supplied material;
- confirmation or decision;
- current task and proportionate status;
- output and next action;
- archive or resume point.

Typical internal objects:

- job or work package;
- payload and route score;
- model prompt and tool call;
- execution log and state transition;
- worker or agent topology.

The system performs internal routing. Ask the user to choose an internal mode or executor only when that choice has genuine product value.

## Real Execution

Scripted messages are suitable for deterministic state such as “uploaded” or “saved” when the state actually occurred. Conversation should use the intended model path, and execution success should be driven by a real tool or worker result.

A production path should connect:

```text
request -> authorization -> execution -> result -> visible return -> durable save
```

When execution is unavailable, present the real boundary and a useful next action. A timer or canned response is not evidence of file reading, image generation, publication, or any other external action.

## Visible Deliverable Contract

For a generated object that matters to the user, make clear:

1. Where it is now.
2. What happens next.
3. Who or what performs that step.
4. Where progress or the result returns.
5. Where the durable result is saved or archived.

Transitional work packages may be visible during a prototype, but they should still close this loop. A mature product can keep the package internal and surface only confirmation, progress, output, and recovery.

## File Intake

- Read supplied text through a supported path.
- Use an actual parser or execution layer for formats that require one.
- Keep file contents out of input fields unless the user intentionally pasted them.
- If the current layer cannot access a local path, route to the available execution layer or state the required handoff precisely.
- Return the result to the user's current work context instead of requiring manual shuttling between model and executor.

## Workbench Regression Areas

When they are affected, check:

- composer consistency, including send/newline behavior and unwanted browser autocomplete;
- viewport stability after sending;
- Chat remaining the main surface when conversation is the primary job;
- side panels remaining supporting surfaces;
- one authoritative task state across Chat, navigation, and status views;
- ordinary conversation using the intended model path;
- deterministic status messages reflecting real state;
- no false execution claims or exposed route/internal IDs;
- every file type using a supported read path;
- task, executor, return location, save location, archive, and resume behavior;
- context cleanup using a capacity signal appropriate to the actual environment rather than a universal fixed percentage.
