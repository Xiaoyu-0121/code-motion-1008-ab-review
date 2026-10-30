# 独立耗时判定（A/B）

判定依据：仅根据 `metrics.jsonl`、`runs.jsonl`、`cases.json` 重新推导。未把 `judge_report.md` 当作事实；未查看 plugin 源码。

计入均值/标注的完整成对：**9** 对（排除 **c08**——base 未交付 `delivered=false`）。  
Δ = perf − base（分钟）。|Δ| ≤ 2 → **噪声**。某项「声称改动」只有在对应信号朝正确方向变化，**且**该阶段能解释 Δ 中有意义的一部分时，才算归因成立。

---

## 1. 总评

九对完整成片的墙钟耗时均值差约 **−2.0 分钟**（base 38.0 → perf 36.0），落在噪声带；中位数（−5.2）被少数「计划/结构分叉」的大起大落拉偏，并不是某套机制稳定奏效。本轮四项声称的耗时改动**没有可信因果效应**：async 相关信号（`main_poll_*`、`resume_gap`）平均持平甚至变差；scoped onboarding 失败——即便 `child_to_first_write` 偶有下降，`children_reading_engine` 仍接近打满（常为 100%）；`asset_cp` / `asset_retype_or_find` 方向不稳定，且从不足以解释数分钟级 Δ；`entry_fixes` 两边本来就是 0。观察到的变快，以及 c02 的大幅变慢，更宜标为 **agent 随机性 / 执行计划分叉**（子 agent 数量翻转、setup 或 post_children 文字阶段膨胀、render/compress 形态变化），而不是补丁生效。

---

## 2. 分 case 表

| case | base | perf | Δ | 标注 | 证据 |
|------|------|------|---|------|------|
| c01-earbuds-night-studio-promo | 41.9 | 36.7 | −5.2 | 随机性 | 结构分叉：base 3 个 child vs perf 0；retype 0→1（资产信号反了）；无可比的 poll/onboarding 信号。 |
| c02-shift-scheduling-saas-walkthrough | 37.1 | 63.5 | +26.4 | 随机性 | setup +9.3、children_wall +14.8；kids 5→6；poll/resume 持平；engine 读者 5→6；asset_cp 1→0。 |
| c03-pythagoras-hand-drawn-explainer | 33.9 | 37.5 | +3.6 | 随机性 | 主要是 setup +3.7；perf 反而增加 poll 240s（async 更差）；engine 读者 3→4；asset/entry 持平。 |
| c04-maritime-silk-road-vintage-map | 46.7 | 39.2 | −7.5 | 随机性 | Δ 几乎全在 post_children −6.9；wall/setup 持平；first_write 变差；retype 0→2；poll 5s→0 买不回 7.5 分钟。 |
| c05-coffee-brand-annual-data | 32.5 | 29.2 | −3.3 | 噪声 | 刚过 ±2；wall −4.1 但 engine 读者 3→4（scoped 失败）；poll 持平；cp 0→1 太小，解释不了 Δ。 |
| c06-hackathon-teaser-slam-type | 23.6 | 24.4 | +0.8 | 噪声 | \|Δ\|&lt;2；结构分叉 base 5 kids vs perf 0；声称信号不适用/持平。 |
| c07-pixel-office-week-short | 33.6 | 25.0 | −8.6 | 随机性 | wall/post 下降，但 poll 0→2（250s）、engine 读者 3→4，与 async/scoped 主张矛盾；主对话 text 21.4→6.6，属计划分叉。 |
| c08-kids-waste-sorting-toy-3d | 97.7* | 44.9 | — | 排除 | base 未交付；不计入均值。 |
| c09-pour-over-coffee-papercut-howto | 50.3 | 32.8 | −17.5 | 随机性 | 约 −10.7 来自 post_children，另有 setup −5；resume_gap 0.1→2.0（async 更差）；asset_cp 1→0；first_write −1.9 ≪ Δ。 |
| c10-neon-filaments-art-piece | 42.7 | 35.9 | −6.8 | 随机性 | 结构分叉 0→1 child；renders 5→3 / 主进程 engine 读 13→4；虽有 asset_cp↑ retype↓，但解释不了约 7 分钟。 |

\*未交付

完整成对均值 Δ：**−2.0 分钟**（中位 **−5.2**）。

---

## 3. 四项声称改动的可信度

| # | 声称 | 期望信号 | 完整集合上的表现 | 可信？ |
|---|------|----------|------------------|--------|
| 1 | 异步 children | ↓ `main_poll_cmds` / `main_poll_sleep_s` / `resume_gap` | 双边都有 kids 的 6 对：poll_cmds 0.17→0.50，poll_sleep 0.8→81.7s，resume_gap 0.20→0.55 — **方向反了**。信号正确且能解释实质 Δ 的 case：**0**。 | **否**（0/9 可归因） |
| 2 | 收窄 builder 上手 | ↓ `children_reading_engine`，↓ `child_to_first_write` | engine 读者仍几乎全员（均值 3.83→4.33）。first_write 在 5/6 双边-kids 对上均值下降，但 engine 门槛未过，且 first_write 节省单独解释不了大 Δ。 | **否**（最多弱 first_write 抖动；**0** 例清晰归因） |
| 3 | 资产用 `cp` | ↑ `asset_cp`，↓ `asset_retype_or_find` | 方向对：c05、c10。方向错：c01、c02、c04、c09。其余持平。没有任何一对靠资产信号解释有意义的 Δ。 | **否**（信号不稳；**0** 例耗时因果） |
| 4 | 公开 entry 路径 | ↓ `entry_fixes` | **每条 thread 都是 0→0**——无变化，无从归因。 | **否**（0/9；已在地板） |

**汇总：** 声称改动带来可信耗时效应 = **0/4**。标为「由改动引起」= **0**；随机性 = **6**；噪声 = **2**；排除 = **1**。

---

## 4. 质量–耗时权衡：建议人工看片

墙钟波动大、可能存在计划/质量与速度交换（或交付失败）的 case：

| case | Δ | 为何要看 |
|------|---|----------|
| **c02** | **+26.4** | perf 明显更慢；children_wall + setup 更重；6 vs 5 kids——确认是更完整还是卡住。 |
| **c09** | **−17.5** | 完整对里最大「赢」；主要由 post_children 塌缩 + 主对话变短——核对成片剪辑/质量是否逊于 base。 |
| **c07** | **−8.6** | 大赢但 poll↑ 与主张矛盾；确认成片质量是否对齐 base。 |
| **c04** | **−7.5** | 赢几乎全在 post_children，不是声称信号——对比收尾质量。 |
| **c10** | **−6.8** | 架构不同（0 vs 1 child）+ 更少 render——艺术片质量核验。 |
| **c01** | **−5.2** | perf 主进程独干（0 children）vs base 3 kids——结构质量风险。 |
| **c08** | 不适用（base 未交付） | base 从未交付 `.mp4`；视为 base 交付/质量失败，不是耗时胜利。 |

仅凭耗时不必优先看片：**c03**（小幅 +3.6）、**c05** / **c06**（接近噪声）。
