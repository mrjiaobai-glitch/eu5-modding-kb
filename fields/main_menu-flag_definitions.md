# main_menu/common/flag_definitions（旗帜规则）

> **一句话**：旗帜规则：列表加 flag_definition 的完整取旗流程、字段频次与触发器作用域对照表。
> **什么时候看**：给国家配旗、写条件旗或宗主角标，要核对优先级与作用域时翻这篇。
> **体量**：67 行 · 约 4 分钟通读

来源：**文件头自带官方字段文档**（`00_flag_definitions.txt` 第 1–33 行注释即完整 schema + 作用域表）+ 9,622 行数据实查（**259 个列表 / 1,133 个 `flag_definition`**）。

> 机制全貌（含 COA 本体、随机池、图集）见 `vanilla\vanilla-heraldry-and-flags.md`；本档是**字段权威 + 原版实测**。

## 文件结构

```
FLAG_DEFINITION_LIST = {          # 列表名；国家会找与自己 tag 同名的列表
    includes = ANOTHER_LIST       # 原版 0 处使用
    flag_definition = { … }       # 可重复；通过 trigger 的候选中取 priority 最高者
}
# DEFAULT 列表【永远】被包含；若没有任何定义适用 → 直接拿 tag 当 COA_KEY
```

- 列表名 = **国家 tag**（原版 258 个 tag 列表 + 1 个 `DEFAULT`）。
- 同一个 tag 可以有多个 `flag_definition`，按 `priority` 竞争（例：`ENG` 有 `priority = 1` 的常旗与 `priority = 2` 的 `ENG_nordic` 变体）。

## 字段（实查频次）

| 字段 | 频次 / 1,133 | 说明 |
|---|---|---|
| `coa` | **1,133（100%）** | 主旗。`[list] COA_KEY` 的 `list` 关键字表示从 `template_lists\` 的池子里随机 |
| `priority` | **1,133（100%）** | 有效候选中取最高（默认文件注释未给默认值，原版全部显式写） |
| `trigger` | 875 | 生效条件；作用域见下表 |
| `allow_overlord_canton` | 108 | 默认 `no`；允许在旗角放宗主角标 |
| `subject_canton` | 105 | 施加给**自己附属国**的角标 COA 键 |
| `overlord_canton_scale` | 91 | 角标缩放（默认 `{ 0.5 0.5 }`） |
| `overlord_canton_offset` | 2 | 角标位移（默认 `{ 0 0 }`） |
| `coa_with_overlord_canton` | 1 | 带角标版本单独指定（默认 = `coa`） |
| `includes` | **0** | 文档有、原版零使用 |
| `allow_revolutionary_indicator` / `revolutionary_canton` | **0** | 未启用（文件自注 "revolutionary is not in use for the moment"） |

## `trigger` 作用域（文件头原表）

| | 已存在的国家 | 释放一个国家 | 成立国家 |
|---|---|---|---|
| `root` | definition | definition | definition |
| `target` | country | N/A | N/A |
| `initiator` / `actor` | N/A | player | player |
| `overlord` | direct overlord（若存在） | player | direct overlord（若存在） |

## 文件头附带的常量与色码

- 画布：`@coa_width = 768`、`@coa_height = 512`。
- 角标缩放预设：`cross` 333×205 / `sweden` 255×204 / `norway` 192×192 / `denmark` 220×220（原版写法都 `+ 0.001` 防抖）、`@third`、`@usa_canton_width = 0.5`、`@usa_canton_height = @[1/13*7]`。
- 时代触发器别名（写 trigger 时直接用）：1342 `coa_def_renaissance_age_2_trigger`、1437 `coa_def_discovery_age_3_trigger`、1537 `coa_def_reformation_age_4_trigger`、1637 `coa_def_absolutism_age_5_trigger`、1737 `coa_def_revolutions_age_6_trigger`。
- 纹章学色码（注释）：A 银 / O 金 / B 蓝 / G 红 / S 黑 / V 绿 / P 紫 / M 棕 / Z 黑白相间 / E 貂皮 / N 橙。

## 原版实测

- `DEFAULT` 列表管**殖民地旗**与**海盗随机旗**：文件自注 "this overrule the priority of any prescripted tag (ie CAN, USA)"——**DEFAULT 的优先级会盖过预设 tag**（原版 `DEFAULT` 的 `priority = 500`）。
- 拿旗流程：`tag` → 找同名列表（+ `DEFAULT`）→ 逐个 `flag_definition` 评 `trigger` → 取 `priority` 最高 → `coa`（可能是 `list`，则随机）→ 叠加宗主角标（`subject_canton` / `allow_overlord_canton`）。

## 审查要点

- **列表名必须是真实 tag**（3 字母）；写错不报错，只是永远不命中。
- **`priority` 必写**：原版 1,133/1,133 都有——漏写会与其它定义同分，取舍不可控。
- **别照抄 `includes` / `allow_revolutionary_indicator` / `revolutionary_canton`**：文档里有、原版零使用（后者文件自注未启用）。
- **`subject_canton` 与 `allow_overlord_canton` 是两侧配合**：附属国旗上的角标由**宗主**的定义提供（宗主写 `subject_canton`），附属国侧则用 `allow_overlord_canton` 表示"我的旗允许被加角标"。
- **`coa` 指向的键必须在 `coat_of_arms` 里存在**（或写成 `list "<池名>"`）；审查时把两者对照（见 `fields\main_menu-coat_of_arms.md`）。
- 未在 readme 中说明：**本类目没有 readme**，但**文件头 17 行注释是官方 schema**（比大多数类目更可靠）；优先级同分时的行为、图集重建时机未文档化。
