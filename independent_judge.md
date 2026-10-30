# Independent latency judge (A/B)

Judge: re-derived from `metrics.jsonl` + `runs.jsonl` + `cases.json` only. Did not treat `judge_report.md` as ground truth. Did not inspect plugin source.

Completed pairs for means/labels: **9** (exclude **c08** — base `delivered=false`).  
Δ = perf − base (minutes). |Δ| ≤ 2 → noise. A claimed change counts only if its matching signal moves the right way **and** that phase explains a meaningful share of Δ.

---

## 1. Overall verdict

Across nine completed pairs the mean wall-time delta is about **−2.0 min** (base 38.0 → perf 36.0), which is at the noise floor; the median (−5.2) is pulled by a few large plan-driven swings, not by a consistent mechanism win. The four claimed latency changes do **not** show credible causal effect: async signals (`main_poll_*`, `resume_gap`) are flat or worse on average; scoped-onboarding fails because `children_reading_engine` stays saturated (often 100% of kids) even when `child_to_first_write` ticks down; `asset_cp` / `asset_retype_or_find` move inconsistently and never explain multi-minute deltas; `entry_fixes` are already zero on both sides. Observed wins and the large c02 regression are better labeled agent randomness / plan divergence (child-count flips, setup or post_children text blow-ups, render/compress shape changes) than attributed to the claimed patches.

---

## 2. Per-case table

| case | base | perf | Δ | label | evidence |
|------|------|------|---|-------|----------|
| c01-earbuds-night-studio-promo | 41.9 | 36.7 | −5.2 | randomness | Plan split: base 3 kids vs perf 0; retype 0→1 (asset signal wrong); no comparable poll/onboarding signals. |
| c02-shift-scheduling-saas-walkthrough | 37.1 | 63.5 | +26.4 | randomness | Setup +9.3 and children_wall +14.8; kids 5→6; poll/resume flat; kids_engine 5→6; asset_cp 1→0. |
| c03-pythagoras-hand-drawn-explainer | 33.9 | 37.5 | +3.6 | randomness | Mostly setup +3.7; perf added poll 240s (async worse); kids_engine 3→4; asset/entry flat. |
| c04-maritime-silk-road-vintage-map | 46.7 | 39.2 | −7.5 | randomness | Δ ≈ post_children −6.9; wall/setup flat; first_write worse; retype 0→2; poll 5s→0 cannot buy 7.5 min. |
| c05-coffee-brand-annual-data | 32.5 | 29.2 | −3.3 | noise | Barely past ±2; wall −4.1 but kids_engine 3→4 (scoped fail); poll flat; cp 0→1 too small to explain Δ. |
| c06-hackathon-teaser-slam-type | 23.6 | 24.4 | +0.8 | noise | \|Δ\|&lt;2; plan split base 5 kids vs perf 0; claimed signals N/A / flat. |
| c07-pixel-office-week-short | 33.6 | 25.0 | −8.6 | randomness | Wall/post down but poll 0→2 (250s) and kids_engine 3→4 contradict async/scoped; text 21.4→6.6 plan diverge. |
| c08-kids-waste-sorting-toy-3d | 97.7* | 44.9 | — | excluded | Base undelivered; excluded from mean. |
| c09-pour-over-coffee-papercut-howto | 50.3 | 32.8 | −17.5 | randomness | ~−10.7 from post_children + −5 setup; resume_gap 0.1→2.0 (async worse); asset_cp 1→0; first_write −1.9 ≪ Δ. |
| c10-neon-filaments-art-piece | 42.7 | 35.9 | −6.8 | randomness | Plan split 0→1 child; renders 5→3 / main engine reads 13→4; asset_cp↑ retype↓ present but cannot explain ~7 min. |

\*undelivered

Completed-pair mean Δ: **−2.0 min** (median **−5.2**).

---

## 3. Credence of the four claimed changes

| # | Claim | Expected signal | Across completed set | Credible? |
|---|--------|-----------------|----------------------|-----------|
| 1 | async children | ↓ `main_poll_cmds` / `main_poll_sleep_s` / `resume_gap` | Both-kids n=6: poll_cmds 0.17→0.50, poll_sleep 0.8→81.7s, resume_gap 0.20→0.55 — **wrong direction**. Right-way cases with material Δ share: **0**. | **No** (0/9 attributable) |
| 2 | scoped onboarding | ↓ `children_reading_engine`, ↓ `child_to_first_write` | Engine readers still ~all kids (mean 3.83→4.33). first_write mean fell in 5/6 both-kids pairs, but engine-read gate failed and first_write savings never explain large Δ alone. | **No** (at most weak first_write ticks; **0** cases clear attribution) |
| 3 | asset `cp` | ↑ `asset_cp`, ↓ `asset_retype_or_find` | Right: c05, c10. Wrong: c01, c02, c04, c09. Flat elsewhere. No case where asset signal explains a meaningful share of Δ. | **No** (signal flaky; **0** latency-causal) |
| 4 | public entry path | ↓ `entry_fixes` | **0→0 on every thread** — no movement, nothing to attribute. | **No** (0/9; already at floor) |

**Summary counts:** credible latency effect from claimed changes = **0 of 4**. Cases labeled caused-by-claims = **0**; randomness = **6**; noise = **2**; excluded = **1**.

---

## 4. Quality–time tradeoff — human film review

Large wall swings where plan/quality may have traded with speed (or delivery failed):

| case | Δ | Why review |
|------|---|------------|
| **c02** | **+26.4** | Perf much slower; heavier children_wall + setup; 6 vs 5 kids — check if slower run is more complete or just stuck. |
| **c09** | **−17.5** | Largest completed win; dominated by post_children collapse and less main text — verify cut/quality vs base. |
| **c07** | **−8.6** | Big win with contradictory poll↑; confirm film quality matches base. |
| **c04** | **−7.5** | Win almost all in post_children, not claimed signals — compare finish quality. |
| **c10** | **−6.8** | Different architecture (0 vs 1 child) + fewer renders — art-piece quality check. |
| **c01** | **−5.2** | Perf did main-only (0 children) vs base 3 kids — structural quality risk. |
| **c08** | n/a (base undelivered) | Base never delivered `.mp4`; treat as delivery/quality failure on base, not a latency win. |

Not flagged for film review on latency grounds alone: **c03** (small +3.6), **c05** / **c06** (near noise).
