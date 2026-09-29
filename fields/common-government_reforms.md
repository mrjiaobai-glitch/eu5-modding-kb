# common/government_reforms（政府改革）

> **一句话**：政府改革字段：三档修正块与实施时长、出现条件，以及全局互斥标记、槽位来自革新、实施不花钱三条原版事实。
> **什么时候看**：写政体改革、判断互斥标记是否挤掉原有路线，或核对社会价值门槛时看。
> **体量**：146 行 · 约 7 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\government_reforms\` 全量 **6 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 40 | 2 | 800（6）、400（6）、600（5）、100（4）、900（4） |
| `icon` | 3 | 1 | soyurghal_governor_reform（2）、tawantinsuyu_monarchy（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`years`**（7 种）：2（262）、1（29）、0（3）、3（2）、4（2）、10（1）、0.25（1）
- **`age`**（6 种）：age_1_traditions（59）、age_3_discovery（27）、age_4_reformation（26）、age_2_renaissance（20）、age_6_revolutions（15）、age_5_absolutism（13）
- **`unique`**（1 种）：yes（129）
- **`government`**（5 种）：monarchy（49）、republic（26）、theocracy（5）、tribe（1）、steppe_horde（1）
- **`major`**（1 种）：yes（58）
- **`content_priority`**（11 种）：800（6）、400（6）、600（5）、100（4）、900（4）、300（4）、1100（3）、200（3）、1000（2）、700（2）、500（1）
- **`icon`**（2 种）：soyurghal_governor_reform（2）、tawantinsuyu_monarchy（1）
- **`block_for_rebel`**（1 种）：yes（3）
- **`months`**（2 种）：6（2）、3（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `days` | 0 次（6 档） | **有**（写在别的类目） |
| `female_regnal_names` | 0 次（6 档） | 全库也没有 → 疑似废弃字段 |
| `on_fully_activated` | 0 次（6 档） | **有**（写在别的类目） |
| `province_modifier` | 0 次（6 档） | **有**（写在别的类目） |
| `weeks` | 0 次（6 档） | 全库也没有 → 疑似废弃字段 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `societal_values` | 51 | 2 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `has_or_had_tag` | 147 | potential、OR、allow、AND |
| `OR` | 89 | culture、capital、custom_tooltip、potential |
| `NOT` | 85 | custom_tooltip、french_ducal_vassal_reform、locked、any_child |
| `government_reform_slots` | 81 | country_modifier |
| `has_unlocked_government_reform_trigger` | 71 | potential、allow |
| `has_variable` | 59 | NOT、custom_tooltip、NOR、locked |
| `culture` | 59 | potential、locked、allow、OR |
| `type` | 56 | has_unlocked_government_reform_trigger、is_locked_mechanic |
| `global_crown_estate_power` | 46 | country_modifier |
| `has_reform` | 41 | potential、NOR、allow、NOT |
| `mechanic` | 41 | is_locked_mechanic |
| `is_locked_mechanic` | 41 | locked、custom_tooltip |
| `nobles_estate_target_satisfaction` | 27 | country_modifier |
| `country_cabinet_efficiency` | 26 | country_modifier |
| `global_nobles_estate_power` | 25 | country_modifier |
