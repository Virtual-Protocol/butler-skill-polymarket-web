# Changelog

## 0.1.3

- Geoblock: compare the check's `country` with the owner's first. A different
  country is a proxy miss (Browserbase falls back to the closest exit, e.g.
  Singapore for Malaysia): set the country again and recheck once, never call
  it a block. 2026-10-22 run told a Malaysian owner Polymarket was closed.

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
