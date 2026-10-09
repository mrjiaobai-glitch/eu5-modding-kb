# in_game/common：六个小型注册表（insults / scripted_country_names / building_categories / location_ranks / historical_scores / hegemons）

> **一句话**：六个无 readme 小型注册表的实查：侮辱文本、脚本化国名、建筑类别、地点等级、历史评分与霸权。
> **什么时候看**：要动侮辱语、自动国名、建筑音效归类、霸权条件这类小表时翻这篇。
> **体量**：196 行 · 约 9 分钟通读

来源：六个类目全部**无 readme**，逐档实查（2026-09）。逐个给出"条目数 / 字段 / 用法 / 坑"。

---

## 一、`insults`（侮辱文本，73 条）

```txt
insult_default = {
    trigger = { always = yes }                        # 73/73 条都有 trigger
}
```

- 1 档 / 10,281 B / 715 行 / **73 条**，每条**只有 `trigger`**（文本在 loc，键 = 条目名）。
- 用法：国家交互"侮辱"里按 trigger 挑一条（`scope:actor` 可用，原版有 `insult_default2` 这类按条件的变体）。
- **审查**：条目名即 loc 键；加条目要补 loc，否则显示 raw key。loc 全表见 `tools\loc-keys.md`。

## 二、`scripted_country_names`（脚本化国名，12 条）

```txt
jamaica_scripted_country_name = {
    country_trigger = { … }     # 常规国家 trigger（root = 国家）
    capital_trigger = { … }     # 首都须通过的 trigger
    #（还有命名用字段；见档内其它条目）
}
```

- 1 档 / 8,148 B / 378 行 / **12 条**，用于"同一 tag 按文化/首都/宗主自动换国名"（原版 `new_granada_scripted_country_name` 用 `overlord ?= { … }` + 文化/核心地点判定）。
- **审查**：两个 trigger 的作用域不同（`country_trigger` = 国家、`capital_trigger` = 首都地点），混用是高频错误；名字本体是 loc 键。

## 三、`building_categories`（建筑类别 = 音效桶，15 条）

```txt
rgo_building_category = {
    # Buildings classified here (mercury_patio, lumber_mill, stone_quarry, ...) are extractive
    # industry that gets built like any other building, so they map to the industry sound bucket.
    # True RGO upgrades take a separate code path (construction_rgo_<method>) that doesn't read this.
    audio_category = industry
}
basic_industry_category = { audio_category = industry }
```

- 1 档 / 1,090 B / 69 行 / **15 条**，字段就一个 `audio_category`。
- **官方注释原文即重点**：这一类目是**音效桶**，`rgo_building_category` 里的建筑**是普通建筑**；**真 RGO 升级走 `construction_rgo_<method>` 这条引擎路径，不读这里**（详见 `vanilla\vanilla-production-and-buildings.md` §2.6 的陷阱）。
- **审查**：给建筑分类只影响音效/归类，不会让它变成 RGO。

## 四、`location_ranks`（地点等级，4 级）

```txt
#Note:  logic assumes that anything below a rank in the list is worse
megalopolis = {
    rank_modifier = {
        local_burghers_desired_pop = 4.0
        local_laborers_desired_pop = 4.0
        local_soldiers_desired_pop = 1.00
        local_nobles_desired_pop   = 0.200
        local_clergy_desired_pop   = 0.400
        …
    }
}
```

- 1 档 / 3,729 B / 203 行 / **4 条**（乡村 → 大都市的等级阶梯）。
- **机制**：每级给一组**逐 POP 的"期望人口"倍率**（`local_<pop>_desired_pop`），地点升级/降级时按它调整人口构成。
- **审查**：**顺序即等级**（注释原文："列表里靠下的等级更差"）——插新等级要放对位置；改倍率会直接影响城市人口结构。

## 五、`historical_scores`（历史评分，21 条）

```txt
hs_napoleon = { tag = FRA_revolutionary_republic   score = 40000 }
hs_suleyman = { tag = TUR                          score = 38000 }
```

- 1 档 / 1,077 B / 104 行 / **21 条**，字段两个：`tag` + `score`。
- 用途：结算界面/排行榜的"历史地位"对照分值。
- **审查**：`tag` 必须是真实国家（可以是**成立后的新 tag**，如 `FRA_revolutionary_republic`）。

## 六、`hegemons`（霸权，5 种）

```txt
military_hegemon = {
    gain = {                                     # 获得条件
        is_great_power = yes
        is_subject = no
        NOT = { any_other_great_power = { regular_army_size > root.regular_army_size } }
    }
    lose = {                                     # 失去条件（★ 带滞回：×1.1）
        OR = {
            any_other_great_power = { regular_army_size > { value = root.regular_army_size  multiply = 1.1 } is_subject = no }
            is_subject = yes
        }
    }
    modifier = {                                 # 霸主修正
        global_war_score_efficiency = 0.2
        allow_diplomacy_violate_sovereignty = yes
        allow_cabinet_soldiers_as_workforce = yes
    }
}
```

- 5 档 / 各 0.5 KB / **5 种**：经济 / 海军 / 军事 / 外交 / 文化（`0_economic_…` ~ `4_cultural_…`）。
- 字段各 5/5：`gain` / `lose` / `modifier`。**`gain` 与 `lose` 是两套阈值（滞回）**——军事霸权的 lose 用 `multiply = 1.1`（要比对手强 10% 才被抢走）。
- **审查**：`lose` 用 `OR` 包多个失去条件（对手反超 **或** 自己变成附属国）；`modifier` 里的键须在 `modifier_type_definitions` 注册。机制全景与实际常量见 `vanilla\vanilla-government-and-reform.md`（`HEGEMONY_LOST_MONTHS = 120`）。

---

## 通用审查要点（六个类目共有）

- **条目名 = loc 键**（除 `location_ranks` / `building_categories` / `hegemons` 这类引擎枚举），漏键显示 raw key。
- 引用类字段（`tag` / `estate` / 修正键 / 建筑 id）都要回原版对应目录核对存在性 → `tools\audit-ids.md`。
- 六个类目**都没有 readme**，字段以本档与档内注释为准。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\small-registries\` 全量 **10 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `score` | 21 | 1 | 16000（1）、14000（1）、40000（1）、36000（1）、32000（1） |
| `audio_category` | 15 | 1 | generic（5）、industry（3）、military（3）、economy（2）、religion（1） |
| `upgrade_from` | 2 | 1 | city（1）、town（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`audio_category`**（6 种）：generic（5）、industry（3）、military（3）、economy（2）、religion（1）、culture（1）
- **`show_in_label`**（2 种）：yes（3）、no（1）
- **`institution_spawn_chance_factor`**（4 种）：4（1）、2（1）、3（1）、1（1）
- **`frame_tier`**（4 种）：4（1）、2（1）、3（1）、1（1）
- **`color`**（4 种）：color_rank_empire（1）、color_rank_county（1）、color_rank_duchy（1）、color_rank_kingdom（1）
- **`tier`**（4 种）：4（1）、2（1）、3（1）、1（1）
- **`is_established_city`**（1 种）：yes（3）
- **`build_time`**（3 种）：1825（1）、730（1）、365（1）
- **`construction_demand`**（3 种）：build_megalopolis_demand（1）、build_town_demand（1）、build_city_demand（1）
- **`upgrade_from`**（2 种）：city（1）、town（1）
- **`max_rank`**（1 种）：yes（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `tag` | 21 | 30 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_construct_weight` | 0 次（10 档） | 全库也没有 → 疑似废弃字段 |
| `Note` | 0 次（10 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `trigger` | 73 | 1 |
| `country_trigger` | 12 | 1 |
| `capital_trigger` | 12 | 1 |
| `location_trigger` | 12 | 1 |
| `modifier` | 5 | 5 |
| `gain` | 5 | 5 |
| `lose` | 5 | 5 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `OR` | 53 | lose、AND、OR、country_trigger |
| `scope:recipient` | 44 | trigger |
| `scope:actor` | 31 | trigger |
| `tag` | 30 | any_subject_or_below、scope:actor、scope:recipient、OR |
| `NOT` | 15 | OR、gain、scope:actor、scope:recipient |
| `is_subject` | 15 | lose、gain、OR |
| `always` | 13 | trigger |
| `religion` | 12 | scope:recipient、OR |
| `any_primary_or_accepted_culture` | 11 | any_subject_or_below、OR |
| `culture.language` | 10 | NOT、scope:actor、scope:recipient、OR |
| `culture` | 10 | scope:actor、country_trigger、scope:recipient、OR |
| `has_reform` | 8 | scope:actor、scope:recipient |
| `area` | 8 | location_trigger、capital_trigger、OR |
| `religion.group` | 8 | NOT、scope:actor、scope:recipient、OR |
| `any_other_great_power` | 7 | NOT、OR |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `tag` | 21 | 30 |
