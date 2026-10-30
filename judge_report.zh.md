# Round-1 耗时归因（信号判定器）

> 说明：这是按「信号是否触发 + 阶段耗时是否对得上」自动归类的结果。独立判定（见 `independent_judge.zh.md`）更严格，**不采信**本报告里若干「attributed」标签；两份请对照看。  
> 本报告计入 n=10（含 c08 未交付对），均值会受 c08 的 −52.8 扭曲；独立判定只用 9 对完整成片。

均值 Δ **−7.1** 分钟；中位 **−6.0**；n=10  
类别：attributed=5，inconclusive=1，mixed=1，noise=1，random=2

**归因规则：** 仅当 round-1 对应信号触发，且节省的时间落在匹配阶段（poll/resume、child 首次写入/engine 阅读、资产重打字、entry 修复）时标为 **attributed**。仅 setup 主导、或 render/compress/子数量分叉且无匹配信号 → **random**。±2 分钟 → **noise**。

Δ = perf − base。负值 = perf 更快。

| case | base | perf | Δ | 类别 | 原因 | 触发的改动 |
|---|---:|---:|---:|---|---|---|
| c01-earbuds-night-studio-promo | 41.9 | 36.7 | −5.2 | random | compress_context 差异；child 数量分叉 | — |
| c02-shift-scheduling-saas-walkthrough | 37.1 | 63.5 | +26.4 | mixed | compress_context 差异；可解释≈2.0 / +26.4 | scoped_onboarding |
| c03-pythagoras-hand-drawn-explainer | 33.9 | 37.5 | +3.6 | random | setup 主导（+3.7），超出 round-1 范围 | — |
| c04-maritime-silk-road-vintage-map | 46.7 | 39.2 | −7.5 | attributed | 信号 async_children；可解释≈0.1 / −7.5 | async_children |
| c05-coffee-brand-annual-data | 32.5 | 29.2 | −3.3 | attributed | 信号 scoped_onboarding；可解释≈3.6 / −3.3 | scoped_onboarding |
| c06-hackathon-teaser-slam-type | 23.6 | 24.4 | +0.8 | noise | 落在 ±2 分钟内 | — |
| c07-pixel-office-week-short | 33.6 | 25.0 | −8.6 | attributed | 信号 scoped_onboarding；可解释≈4.5 / −8.6 | scoped_onboarding |
| c08-kids-waste-sorting-toy-3d | 97.7 | 44.9 | −52.8 | inconclusive | base 未交付 | asset_cp |
| c09-pour-over-coffee-papercut-howto | 50.3 | 32.8 | −17.5 | attributed | 信号 scoped_onboarding；可解释≈3.2 / −17.5 | scoped_onboarding |
| c10-neon-filaments-art-piece | 42.7 | 35.9 | −6.8 | attributed | 信号 asset_cp；可解释≈0.0 / −6.8 | asset_cp |

---

## 分 case 细节

### c01-earbuds-night-studio-promo
- 阶段 Δ：`total: -5.2`
- 信号摘要：双边均无 poll；perf 无 child（base 3 个且 3/3 读 engine）；perf 出现 asset retype。
- 类别：**random** — compress_context 差异；child 数量分叉

### c02-shift-scheduling-saas-walkthrough
- 阶段 Δ：`setup +9.3`，`children_wall +14.8`，`post_children +2.3`，`resume_gap 0`，`total +26.4`
- 信号摘要：poll/resume 持平；engine 读者 5→6；first_write 7.64→6.28；asset_cp 1→0，retype 双边均为 1。
- 类别：**mixed** — compress_context 差异；可解释≈2.0 / +26.4

### c03-pythagoras-hand-drawn-explainer
- 阶段 Δ：`setup +3.7`，`children_wall -2.3`，`post_children +2.2`，`resume_gap +0.5`，`total +3.6`
- 信号摘要：perf 增加 poll（240s）；engine 读者 3→4；asset retype 双边均为 1。
- 类别：**random** — setup 主导（+3.7），超出 round-1 范围

### c04-maritime-silk-road-vintage-map
- 阶段 Δ：`setup +0.2`，`children_wall -0.8`，`post_children -6.9`，`resume_gap +0.1`，`total -7.5`
- 信号摘要：poll 1→0（仅约 5s）；engine 读者 4/4→4/4；first_write 反而 8.22→9.6；retype 0→2。
- 类别：**attributed** — 信号 async_children；可解释≈0.1 / −7.5  
  （独立判定认为：5 秒 poll 解释不了 −7.5，主因在 post_children，应标随机性。）

### c05-coffee-brand-annual-data
- 阶段 Δ：`setup +0.7`，`children_wall -4.1`，`post_children +0.1`，`total -3.3`
- 信号摘要：poll 持平；engine 读者 3→4；first_write 7.4→5.72；cp 0→1。
- 类别：**attributed** — 信号 scoped_onboarding；可解释≈3.6 / −3.3  
  （独立判定：engine 读者未降，刚过噪声带，倾向 noise。）

### c06-hackathon-teaser-slam-type
- 阶段 Δ：`total +0.8`
- 信号摘要：perf 0 child vs base 5；声称信号大多不适用。
- 类别：**noise** — 落在 ±2 分钟内

### c07-pixel-office-week-short
- 阶段 Δ：`setup -0.6`，`children_wall -4.4`，`post_children -3.6`，`resume_gap -0.4`，`total -8.6`
- 信号摘要：perf poll 0→2（250s，与 async 主张相反）；engine 读者 3→4；first_write 7.13→4.58。
- 类别：**attributed** — 信号 scoped_onboarding；可解释≈4.5 / −8.6  
  （独立判定：poll↑ + engine 未降，与主张矛盾，标随机性/计划分叉。）

### c08-kids-waste-sorting-toy-3d
- 阶段 Δ：`total -52.8`（不可比）
- 信号摘要：base 未交付；perf 有 4 children；base 有 asset retype。
- 类别：**inconclusive** — 未交付

### c09-pour-over-coffee-papercut-howto
- 阶段 Δ：`setup -5.0`，`children_wall -1.9`，`post_children -10.7`，`resume_gap +1.9`，`total -17.5`
- 信号摘要：resume_gap 变差；engine 读者 5→4；first_write 6.1→4.2；asset_cp 1→0。
- 类别：**attributed** — 信号 scoped_onboarding；可解释≈3.2 / −17.5  
  （独立判定：主因 post_children/setup，first_write 节省远小于 Δ，标随机性。）

### c10-neon-filaments-art-piece
- 阶段 Δ：`total -6.8`
- 信号摘要：结构 0→1 child；retype 2→0，cp 0→1。
- 类别：**attributed** — 信号 asset_cp；可解释≈0.0 / −6.8  
  （独立判定：资产信号解释不了约 7 分钟，结构分叉为主，标随机性。）

---

## 与独立判定的关系（速览）

| 维度 | 信号判定器（本文件） | 独立判定 |
|------|----------------------|----------|
| 完整对均值 | 含 c08 时 −7.1（失真） | 排除 c08：**−2.0** |
| 四项改动是否可信 | 偏乐观（5 例 attributed） | **均不可信（0/4）** |
| 建议 | 作信号扫描参考 | **以独立判定为准做合入决策** |
