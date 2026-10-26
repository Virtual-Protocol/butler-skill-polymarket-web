# Changelog

## 0.1.28

- Step 4 reads cash from the API too, so "my balance" needs no browser.
  `pmweb.py cash <PM_WALLET>` reads the pUSD address from Polymarket's
  contracts page, then `bevo-rpc 137 eth_call` `balanceOf` on Polygon.
  No `cash $` line (RPC down): read **Cash** atop the site, as before.
- Withdraw 9b asks with step 4's cash; the portfolio page opens only after
  the yes. Rule 2 ("They have it") uses step 4's cash.
- `bevo-rpc` added to `requires.bins`.
- Trims to stay under the 12,000-char cap (no rule removed): reader helpers
  deduplicated, failure table as a list.

## 0.1.27

- Wallet login instead of email (branch `wallet-login`, needs virtuals-agent
  PR #269 and bevo-server PR #1227). Step 3: `browser_connect_wallet`, then
  **Log in** → **Rabby**; the sign-in is an owner card, and the Butler checks
  `browser_connect_wallet` every ~20 s for its outcome (up to 5 min).
- Bet and sell: Enable Trading and each order are owner cards; wait as in 3d.
- Claim and withdraw are not signed in this build: say so, stop.
- Security 4: expect only the sign-in, Enable Trading and order cards.

## 0.1.26

- Step 10: log out. Open the portfolio, click the picture, then `browser_act`
  "click Log out in the open menu" at once. Log out sits below the language
  row, off screen, so the Butler used to loop looking for it. If the click
  misses: scroll the open menu down, click once more.
- Trims to stay under the 12,000-char cap (no rule removed).

## 0.1.25

- The Platforms row updates only after a fund or a withdraw, never after
  a bet, sell or claim (those only swap cash and shares). The 0.1.24 bet
  spent 5 steps (~1.5 min) on it. Owner's call, 2026-10-26.
- Buy and sell click with `browser_act` and the button's exact words
  ("click the blue Buy Yes button"): the 0.1.24 bet's first, vaguer
  submit click missed and cost 3 steps. After the click, one check only.

## 0.1.24

- Bet turn on 0.1.23 ($2 Fed No change): 4+ min spent typing into
  **Amount** (typed, re-typed, clicked the field, re-checked). The box
  does not take typing reliably. Step 7b now builds the dollars with the
  form's **+$1** / **+$5** / **+$10** buttons, and 7c reads Amount back:
  not their dollars, stop (a wrong Amount would bet the wrong sum).

## 0.1.23

- Bet and sell now ask from the API, with no browser in the ask turn, and do
  every browser step after the yes, in one turn. Why: the sell test filled
  the form in the ask turn (3m 06s), the owner answered 3 min later, and
  the click turn looped for 8+ min clicking Sell with nothing sold. The
  form had most likely gone stale, or the session had been closed for
  idling. Filling and clicking in the same turn removes that gap, and the
  question comes in seconds.
- The 2¢ rule now compares the form's price with the API price the owner
  said yes to (7c, 8b).

## 0.1.22

- Step 7c: the first-bet pop-up was a trading-terms pop-up, not only
  Enable Trading. It now names both. (The 0.1.19 run's screenshot also
  confirmed the blue **Buy Yes** under "To win" and the form header
  "<row> · Yes".)

## 0.1.21

- First real bet ($3 Lamine Yamal Yes, 0.1.19): the ask was right, but
  after the yes it ran 6+ min and 30+ steps. The form has no **Trade**
  button: the submit is the blue **Buy Yes** / **Buy No** under Amount. It
  hunted for "Trade", clicked Buy Yes several times, hit the Enable
  Trading pop-up, then went looking for its wallet and deposit address to
  read positions.
- Step 7 now names the blue **Buy Yes** / **Buy No** button, clicks it
  once, clicks it once more only after the Enable Trading pop-up, then
  checks only that cash dropped and replies: no positions, no address
  hunts. Sell names the blue **Sell** button. "Trade" is gone everywhere.
- Trims: the `/value` call (positions has each worth), a shorter budget row.

## 0.1.20

- Sell (close a bet) got a click list in step 8: open the position's own
  market page, **Sell**, the outcome, the shares (all: **Max**), one read
  of question/outcome/shares/payout, ask; after the yes read the payout
  again (2¢ rule), **Trade** once, reply with no checks.
- `pmweb.py positions` prints each position's `url`
  (`/event/<eventSlug>/<slug>`, fields checked live on the data API)
  instead of a slug.
- Cut rules the steps now hold: "say the bad case" (the step 6 question
  has it), "they said yes" and "the price held" (steps 7a and 8d), and the
  "price moved" failure row.

## 0.1.19

- Bet got a click list (steps 6 and 7).
- `pmweb.py event` now prints a `url` per row:
  `polymarket.com/event/<event>/<market>` opens a page for that one market
  (checked live: its title is the market's question). No row hunting on
  events with many rows, and step 6c checks the page's question matches
  the row they chose.
- Step 7: one read of "Avg. Price" right before **Trade** (the 2¢ rule),
  then **Trade** once. **Enable Trading** pops up on the first bet: click
  through it once, then **Trade** once more; never look for it. Reply
  with shares and price, no other checks.
- The question names the market, so the owner sees which one.
- Trims to fit the cap: the bad-case rule, the price-held rule and its
  failure row now point at steps 6 and 7 instead of repeating them.

## 0.1.18

- Withdraw $5 took ~5 min / 15 steps on 0.1.17. Half the steps were
  `browser_observe` before each click, which the login rule asked for.
  Clicks now go straight to `browser_act`; observe only when a click misses.
- Base took 3 tries: the chain menu is two clicks (open, then pick).
  Step 9e now says so, for chain and token. It also says "no reads until
  f" (the run read the form 3 times early).
- The withdraw question is now word for word, with the amount, USDC on
  Base and the wallet's first 6 and last 4 (the run asked "confirm the
  $5 site withdrawal").

## 0.1.17

- Claim got a click list (step 8), from the 3m 33s / 14-step redeem run:
  Portfolio → **Redeem** on the row → close the auto-redeem pop-up (never
  turn it on) → **Redeem** in the "Redeem Yes for $X" pop-up → "Cashed
  Out" → close any pop-up and reply. The run lost steps to a double click,
  two early API checks, a button/dialog check and a balance re-check.
- Sell and claim now always ask first: the run took "can you help redeem
  the win" as the yes.
- Trims to fit the cap: When to use is one paragraph; dropped the "Order
  failed" row (Idempotency covers it) and a "Say to the owner" line the
  steps now cover.

## 0.1.16

- Withdraw skipped the question: the owner wrote "withdraw all my cash" and
  the Butler took that as the yes. Step 9b now says always ask, then wait;
  the request is not the yes. Security rule 5 says the yes must answer
  your question.
- Step 9g ends with "Then reply; no other checks", so it stops reading
  positions after the withdraw.
- Dropped the "daily budget" line (the failure table already covers it)
  and a few words to fit the cap.

## 0.1.15

- Withdraw got stuck 5+ min on the token dropdown: it ran a form check
  early, then clicked USDC again and again "to be sure". Step e now says:
  pick chain first, then token; if a value already shows, leave it; one
  click each; no re-click and no check before step f.
- Step f is one `browser_extract` of all four fields. Small word trims to
  fit the cap.

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
