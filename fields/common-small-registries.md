# in_game/common：六个小型注册表（insults / scripted_country_names / building_categories / location_ranks / historical_scores / hegemons）

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
