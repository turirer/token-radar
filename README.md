# Token Radar — pre-token protocols ranked by implied value

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/turirer/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/turirer/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Entry for [make.fun](https://make.fun/bounty/token-radar-01) competition `token-radar-01` — $1,325, entries close
2026-12-14T10:00:00Z. A radar for protocols that record no token yet: it ranks
them by an implied value computed from the snapshot's own figures and shows a
reader how to farm each one.

Referral: https://make.fun/r/d6btpjsv

## The model (radar-2)

```
implied = round( tvl_usd
  x category_multiple
  x (1 + clamp(change_7d, -60, 150) / 100 x 0.6)
  x (0.55 + min(age_days, 1460) / 1460 x 0.45)
  x (1 + 0.03 x min(chains - 1, 5)) )
```

- **TVL** — the protocol's size, from the DefiLlama snapshot of 14 Nov 2026.
- **Category multiple** — what a token in this category is worth per dollar of
  TVL (DEXs 0.38x, bridges 0.15x, and the rest in between; the table is stated
  in `lib/model.ts` and on the dashboard, so a reader can disagree with it).
- **Growth** — seven-day TVL change, clamped, so a hot week lifts a protocol
  but never decides it alone.
- **Maturity** — how long it has run with no token; an unlisted age uses 365 days.
- **Chains** — a distribution proxy: more chains, more farmers reached.

No protocol name is ever special-cased. The board covers all 40 watchlist
protocols plus every other rankable pre-token protocol below the $50M floor
(226 rows). Wrapped, staked and bridged products of tokens that already trade
are set aside with the reason, never ranked as pre-token. Live-token protocols
are shown separately with their market caps — a fact, not a model.

## The dashboard

`dashboard/` is a static page with no network: a sortable, filterable board
(search, category, chain, watchlist-only, growing-only), a detail view for
every protocol with its model breakdown and a 7-step farm plan (each step with
a cost and a time), a live-token view, a set-aside view, and a top-10 chart.
Every byte it shows is in `data.js`, written by `main.ts` from the same board.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input /tmp/token-radar-kit/input/snapshot.json
```

That writes `board.json`, `data.js`, `dashboard/`, `screenshots/` and
`dossier.json` to `out/`. Serve the dashboard with any static file server
from `out/` and open `dashboard/index.html`.

## Files

- `main.ts` — Node 22, stdlib only; the entry program.
- `lib/model.ts` — the radar-2 model, the board builder and the farm plans.
- `dashboard/` — `index.html`, `app.css`, `app.js` (the page, no network).
- `screenshots/` — 3 real renders of the dashboard in a browser.
- `vendor/makefun-board.ts` — the kit's dossier library, unchanged.
- `LICENSE` — MIT.

## Caveats

An implied value is a model, not a price, and it never predicts whether or
when a token is issued. A protocol that records no token may still have one
its DefiLlama record does not carry. Farm only money you can afford to lose,
and only through a protocol's own app.

---

<sub>Entered on [make.fun](https://make.fun/?ref=d6btpjsv&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
