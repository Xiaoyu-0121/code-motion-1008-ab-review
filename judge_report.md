Mean Δ -7.1 min; median -6.0; n=10
Classes: attributed=5, inconclusive=1, mixed=1, noise=1, random=2

Attribution rule: a case is **attributed** only when a round-1 signal fires AND the saved minutes line up with that phase (poll/resume, child first-write/engine reads, asset retype, entry fix). Setup-only or render-count divergences without those signals are **random**. ±2 min is **noise**.

# Round-1 latency attribution

perf − base. Negative = perf faster.

| case | base | perf | Δ | class | why | changes fired |
|---|---:|---:|---:|---|---|---|
| c01-earbuds-night-studio-promo | 41.9 | 36.7 | -5.2 | random | compress_context difference; child count diverged | — |
| c02-shift-scheduling-saas-walkthrough | 37.1 | 63.5 | +26.4 | mixed | compress_context difference; explained≈2.0 of +26.4 | scoped_onboarding |
| c03-pythagoras-hand-drawn-explainer | 33.9 | 37.5 | +3.6 | random | setup-dominated (+3.7), outside round-1 scope | — |
| c04-maritime-silk-road-vintage-map | 46.7 | 39.2 | -7.5 | attributed | signals ['async_children']; explained≈0.1 of -7.5 | async_children |
| c05-coffee-brand-annual-data | 32.5 | 29.2 | -3.3 | attributed | signals ['scoped_onboarding']; explained≈3.6 of -3.3 | scoped_onboarding |
| c06-hackathon-teaser-slam-type | 23.6 | 24.4 | +0.8 | noise | within ±2 min | — |
| c07-pixel-office-week-short | 33.6 | 25.0 | -8.6 | attributed | signals ['scoped_onboarding']; explained≈4.5 of -8.6 | scoped_onboarding |
| c08-kids-waste-sorting-toy-3d | 97.7 | 44.9 | -52.8 | inconclusive | undelivered | asset_cp |
| c09-pour-over-coffee-papercut-howto | 50.3 | 32.8 | -17.5 | attributed | signals ['scoped_onboarding']; explained≈3.2 of -17.5 | scoped_onboarding |
| c10-neon-filaments-art-piece | 42.7 | 35.9 | -6.8 | attributed | signals ['asset_cp']; explained≈0.0 of -6.8 | asset_cp |

## c01-earbuds-night-studio-promo
- phase Δ: {'total': -5.2}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": 0.2, "perf_resume": null}, "scoped_onboarding": {"base_engine_kids": 3, "perf_engine_kids": null, "base_kids": 3, "perf_kids": 0, "base_1st": 6.47, "perf_1st": null}, "asset_cp": {"base_retype": 0, "perf_retype": 1, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **random** — compress_context difference; child count diverged

## c02-shift-scheduling-saas-walkthrough
- phase Δ: {'setup': 9.3, 'children_wall': 14.8, 'post_children': 2.3, 'resume_gap': 0.0, 'total': 26.4}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": 0.2, "perf_resume": 0.2}, "scoped_onboarding": {"base_engine_kids": 5, "perf_engine_kids": 6, "base_kids": 5, "perf_kids": 6, "base_1st": 7.64, "perf_1st": 6.28}, "asset_cp": {"base_retype": 1, "perf_retype": 1, "base_cp": 1, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **mixed** — compress_context difference; explained≈2.0 of +26.4

## c03-pythagoras-hand-drawn-explainer
- phase Δ: {'setup': 3.7, 'children_wall': -2.3, 'post_children': 2.2, 'resume_gap': 0.5, 'total': 3.6}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 1, "base_poll_s": 0, "perf_poll_s": 240, "base_resume": 0.2, "perf_resume": 0.7}, "scoped_onboarding": {"base_engine_kids": 3, "perf_engine_kids": 4, "base_kids": 3, "perf_kids": 4, "base_1st": 7.1, "perf_1st": 6.53}, "asset_cp": {"base_retype": 1, "perf_retype": 1, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **random** — setup-dominated (+3.7), outside round-1 scope

## c04-maritime-silk-road-vintage-map
- phase Δ: {'setup': 0.2, 'children_wall': -0.8, 'post_children': -6.9, 'resume_gap': 0.1, 'total': -7.5}
- signals: `{"async_children": {"base_poll_cmds": 1, "perf_poll_cmds": 0, "base_poll_s": 5, "perf_poll_s": 0, "base_resume": 0.1, "perf_resume": 0.2}, "scoped_onboarding": {"base_engine_kids": 4, "perf_engine_kids": 4, "base_kids": 4, "perf_kids": 4, "base_1st": 8.22, "perf_1st": 9.6}, "asset_cp": {"base_retype": 0, "perf_retype": 2, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **attributed** — signals ['async_children']; explained≈0.1 of -7.5

## c05-coffee-brand-annual-data
- phase Δ: {'setup': 0.7, 'children_wall': -4.1, 'post_children': 0.1, 'resume_gap': 0.0, 'total': -3.3}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": 0.2, "perf_resume": 0.2}, "scoped_onboarding": {"base_engine_kids": 3, "perf_engine_kids": 4, "base_kids": 3, "perf_kids": 4, "base_1st": 7.4, "perf_1st": 5.72}, "asset_cp": {"base_retype": 0, "perf_retype": 0, "base_cp": 0, "perf_cp": 1}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **attributed** — signals ['scoped_onboarding']; explained≈3.6 of -3.3

## c06-hackathon-teaser-slam-type
- phase Δ: {'total': 0.8}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": 0.3, "perf_resume": null}, "scoped_onboarding": {"base_engine_kids": 5, "perf_engine_kids": null, "base_kids": 5, "perf_kids": 0, "base_1st": 3.82, "perf_1st": null}, "asset_cp": {"base_retype": 0, "perf_retype": 0, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **noise** — within ±2 min

## c07-pixel-office-week-short
- phase Δ: {'setup': -0.6, 'children_wall': -4.4, 'post_children': -3.6, 'resume_gap': -0.4, 'total': -8.6}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 2, "base_poll_s": 0, "perf_poll_s": 250, "base_resume": 0.4, "perf_resume": 0.0}, "scoped_onboarding": {"base_engine_kids": 3, "perf_engine_kids": 4, "base_kids": 3, "perf_kids": 4, "base_1st": 7.13, "perf_1st": 4.58}, "asset_cp": {"base_retype": 0, "perf_retype": 0, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **attributed** — signals ['scoped_onboarding']; explained≈4.5 of -8.6

## c08-kids-waste-sorting-toy-3d
- phase Δ: {'total': -52.8}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": null, "perf_resume": 0.2}, "scoped_onboarding": {"base_engine_kids": null, "perf_engine_kids": 4, "base_kids": 0, "perf_kids": 4, "base_1st": null, "perf_1st": 6.62}, "asset_cp": {"base_retype": 1, "perf_retype": 0, "base_cp": 0, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **inconclusive** — undelivered

## c09-pour-over-coffee-papercut-howto
- phase Δ: {'setup': -5.0, 'children_wall': -1.9, 'post_children': -10.7, 'resume_gap': 1.9, 'total': -17.5}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": 0.1, "perf_resume": 2.0}, "scoped_onboarding": {"base_engine_kids": 5, "perf_engine_kids": 4, "base_kids": 5, "perf_kids": 4, "base_1st": 6.1, "perf_1st": 4.2}, "asset_cp": {"base_retype": 0, "perf_retype": 0, "base_cp": 1, "perf_cp": 0}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **attributed** — signals ['scoped_onboarding']; explained≈3.2 of -17.5

## c10-neon-filaments-art-piece
- phase Δ: {'total': -6.8}
- signals: `{"async_children": {"base_poll_cmds": 0, "perf_poll_cmds": 0, "base_poll_s": 0, "perf_poll_s": 0, "base_resume": null, "perf_resume": 0.2}, "scoped_onboarding": {"base_engine_kids": null, "perf_engine_kids": 0, "base_kids": 0, "perf_kids": 1, "base_1st": null, "perf_1st": 2.7}, "asset_cp": {"base_retype": 2, "perf_retype": 0, "base_cp": 0, "perf_cp": 1}, "entry_path": {"base_entry_fixes": 0, "perf_entry_fixes": 0}}`
- class: **attributed** — signals ['asset_cp']; explained≈0.0 of -6.8
