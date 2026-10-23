# Changelog

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
