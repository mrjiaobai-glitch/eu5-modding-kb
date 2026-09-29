# common/government_reforms（政府改革）

> **一句话**：政府改革字段：三档修正块与实施时长、出现条件，以及全局互斥标记、槽位来自革新、实施不花钱三条原版事实。
> **什么时候看**：写政体改革、判断互斥标记是否挤掉原有路线，或核对社会价值门槛时看。
> **体量**：84 行 · 约 4 分钟通读

来源：`in_game\common\government_reforms\readme.txt`（22 行）+ 6 个数据文件 **328 项改革** 的实际用法

## 字段（readme 声明）

```
<reform id> = {
    age = <age key>                 # 可选：从哪个时代起可用
    government = <政体 key>          # 可选：仅该政体支持
    major = yes/no                  # ⭐ 互斥改革：每国只能实施一项
    unique = yes/no                 # 额外 UI 说明
    block_for_rebel = yes/no        # 叛军不可用
    locked = { <trigger> }          # 锁死，不可交互
    male_regnal_names = { <名字…> }   # 采用后统治者的男名库（readme 另有 female_ 版）
    female_regnal_names = { <名字…> }
    potential = { <trigger> }       # root = country，是否出现
    allow = { <trigger> }           # root = country，是否可以开始
    years / months / weeks / days = <int>   # 实施时长；修正按完成度缩放
    on_activate = { <effect> }      # root = country，被选中时
    on_fully_activated = { <effect> }  # 实施到 100% 时（无时长则立即）
    on_deactivate = { <effect> }    # 被移除时
    country_modifier = { <scaled & triggered modifier> }   # 施加于整个国家
    province_modifier = { … }       # 施加于省份
    location_modifier = { … }       # 施加于地点
    societal_values = { <focus> }   # 需要的"社会价值焦点"（见 common-societal_values.md）
}
```

三个 modifier 块都支持 `scale = <script>`（按实施进度缩放）与 `potential_trigger`（是否生效）。

## 原版实测（328 项 / 6 文件）

| 文件 | 项数 |
|---|---|
| `country_specific.txt` | **210** |
| `common.txt` | 88 |
| `monarchy.txt` / `republic.txt` / `theocracy.txt` | 12 / 11 / 6 |
| `steppe_horde.txt` | 1 |

| 字段 | 出现 | 率 | 备注 |
|---|---|---|---|
| `country_modifier` | 328 | 100% | 改革的本体就是它 |
| `years` | 300 | 91% | **2 年 262 项**；1 年 29；0 年 4；3–4 年各 2；10 年 1 |
| `potential` | 239 | 73% | |
| `age` | 160 | 49% | 6 个时代 |
| `unique` | 129 | 39% | 纯 UI |
| `allow` | 98 | 30% | |
| `government` | 82 | 25% | |
| `locked` | 75 | 23% | |
| **`major`** | **58** | 18% | 每国仅一项 |
| `societal_values` | 51 | 16% | 值形如 `humanist_focus` |
| `content_priority` | 40 | 12% | 原版值 300 / 400（DLC 与国别内容排序用） |
| `on_activate` / `on_deactivate` | 13 / 8 | 4% / 2% | |
| `location_modifier` | 6 | 2% | |
| `months` | 3 | 1% | readme 的 weeks/days 原版未用 |
| `icon` | 3 | 1% | |
| `block_for_rebel` | 3 | 1% | |
| `male_regnal_names` | 1 | 0% | `female_regnal_names` 原版 0 用 |

## ⚠️ 改革**没有价格**——门槛是槽位 + 社会价值

- `prices\` 里与改革相关的只有 `remove_government_reform = { stability = 20  righteousness = 10 }`（拆除代价），**没有"实施改革"的价格**
- **槽位来自革新**：`advances\` 中 14 条带 `government_reform_slots = 1`（6 个时代各 1 + 8 条国别/文化专属：中国、伊尔汗、塞尔维亚、瑞典、托斯卡纳、希腊文化组、罗马尼亚文化组、那不勒斯文化）
- 社会价值门槛：defines `SOCIAL_VALUE_REQUIREMENT_FOR_REFORM = 50`；界面预告 `GOVERNMENT_REFORM_SOCIETAL_VALUE_REQUIREMENT_FORECAST_IN_MONTHS = 12` / `..._CUTOFF_IN_MONTHS = 24`
- `major = yes` 的 58 项**全局互斥**：换 major 等于换国家路线

## 相关触发器 / 效果

`has_reform`（`country_triggers.txt:546`）、`num_reforms`、`num_open_reform_slots`、`is_unique_reform`、`is_major_reform`；效果 `add_reform`、`remove_reform`；脚本封装 `unlock_government_reform_effect` / `lock_government_reform_effect` / `has_unlocked_government_reform_trigger_text`。

## 审查要点

- **`years` 基本必备**（原版 91%）——实施期决定修正按进度缩放；漏写就是立刻全额生效。
- `country_modifier` 里的修正键必须在 `modifier_type_definitions` 注册，否则静默无效。
- `major = yes` 是全局排他：新增 major 会挤掉同政体下原有路线，改前先数一遍该 `government` 已有的 major。
- `societal_values` 填的是 **focus 标签**（纯本地化键，本体无定义块）；写错不报错，只是永远不满足条件。
- `age` / `government` / `potential` 写错 → 改革在界面里**根本不出现**。
- 未在 readme 中说明：本地化键格式（`<id>` 同名 + `_desc`）、`content_priority` 语义、槽位来自革新、以及"实施改革不花钱"这一事实。
