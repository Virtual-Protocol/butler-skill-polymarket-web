# Speed log

Browser-only flow (Browserbase), test Butler `01a01889-…415c` on staging, image
`virtuals-agent:polymarket-browser-test`. Time = the app's "Worked for", from the
owner's message to the Butler's last step.

Target: one action in 1–2 min; 2–3 min is acceptable.

| Date | Action | Skill | Time | Steps | Result | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| 2026-10-22 | Login (first) | 0.1.6 | 11m 50s | 46 | logged in | Old prose steps. Lost ~6 min to a doubled email and a stale code. |
| 2026-10-26 | Login (first) | 0.1.9 | 2m 50s | — | logged in | First click list (a–g). About 4x faster. |
| 2026-10-26 | Deposit $3 | 0.1.10 | 6m 00s | 24 | no card | Re-checked login, opened the chain menu, then tried to decode the QR picture and `pip install` a QR reader. |
| 2026-10-26 | Deposit $3 | 0.1.11 | 1m 16s | 6 | card 5012 | Not a fair run: skipped Transfer Crypto and reused an address it remembered. Led to the "read fresh every time" rule. |
| 2026-10-26 | Deposit $3, new chat | 0.1.12 | 2m 45s | 15 | card 5017, right address (`0x489d…7f4a`) | Includes 2 steps of skill install. 4 steps lost to geoblock/page-load retries. 4 steps after the card (approval status, Platforms duty checks). |

## Where the time still goes (deposit, 0.1.12)

```
install + load skill            2 steps   (one-off, not part of a real deposit)
geoblock + page-load retries    5 steps   (proxy error page, then retries)
top bar, Deposit, address       3 steps   (the actual work)
send card                       1 step
after the card                  4 steps   (status check, Platforms watcher duty)
```

Next cuts to try: stop after the card is filed (no status or duty checks in the
same turn), and one geoblock retry at most before saying so.
