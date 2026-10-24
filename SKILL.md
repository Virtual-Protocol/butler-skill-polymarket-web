---
name: polymarket
description: "Bet on Polymarket from Butler's wallet: find a market and its odds, then bet, sell, redeem, deposit or withdraw. Chat acts are cards; a duty bets, sells or redeems inside an owner-set allowance."
version: 1.1.0
metadata: {"butler":{"moneyMoving":true,"keywords":["polymarket","prediction market","trending on polymarket","polymarket odds","bet on","polymarket bet","buy yes","buy no","my polymarket","sell my polymarket","redeem polymarket","withdraw from polymarket","deposit to polymarket"],"requires":{"bins":["acp","bevo-read","curl"]}}}
---

## When to use

Your owner names **Polymarket** (a share pays $1 if its outcome happens) and wants to:

- **Look** - "what's trending on Polymarket?", "odds on the Fed decision": step 2 only.
- **See their bets** - "my Polymarket balance", "my bets there": step 1, then stop.
- **Fund** - "put $10 into Polymarket": steps 1, 3, 4 with `deposit`.
- **Bet** - "bet $2 on Yes for the Fed cut": steps 1 to 4.
- **Sell or claim** - "sell half my Yes", "cash out my win": steps 1, 3, 4 with `sell` or
  `redeem`.
- **Withdraw** - "take my money out of Polymarket": steps 1, 3, 4 with `withdraw`.
- **Automate** - "each morning, bet $1 if Yes is under 30¢": *In a duty* below.

Not this skill: Hyperliquid perps (`acp trade`), Variational, Papertrade, or a bet your
owner did not say is on Polymarket. In chat every act is a card the owner approves. A duty
acts only inside the allowance its owner set (see *In a duty*). Polymarket may be off:
relay the refusal.

**No browser, ever.** polymarket.com is blocked in your browser. Reading markets is `curl`
to Polymarket's public API; everything else is `bevo-read` and `acp polymarket`.
bevo-server does the rest after the owner approves.

## Before you start

1. **Figures come only from this turn's reads and the card.** An unreadable account is not
   an empty one. Never work out a price, a payout, shares or a PnL yourself.
2. **Chat or duty.** In chat, every act is a card. In a duty, follow *In a duty* below.
3. **A bet can lose all of it.** Say the bad case first, in their words: *"If Yes does not
   happen, the whole $2 is gone."* The card shows the worst price and the shares; tell them
   to read it before they approve.
4. **The account is Butler's wallet's own.** The first bet, sell or redeem card sets it up
   (the card says "First time"). Never ask your owner for an email, a code, a password or a
   key.
5. **Never check approvals, allowances or the deposit wallet on chain yourself.** Setup
   happens inside the card's run on the server, so a zero allowance before filing is
   expected.
6. **A failed card: report it as written.** Filing again (new key) is the owner's call.
   Never refuse because earlier cards failed. Never build your own workaround, and never
   research the relayer or keys.

### Never file a bet without all three

1. **They chose it:** the market, the outcome and the dollar amount, in their own words.
   "You pick" is not a choice: ask.
2. **They said yes to your say-back.** In chat: say back the exact market question,
   outcome name and dollars from step 2, then **end your reply and wait**. File only after
   their next message says yes. Never say it back and file in the same reply, even when
   they named the market themselves. In a duty: the owner's armed allowance and its market
   list are the yes.
3. **The read is fresh:** step 1 ran after their last answer (or its `asOf` is under 60 s
   old), so you know their cash and whether they already hold that market.

### In a duty

- A duty may only `bet`, `sell` and `redeem`, with the same commands. Never `deposit` or
  `withdraw`: those are chat only.
- No card per act only when the owner armed a **Polymarket allowance** on that duty: a
  total budget, a max per bet, runs a day (default 6), and optionally a list of markets.
  Sells and redeems do not refill the budget. The market list applies to sell and redeem
  too.
- A duty sells or redeems only bets that same duty placed; `--percent` is of the duty's
  own shares.
- An armed run answers status `accepted` with `auto: true`: no card.
- Any answer with a `code` sent nothing: not spent, and its key is used up, so the next
  try needs a new key. Only no answer or a timeout is unclear: resend that, same key.
- The first bet must happen from chat: it sets the account up ("First time").
- The result posts in the duty's thread.

**Proposing a duty.** After filing, call `duty_pocket` with `polymarket`
(`budgetUsd`, `maxBetUsd`, `runsPerDay`, `markets`: the slug) from the owner's figures. Only
their hold on that card arms it; never say it is set. Say the bad case: a duty asks no one
before it bets. Example:

> *"Each morning, if 'Will X happen?' Yes is under 30¢, I bet $1 on Yes. Each bet can
> lose the whole $1. Hold the card to allow budget $10, max $1 a bet, 3 runs a day."*

## Procedure

1. [FIXED] Read the account, before any act and whenever asked:

   ```sh
   bevo-read get /polymarket/account
   ```

   Relay it as given (`references/account.md`). `hasAccount: false` means no Polymarket
   account yet: the first bet card sets it up; a sell, redeem or withdraw has nothing to act
   on. Within 60 s of a good read the server returns that read again, with its `asOf`.
   `POLYMARKET_READ_BUSY` means a read is still running: wait, then read once more; never
   loop. A failed read: say you could not check it.

2. [ADAPT] Find the market, read only (`references/markets.md` has every query):

   ```sh
   curl -sS "https://gamma-api.polymarket.com/public-search?q=<WORDS>&events_status=active&limit_per_type=5"
   curl -sS "https://gamma-api.polymarket.com/events?slug=<EVENT_SLUG>"
   ```

   GET only, never another host. Show each choice as its question, outcome names and
   prices in cents, and end date. Several fit: ask which. Then say back the one they chose,
   exactly, then wait for yes: *"Fed decision in October: 25 bps decrease, Yes at 64¢,
   $2?"* Keep that market's
   `slug` (the market's own, not the event's) and the outcome name exactly as the API
   spells it. A market with `closed: true` or `acceptingOrders: false` takes no bet.

   Match the ask to step 1 (a position's `market` is the slug `--market` takes):
   - **Bet**: at least $1, at most `cash`. Short: offer a deposit first, as its own card.
   - **Sell**: only a position they hold: `--shares` (up to 2 decimals, at most its
     `shares`) or `--percent` 1 to 100.
   - **Redeem**: only a position marked `redeemable: true`.
   - **Withdraw**: at most `cash`.

3. [FIXED] File the card, once, with one key for this errand
   (`references/commands.md` has every form):

   ```sh
   acp polymarket bet --market <MARKET_SLUG> --outcome "<OUTCOME>" --usd <USD> --idempotency-key <KEY>
   acp polymarket sell --market <MARKET_SLUG> --outcome "<OUTCOME>" --shares <N> --idempotency-key <KEY>
   acp polymarket sell --market <MARKET_SLUG> --outcome "<OUTCOME>" --percent <1-100> --idempotency-key <KEY>
   acp polymarket redeem --market <MARKET_SLUG> --idempotency-key <KEY>
   acp polymarket deposit --amount <USD> --idempotency-key <KEY>
   acp polymarket withdraw --amount <USD> --idempotency-key <KEY>
   ```

   The outcome goes in double quotes (`--outcome "Team Liquid"`), never single quotes. A
   `'` inside is fine (`--outcome "Don't Know"`); only a `"` or a line break is refused. In chat the reply is an approval card:
   tell your owner what it does, and to approve it in the app.

4. [FIXED] Wait for the card's outcome; never file it again to find out:

   ```sh
   bevo-read request <KEY> --route polymarket
   ```

   - `pending` - still with the owner: say so and stop until they come back.
   - `signed` - approved and running. The chat is told when it ends.
   - `confirmed` - done: relay `approvalOutcome.summary` and its `facts` as written.
   - `failed` - give `approvalFailureReason` as written. If it says "check Polymarket
     before asking again": run step 1 and never file it again.
   - `rejected` or `expired` - nothing ran; say so.

   The server also posts how a card ended in this chat; relay that, don't add to it.

## Idempotency and retries

One key per errand, such as `pm:fed-cut:yes:1`. On an error, a timeout or an unclear
answer, **do not re-run** `acp polymarket`: a second bet is a second bet. Look it up as in
step 4. When a hint says to file it again, that always means a **new** `--idempotency-key`. A `curl` error in step 2: try once more, then say you could not read the market.
Never guess a price.

## Failure handling

| Answer | What to do |
| --- | --- |
| `POLYMARKET_NOT_ENABLED` | Not available on this account yet. Say so; don't retry. |
| `POLYMARKET_SIGNER_NOT_READY` | Ask your owner to enable Butler's signer in the app, then file it again with a new key. |
| `POLYMARKET_CHAT_ONLY` | An older server: this act is chat only. Tell the owner in chat. |
| `POLYMARKET_DUTY_NOT_ARMED` | This duty has no Polymarket allowance. Ask the owner to set one on the duty in the app. |
| `POLYMARKET_DUTY_OP` | Deposit and withdraw are chat only. Ask the owner in chat. |
| `POLYMARKET_DUTY_MAX_BET` | Over the duty's max per bet. Don't retry bigger; the owner can raise it. |
| `POLYMARKET_DUTY_BUDGET` | Budget used up; the owner got a note. Bet no more. |
| `POLYMARKET_DUTY_MARKET` | Market not on the duty's list. Don't retry; the owner can add it. |
| `POLYMARKET_DUTY_RUNS` | Today's runs are used. Skip; never loop. |
| `POLYMARKET_DUTY_NOT_OWN` | This duty did not open that position. Leave it; the owner can sell it from chat. |
| `POLYMARKET_SETUP_NEEDED` | No account yet. The owner places one bet from chat first. |
| `POLYMARKET_COUNTRY_STALE` | Owner's country check is over 7 days old. Ask them to open the Virtuals app once; never say they are blocked. |
| `POLYMARKET_DUTY_BROWSER_RESERVE` | Browser sessions left today are kept for chat. Skip; try tomorrow. |
| `POLYMARKET_BAD_COMMAND` | Fix the command from its hint (`references/commands.md`); nothing was filed. |
| `POLYMARKET_GEOBLOCKED` | Not available in your owner's country; nothing was sent (a duty is paused). Say so and stop. Never try anything else: no other country, no VPN. |
| `POLYMARKET_GEOBLOCK_UNCHECKED` | Their country could not be checked, or is unknown; nothing was sent. Never say they are blocked. Ask them to open the Virtuals app once, then file again with a new key. |
| `POLYMARKET_READ_BUSY` | A read is still running. Wait, then read once more; never loop. Not an empty account. |
| `POLYMARKET_UNAVAILABLE`, `POLYMARKET_RUN_IN_PROGRESS`, `POLYMARKET_BROWSER_UNAVAILABLE` | Could not run now (a deposit's bridge refusal is `POLYMARKET_UNAVAILABLE` too). Wait a minute, file once more with a new key; if refused again, stop and tell the owner. |
| `POLYMARKET_BROWSER_BUDGET` | Daily budget spent. Don't retry today; tell the owner it works again tomorrow. |
| `POLYMARKET_READ_FAILED` | Could not read Polymarket. Say you could not check it. |
| A card failed with nothing sent | Give the reason as written; nothing moved. Filing again is the owner's call. |
| A card failed with "file again with a new key" | The account needed setup that card did not include. File it once more with a new key. |
| A card failed with "check Polymarket before asking again" | Step 1; tell the owner what it shows. Never resend. |
| `POLYMARKET_MARKET_CLOSED` | The market is not taking orders: say so. Offer `redeem` once it is redeemable. |
| No market fits the search | Say so, and ask for other words. Never pick a near match yourself. |

## Limits

- **Bets and sells are market orders:** they fill now, up to the worst price on the card,
  or not at all. No limit orders, no orders that wait for a price.
- **Deposit:** at least $2, USDC on Base from Butler's wallet.
- **Withdraw:** at least $3, back to Butler's wallet only, as USDC on Base.
- **Bets and sells:** a bet is at least $1; otherwise Polymarket's own minimums. No other
  cap: in chat the owner's approval is the cap, in a duty its allowance. Never add a
  limit your owner did not ask for.
- **Markets:** any Polymarket lists and that still takes orders.

## Say to the owner

- Prices as cents and chances: "Yes at 64¢, about a 64% chance".
- A bet: the bad case first, then the card. *"If Yes does not happen, the whole $2 is gone.
  The card shows the worst price and about how many shares. Approve it in the app."*
- The market by its question and outcome name, never by its slug.
- After a sell, a redeem or a withdraw: what the card's outcome says, and nothing else.
