# Finding the market and the outcome

Market data is public: `curl` to `https://gamma-api.polymarket.com`, GET only, no key.
It moves nothing and files no card. Never open polymarket.com in the browser, and never
send anything to Polymarket yourself: bets go only through `acp polymarket`.

## The queries

```sh
curl -sS "https://gamma-api.polymarket.com/public-search?q=<WORDS>&events_status=active&limit_per_type=5"
curl -sS "https://gamma-api.polymarket.com/events?active=true&closed=false&order=volume24hr&ascending=false&limit=10&offset=0"
curl -sS "https://gamma-api.polymarket.com/events?slug=<EVENT_SLUG>"
curl -sS "https://gamma-api.polymarket.com/markets?slug=<MARKET_SLUG>"
```

- **Search** (first line): the owner's words, URL-encoded (`fed%20cut`). The answer's
  `events[]` each hold some of their `markets[]`; search may not list every market of an
  event.
- **Trending** (second line): the 10 events with the most money traded today
  (`volume24hr`). "More" is `offset=10`, then `20`.
- **One event, in full** (third line): every market in it. Use it before you confirm a
  choice, so you see all of the event's markets.
- **One market** (fourth line): to check a market again just before filing, by its own
  `slug`.

## How Polymarket is shaped

An **event** is the question people talk about ("Fed decision in October"). It holds one
or more **markets**. Each market has two **outcomes**, and the card needs the market's
`slug` and one outcome name.

Two shapes, and they read differently:

1. **One market, named outcomes.** A game: the market `nba-lac-lal-2026-10-23` has the
   outcomes `Clippers` and `Lakers`. "Bet on the Lakers" is
   `--market nba-lac-lal-2026-10-23 --outcome Lakers`.
2. **Many markets, Yes or No each.** "Fed decision in October" holds one market per
   choice: `25 bps decrease`, `No change`, and so on, in `groupItemTitle`. Each has the
   outcomes `Yes` and `No`. "Bet the Fed cuts 25" is the `25 bps decrease` market's own
   `slug` with `--outcome Yes`. "Bet against it" is `--outcome No` on the same market.

An event can hold side markets too ("Game 1 Winner", "team to score first"). Read
`question` and `groupItemTitle`, never guess from the slug.

## The fields to read

| Field | Means |
| --- | --- |
| `events[].title`, `events[].slug`, `events[].endDate` | the event, its slug (for the event query and a link) and when it ends |
| `markets[].question` | the full question this market settles |
| `markets[].groupItemTitle` | the choice's short name in a many-market event |
| `markets[].slug` | **the value for `--market`**; never the event's slug |
| `markets[].outcomes` | the two outcome names, a JSON string such as `["Yes", "No"]`; `--outcome` takes one exactly as spelled |
| `markets[].outcomePrices` | the price of each outcome, in the same order, in dollars (`0.64` is 64¢, about a 64% chance) |
| `markets[].active`, `closed`, `acceptingOrders` | a bet needs `active: true`, `closed: false`, `acceptingOrders: true` |
| `markets[].endDate` | when this market ends |

Prices here are a guide for choosing. The card's worst price and shares are the real ones.
Ignore choices priced under 1¢ or over 99¢ when listing many, unless the owner asks for
them: they are all but settled.

## Choosing, with the owner

1. Show at most five choices: the question or choice name, each outcome with its price in
   cents, the end date.
2. Several fit, or the words are loose ("the election"): ask which. Never pick a near
   match yourself.
3. Say back the one they chose, exactly, and wait for yes: *"Lakers vs Clippers tonight:
   Lakers at 58¢, $5?"*
4. Keep the market `slug` and the outcome name for `SKILL.md` step 3. If their outcome is
   not one of the market's two names, ask; never map it yourself.

The owner may want to look at it: the event page is
`https://polymarket.com/event/<EVENT_SLUG>`. Give them the link; never open it yourself.

## When a query fails

A `curl` error or an empty `events[]` is not "no such market". Try once more; then say you
could not read Polymarket's markets right now.
