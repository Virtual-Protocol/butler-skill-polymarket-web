# Backup: click steps in your own browser

Use this file ONLY when SKILL.md's fallback rule sends you here: the server
answered `not done` (nothing was sent), your owner said yes to trying by hand,
and you are not in a duty. Never after `unknown`, `rejected`, `failed` or a
`CODE: message` refusal.

These are the click steps of 0.1.30. Each signature is a card your owner taps.
Find each button by its **visible words**, never by place. Click with
`browser_act` at once; `browser_observe` only when a click misses; screenshot
only when "See" fails. Each "Else" runs once; then stop, say what you see.

## B1. Connect from your owner's country, then log in

Ask their country once; `browser_set_country`; `browser_navigate` to
`https://polymarket.com/api/geoblock`. `country` not theirs: set it again,
recheck once, then say you could not connect. Theirs and `blocked: true`:
stop. **Never use another country to get around it.**

a. `browser_navigate` to `https://polymarket.com`. See: **Log in** on top.
   Else **Cash** or **Portfolio**: you are in; go to g.
b. `browser_connect_wallet`, once. Never an email.
c. Click **Log in**, then **Rabby** (often under **More methods**, or under
   **Connect Wallet/Social** on the other log-in window; never the MetaMask
   tile). Missing: reload once, click again. "Connector not
   found": **Back**, wait 2 s, **Rabby** again (twice at most).
d. A sign-in card. Say once: *"Tap the card in your app."* Every ~20 s,
   `browser_connect_wallet`: last request's `outcome`. Signed: go on.
   Rejected, refused or 5 min: stop, say so.
e. "Choose a username": `bt` + your wallet's last 6 characters; tick the
   terms; **Continue**. Never ask your owner.
f. **Enable Trading** window (a new account; it may also open at the first
   bet): never close it. Click **Deploy**, then each **Sign**, in order, one
   at a time. Each sign is a card: say *"Tap the card in your app."* and
   wait as in d. Anything else optional: **Skip** or close. See: **Cash** or
   **Portfolio** at the top. Else "Page not found": back to b.
g. Note **your Polymarket wallet address** (profile or deposit).

## B2. Cash and bets (API, no browser)

With B1g's address (or the `Polymarket wallet` line of
`acp site polymarket positions`):

```sh
python3 /tmp/pmweb.py cash <PM_WALLET>
curl -sS "https://data-api.polymarket.com/positions?user=<PM_WALLET>&sizeThreshold=0.01" | python3 /tmp/pmweb.py positions
```

No `cash $`: read **Cash** atop the site.

## B3. Fund: read the deposit address

Do only these steps, every time: never an address you remember.

a. `browser_navigate` to `https://polymarket.com`. **Log in** on top: B1
   first.
b. Click **Deposit**. See: a pop-up.
c. Click **Transfer Crypto**. See: **Your deposit address**. Leave the chain
   menu alone: one address for every EVM chain; Base takes $3+.
d. `browser_extract` the full address under **Your deposit address**. Use it
   only as `0x` + 40 hex. Cut short (`…`): `browser_observe` that field once.
   Still short: stop, tell your owner. Never a screenshot, QR code or script.
e. Go back to SKILL.md's backup send step and send to it at once.
f. Once `confirmed`, cash updates (~10 s); else after 2 min reload once.

## B4. Bet

a. `browser_navigate` to the market row's `url`. See: the form, **Buy**
   picked.
b. Click **Yes** or **No** in the form. Make the dollars with the **+$1**,
   **+$5**, **+$10** buttons ($2 = **+$1** twice); no typing. Leave the
   **Market** menu.
c. One `browser_extract`: the page's question, outcome, Amount, "Avg. Price".
   Not their row or dollars: stop. Over 2¢ worse than you said: ask again.
d. `browser_act` "click the blue Buy Yes button" (or Buy No), once. Its order
   card: wait as in B1d.
e. A pop-up covers it (terms, **Enable Trading**): do it as in B1f (Deploy,
   then each Sign; each sign is a card). Then the blue button once more.
   Else never again.
f. One check: cash down by the amount. Reply: shares, price. Unclear: stop,
   say so.

## B5. Sell or claim

**Sell** (all shares unless they name fewer):

a. `browser_navigate` to the position's `url`. In the form click **Sell**,
   then the outcome they hold. Shares (all: **Max**).
b. One `browser_extract`: question, outcome, shares, payout. Not theirs:
   stop. Over 2¢ a share worse: ask again.
c. `browser_act` "click the blue Sell button", once. Its card: wait as in
   B1d. One check: cash up. Refused or no change: stop, say so.

**Claim** (B2 says `REDEEMABLE`):

a. `browser_navigate` to the position's `url`. **Claim winnings** in the
   side panel: click it once, go to c. Never the row's **Claim**: it opens
   a "Cash out" window, which is a sell.
b. Else `browser_navigate` to `https://polymarket.com/portfolio`. Click
   **Redeem** on that row. An auto-redeem pop-up ("Get Paid Instantly"):
   close it (B8 is only for your owner's ask), click **Redeem** again.
   See: **Redeem Yes … Receive $13.04**. Click **Redeem $13.04** once.
c. Its card: wait as in B1d. Close any pop-up. Reply; no checks.

## B6. Withdraw

a. `bevo-read me`: `user.agentWalletAddress` is the only address.
b. On `https://polymarket.com/portfolio` (**Log in**: B1 first): click
   **Withdraw**, then **Crypto address**.
c. Type a's address, then the amount (all: **Max**).
d. Click **Receive chain**, then **Base**. Click **Receive token**, then
   **USDC**. Already shown: skip it. No re-click; no reads until e.
e. One `browser_extract` of all four. The address must match a in all 42
   characters; else retype once, then stop. Unreadable: stop, say so.
f. Click **Withdraw** once. See: cash drops. Refused or no change: stop, say
   so. Reply; no checks.

## B8. Auto-claim

Only when your owner asked to turn on auto-claim (SKILL.md step 13).

a. `browser_navigate` to `https://polymarket.com/settings` (**Log in**: B1
   first). Or click your picture, top right, then **Account**.
b. Click **Trading** in the left list. See: **Auto-redeem wins**.
   Already on: say so, stop.
c. Click its **Enable** once. Its card is one signature (3 approvals, no
   money): wait as in B1d. Reply; no checks.

## B7. Log out

After a backup run only: your own browser. Your owner's "log out" is
SKILL.md step 11 (the server's login). Your browser keeps
Polymarket's saved login across chats, but shows **Log in** until your
wallet is connected. So first `browser_connect_wallet`, then go to the
portfolio. **Log in** on top now: done (out). Else click your picture, top
right. `browser_act` "click Log out in
the open menu" (off screen; missed: "scroll the open menu down", once). One
look: **Log in** on top = out. Reply; never reopen.

## Never, in the backup

- **Never click Buy, Sell, Redeem or Withdraw twice** after an unclear
  result: read B2 first. A 2nd click is a 2nd bet.
- A captcha or check you cannot pass: say so, stop.
- Browser budget spent: say so; no browser today.
