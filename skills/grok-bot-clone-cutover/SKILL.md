---
name: grok-bot-clone-cutover
description: >-
  Use when a Grok Bot conversation is fat and burning weekly usage, when a
  live lead must move to a new empty bot, or when someone says 轉移 / cutover /
  建立副本 / blank successor.
---
# Grok Bot blank transfer

A fat thread burns weekly usage on every turn. Thin Grok Bot is not enough. End the old thread. Do not copy it.

Official Duplicate takes the conversation with it. That is the failure this SOP is for. Make a **blank new bot** (settings only). Memory does not follow. Write a handoff first.

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `HANDOFF` — usually `$PIPELINE_ROOT/handoff.md`
- `HELPER` — any helper whose description or secrets bake a bot id

Thin-bot first: [thin-bot](sand-workflow:thin-bot).

## When not to

- The bot is still thin. Use [thin-bot](sand-workflow:thin-bot).
- You only want a new chat with the same bot. This product has no such button.
- No handoff file yet. Do not create the new bot first.

## Do

1. Write a short handoff on the shared computer (`$HANDOFF` or `$PIPELINE_ROOT/handoffs/handoff-*.md`): live id, retiring id, teammate ids, cron list (which stay paused), approvals / safety, what the new bot reads first. Complete when that file exists and names both ids.
2. Ask the operator to create a **blank** new bot, rename it, and paste the new id into the handoff. Do not use Duplicate / 建立副本.
3. Pause every listener and scheduled routine on the **old** bot first. Two listeners on the same channel reply twice.
4. New bot reads the handoff. Recreate only the **enabled** old routines (same name, prompt, trigger). Leave paused ones paused (watch/chase routines stay paused if they were paused). A blank bot inherits none of them.
5. Copy `connector-secrets/<old-id>/` to `connector-secrets/<new-id>/`. Retarget any `HELPER` that bakes the old id.
6. Update every teammate description **before** delete: new lead id, and "do not SendToAgent the old id". Change only that sentence. Do not rewrite names.
7. Prove one pipe: one channel (or equivalent) message, exactly one bot replies. Ignore the new bot's own mouthpiece. No 收到 fan-out.
8. Operator deletes the old bot: sidebar, right-click the row → Delete. This app has no Hide. Do not delete before steps 6 and 7.

Complete when `$HANDOFF` names the new live id, teammate descriptions point at it, the one-pipe proof passed, and the old bot is deleted.

## Do not

- Use Duplicate. The copy stays fat.
- Change teammate descriptions after delete, or leave them pointing at the old id.
- Reply to the old bot after the new id is live.
- Enable a paused watch/chase routine "just in case".
- Leave old listeners / cron running.
- Fan out daily cards, transcripts, or 收到.
- Put this SOP in a project-repo `AGENTS.md`.

## After

The blank bot is the lead. Global view is the handoff file plus teammate files, not the deleted transcript.
