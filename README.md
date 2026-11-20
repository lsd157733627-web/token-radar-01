# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/lsd157733627-web/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/lsd157733627-web/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar ranks every pre-token protocol in DefiLlama's sealed snapshot by the implied value
of the token it may one day issue, and shows a reader how to farm each one. 226 rows: the full
40-protocol watchlist plus every other rankable pre-token protocol below the $50M floor. The value
model is calibrated from the snapshot itself — each category's multiple is the median mcap / TVL
of the live protocols in that category, shrunk toward the global median — not a flat multiple, and
never a hardcoded list of names. The dashboard is a static page with no network: a sortable,
filterable board, a detail view with the model's full arithmetic and a six-step farm plan per
protocol, a live-token view set aside with market caps, and a sandbox that re-scores the board
when a constant changes.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input /input/snapshot.json
```

Node 22 or later, standard library only. The program reads the sealed snapshot, writes the
ranked board to `out/board.json`, the dashboard's data to `out/data.js`, copies `dashboard/` and
`screenshots/` to `out/`, and writes `out/dossier.json`. Open `out/dashboard/index.html` in a
browser to use the radar.

## The model (radar-2)

```
implied = round( tvl_usd x multiple(category)
                 x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
                 x (0.6 + min(age_days, 1095) / 1095 x 0.4)
                 x (1 + 0.02 x min(chains, 5)) )
multiple(category) = clamp( (n x median(mcap / TVL of live protocols in the category)
                              + 8 x 0.2138) / (n + 8), 0.05, 0.80 )
```

Age is the time since DefiLlama first listed the protocol. "No token" means the protocol
records no token in DefiLlama, never that it has none, and nothing here predicts whether or when
a token is issued.

---

<sub>Entered on [make.fun](https://make.fun/?ref=kjbhpbb1&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
