# Every Polymarket command

Each one files an approval card; nothing runs until the owner approves it. bevo-server
parses the command strictly: an unknown flag, a repeated one or a stray word is
`POLYMARKET_BAD_COMMAND` and nothing is filed. Every one takes
`--idempotency-key <KEY>`. The exact lines to run are in `SKILL.md` step 3; this file is
what each one does.

| Command | What the card does |
| --- | --- |
| `bet --market <MARKET_SLUG> --outcome <OUTCOME> --usd <USD>` | Buys shares of that outcome for that many dollars, now |
| `sell --market <MARKET_SLUG> --outcome <OUTCOME> --shares <N>` | Sells that many shares of the outcome, now |
| `sell --market <MARKET_SLUG> --outcome <OUTCOME> --percent <1-100>` | Sells that share of the position, now (`100` sells all) |
| `redeem --market <MARKET_SLUG>` | Claims an ended market's winning shares as cash |
| `deposit --amount <USD>` | Moves that many dollars of USDC on Base from Butler's wallet into the Polymarket account |
| `withdraw --amount <USD>` | Moves that much cash out, back to Butler's wallet as USDC on Base |

All are under `acp polymarket`. `<MARKET_SLUG>` is a market's own `slug`
(`references/markets.md`), or a position's `market` from the account read.
`<OUTCOME>` is one of that market's two outcome names, spelled as Polymarket spells it.

## How each one behaves

- **bet** is a market order that fills now, in part or in full, at the worst price on the
  card or better; any part it cannot fill at that price is dropped, never left waiting.
  The card shows the market question, the outcome, the dollars, the worst price and the
  estimated shares. The order is signed only with the values the owner approved.
- **sell** takes exactly one of `--shares` and `--percent`, and only from a position they
  hold. More shares than they hold is refused with nothing sent.
- **redeem** is for a market that has ended, where the read shows `redeemable: true`. It
  claims every redeemable share in that market.
- **deposit** is at least $2. It stops before signing anything when Butler's wallet holds
  less USDC on Base than the amount. The deposit address is fetched by bevo-server; never
  look one up or send to one yourself.
- **withdraw** is at least $3, at most the read's `cash`. It goes only to Butler's wallet,
  as USDC on Base, and may take a few minutes to land.
- **The first money card** on a new account also sets the account up, in the same run the
  owner approved. The card says so. Nothing to file for it.

There is no dollar cap of Butler's own: the owner's approval is the cap, and the bounds are
Polymarket's own minimums, the account's cash and the wallet's balance.

## When it is refused

| Answer | Means |
| --- | --- |
| `POLYMARKET_BAD_COMMAND` | A flag is wrong or missing; fix it from the hint. Nothing was filed |
| `POLYMARKET_GEOBLOCKED` | Polymarket is not available in the owner's country. Nothing was sent. Never try another country or a VPN |
| `POLYMARKET_NOT_ENABLED` | Not on for this account yet |
| `POLYMARKET_SIGNER_NOT_READY` | The owner enables Butler's signer in the app first |
| `POLYMARKET_CHAT_ONLY` | Filed from a duty; Polymarket is chat only |
