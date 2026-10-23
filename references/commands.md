# Every Polymarket command

In chat each one files an approval card; nothing runs until the owner approves it. In a
duty, see "From a duty" below. bevo-server
parses the command strictly: an unknown flag, a repeated one or a stray word is
`POLYMARKET_BAD_COMMAND` and nothing is filed. Every one takes
`--idempotency-key <KEY>`. Write the outcome in double quotes, `--outcome "Team Liquid"`:
one double-quoted value per flag, no escapes, never single quotes. A `'` inside the double
quotes is fine (`--outcome "Don't Know"`); only a value holding a `"` or a line break is
refused before anything is filed. The exact lines to run are in `SKILL.md` step 3; this file is
what each one does.

| Command | What the card does |
| --- | --- |
| `bet --market <MARKET_SLUG> --outcome "<OUTCOME>" --usd <USD>` | Buys shares of that outcome for that many dollars (at least $1), now |
| `sell --market <MARKET_SLUG> --outcome "<OUTCOME>" --shares <N>` | Sells that many shares (up to 2 decimals) of the outcome, now |
| `sell --market <MARKET_SLUG> --outcome "<OUTCOME>" --percent <1-100>` | Sells that share of the position, now (`100` sells all) |
| `redeem --market <MARKET_SLUG>` | Claims an ended market's winning shares as cash |
| `deposit --amount <USD>` | Moves that many dollars of USDC on Base from Butler's wallet into the Polymarket account |
| `withdraw --amount <USD>` | Moves that much cash out, back to Butler's wallet as USDC on Base |

All are under `acp polymarket`. `<MARKET_SLUG>` is a market's own `slug`
(`references/markets.md`), or a position's `market` from the account read.
`<OUTCOME>` is one of that market's two outcome names, spelled as Polymarket spells it.

## How each one behaves

- **bet** is a market order that fills now, in part or in full, at the worst price on the
  card or better; any part it cannot fill at that price is dropped, never left waiting.
  The card shows the market question, the outcome, the dollars, the price now, the worst
  price and the estimated shares. The order is signed only with the values the owner
  approved.
- **sell** is the same kind of order the other way: it fills now at the worst price on the
  card or better, and any part it cannot fill is dropped; those shares stay theirs. The
  card shows the shares, the price now, the worst price and the estimated return. It takes
  exactly one of `--shares` (a positive number, up to 2 decimals) and `--percent` (a whole
  number, 1 to 100), and only from a position they hold.
- **redeem** is for a market that has ended, where the read shows `redeemable: true`. It
  claims every redeemable share in that market.
- **deposit** is at least $2. It is a plain send card from Butler's wallet. It stops before signing anything when Butler's wallet holds
  less USDC on Base than the amount. The deposit address is fetched by bevo-server; never
  look one up or send to one yourself.
- **withdraw** is at least $3, at most the read's `cash`. It goes only to Butler's wallet,
  as USDC on Base, and may take a few minutes to land.
- **The first bet, sell or redeem card** on an account also sets the account up, in the
  same run the owner approved. The card shows a "First time" line. Nothing to file for it.

There is no dollar cap of Butler's own: the owner's approval is the cap, and the bounds are
Polymarket's own minimums, the account's cash and the wallet's balance.

## When it is refused

| Answer | Means |
| --- | --- |
| `POLYMARKET_BAD_COMMAND` | A flag is wrong or missing; fix it from the hint. Nothing was filed |
| `POLYMARKET_GEOBLOCKED` | Polymarket is not available in the owner's country. Nothing was sent. Stop: never try another country, a VPN or anything else |
| `POLYMARKET_GEOBLOCK_UNCHECKED` | The owner's country could not be checked, or is unknown. Nothing was sent. Never say they are blocked: ask them to open the Virtuals app once, then file again with a new key |
| `POLYMARKET_MARKET_CLOSED` | The market is not taking orders, or nobody is buying or selling that outcome now |
| `POLYMARKET_NOTHING_TO_REDEEM` | Butler holds no redeemable shares in that market |
| `POLYMARKET_UNAVAILABLE` | Polymarket did not answer, or a deposit's bridge refused. Wait a minute, file once more with a new key; then stop and tell the owner |
| `POLYMARKET_RUN_IN_PROGRESS` | Another Polymarket act is running. Same as above: wait a minute, one more try with a new key, then stop |
| `POLYMARKET_BROWSER_BUDGET` | The owner's daily browser budget is spent. Nothing was sent. Do not retry today: tell the owner it works again tomorrow (then a new key) |
| `POLYMARKET_BROWSER_UNAVAILABLE` | No server browser was free in the owner's country. Same as above; never another country |
| `POLYMARKET_NOT_ENABLED` | Polymarket is switched off for now. Tell the owner; don't retry |
| `POLYMARKET_SIGNER_NOT_READY` | The owner enables Butler's signer in the app first, then file again with a new key |
| `POLYMARKET_CHAT_ONLY` | An older server: this act is chat only |
| `POLYMARKET_DUTY_NOT_ARMED` | The duty has no Polymarket allowance. The owner sets one on the duty in the app |
| `POLYMARKET_DUTY_OP` | `deposit` or `withdraw` from a duty. Chat only |
| `POLYMARKET_DUTY_MAX_BET` | The bet is over the duty's max per bet |
| `POLYMARKET_DUTY_BUDGET` | The bet is over what is left of the duty's budget. The owner gets one note |
| `POLYMARKET_DUTY_MARKET` | The market is not on the duty's market list |
| `POLYMARKET_DUTY_RUNS` | The duty used its runs a day (rolling 24 h; every Polymarket run counts) |
| `POLYMARKET_DUTY_NOT_OWN` | A sell or redeem of a position this duty did not open |
| `POLYMARKET_SETUP_NEEDED` | The account needs first-time setup. The owner places one bet from chat |
| `POLYMARKET_COUNTRY_STALE` | The owner's country was last checked over 7 days ago. They open the Virtuals app once. Never say they are blocked |
| `POLYMARKET_DUTY_BROWSER_RESERVE` | Fewer than 10 of the owner's 40 daily browser sessions are left; those stay for chat |

## When a card fails

A failed card's `approvalFailureReason` that says **"check Polymarket before asking
again"** means nobody knows whether money moved. Run `bevo-read get /polymarket/account`,
tell the owner what it shows, and never file that card again.

A failed card that says **"file again with a new key"** means the account needed setup
that card did not include (it had no "First time" line). Nothing moved: file it once more
with a new `--idempotency-key`.

Any hint that says to file again means a new `--idempotency-key`, never the old one.

## From a duty

A duty runs the same `bet`, `sell` and `redeem` lines, never `deposit` or `withdraw`. It
files no card when the owner armed a Polymarket allowance on the duty in the app (owner
only; the Butler can only propose one):

- **Budget:** the total the duty may spend on bets. Sell and redeem money does not refill it.
  A bet that finished "not done" gives its dollars back; an unknown result counts as spent.
- **Max per bet:** at least $1, at most the budget.
- **Runs a day:** 1 to 24, default 6, over a rolling 24 hours.
- **Markets:** optional list of market slugs; empty means any market.

A duty sells or redeems only positions that same duty opened. A run that would need
first-time account setup is refused: the owner places one bet from chat first. Each run's
result posts in the duty's thread. `POLYMARKET_GEOBLOCKED` pauses the duty and tells the
owner once.
