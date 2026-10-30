# common/scripted_triggers（脚本化触发器）

> **一句话**：脚本化触发器的定义与 $参数$ 文本替换用法，以及 custom_description 键不能与触发器键同名的坑。
> **什么时候看**：抽公共触发器、写带参数的触发器，或遇到 missing trigger 报错时翻这篇。
> **体量**：224 行 · 约 11 分钟通读

来源：`in_game\common\scripted_triggers\readme.txt`

## 定义与使用

```
my_simple_trigger = { <list of triggers> }
# 使用：my_simple_trigger = yes
```

## 自定义参数（$键$ 文本替换）

```
my_argument_trigger = { prestige = $value$ }
# 使用：my_argument_trigger = { value = 12 }

my_scoped_trigger = {
    $target$ = { prestige = $value$ }
    is_enemy_of = $target$
}
# 使用：my_scoped_trigger = { target = scope:my_scope value = 13 }
```

- 参数可拼接 trigger 名：`my_argumented_trigger = { my_argumented_trigger_$type$ = yes }`

## 重要坑（readme 原文强调）

- **custom_description 键不能与 trigger 键相同**——相同会导致 custom_description 无法拾取 subject/object/value（写法同 scripted_effects）。

## 审查要点

- 参数化 trigger 拼错参数引发大量 "missing trigger" 报错。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\scripted_triggers\` 全量 **30 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `has_institution` | 12 | 1 | institution:professional_armies（2）、institution:banking（2）、institution:manufactories（1）、institution:military_revolution（1）、institution:pike_and_shot（1） |
| `country_rank_is_what` | 4 | 1 |  |
| `modifier:blocked_from_being_leader` | 3 | 2 | no（3） |
| `gfx_culture_applicable` | 3 | 1 | military_order_gfx（1）、$RELIGION_FLAG$（1）、$CULTURE_FLAG$（1） |
| `has_exploration` | 3 | 1 | no（3） |
| `has_exploration_construction` | 3 | 1 | no（3） |
| `area:granada_area` | 3 | 1 |  |
| `is_linked_to_foreign_building` | 2 | 1 | no（2） |
| `area:murcia_area` | 2 | 1 |  |
| `area:andalusia_area` | 2 | 1 |  |
| `c:CHI` | 1 | 1 |  |
| `modifier:blocked_from_being_ruler` | 1 | 1 | no（1） |
| `can_become_a_ruler` | 1 | 1 | yes（1） |
| `is_location_colonial` | 1 | 1 | yes（1） |
| `capital.sub_continent` | 1 | 1 | sub_continent:western_europe（1） |
| `knows_country` | 1 | 1 | $target$（1） |
| `game_is_initialized` | 1 | 1 | yes（1） |
| `has_unit` | 1 | 1 | no（1） |
| `modifier:uses_parliament_for_law_votes` | 1 | 1 | yes（1） |
| `clan_has_valid_landable_location` | 1 | 1 | yes（1） |
| `is_loyal` | 1 | 1 | yes（1） |
| `battle_for_the_strait_castile_holds_the_strait` | 1 | 1 | yes（1） |
| `is_holy_site_for` | 1 | 1 | religion:hellenism_religion（1） |
| `current_papal_authority` | 1 | 1 |  |
| `modifier:is_praefecta` | 1 | 1 | yes（1） |
| `num_rebels` | 1 | 1 | 0（1） |
| `is_geographically_correct` | 1 | 1 | yes（1） |
| `dominant_language` | 1 | 1 |  |
| `has_global_variable_list` | 1 | 1 | wokou_nation_list（1） |
| `is_eligible_for_marriage` | 1 | 1 | yes（1） |
| `is_mercenary_leader` | 1 | 1 | no（1） |
| `has_or_had_great_ruler` | 1 | 1 | no（1） |
| `modifier:blocked_from_cabinet` | 1 | 1 | no（1） |
| `modifier:excluded_from_paying_tithe` | 1 | 1 | no（1） |
| `has_chivalric_order` | 1 | 1 | yes（1） |
| `scope:old_heir_selection` | 1 | 1 | heir_selection:bishopric_elective（1） |
| `battle_for_the_strait_morocco_holds_the_strait` | 1 | 1 | yes（1） |
| `modifier:may_explore` | 1 | 1 | yes（1） |
| `battle_for_the_strait_granada_holds_the_strait` | 1 | 1 | yes（1） |
| `modifier:slavery_blocked` | 1 | 1 | no（1） |
| `portrait_wear_armor_trigger` | 1 | 1 | yes（1） |
| `is_holding_of_any_religious_order` | 1 | 1 | no（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`has_institution`**（10 种）：institution:professional_armies（2）、institution:banking（2）、institution:manufactories（1）、institution:military_revolution（1）、institution:pike_and_shot（1）、institution:artillery_institution（1）、institution:printing_press（1）、institution:scientific_revolution（1）、institution:new_world（1）、institution:renaissance（1）
- **`exists`**（7 种）：this（3）、union（3）、$work_of_art$（2）、ruler（1）、international_organization:middle_kingdom（1）、yes（1）、scope:recipient（1）
- **`has_owner`**（1 种）：yes（9）
- **`culture`**（1 种）：culture:cossack_culture（1）
- **`has_trait`**（8 种）：pockmarked_trait（1）、bubonic_plague_trait（1）、sickly（1）、healthy（1）、hunchback（1）、smallpox_trait（1）、disfigured（1）、scarred（1）
- **`is_subject`**（1 种）：no（7）
- **`has_building_with_at_least_one_level`**（5 种）：university（3）、naval_base（1）、printing_press_shop（1）、tools_guild（1）、gun_smith（1）
- **`owns`**（6 种）：location:jerusalem（1）、location:antioch（1）、location:alexandria（1）、location:constantinople（1）、location:rome（1）、location:tahert（1）
- **`country_exists`**（5 种）：c:CAS（1）、c:CHI（1）、c:GRA（1）、this（1）、c:MOR（1）
- **`at_war`**（1 种）：no（5）
- **`in_civil_war`**（1 种）：no（5）
- **`is_alive`**（1 种）：yes（4）
- **`dominant_culture`**（2 种）：scope:actor.culture（1）、culture:catalan（1）
- **`is_artist`**（2 种）：no（3）、yes（1）
- **`government_type`**（3 种）：government_type:theocracy（2）、government_type:monarchy（1）、government_type:republic（1）
- **`save_temporary_scope_as`**（3 种）：root_country（2）、evaluated_scope（1）、this_country（1）
- **`has_discovered_area`**（3 种）：area:southern_africa_coast_area（1）、area:gulf_of_africa_sea（1）、area:moluccas_area（1）
- **`has_ruler`**（1 种）：yes（3）
- **`has_any_active_disaster`**（1 种）：no（3）
- **`has_estate`**（3 种）：estate_type:peasants_estate（1）、estate_type:nobles_estate（1）、estate_type:burghers_estate（1）
- …另有 74 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 20）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `exists` | 12 | 41 |
| `has_owner` | 9 | 17 |
| `has_trait` | 8 | 12 |
| `is_subject` | 7 | 25 |
| `has_building_with_at_least_one_level` | 7 | 10 |
| `owns` | 6 | 14 |
| `in_civil_war` | 5 | 11 |
| `country_exists` | 5 | 20 |
| `at_war` | 5 | 22 |
| `is_alive` | 4 | 18 |
| `save_temporary_scope_as` | 4 | 14 |
| `is_artist` | 4 | 7 |
| `dominant_culture` | 4 | 11 |
| `government_type` | 4 | 31 |
| `dominant_religion` | 3 | 5 |
| `has_variable` | 3 | 38 |
| `is_adult` | 3 | 18 |
| `is_port` | 3 | 11 |
| `international_organization_has_policy` | 3 | 18 |
| `has_estate` | 3 | 15 |
| … | 另有 10 个修正名 | | |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `Example` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `is_enemy_of` | 0 次（30 档） | **有**（出现在 14 个类目） |
| `my_argument_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `my_argumented_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `my_argumented_trigger_hello` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `my_scoped_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `my_simple_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `my_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `prestige` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `Usage` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `OR` | 181 | 21 |
| `custom_description` | 102 | 11 |
| `custom_tooltip` | 87 | 14 |
| `trigger_if` | 61 | 9 |
| `NOT` | 31 | 10 |
| `trigger_else` | 29 | 8 |
| `owner` | 15 | 2 |
| `trigger_else_if` | 12 | 3 |
| `NOR` | 9 | 5 |
| `culture` | 8 | 4 |
| `any_international_organization_member` | 8 | 1 |
| `capital` | 7 | 2 |
| `any_buildings_in_location` | 6 | 1 |
| `AND` | 6 | 1 |
| `any_country_with_special_status_of_type` | 3 | 1 |
| … | 另有 25 种 | |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `text` | 238 | custom_description、custom_tooltip |
| `NOT` | 176 | any_institutions_embraced、is_junior_partner、trigger_else、battle_for_the_strait_morocco_requirements_met |
| `OR` | 148 | OR、trigger_else、can_create_subject_alliance、has_had_situation |
| `AND` | 123 | trigger_if、NOT、?、custom_tooltip |
| `limit` | 120 | trigger_if、ordered_country_with_special_status_of_type、trigger_else_if、ordered_international_organization_member |
| `has_variable` | 116 | NOR、any_character、custom_description、OR |
| `value` | 91 | custom_description、subtract、OR、trigger_else |
| `region` | 81 | OR、capital、any_owned_location |
| `location_key` | 78 | OR |
| `exists` | 57 | trigger_if、custom_description、OR、any_international_organizations_member_of |
| `raw_material` | 52 | any_location_in_market、NOR、OR |
| `this` | 51 | any_attacker、AND、any_defender、NOR |
| `has_country_modifier` | 51 | OR |
| `trigger_if` | 51 | get_most_likely_candidate_for_seniority、trigger_if、can_change_primary_culture、AND |
| `culture` | 50 | unit_iberian_ship_location_trigger、AND、unit_catalan_ship_location_trigger、unit_akritai_location_trigger |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `exists` | 12 | 41 |
| `has_owner` | 9 | 17 |
| `has_trait` | 8 | 12 |
| `is_subject` | 7 | 25 |
| `has_building_with_at_least_one_level` | 7 | 10 |
| `owns` | 6 | 14 |
| `in_civil_war` | 5 | 11 |
| `country_exists` | 5 | 20 |
| `at_war` | 5 | 22 |
| `is_alive` | 4 | 18 |
| `save_temporary_scope_as` | 4 | 14 |
| `is_artist` | 4 | 7 |
| `dominant_culture` | 4 | 11 |
| `government_type` | 4 | 31 |
| `dominant_religion` | 3 | 5 |
| `has_variable` | 3 | 38 |
| `is_adult` | 3 | 18 |
| `is_port` | 3 | 11 |
| `international_organization_has_policy` | 3 | 18 |
| `has_estate` | 3 | 15 |
| … | 另有 10 个 | |
