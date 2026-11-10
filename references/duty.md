# In a duty: its code and its rules

SKILL.md's "In a duty" sends you here when you write a Polymarket duty's code.
A duty bets from its **own duty wallet** and that wallet's own Polymarket
account. It never reads or uses your main wallet's Polymarket account.

## Its own wallet

`<ME>` below is the duty wallet's address: the `acp wallet list` row
`bound to duty <id>`. Never `bevo-read me`'s `user.agentWalletAddress` (that
is your main wallet). Cannot find it: the duty reads its cash with
`acp site polymarket positions` instead.

The server signs the duty wallet up on Polymarket, does Enable Trading, turns
auto-claim on and keeps it on. Its code never runs `login` or `auto-redeem`
and never looks for those steps.

## Each run

1. Every `acp site polymarket` action runs inside the owner's limit: no card,
   no ask.
2. Before every bet, even if the owner did not ask for it, call `pm_cash()`.
   Call `pm_positions()` when the duty needs its bets (a sell, or "skip a
   market I already hold").
3. Cash short of the bet: `acp site polymarket deposit --usd <shortfall, at
   least 3>`. A new duty's cash is $0, so its first run always tops up:
   code without this step never bets. Its summary says `Deposited:`: bet
   now. Else bet on a later run.
4. Refused `POLYMARKET_DUTY_NOT_ARMED`: the owner has not held the limit card
   yet. Wait. Keep no key. Never `bevo.fail`.
5. A top-up or bet answered `not_done` or `failed` moved nothing. Try it again
   on a later run, under a NEW key, at most 3 times. A top-up that found too
   little in the duty wallet waits for the owner to add some.
6. Refused with any other code: it will refuse again. `bevo.fail` with that
   reason at once. No retry.
7. Never retry after `unknown_outcome` or a pending answer: those may have
   moved money.
8. Never the backup clicks (`references/clicks.md`) in a duty. Any other
   answer: report and stop.

## The cash and bets reader

Copy this into the duty's code. `pm_cash()` uses only `bevo-rpc` and the
contracts page. Each function raises on any error: then read
`acp site polymarket positions` instead.

```python
import json, re, subprocess, urllib.request
ME = "<ME>"  # the DUTY wallet's address, written in when you file
def _pm_call(to, data):
    r = subprocess.run(["bevo-rpc", "137", "eth_call", json.dumps([{"to": to, "data": data}, "latest"])], stdout=-1, check=True, timeout=60).stdout
    return json.loads(r)
def _pm_addr(name):  # from Polymarket's contracts page, never typed in
    t = urllib.request.urlopen("https://docs.polymarket.com/resources/contracts.md", timeout=15).read().decode()
    return re.search(name + r"[^`]*`(0x\w{40})", t)[1]
def _pm_wallet():
    return _pm_call(_pm_addr("Deposit Wallet Factory"), "0x04f1d3c7" + ME.lower()[2:].zfill(64))[-40:]
def pm_cash():
    return int(_pm_call(_pm_addr("CollateralToken"), "0x70a08231" + _pm_wallet().zfill(64)), 16) / 1e6
def pm_positions():
    """Each bet: market (slug), outcome, shares, worth, won (redeemable and worth > 0)."""
    u = f"https://data-api.polymarket.com/v2/positions?user=0x{_pm_wallet()}&limit=500"
    rows = json.load(urllib.request.urlopen(urllib.request.Request(u, headers={"User-Agent": "butler-skill-polymarket-web"}), timeout=15))["data"]
    return [{"market": x["slug"], "outcome": x["outcome"], "shares": x["current_size"], "worth": x["current_value"],
             "won": bool(x.get("redeemable")) and x["current_value"] > 0} for x in rows if x["current_size"] >= 0.01]
```
