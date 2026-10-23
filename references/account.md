# Reading the Polymarket account

One read, not a card: it moves nothing. It is refused from a duty. When it answers
`POLYMARKET_READ_BUSY`, wait and read again once; never loop.

## `bevo-read get /polymarket/account`

```json
{ "hasAccount": true, "depositWallet": "...", "cash": "12.40", "positions": [ ... ], "openOrders": [ ... ] }
```

| Field | Means |
| --- | --- |
| `hasAccount` | `false` when Butler's wallet has no Polymarket account yet; the first deposit or bet card sets one up |
| `depositWallet` | the account's Polymarket wallet. Never send money to it yourself: `deposit` does that |
| `cash` | dollars ready to bet or withdraw |
| `positions[]` | each bet they hold, below |
| `openOrders` | orders still resting on Polymarket. Bets here fill now or not at all, so this is normally empty; relay any as given |

### A position

| Field | Means |
| --- | --- |
| `market` | the market it is in; use it as `--market` for `sell` or `redeem` |
| `outcome` | the outcome held, such as `Yes` or `Lakers`; use it as `--outcome` for `sell` |
| `shares` | how many shares; each pays $1 if the outcome happens |
| `avgPrice` | the average price paid per share, in dollars |
| `value` | what the shares are worth now |
| `redeemable` | `true` once the market has ended and the shares can be claimed with `redeem` |

Relay the figures as given, prices as cents ("paid 64¢"). Any of them can be `null`: say
it is unknown, never zero. Never work out a PnL yourself; give `value` and `avgPrice` and
let the owner see the difference.

## When the read fails

A non-zero exit, `POLYMARKET_READ_BUSY` or `POLYMARKET_READ_FAILED` is **not** an empty
account. Say you could not check it, and give the server's `hint`. `POLYMARKET_CHAT_ONLY`
means you are in a duty: the read is chat only.
