# common/scripted_effects（脚本化效果）

> **一句话**：脚本化效果的定义与 $参数$ 文本替换用法，以及 custom_description 键不能与效果键同名的坑。
> **什么时候看**：抽公共效果、写带参数的效果，或遇到 missing effect 报错时翻这篇。
> **体量**：363 行 · 约 17 分钟通读

## 目录

- [定义与使用](#定义与使用)
- [自定义参数（$键$ 文本替换）](#自定义参数键-文本替换)
- [重要坑（readme 原文强调）](#重要坑readme-原文强调)
- [审查要点](#审查要点)
- [本体实测补缺（2026-09 普查）](#本体实测补缺2026-09-普查)
  - [一、原版在用、readme 未声明的字段](#一原版在用readme-未声明的字段)
  - [二、取值白名单（本体出现过的值 + 次数）](#二取值白名单本体出现过的值--次数)
  - [三、该用哪些修正（本体在这个类目里实际用过，前 20）](#三该用哪些修正本体在这个类目里实际用过前-20)
  - [四、readme 声明、但本类目内原版 0 使用](#四readme-声明但本类目内原版-0-使用)
  - [五、深度 1 的块（子条目：政策／变体／子类型等）](#五深度-1-的块子条目政策变体子类型等)
  - [六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）](#六块内键最常见的前-15modifier--trigger--effect-里实际写的)
  - [七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）](#七引擎脚本命令通用键出现在-5-个类目不是本类目的字段-schema)

来源：`in_game\common\scripted_effects\readme.txt`

## 定义与使用

```
my_simple_effect = { <list of effects> }
# 使用：my_simple_effect = yes
```

## 自定义参数（$键$ 文本替换）

```
my_argument_effect = {
    add_prestige = $value$
}
# 使用：my_argument_effect = { value = 12 }

my_scoped_effect = {
    $target$ = { add_prestige = $value$ }
    take_over_all_wars = $target$
}
# 使用：my_scoped_effect = { target = scope:my_scope value = 13 }
```

- 参数是纯文本替换，可拼接出效果名：
```
my_argumented_effect_hello = { add_stability = 5 }
my_argumented_effect = { my_argumented_effect_$type$ = yes }
# 使用：my_argumented_effect = { type = hello }
```

## 重要坑（readme 原文强调）

- **custom_description 键不能与 effect 键相同**——相同会导致 custom_description 无法拾取 subject/object/value。
  - ❌ `my_effect = { custom_description = { text = my_effect object = <whatever> } }`
  - ✅ `my_effect = { custom_description = { text = my_effect_text object = <whatever> } }`

## 审查要点

- 参数化效果拼错参数（如 `$value$` 未传）会引发大量 "missing effect" 报错。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\scripted_effects\` 全量 **22 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `reset_societal_value` | 13 | 1 |  |
| `clear_variable_list` | 5 | 1 | nations_to_make_tributary（1）、visited_countries（1）、potential_enemy_list（1）、characters_on_board（1）、visited_ports（1） |
| `execute_prisoners` | 2 | 1 |  |
| `show_event_target_work_of_art_definition` | 2 | 1 |  |
| `set_as_designated_heir` | 2 | 1 | $target$（1） |
| `show_event_target_mission` | 2 | 1 |  |
| `set_revolution` | 2 | 1 | no（1）、yes（1） |
| `add_bureaucracy` | 2 | 1 | bureaucracy_type:central_secretariat_bureaucracy（1）、bureaucracy_type:imperial_censorate_bureaucracy（1） |
| `show_event_target_mission_task` | 2 | 1 |  |
| `council_of_trent_force_end_effect` | 2 | 1 | yes（2） |
| `location:shikama` | 1 | 1 |  |
| `add_subunit_strength` | 1 | 1 | heavy_melee_damage（1） |
| `location:satsuma` | 1 | 1 |  |
| `location:nakata` | 1 | 1 |  |
| `sell_prisoners_into_slavery` | 1 | 1 |  |
| `show_event_target_unit_ability` | 1 | 1 |  |
| `remove_commander` | 1 | 1 | $target$（1） |
| `location:fuwa_mino` | 1 | 1 |  |
| `show_event_target_culture_group` | 1 | 1 |  |
| `call_io_parliament` | 1 | 1 |  |
| `show_event_target_production_method` | 1 | 1 |  |
| `location:aki` | 1 | 1 |  |
| `capital.province` | 1 | 1 |  |
| `location:higashikubiki` | 1 | 1 |  |
| `set_societal_value` | 1 | 1 |  |
| `location:kume_houki` | 1 | 1 |  |
| `show_event_target_unit_type` | 1 | 1 |  |
| `show_event_target_international_organization` | 1 | 1 |  |
| `show_event_target_country_interaction` | 1 | 1 |  |
| `set_patriarch_variables` | 1 | 1 |  |
| `show_event_target_institution` | 1 | 1 |  |
| `request_ransom_prisoners` | 1 | 1 |  |
| `location:imizu` | 1 | 1 |  |
| `show_event_target_climate` | 1 | 1 |  |
| `show_event_target_advance_type` | 1 | 1 |  |
| `reset_character_location` | 1 | 1 |  |
| `location:mii` | 1 | 1 |  |
| `show_event_target_parliament_issue` | 1 | 1 |  |
| `location:ashida` | 1 | 1 |  |
| `location:awaji` | 1 | 1 |  |
| `show_event_target_pop_type` | 1 | 1 |  |
| `location:tsukama` | 1 | 1 |  |
| `show_event_target_topography` | 1 | 1 |  |
| `show_event_target_cabinet_action` | 1 | 1 |  |
| `remove_local_variable` | 1 | 1 | union_levels（1） |
| `location:gunma` | 1 | 1 |  |
| `location:tagata` | 1 | 1 |  |
| `hussite_wars_resolve_current_war_effect` | 1 | 1 | yes（1） |
| `gt_friend_encounter_roll_effect` | 1 | 1 | yes（1） |
| `show_event_target_artist_type` | 1 | 1 |  |
| `location:nyuu` | 1 | 1 |  |
| `show_event_target_religious_school` | 1 | 1 |  |
| `location:mikasa` | 1 | 1 |  |
| `location:ooita` | 1 | 1 |  |
| `location:houmi` | 1 | 1 |  |
| `location:kuwada` | 1 | 1 |  |
| `show_event_target_language` | 1 | 1 |  |
| `move_art` | 1 | 1 | $location$（1） |
| `location:kamioono` | 1 | 1 |  |
| `location:nagaoka` | 1 | 1 |  |
| `show_event_target_formable_country` | 1 | 1 |  |
| `show_event_target_religion_group` | 1 | 1 |  |
| `location:saga` | 1 | 1 |  |
| `show_event_target_disease` | 1 | 1 |  |
| `location:kasa` | 1 | 1 |  |
| `location:katsushika` | 1 | 1 |  |
| `show_event_target_demand` | 1 | 1 |  |
| `show_event_target_location_rank` | 1 | 1 |  |
| `location:kaya` | 1 | 1 |  |
| `show_event_target_peace_treaty` | 1 | 1 |  |
| `show_event_target_estate_privilege` | 1 | 1 |  |
| `location:koyu` | 1 | 1 |  |
| `hire_prisoners_as_mercenaries` | 1 | 1 |  |
| `show_event_target_policy` | 1 | 1 |  |
| `show_event_target_generic_action` | 1 | 1 |  |
| `location:sawata` | 1 | 1 |  |
| `location:abe` | 1 | 1 |  |
| `set_participated_in_parliament` | 1 | 1 |  |
| `location:iga` | 1 | 1 |  |
| `location:onyuu` | 1 | 1 |  |
| `change_development` | 1 | 1 | development_weak_penalty（1） |
| `location:oki` | 1 | 1 |  |
| `gt_party_trait_roll_effect` | 1 | 1 | yes（1） |
| `location:toyoura` | 1 | 1 |  |
| `show_event_target_hegemony` | 1 | 1 |  |
| `show_event_target_government_type` | 1 | 1 |  |
| `set_revolution_target` | 1 | 1 | yes（1） |
| `show_event_target_holy_site_definition` | 1 | 1 |  |
| `location:soo` | 1 | 1 |  |
| `set_art_owner` | 1 | 1 | $country$（1） |
| `show_event_target_religious_focus` | 1 | 1 |  |
| `location:izumi` | 1 | 1 |  |
| `show_event_target_trait` | 1 | 1 |  |
| `show_event_target_avatar` | 1 | 1 |  |
| `grant_effects_of_opinion` | 1 | 1 | yes（1） |
| `location:tsuga` | 1 | 1 |  |
| `change_max_raw_material_workers` | 1 | 1 | 1（1） |
| `show_event_target_country_rank` | 1 | 1 |  |
| `location:nakajima` | 1 | 1 |  |
| `show_event_target_disaster_type` | 1 | 1 |  |
| `location:miyagi` | 1 | 1 |  |
| `location:ochi` | 1 | 1 |  |
| `show_event_target_religious_aspect` | 1 | 1 |  |
| `location:naka_iwami` | 1 | 1 |  |
| `location:joutou` | 1 | 1 |  |
| `location:tomata` | 1 | 1 |  |
| `location:kamiura` | 1 | 1 |  |
| `location:shima` | 1 | 1 |  |
| `location:kurita` | 1 | 1 |  |
| `location:awa` | 1 | 1 |  |
| `location:suzuka` | 1 | 1 |  |
| `show_event_target_goods` | 1 | 1 |  |
| `location:osaka` | 1 | 1 |  |
| `show_event_target_recruitment_method` | 1 | 1 |  |
| `location:akita_kumamoto` | 1 | 1 |  |
| `location:ibaraki` | 1 | 1 |  |
| `show_event_target_religion` | 1 | 1 |  |
| `show_event_target_relation_type` | 1 | 1 |  |
| `location:tama` | 1 | 1 |  |
| `show_event_target_vegetation` | 1 | 1 |  |
| `location:hoi` | 1 | 1 |  |
| `show_event_target_holy_site_type` | 1 | 1 |  |
| `location:nomi` | 1 | 1 |  |
| `location:kaisou` | 1 | 1 |  |
| `show_event_target_sub_unit_category` | 1 | 1 |  |
| `show_event_target_language_family` | 1 | 1 |  |
| `location:aya` | 1 | 1 |  |
| `show_event_target_parliament_agenda` | 1 | 1 |  |
| `show_event_target_building_type` | 1 | 1 |  |
| `show_event_target_culture` | 1 | 1 |  |
| `show_event_target_estate_type` | 1 | 1 |  |
| `location:nakatsu` | 1 | 1 |  |
| `show_event_target_casus_belli` | 1 | 1 |  |
| `show_event_target_resolution` | 1 | 1 |  |
| `show_event_target_heir_selection` | 1 | 1 |  |
| `location:shimoagata` | 1 | 1 |  |
| `show_event_target_levy_setup` | 1 | 1 |  |
| `location:kamakura` | 1 | 1 |  |
| `location:ou` | 1 | 1 |  |
| `show_event_target_government_reform` | 1 | 1 |  |
| `show_event_target_god` | 1 | 1 |  |
| `location:noto_jap` | 1 | 1 |  |
| `show_event_target_child_education` | 1 | 1 |  |
| `change_country_name` | 1 | 1 | $tag$（1） |
| `show_event_target_law` | 1 | 1 |  |
| `show_event_target_regency_type` | 1 | 1 |  |
| `show_event_target_societal_value_type` | 1 | 1 |  |
| `show_event_target_character_interaction` | 1 | 1 |  |
| `hussite_wars_recompute_relative_strength_effect` | 1 | 1 | yes（1） |
| `reset_character_religious_figure` | 1 | 1 |  |
| `show_event_target_religious_faction` | 1 | 1 |  |
| `show_event_target_subject_type` | 1 | 1 |  |
| `location:ichihara` | 1 | 1 |  |
| `location:higashimatsuura` | 1 | 1 |  |
| `location:yatsushiro` | 1 | 1 |  |
| `location:takaichi` | 1 | 1 |  |
| `set_up_patriarch` | 1 | 1 |  |
| `location:saba` | 1 | 1 |  |
| `location:keta_tajima` | 1 | 1 |  |
| `location:toyoda` | 1 | 1 |  |

### 二、取值白名单（本体出现过的值 + 次数）

- **`set_variable`**（1 种）：comperator_var_current_highest_value_1_comperator（1）
- **`add_gold`**（1 种）：100（1）
- **`save_scope_as`**（5 种）：relic_finder（3）、root_country（2）、target_pop（2）、prisoner_remove_from_cabinet_scope（1）、assimilating_country（1）
- **`add_policy_to_international_organization`**（7 种）：policy:independent_acting_patriarchs_tenet（1）、policy:accept_essence_tenet（1）、policy:high_christology（1）、policy:allow_christological_debates_tenet（1）、policy:independent_authorities_tenet（1）、policy:local_traditions_and_rites（1）、policy:allow_double_sabbath_tenet（1）
- **`save_temporary_scope_as`**（7 种）：hw_joiner（1）、organization_scope（1）、root_country（1）、cardinal_seat_location（1）、member_state（1）、hw_side_member（1）、iw_reconcile_league（1）
- **`clear_global_variable_list`**（3 种）：hussite_wars_catholic_side_list（2）、hussite_wars_hussite_side_list（2）、sengoku_strong_daimyo_list（1）
- **`clear_variable_list`**（5 种）：nations_to_make_tributary（1）、visited_countries（1）、potential_enemy_list（1）、characters_on_board（1）、visited_ports（1）
- **`add_prestige`**（3 种）：prestige_mild_bonus（1）、prestige_severe_bonus（1）、prestige_extreme_bonus（1）
- **`move_country`**（3 种）：$country$（1）、scope:expedition_initiator（1）、var:chi_captured_emperor_origin（1）
- **`add_reform`**（2 种）：$reform$（1）、government_reform:colonial_subject（1）
- **`set_as_designated_heir`**（1 种）：$target$（1）
- **`set_revolution`**（2 种）：no（1）、yes（1）
- **`change_prosperity`**（1 种）：prosperity_weak_penalty（1）
- **`council_of_trent_force_end_effect`**（1 种）：yes（2）
- **`add_stability`**（2 种）：stability_extreme_bonus（1）、stability_mild_penalty（1）
- **`add_bureaucracy`**（2 种）：bureaucracy_type:central_secretariat_bureaucracy（1）、bureaucracy_type:imperial_censorate_bureaucracy（1）
- **`change_government_type`**（2 种）：government_type:monarchy（1）、government_type:republic（1）
- **`remove_location_modifier`**（2 种）：chi_grand_canal_destruction（1）、chi_grand_canal（1）
- **`hussite_wars_recompute_relative_strength_effect`**（1 种）：yes（1）
- **`change_culture`**（1 种）：$country$.culture（1）
- …另有 18 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 20）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `remove_variable` | 27 | 20 |
| `remove_country_modifier` | 18 | 12 |
| `save_scope_as` | 9 | 21 |
| `save_temporary_scope_as` | 7 | 14 |
| `add_policy_to_international_organization` | 7 | 8 |
| `clear_global_variable_list` | 5 | 5 |
| `move_country` | 3 | 9 |
| `add_stability` | 2 | 12 |
| `add_reform` | 2 | 11 |
| `remove_location_modifier` | 2 | 6 |
| `change_government_type` | 2 | 10 |
| `reverse_add_opinion` | 1 | 7 |
| `set_capital` | 1 | 8 |
| `trigger_event_non_silently` | 1 | 19 |
| `change_culture` | 1 | 5 |
| `leader_country` | 1 | 13 |
| `change_religion` | 1 | 7 |
| `add_government_power` | 1 | 11 |
| `remove_global_variable` | 1 | 5 |
| `add_legitimacy` | 1 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `Example` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_argument_effect` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_argumented_effect` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_argumented_effect_hello` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_effect` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_scoped_effect` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `my_simple_effect` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |
| `take_over_all_wars` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `Usage` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `if` | 184 | 18 |
| `custom_description` | 178 | 7 |
| `custom_tooltip` | 87 | 10 |
| `else_if` | 77 | 6 |
| `hidden_effect` | 38 | 8 |
| `else` | 36 | 13 |
| `change_societal_value` | 17 | 1 |
| `scope:expedition` | 14 | 2 |
| `set_variable` | 13 | 6 |
| `add_gold` | 12 | 5 |
| `every_in_global_list` | 10 | 2 |
| `add_country_modifier` | 10 | 2 |
| `international_organization:hre` | 7 | 1 |
| `every_pop` | 6 | 2 |
| `change_building_level_in_location` | 6 | 2 |
| … | 另有 25 种 | |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 749 | if、every_international_organization_member、every_present_culture_in_country、every_international_organization_owned_location |
| `value` | 538 | if、drop_antagonism_bomb、add_legitimacy、add_religious_influence |
| `if` | 308 | every_in_list、international_organization:catholic_church、every_international_organization_member、situation:hussite_wars |
| `text` | 263 | custom_tooltip、custom_description |
| `name` | 197 | add_to_variable_map、add_to_global_variable_list、limit_variable、remove_list_variable |
| `set_variable` | 184 | if、situation:rise_of_the_ottomans、situation:hussite_wars、custom_description |
| `add` | 127 | add_doom、modifier、else、change_prosperity |
| `NOT` | 124 | law、scope:new_hre_league_leader、any_ownable_location_in_area、scope:target_heir_religion_policy |
| `remove_variable` | 114 | situation:council_of_trent、on_ibadi_ruler_change、situation:rise_of_the_ottomans、scope:old_country |
| `multiply` | 107 | add_prestige、change_development、multiply、order_by |
| `save_scope_as` | 91 | if、every_international_organization_member、custom_description、create_mercenary |
| `target` | 81 | drop_antagonism_bomb、has_mutual_scripted_relation、add_character_to_first_open_cabinet、add_opinion |
| `else` | 66 | scope:actor、c:PAP、fraction、international_organization:catholic_church |
| `modifier` | 65 | weight、remove_trust_equilibrium、reverse_add_trust_equilibrium、has_opinion |
| `type` | 63 | add_temporary_demand、raise_levies、add_estate_satisfaction、remove_casus_belli |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `remove_variable` | 27 | 20 |
| `remove_country_modifier` | 18 | 12 |
| `save_scope_as` | 9 | 21 |
| `save_temporary_scope_as` | 7 | 14 |
| `add_policy_to_international_organization` | 7 | 8 |
| `clear_global_variable_list` | 5 | 5 |
| `move_country` | 3 | 9 |
| `add_stability` | 2 | 12 |
| `add_reform` | 2 | 11 |
| `remove_location_modifier` | 2 | 6 |
| `change_government_type` | 2 | 10 |
| `reverse_add_opinion` | 1 | 7 |
| `set_capital` | 1 | 8 |
| `trigger_event_non_silently` | 1 | 19 |
| `change_culture` | 1 | 5 |
| `leader_country` | 1 | 13 |
| `change_religion` | 1 | 7 |
| `add_government_power` | 1 | 11 |
| `remove_global_variable` | 1 | 5 |
| `add_legitimacy` | 1 | 9 |
