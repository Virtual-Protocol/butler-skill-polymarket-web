# Changelog

## 0.1.14

- Withdraw: two explicit stops so the Butler never improvises. Step f: the
  address box can't be read back: stop, never a screenshot or a script.
  Step g: the Withdraw click is refused or does nothing: stop, never
  another way.
- Dropped a Limits line that repeated "the account is yours", and shortened
  "your owner only answers yes or no", to fit the cap.

## 0.1.13

- Withdraw (step 9) is a click list a-g from the 2026-10-23 walkthrough:
  `bevo-read me` for the one allowed address, /portfolio, read Cash and ask
  first, then Withdraw, Crypto address, type address + amount (Max for all),
  Receive token USDC, Receive chain Base, check all 42 characters, click
  Withdraw once. Asking before the form means no form left open while the
  owner answers. No screenshot.
- Dropped the screenshot from the bet form and the captcha row.
- Shorter retry rules and wording to fit the 12,000-char cap.

## 0.1.12

- Fund: read the deposit address fresh on every deposit, never one
  remembered from an earlier chat or run. The 2026-10-26 run skipped
  Transfer Crypto and sent to an address it already knew (right this time,
  but wrong if it is ever logged in to another account).

## 0.1.11

- Fund: the 2026-10-26 run took 6 min / 24 steps (re-checked login, opened
  the chain menu, then tried to decode the QR picture and install a QR
  reader). Now: open the site, Deposit, Transfer Crypto, read the field
  under "Your deposit address", send. Never a screenshot, a QR code, or a
  command or package to read the address; leave the chain menu alone.
- Shorter "When to use" and failure table to fit the 12,000-char cap.

## 0.1.10

- Fund (step 5) is a click list a-e like login: Deposit, Transfer Crypto,
  read the one EVM address and the $3 Base minimum, keep the pop-up open,
  send the `bevo-send` card, watch the pop-up's balance update by itself.
  From the 2026-10-23 walkthrough (the chain menu is not needed).
- Trimmed two lines that repeated other rules, to stay under 12,000 chars.

## 0.1.9

- Login (step 3) is now a click list a-g: each line says what to click or
  type, what the screen should show after ("See"), and the one fix to try
  when it does not ("Else"). Buttons are found by their visible words. The
  same list is meant to become a click helper's step list later.
- An Enable Trading pop-up after login is closed, never searched for; it
  comes back at the first bet.

## 0.1.8

- Login: `browser_type_email` (`submit: true`) and `browser_type_code` with
  `fromInbox` (virtuals-agent). The inbox polling, `extract-otp` and the email
  read-back are gone from the skill: about 6 fewer steps. Needs the agent
  build that has both tools.

## 0.1.7

- `acp email extract-otp` takes `--message-id <id>` (acp-cli 1.0.38); the bare
  `<id>` form failed once on every login.

## 0.1.6

- New account: fill "Choose a username" with the email's local part, tick the
  terms, Continue; never ask the owner. The 2026-10-22 run stopped there to ask.

## 0.1.5

- Speed (2026-10-22 run logged in but took 11m 50s, 46 steps):
  - Clear the email field before typing, then read it back. The field kept an
    old address and act typed a second copy after it, so no code was sent.
  - Use only a Polymarket mail that arrived after this Continue. The run took
    a dead code from an earlier attempt.
  - One `browser_observe` per click, no screenshot unless something is wrong.
  - Write the reader only before steps 2 and 4, not before a login.

## 0.1.4

- Login code: type it with `browser_type_code` (virtuals-agent), never
  `browser_act`. Even with the skill, act put the code into polymarket.com's
  search bar twice on 2026-10-22.

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
