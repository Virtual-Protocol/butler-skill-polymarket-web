# Changelog

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
