# Speed log

**0.2.0 (server `acp site polymarket` command): timings are not measured yet.**
Every row below is the 0.1.x click flow in the Butler's own browser, which is
now only the backup (`references/clicks.md`).

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
| 2026-10-26 | Withdraw $3 | 0.1.14 | 5m 10s+ | — | stopped by owner | Stuck on the Receive token dropdown: checked the form early, then clicked USDC again "to be sure". |
| 2026-10-26 | Withdraw $3 (all) | 0.1.15 | 4m 55s | 16 | done, $3.00 USDC on Base to `0x6279…ea1c` | Skipped the yes/no question (took "withdraw all" as the yes). Read positions after, found the Lula win. |
| 2026-10-26 | Redeem won bet (Lula Yes, 13 shares) | 0.1.16 | 3m 33s | 14 | done, cash $0.00 → $13.04 | Old prose step 8, no click list. Owner OK with the time. Did not ask yes/no first ("can you help redeem the win" taken as the yes). |
| 2026-10-26 | Withdraw $5 | 0.1.17 | ~5m | 15 | done, cash $13.04 → $8.04 | Asked first (vague: "confirm the $5 site withdrawal"). 3 observes before clicks, 3 early form reads, Base picked 3 times. |
| 2026-10-26 | Withdraw $3 (click turn) | 0.1.18 | 4m 15s | 12 | done, cash $8.04 → $5.04, tx `0x2e…9059` | Owner: acceptable for now. Ask turn was correct word for word, but it first read the API skill's ACP account (both skills answer "Polymarket"). Extra: a form read before amount, "Find the submit button", a 2nd submit-click step, 2 cash checks after. |
| 2026-10-26 | Search + bet $3 (Yamal Yes) | 0.1.19 | ask turn ~1–2m; bet turn 7m 28s | 39 (bet turn) | 1 bet filled: 32.3 shares at ~9.6¢, cash $5.04 → $1.96 | Search, row link and question were right. Skill said "Trade"; the real button is the blue **Buy Yes**, so it hunted and re-clicked. A trading-terms pop-up came first. Then ~15 steps hunting its wallet/deposit address to read positions. Fixed in 0.1.21–0.1.22. |
| 2026-10-26 | Sell all (32.25 Yamal Yes) | 0.1.22 | ask 3m 06s / 9; click turn 8m+ / 10+, stopped | 9 + 10 | nothing sold | Ask filled the form first. Owner answered 3 min later; click turn looped on Sell (~48 s a step). Led to 0.1.23: ask from API, fill + click after the yes. |
| 2026-10-26 | Sell all (32.25 Yamal Yes) | 0.1.23 | ask 52s / 12; click 3m 00s / 16; total ~4m | 28 | sold, ~$2.64 back, cash $1.96 → $4.60 | Way B: ask from API (was 3m 06s). Filled at ~8¢ vs 9.2¢ asked (inside the 2¢ rule, but -10% on a cheap share). Extra steps: Platforms row update + watcher offer. Owner: OK. |
| 2026-10-26 | Bet $2 (Fed No change Yes) | 0.1.23 | ask 1m 25s / 6; bet 7m 24s / 20 | 26 | filled 2.1 shares at 95¢, cash $4.60 → $2.60 | Ask from API worked. Bet turn: page opened in 1 step, then ~12 steps typing into Amount. Plus the Platforms row refresh. Fixed in 0.1.24 (+$ buttons). |
| 2026-10-26 | Bet $1 (Fed No change Yes) | 0.1.24 | ask 1m 25s / 7; bet 5m 16s / 15 | 22 | filled ~1.05 shares at 95¢, cash $2.60 → $1.60 | Owner: OK. No Amount fight. Lost: first submit click missed (+2 looks), 2 checks after, then 5 steps of Platforms-row bookkeeping (~1.5 min). |
| 2026-10-26 | Sell all (3.15 Fed No change Yes) | 0.1.24 | click turn 3m 33s / 16 | 16 + ask | sold at 94¢ for $2.97, cash $1.60 → $4.56, no positions left | Owner: OK. Still on 0.1.24, so the Platforms steps were included. |
| 2026-10-26 | Bet $1 (Fed No change Yes) | 0.1.25 | ask 1m 24s / 6; bet 3m 55s / 18 | 24 | Butler said "filled 1.05 shares at 95¢"; confirmed later: cash $3.56 + 1.05 Fed Yes shares | Ask from API, no browser. Bet turn: the browser helper refused the Buy Yes click twice ("per user instructions do not click it; report it instead") — the order-guard in `virtuals-agent/src/lib/browser.ts:114`. Earlier bets passed this guard by chance. Still 1m 21s faster than 0.1.24 (5m 16s). |
| 2026-10-26 | Log out | 0.1.26 | 2m 26s | 11 | logged out; header shows Log in / Sign up | Before 0.1.26 it looped: Log out sits below the language row, off screen. Now `browser_act` clicks it directly. Still ~7 steps over the 3–4 planned. |
| 2026-10-26 | Deposit $3, new chat | 0.1.12 | 2m 45s | 15 | card 5017, right address (`0x489d…7f4a`) | Includes 2 steps of skill install. 4 steps lost to geoblock/page-load retries. 4 steps after the card (approval status, Platforms duty checks). |
| 2026-10-26 | Deposit $3, same chat as failed card 5074 | 0.2.2 | 4m 41s / 24 to the card; 1m 34s / 5 after the tap; ~6m 15s total | 29 | card 5077 (plain send), $3 landed, cash $0.00 → $3.00 | **Not the server path.** No `acp site polymarket deposit` call. Butler took the backup it had offered after 5074 in this chat: logged in by hand in its own browser, read the deposit address, then `bevo-send`. Server log: `positions` 15:34:45, send card 15:39:14, `positions` twice after. Server deposit (#1238) still untested. |
| 2026-10-26 | Bet $1 (Mbappé Ballon d'Or Yes), new chat | 0.2.2 | card in 59s; failed ~1m 12s after the tap | — | card 5080 not done, nothing bought | **First server-path bet.** Card in 59 s (no browser before the card). After the tap: "Enable Trading steps did not finish (the page shows: Close)": the server gave up while the API-key Sign was still being answered. Fixed in server #1239. |
| 2026-10-27 | Log out, new chat | 0.2.3 | 1m 38s | 13 | correct: "not logged in, nothing to log out" | Server path keeps no login, and its own browser was already logged out. It opened its browser to check and also read positions: both extra. |
| 2026-10-27 | Log in (Butler's own browser) | 0.2.3 | 42s | 6 | logged in, header Portfolio $5.61 / Cash $3.00 | No sign-in card: injecting the wallet (Rabby look) brought back the saved Polymarket session from the earlier backup deposit, so the page asked for no new signature. Not the server path (that keeps no login). |
| 2026-10-27 | Bet $1 (Mbappé Yes), server run on Mac, test wallet | server #1241 | 21s (server run only, no chat/card) | — | **done**: 3.69 shares at worst 28.7¢ | First filled order on the server path. Login → Enable Trading API-key sign → Buy → Terms of Use pop-up → Accept → Buy again → order signed (CTF Exchange v2). Cards 5080 and 5084 failed on the steps fixed by #1239–#1241. |
| 2026-10-27 | Trending ("what's trending on polymarket?"), new chat | site-command (version not checked) | 10s | 3 | top 10 by 24h volume, read 17:30 UTC | Public data, no browser, no card. Old best for search + ask was 1m 25s. |
| 2026-10-27 | Positions ("show my polymarket") | site-command (version not checked) | 9s (+1s follow-up line) | 2 (+1) | $3.00 cash, NATO x Russia Yes 15.7894 sh worth $2.61 | `acp site polymarket positions`, no browser. Extra step: a 1-step "anything you want done?" offer after. |
| 2026-10-27 | Bet $1 (Mbappé Yes), cold (first run on the kept login) | server #1244 (staging ef06dd04) | 2m 14s | — | **done**: 5.18 shares at worst 19.7¢ (shown 17.8¢), cash $3.00 → $1.98 | First server-path bet filled from chat. Its first try had failed on the geoblock check (card 5085, before the retry fix). Butler's reply ended with "well inside your $50 cap" (skill security 5). |
| 2026-10-27 | Sell all (5.18 Mbappé Yes), warm (right after the cold bet) | server #1244 (staging ef06dd04), skill 0.2.5 or older | 1m 36s | 3 | **done**: sold at worst 18.2¢, ~$0.97 back, cash $1.98 → $2.95 | 38 s faster than the cold bet (2m 14s); best click-flow sell was 3m 00s. Butler then posted the same result a 2nd time (1:43 AM), and both still named the $50 cap (skill not yet reinstalled to 0.2.6). |
| 2026-10-27 | Deposit $3 (card 5089) | server #1244 (staging ef06dd04) | tap → fail 55s | — | not done, nothing sent, cash $2.95 | Button label is now "Transfer Crypto No limit • Instant"; the exact match missed it. Fixed in server #1246. Butler offered the backup clicks (asks first, as the skill says). |
| 2026-10-27 | Log out ("can you logout from polymarket") | server #1244 (staging), skill 0.2.5+ | 7s | 1 | logged out: server's saved login cleared, no card | `acp site polymarket logout`, no browser. Was 2m 26s / 11 steps on the click flow (0.1.26), and looped on 0.2.4. Next Polymarket run logs in again (cold). |
| 2026-10-27 | Deposit $3 (card 5090) | server #1246 (staging 7b034a9a) | tap → fail 63s | — | not done, nothing sent | Got past Transfer Crypto. The address sits in a read-only box (input), not in the page text. Fixed in server #1248. |
| 2026-10-27 | Log in (card 5091), cold | server #1247 (staging a8a3e017), skill 0.2.7 | tap 13s + run 55s | — | **done**: logged in, login kept | `acp site polymarket login`, one card, no money. Butler ended its turn at 14s ("tell me when you've held it") instead of waiting for the result; the owner had to say "Done". |
| 2026-10-27 | Bet $1 (Mbappé Yes, card 5092), warm (right after `login`) | server #1247 (staging a8a3e017), skill 0.2.7 | card in 11s; tap 8s + run 56s; ~1m 15s total | 2 | **done**: bought Yes for at most $1.00 (~4.7 shares at 21.1¢) | Warm bet: run 56s vs the cold bet's 2m 14s total. Server posted the result note at 18:20:35, 0.3s after done. Between the tap and that note the chat shows nothing, and Butler said "tell me once you've held it". |

## Summary (2026-10-26, end of day)

Best run per action, first try → best. Target 1–2 min, 2–3 min acceptable.

| Action | First | Best | Skill | Status |
| --- | --- | --- | --- | --- |
| Login | 11m 50s | 2m 50s | 0.1.9 | acceptable |
| Deposit | 6m 00s | 2m 45s | 0.1.12 | acceptable |
| Withdraw | 5m 10s+ (stuck) | 4m 15s | 0.1.18 | owner OK for now |
| Log out | looped (stuck) | 7s (server `logout`, 1 step); click flow 2m 26s | 0.2.5 + server #1244 | in target |
| Claim | — | 3m 33s | 0.1.16 (no click list) | owner OK; click list (0.1.17) untested |
| Search + ask (bet) | ~1–2m | 1m 25s | 0.1.23 | in target |
| Bet (after yes) | 7m 28s | 3m 55s | 0.1.25 | owner OK; ~4 steps lost to the order-guard refusing Buy Yes; fill confirmed |
| Sell (ask + click) | 11m+ (stuck) | ~4m (52s + 3m); click 3m 00s–3m 33s twice | 0.1.23–0.1.24 | owner OK |

Floor: about 20 s per browser step (each click is a model call in the browser
helper), so ~7 steps ≈ 2.5 min. Under 2 min needs a container change (Jev or
fixed clicks), not skill wording.

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

## Planned: wallet login (not built yet, 2026-10-26)

Bosses want a wallet login instead of email, Browserbase only (no Polymarket API
for money, no Builder key, no 100-a-day relayer limit). Plan: put a wallet into the
Butler's own Browserbase page (`browser_connect_wallet`, generic, no site names);
every signature the page asks for goes to `POST /butler-exec/execute`, where
bevo-server checks it, files an owner card, and signs once the owner taps. The key
never enters the browser. Builds on virtuals-agent PR #250 and bevo-server PR #1194
(both merged 2026-10-23, later removed by #261 / #1208).

Where each step happens:

```
[BB]   Browserbase: the Butler's browser on polymarket.com
[API]  Polymarket public API (read only, from the Butler's shell)
[SRV]  bevo-server (checks + signs with the key)
[APP]  owner's phone (taps the card)
```

**1. Login (first time)**

```
[BB]   browser_connect_wallet: wallet put into the page
[BB]   open polymarket.com → Log in → "Rabby" → connect
[BB]   page asks: "sign in"
[SRV]  checks it is Polymarket's sign-in → files card
[APP]  owner taps
[SRV]  signs → signature back to [BB]
[BB]   username / terms → logged in
```

**2. Deposit**

```
[BB]   Deposit → Transfer Crypto → read address
[SRV]  bevo-send → files card
[APP]  owner taps → [SRV] sends USDC
```

**3. Bet (first time adds 2 cards)**

```
[API]  find the market + its url
[BB]   open url → Yes → +$1 +$1 → Buy Yes
[BB]   first time only: Enable Trading
         [SRV] ClobAuth card  → [APP] tap → [SRV] sign → [BB]
         [SRV] approvals card → [APP] tap → [SRV] sign → [BB]
[BB]   page asks to sign the order
[SRV]  checks: own wallet, amount, cap → card "Buy, pay $2, ≥2.10 shares"
[APP]  owner taps
[SRV]  signs → [BB] page sends the order to Polymarket
```

**4. Sell:** same as a bet: [BB] Sell → Max → Sell, then [SRV] card → [APP] tap →
[SRV] sign → [BB].

**5. Claim / Withdraw (only if added; #1194 refused both)**

```
[BB]   Portfolio → Redeem   (or Withdraw → address → Max)
[SRV]  checks: redeem only / bridge address leads to Butler's wallet → card
[APP]  tap → [SRV] sign → [BB] page sends it
```

**6. Logout:** [BB] picture → Log out.

**Rule:** all clicking is in [BB], every signature is checked and signed in [SRV],
and the owner only taps in [APP]. The key never enters [BB].

Expected speed: the same clicks as the email flow, plus the owner's tap per card.
Login ~2 min + 1 tap; first bet ~5 min + 3 taps; next bet ~4 min + 1 tap; sell
~3 min + 1 tap. Making the order card the owner's yes (no chat question first)
would save ~1.5 min per bet and sell.
