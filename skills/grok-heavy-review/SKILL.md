---
name: grok-heavy-review
description: >-
  Use when assembling or running a Grok Heavy post-open review — POSTOPEN
  REVIEW v1 book table (weights/receipts/session), upload full .md, review
  layer only (agree/oppose/WATCH), never mix in best-portfolio advice.
---
# Grok Heavy review

After a market open, review the line's paper book for **logic errors / information errors**. Review only. Do not authorize orders.

## Fill-ins (set once per environment)

- `PIPELINE_ROOT` — shared disk bus
- `HANDOFFS_DIR` — usually `$PIPELINE_ROOT/handoffs/`
- `BOOK_CCY` — book currency
- `HOME_MARKET` / `FOREIGN_MARKET` — session labels (example: home vs overnight)
- `ISSUES_TRACKER` — where review holes are filed (issue tracker or board path)
- **Decision lead** — assembles the packet, SendToAgent the Heavy runner, converges findings
- **Heavy runner** (Implementer) — opens grok.com Heavy, uploads the full `.md`, writes reply files to the bus
- **the operator** — login / 2FA only

## Two layers (do not mix)

1. **Review layer (this skill):** only ask "does the line's recommendation still hold after open?". Output may be `同意` / `反對` / `WATCH` (agree / oppose / WATCH). Do not invent new target weights, do not add size, do not authorize.
2. **Advice layer (another knife / packet):** best allocation (NAV, target band, caps, friction, who sets the benchmark). **Do not** put that in the review packet.

## Hard rules

- `can_authorize` stays false; do not invent share counts; do not place orders
- Heavy verdicts only: `同意` / `反對` / `WATCH`
- **Ban** a short digest in place of the packet; **ban** screenshots-only
- **Input:** prefer upload of the whole file; no upload UI → paste full text + `upload=no` + bytes≈packet
- A market that is not open must not be treated as "post-open live" disproof → `WATCH` + time bound
- Missing "post-open checkable number" or "source actually names the ticker" → that row is WATCH; do not fill from news prose

## Packet = POSTOPEN REVIEW v1 (main table)

```text
# POSTOPEN REVIEW v1
as_of_home: <ISO>
as_of_foreign: NOT_OPEN | <ISO expected>
can_authorize: false
book_nav: <number or MISSING>
book_ccy: <BOOK_CCY>

## Book (required; one row per name; empty cell → that row WATCH)
ticker | action | current_pct | target_pct | delta_shares | last_official_pct_as_of
       | px_ref | px_now | px_chg | weight_now_est | cap_still_breached? Y/N/UNK
       | market | session_state OPEN/CLOSED
       | thesis_type WEIGHT_CAP | FUNDAMENTAL | FLOW | TIMING
       | source_id | source_actually_names_ticker? Y/N
       | blocker (ex-div / foreign-closed / research_stale / none)

## Receipts (one row each)
id | url | date | tickers_named | claim_one_line | supports_action? Y/N/NA

## Locks
ticker | lock_reason | unlock_trigger | still_locked? Y/N

## Forbidden
- do not invent share counts
- do not change can_authorize
- a market that is not open must not be used as post-open live disproof
```

**Minimum viable (missing any → expect more WATCH):** Book action rows + **post-open weight_now / current_pct** + **receipt mapping (named Y/N)** + **market open/closed**.  
News full text may be an appendix, **not** the main table.

### Assemble order (fill these four cells first)

1. Named as_of `weight_now` for TRIM rows
2. Same-day official notice for names in an ex-div / distribution window
3. Foreign market not open → keep WATCH until that open (do not push Heavy to judge EXIT / TRIM there)
4. Receipt: which source named which ticker; if it did not name it, do not hang that row

Write: `$HANDOFFS_DIR/YYYYMMDD-HHMM-heavy-packet-<market>-postopen.md` (`test -s`; main table non-empty).

## Heavy runner delivery

Three paths, each `test -s`:

1. `…-reply.md` — Heavy **full markdown** plus the verdict table
2. `…-findings.md` — six columns + `upload=` / `packet_bytes=` / `paste_or_upload_bytes=` / source packet
3. Optional screenshot (must not replace reply)

Fail: no upload and bytes≪packet; screenshot only; no verdict table.

## Decision lead converge

- Check upload / bytes → report to the operator
- Oppose / WATCH / logic error → file on `ISSUES_TRACKER`
- Update `BOARD`

## Done when

- Review packet = v1 main table (not a prose dump); Heavy received the whole file
- reply is full markdown; holes are filed
