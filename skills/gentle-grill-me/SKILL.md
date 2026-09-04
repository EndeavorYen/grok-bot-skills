---
name: gentle-grill-me
description: >-
  Use this when the ask is vague, a plan needs a stress-test, or the operator
  says gentle grill me / runs /gentle-grill-me. Stress-test the plan, not the
  person. One decision card per round. For relentless original grilling, stay
  on that other skill.
---
# Gentle grill me

Portable adaptation of https://github.com/EndeavorYen/gentle-grill-me (public). This folder is the SKILL.md procedure only.

Speak in the operator's language.

## Mechanics

Interview until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled. Default is one card per round, then wait. `batch` → batch mode; `one` → one-card mode. Mode persists until they switch. Batch at most 3 cards; if the frontier is larger, take the 3 that most unblock the tree.

Each question is one card, one decision. When the host has a multiple-choice question tool (`ask_user_question`, `AskUserQuestion`), use it for the round's cards: options go in that tool. Leave no empty options list. One tool call per round. Markdown list is fallback only when that tool is missing.

Map onto the tool: question = decision title (plus one extra sentence only if the title is not enough); options = the choices, including skip when skip is offered; recommended option first, label marked `(Recommended)`; ➡️ bet in that option's description.

Markdown fallback:

```
### Q1 · <decision title>
<optional extra sentence only if the title is not enough to know the decision>
- <option>
- <option>
➡️ <recommended answer>
```

The title carries the decision. The question body may contain only the decision. Keep out of the body: background, recap of settled nodes, why this is being asked now, a second decision.

Each round the operator answers reshapes the tree. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a later round.

Finding facts is the agent's job. When a frontier question needs a fact from the environment, look it up (or dispatch a sub-agent) and ask only the decisions. Complete the fact-lookup before blocking downstream cards; keep asking unblocked cards up to the cadence cap.

The session is done when the frontier is empty: every branch visited, nothing left silently assumed. Complete when the operator confirms shared understanding. Stay off implementation in this session.

## Stance

Overlay on Mechanics. Keep tree coverage and recommended answers.

### Open, then Round 1

Render this opening in the operator's language, keeping all three ingredients: rationale, acknowledgment, choice. Then immediately Round 1.

I'll pressure-test this plan with you so the weak joints show up before they cost you. Being examined is a common side effect; that isn't the job. Skip a question, ask a batch, or throw out my recommended answers; those are bets, not verdicts. Default is one question. Your call.

### Appraisal

Unless the first message is clearly a low-stakes technical choice, include this on the Round 1 frontier (options include skip):

If this call turns out wrong, what is the costly part?

Options: time / money / reputation / having to reverse something already said / low-stakes — skip this.

### Silent calibration

Keep inferred emotion off the transcript.

- Certain or breezy language → still run the premortem.
- Worst-case hedging → still premortem; skip piling "you missed this".
- Short, sarcastic, or pushback while in batch → switch to one card.
- A short answer that names only some of the options settles those named. Unnamed options stay unselected.

### Wording (questions and recommended answers)

Applies to card title/body, option labels, and ➡️ recommended answers, including paraphrases in the operator's language.

Use: could, might, one option is, my bet is, what would have to be true for this option to work, here's what I think and how I got there — where does this break.

Keep out: must, should, obviously, why would you think, the right answer, don't you agree, how could you choose that.

Critique the plan. Treat the current choice as a live hypothesis.

### Recommended answers

Default compressed ➡️ shape: one sentence on when their current option holds + `My bet:` ... + reject if it doesn't fit. Only the premortem round expands a cause list.

If the operator rejects the same class of recommended answer twice this session, stop offering that class.

### Skip

Skip = explicitly deferred, not settled. Deferred nodes stay on the frontier as open assumptions. Closing recap lists them as open assumptions. A skip visits the node and leaves it open; leave it off the next unanswered round. Empty for close = no unanswered non-deferred question remains.

### Conflicts

When a later answer contradicts a settled node, put that contradiction on this round's frontier as one question: keep the earlier node, replace it with the later answer, or split (earlier stays the goal, later is execution). Append the supersession to the grill log before the next question.

### Intensity

Intensity is cadence only. It leaves tree coverage and wording contract intact. Default is one card. Batch is opt-in.

### Premortem

After the subject and success criteria are clear, run one premortem round before deep solution branches. If new substantive claims appear later, run another before close.

Past tense: assume the plan as it now exists has already failed. The subject of failure is the plan. Offer 2–3 most likely causes as recommended answers. The operator may pick one primary cause; unranked listed causes stay unselected.

### Persist

After every settle, skip, or supersede, append one JSONL record to `.gentle-grill/grill-log.jsonl` **before asking the next question**. The file is append-only. If append fails, fix it before the next question.

Minimum fields: `id`, `question`, `options`, `chosen`, `rejected`, `status` (`settled` | `skipped` | `superseded`), `supersedes` (if any).

Complete when the new line exists on disk.

### Close

When the frontier is empty, close with a decision log rendered from `.gentle-grill/grill-log.jsonl`: settled, deferred, open assumptions, superseded nodes, bets the operator rejected. Read the file; skip inventing the close log from chat memory. Ask for confirmation. After the operator confirms the close log, this session stays off implementation. A later session reads the file first.

Complete when the close log is shown, confirmed, and this session has stopped.
