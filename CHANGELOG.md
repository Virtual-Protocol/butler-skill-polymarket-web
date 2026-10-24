# Changelog

## 1.1.0

- Duties can use Polymarket: `bet`, `sell` and `redeem` only, never `deposit` or
  `withdraw`. No card per act when the owner armed a Polymarket allowance on the duty
  (budget, max per bet, runs a day, optional markets). A duty sells or redeems only its own
  bets. The first bet must happen from chat (account setup).
- The Butler proposes the allowance in chat; the owner sets it on the duty in the app. The
  Butler never says it is set. Example duty added.
- Chat bets: say the market back, then end the reply and wait for "yes". Never say it back
  and file in the same reply.
- New refusal codes: `POLYMARKET_DUTY_NOT_ARMED`, `POLYMARKET_DUTY_OP`,
  `POLYMARKET_DUTY_MAX_BET`, `POLYMARKET_DUTY_BUDGET`, `POLYMARKET_DUTY_MARKET`,
  `POLYMARKET_DUTY_RUNS`, `POLYMARKET_DUTY_NOT_OWN`, `POLYMARKET_SETUP_NEEDED`,
  `POLYMARKET_COUNTRY_STALE`, `POLYMARKET_DUTY_BROWSER_RESERVE`.
- The account read is allowed from a duty.
- A duty answer with any `code` sent nothing: never count it as spent. Only no answer or
  a timeout is unclear.
- A key that got an answer with a `code` is used up: the next try uses a new key, or it
  only replays the old refusal.
- Never check approvals, allowances or the deposit wallet on chain: setup runs inside the
  card's run. A failed card is reported as written; filing again is the owner's call.
  Never refuse because earlier cards failed, never build a workaround or research the
  relayer or keys.
- Duty runs: `accepted` with `auto: true` (no card); refusals are 403/402/429/409 and
  only `POLYMARKET_RUN_IN_PROGRESS` (409) is retryable; the market list covers sell and
  redeem; a `--percent` sell is of the duty's own shares.

## 1.0.2

- The skill is now named `polymarket` (was `butler-polymarket-web`). The repo name is
  unchanged.
- A `'` inside a double-quoted value is fine: `--outcome "Don't Know"`. Only a `"` or a line
  break is refused.
- "File it again" in any hint means a new `--idempotency-key`.
- `POLYMARKET_RUN_IN_PROGRESS`, `POLYMARKET_BROWSER_BUDGET`, `POLYMARKET_BROWSER_UNAVAILABLE`
  and `POLYMARKET_UNAVAILABLE` (also a deposit's bridge refusal): wait a minute, file once
  more with a new key, then stop and tell the owner.
- `POLYMARKET_GEOBLOCK_UNCHECKED` also covers an unknown owner country: ask the owner to
  open the Virtuals app once, then file again with a new key. Never say they are blocked.
- Account read: within 60 s the server returns the last good read with `asOf`;
  `POLYMARKET_READ_BUSY` only while a read is running. A read under 60 s old counts as
  fresh after the owner's yes.
- A card may fail with "file again with a new key" when the account needed setup the card
  did not include.
- "Polymarket may be off: relay the refusal."

## 1.0.1

Matches the final server runner and container forwarder.

- Outcomes go in double quotes: `--outcome "Team Liquid"`. No single quotes; a value with a
  quote mark or a line break is refused.
- `sell` is a fill-now order with a worst price on the card, like `bet`; `--shares` takes up
  to 2 decimals, `--percent` 1 to 100. A bet is at least $1.
- `POLYMARKET_READ_BUSY`: one account read a minute; wait, never loop.
- A failed card that says "check Polymarket before asking again": read the account, never
  resend.
- New codes: `POLYMARKET_GEOBLOCK_UNCHECKED` (never say the owner is blocked),
  `POLYMARKET_UNAVAILABLE`, `POLYMARKET_MARKET_CLOSED`, `POLYMARKET_NOTHING_TO_REDEEM`.
  `POLYMARKET_GEOBLOCKED`: stop, never try anything else.
- The first bet, sell or redeem card sets the account up and says "First time"; a deposit
  is a plain send card.
- polymarket.com is blocked in the Butler's browser.
- `positions[].market` is the slug `--market` takes; positions also carry `question`.

## 1.0.0

Rewritten in the shape of the Variational skill: no browser at all. bevo-server runs every
money act after the owner approves its card.

- Reads: `bevo-read get /polymarket/account` (cash, positions, open orders). Market search
  stays a public `curl` to `gamma-api.polymarket.com`, GET only (`references/markets.md`).
- Money: `acp polymarket bet|sell|redeem|deposit|withdraw ... --idempotency-key <KEY>`,
  one card each, then `bevo-read request <KEY> --route polymarket`.
- The first money card on a new account also sets it up; no login, email or code steps.
- `POLYMARKET_GEOBLOCKED`: say it is not available in the owner's country; never another
  country or a VPN.
- Limits: deposit at least $2, withdraw at least $3, no other cap (the $50 hold cap is gone).
- Chat only; a duty is refused.
- Dropped: the browser login, the Butler email, `bevo-send` deposits, the Python reader,
  `cat`, `python3` and `bevo-send` from `requires.bins`.
- New references: `commands.md`, `account.md`, `markets.md`.
- Needs the server runner (`POST /butler-exec/polymarket`, route `polymarket`), the
  container's `acp polymarket` group, and the registry validator knowing that group.

## 0.1.2

- Login code: click Continue once, wait on the inbox without touching the page,
  type the code into the first of the six boxes inside the pop-up (2026-10-22
  staging run typed it into search, which closed the pop-up, and looped for
  20 min re-sending codes).
- Pop-up closed or "Page not found": screenshot, start over once, use only the
  newest code.

## 0.1.1

- Set up the Butler's email before the browser when it has none (`setup_email`,
  owner names it): the browser now refuses only an account step without one.
- Login: type the email inside the Log in pop-up, never the search bar
  (2026-10-22 staging run typed it into search).

## 0.1.0

- First draft for the with-skill / without-skill experiment: trending, search and
  positions through the public API; login with the Butler's own email; deposit with
  `bevo-send`; buy, sell, claim and withdraw on polymarket.com.
