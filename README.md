# GTA VI launch-day Twitch CCU forecast — akun 5 rebuild ("Vivid Compass")

Competition: `gta6-twitch-ccu-forecast-01` (make.fun). Forecast five
quantiles (0.05 / 0.25 / 0.5 / 0.75 / 0.95) of Twitch concurrent viewers
for the "Grand Theft Auto VI" category, hours 2–24 of release day
(2026-11-19 UTC), given the hour-1 print.

## Model

Per-hour **ensemble of Theil-Sen and recency-weighted OLS** on
`log(viewers_h)` vs `log(viewers_1)` across the rolling-midnight
comparables, with the **sharpest spread in the family (×0.60)**:

- Theil-Sen: median of pairwise slopes — robust to outlying launches.
  Medians are upper medians, exactly as the backtest computes them.
- WOLS: weights `1.0 + 0.1*i` in list order — newer launches count more.
- Center: average of the two fits. Spread: ensemble residual sd × 0.60,
  clamped to [0.20, 1.00].

Leave-one-out backtest over the 12 rolling comparables: **mean pinball
loss 0.2430**, winning 8 of 12 held-out launches against WOLS ×0.75 —
the best win rate of any variant tested. The sibling ensemble at
spread ×0.75 scored 0.2401; this entry is the family's second, distinct
bet on the sharp-spread idea, differing from every other entry in the
fleet on all 23 hours of the practice case.

## Run

```sh
MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

Stdlib only, deterministic, no network. Reads every `<id>.in`, writes
`<id>.out` with 23 rows (hours 2–24), five quantiles each.

## Rebuild note

This replaces the account's first entry (a plain Theil-Sen forecast,
practice loss 0.173, rank 1.166). Same repo, new history: `makefun
init` root (freshness anchor) + LICENSE + this work.
