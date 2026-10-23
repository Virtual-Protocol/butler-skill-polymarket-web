# butler-skill-polymarket-web

Installs as the skill `polymarket` (its `SKILL.md` name). The repo keeps its old name for now.

Teaches a Butler agent to use [Polymarket](https://polymarket.com) from its own wallet: find
a market and its odds, bet on an outcome, sell or redeem shares, deposit USDC on Base, and
withdraw back to the Butler wallet. In chat each money act is an approval card the owner
approves in the app.

How it works (the same shape as `butler-skill-variational`):

- **No browser in the Butler.** The skill never opens polymarket.com. Market search is a
  public, read-only `curl` to `gamma-api.polymarket.com`.
- **Account read:** `bevo-read get /polymarket/account` (cash, positions, open orders).
- **Money:** `acp polymarket bet|sell|redeem|deposit|withdraw`. The container forwards the
  raw command to bevo-server (`POST /butler-exec/polymarket`), which parses it strictly and
  files one card. After approval, a server runner places the act through Polymarket from
  the owner's real country. The Butler follows the card with
  `bevo-read request <KEY> --route polymarket`.
- **Geoblock:** if Polymarket is closed in the owner's country, the act stops with nothing
  sent. The skill never suggests another country or a VPN.
- **Duties:** a duty can `bet`, `sell` and `redeem` (never deposit or withdraw) with no
  card, inside a Polymarket allowance the owner arms on the duty in the app (budget, max
  per bet, runs a day, optional markets). It sells or redeems only its own bets, and the
  first bet happens from chat.

Needs, before it works end to end: the bevo-server Polymarket runner (kill switch
`POLYMARKET_RUNNER_ENABLED`), the container's `acp polymarket` group, and the registry
validator's `ACP_GROUPS` knowing `polymarket`.

Files:

- `SKILL.md` — the playbook (see the hub's
  [SKILL_STANDARD.md](https://github.com/Virtual-Protocol/butler-skills/blob/main/SKILL_STANDARD.md))
- `references/commands.md` — every command form and what its card does
- `references/account.md` — the account read's fields
- `references/markets.md` — market search and choosing the outcome
