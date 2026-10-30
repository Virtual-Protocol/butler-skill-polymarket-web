# butler-skill-polymarket-web

Teaches a Butler agent to use [Polymarket](https://polymarket.com) for its owner: show
trending markets and odds, read its own Polymarket account (on its Butler wallet), bet,
sell, claim resolved winnings, fund it, and withdraw to the owner's Butler wallet.

Three things shape how it works:

- **Reads go to Polymarket's public API.** Trending markets, search and prices come from
  `gamma-api.polymarket.com`, which needs no key.
- **Money goes through the server's site command.** From 0.2.0 the Butler runs
  `acp site polymarket <action>`; bevo-server does the clicks in its own fresh browser,
  files the owner's card, and reports `done`, `not_done`, `unknown`, `rejected` or
  `failed`. The Butler never gets the browser.
- **The old click steps are a backup only.** `references/clicks.md` holds the 0.1.30
  click-by-click flow in the Butler's own browser. It runs only after `not_done` (nothing
  was sent), with a new yes from the owner, and never in a duty.

Status: experiment draft. 0.2.0 timings are not measured yet (see `BENCHMARK.md`).

- `SKILL.md` — the playbook (see the hub's
  [SKILL_STANDARD.md](https://github.com/Virtual-Protocol/butler-skills/blob/main/SKILL_STANDARD.md))
- `references/clicks.md` — the backup click steps
