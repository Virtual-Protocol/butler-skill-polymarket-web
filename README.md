# butler-skill-polymarket-web

Teaches a Butler agent to use [Polymarket](https://polymarket.com) through the website,
in its cloud browser: show trending markets and odds, open its own Polymarket account
with its Butler email, fund it with USDC on Base, buy and sell outcome shares, claim
resolved winnings, and withdraw to the owner's Butler wallet.

Two things shape how it works:

- **Reads go to Polymarket's public API, money goes through the browser.** Trending
  markets, search, prices and positions come from `gamma-api.polymarket.com` and
  `data-api.polymarket.com`, which need no key. The browser is used only for deposit,
  buy, sell, claim and withdraw, because each session spends the owner's daily budget.
- **The account is the Butler's own.** It signs up with its own email and reads the login
  code from its own inbox; the owner is never asked for an email, code or password. The
  owner says one yes per bet, and approves the deposit card in the app.

Status: experiment draft (0.1.0). It is compared against a Butler with no skill before
deciding whether to keep it.

- `SKILL.md` — the playbook (see the hub's
  [SKILL_STANDARD.md](https://github.com/Virtual-Protocol/butler-skills/blob/main/SKILL_STANDARD.md))
