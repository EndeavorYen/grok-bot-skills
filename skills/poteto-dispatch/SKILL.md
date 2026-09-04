---
name: poteto-dispatch
description: >-
  Use this when a coding lead assigns repo work to an implementer (CloudAgent
  or local build CLI). Dispatch is one line: /poteto-mode <goal>. Same PR #<n>.
  Done when: <runnable proof>. Merge when CI is CLEAN and that condition is met.
---
# Poteto dispatch

pstack / poteto-mode is the quality bar. This skill is the **one-line assignment contract**. The playbook lives in pstack; keep it out of this file.

Heavy work placement: [thin-bot](sand-workflow:thin-bot). CloudAgent model: [cloudagent-model-lock](sand-workflow:cloudagent-model-lock).

## Roles

- **Coding lead** (or Decision lead acting as chief) writes the dispatch line and names the implementer.
- **Implementer** runs poteto-mode against that line (CloudAgent or local build CLI).
- Merge is a lead/operator call after CLEAN + Done-when.

Fill in environment-local bot names. This pack uses role titles only.

## Dispatch line

One line, no extra brief:

```
/poteto-mode <goal>. Same PR #<n>. Done when: <runnable proof>.
```

Rules:

- `<goal>` is the change in operator language, one sentence.
- `Same PR #<n>` pins the existing PR. For a new change, write `Same PR: open one` and the implementer returns the number on first push.
- `<runnable proof>` is an observable: a command, a test name, a URL that shows the behavior, a screenshot path. "looks good" is not a proof.

Complete when the implementer has that exact line as the task prompt (chat, CloudAgent prompt, or CLI `-p`).

## Run

Implementer:

1. Invoke poteto-mode with the dispatch line as the goal.
2. Stay on PR `#n` (or open one and report the number).
3. Run the Done-when proof.
4. Return: PR URL, proof output/path, CI status.

Complete when those four artifacts exist. Lead stays off implementing in the assignment turn.

## Merge gate

Merge when:

- CI is CLEAN (or the lead records named exceptions), and
- the Done-when proof passed on the PR head.

Complete when the lead states CLEAN + proof, then merges or asks the operator to merge.

## Anti-jobs

Keep extra strategy memos, second PRs for the same goal, and silent scope adds off this dispatch. A new goal is a new line.
