# in_game/common/biases（观感来源注册表）

> **一句话**：观感来源注册表：观感变化的原因标签在此登记，共八个档一千余条，含数值、上下限与时限字段及硬编码档提示。
> **什么时候看**：新增观感原因标签，或审查是否删改了硬编码档时翻这篇。
> **体量**：51 行 · 约 3 分钟通读

来源：**无 readme**——8 个档实查（`00_opinion_hardcoded.txt` 5,380 B / `01_opinion_scripted_diplomacy.txt` 9,438 B / `02_opinion_subject_types.txt` 967 B / `03_opinion_from_events.txt` 48,855 B / …），合计 **1,140 个条目**。

## 它是什么

**`opinion = { … type = <这里的键> }` 与各类"观感修正"的来源登记表**——这些键是**观感变化的"原因标签"**，界面会显示"因 <原因> +N 好感"（`modifiers` 体系里的观感侧）。

```txt
########################################
# These are hardcoded, removing may cause problems..
########################################

opinion_dummy = {
    value  = 1
    max    = 1000
    min    = -1000
    months = 12      # 该观感在这么多月后移除
}
```

## 字段实测（以 `00_opinion_hardcoded.txt` 为准）

| 字段 | 频次 | 说明 |
|---|---|---|
| `value` | 76 | 观感值（可正可负） |
| `min` / `max` | 19 / 13 | 钳制范围（原版常见 ±1000） |
| `months` | **1** | 自动移除月数——**原版只有 `opinion_dummy` 这么用**（其余条目不带时限） |

## 八个档的分工

| 档 | 体量 | 装什么 |
|---|---|---|
| `00_opinion_hardcoded.txt` | 5.4 KB | **引擎硬编码侧**——文件头原文明写 "removing may cause problems.." |
| `01_opinion_scripted_diplomacy.txt` | 9.4 KB | 脚本外交行为产生的观感（如边境侵犯、破停战…） |
| `02_opinion_subject_types.txt` | 1 KB | 各附属国类型相关观感 |
| `03_opinion_from_events.txt` | **48.9 KB** | 事件用的观感标签（**最大的一档**，历史事件"因某事 +50 好感"都在这） |
| 其余 4 档 | — | 按主题细分（详见目录） |

## 审查要点

- **`00_opinion_hardcoded.txt` 里的条目"删了可能出问题"**（文件头原文）——审查 mod 时若发现删改，按高风险处理。
- **新增观感标签必须在这里登记**，否则 `opinion = { … type = <新键> }` 静默失效（观感不变、界面无原因文案）。
- 观感标签是**事件/脚本与界面的契约**：登记 + loc 文案（原因文案键）缺一不可。
- ⚠️ **本目录有一处用「名字一行、`{` 另起一行」写法**（`pitfalls.md` §十三 记录的 `biases` 1 处）——用 `key = {` 同行正则统计会漏数、并把嵌套块误当顶层。
- 未在 readme 中说明：本类目**没有 readme**；`value` 的合法区间、`months` 与外交系统的联动、以及 8 个档的加载/覆盖顺序均未文档化。
