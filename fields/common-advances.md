# common/advances（科技/进步）

> **一句话**：讲 advances 科技节点的块格式：时代与前置要求、可研究条件、各类解锁引用，以及研究期间与完成后的修正。
> **什么时候看**：写或审查科技节点、给节点挂解锁项与修正时翻这篇。
> **体量**：160 行 · 约 8 分钟通读

来源：`in_game\common\advances\readme.txt`

## 块格式

```
<advance ID> = {
    age = <age>                     # 该 advance 在哪个时代可用
    icon = <icon>                   # 图标
    requires = <advance>            # 放置在该 advance 之后
    government = <government_type>  # 仅指定政体可用
    country_type = <location/pop/building/army>  # 仅指定国家类型可用
    allow = { <triggers> }          # 满足才可研究
    potential = { <triggers> }      # 满足才显示；注意：advances 不会追溯可见（研究前不满足就永远不显示）
    for = <adm/dip/mil>             # 仅对时代开始时选定该专精的国家显示
    unlock_unit = <unit>
    unlock_ability = <ability>
    unlock_interaction = <interaction>                    # 角色交互
    unlock_country_interaction = <country_interaction>
    unlock_relation_type = <relation_type>
    unlock_building = <building>
    unlock_law = <law>
    unlock_levy = <levy>
    unlock_government_reform = <government_reform>
    unlock_casus_belli = <casus_belli>
    unlock_subject_type = <subject_type>
    unlock_production_method = <production_method>
    allow_children = <yes/no>       # 强制该 advance 为子节点；违反会产生 error log 输出
    <modifiers>                     # 研究完成后获得的修正
    modifier_while_progressing = {
        potential_trigger = <trigger>
        scale = <maths>
        <modifiers>
    }                               # 研究期间满足 potential_trigger 时应用的比例修正
}
```

## 审查要点

- `unlock_*` 引用的目标（unit/ability/interaction/law/levy/reform/cb/subject_type/production_method）须在对应类目中存在。
- `potential` 不满足的 advance 不会因后续条件满足而出现——这是设计语义，不是 bug。
- 子节点约束：`allow_children = yes` 的 advance 若被其他 advance `requires` 引用不符合预期会报错。
- 未在 readme 中说明：本地化键格式、advance ID 命名规则。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\advances\` 全量 **214 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 224 | 32 | 300（46）、500（22）、1000（21）、1100（20）、700（19） |
| `research_cost` | 37 | 3 | -0.75（18）、2.0（14）、2.5（3）、1.0（2） |
| `in_tree_of` | 33 | 4 | global_trade_advance（7）、new_world_advance（5）、scientific_revolution_advance（4）、enlightenment_advance（4）、industrialization_advance（3） |
| `depth` | 29 | 8 | 0（28）、1（1） |
| `starting_technology_level` | 25 | 6 | 1（8）、3（7）、2（6）、4（4） |
| `unlock_town_rights` | 14 | 3 | royal_artisan_rights（1）、royal_textile_rights（1）、scandinavian_tar_privileges（1）、scandinavian_bergslag_privileges（1）、scandinavian_thing_rights（1） |
| `unlock_cabinet_action` | 12 | 2 | reduce_inflation（1）、pop_promote_action（1）、study_institutions（1）、alleviate_concerns（1）、settle_the_frontier（1） |
| `unlock_estate_privilege` | 10 | 4 | cossacks_rada_autonomy（1）、cossacks_explorers（1）、cossacks_register（1）、cossacks_military_service（1）、cossacks_government_positions（1） |
| `unlock_policy` | 9 | 8 | aristocratic_court_policy（1）、tur_kanum_i_osmani（1）、statutes_of_lithuania_policy（1）、copper_coins（1）、hanseatic_coins_policy（1） |
| `unlock_chivalric_order` | 7 | 2 | society_lion_nassau（1）、society_furspang_franconia（1）、society_bengler_mainz（1）、society_with_the_donkey（1）、society_hubertus_cologne（1） |
| `unlock_heir_selection` | 6 | 4 | veche_selection（1）、matrilineal_salic_law（1）、semi_salic_law（1）、fratricide_succesion（1）、salic_law（1） |
| `unlock_road_type` | 4 | 1 | gravel_road（1）、modern_road（1）、paved_road（1）、railroad（1） |
| `pure_tooltip_entry` | 3 | 1 | improves_central_secretariat_bureaucracy_tt（1）、improves_grand_secretariat_bureaucracy_tt（1）、improves_imperial_censorate_bureaucracy_tt（1） |
| `unlock_diplomacy` | 2 | 2 |  |

### 二、取值白名单（本体出现过的值 + 次数）

- **`age`**（6 种）：age_2_renaissance（652）、age_1_traditions（612）、age_3_discovery（529）、age_4_reformation（522）、age_5_absolutism（457）、age_6_revolutions（406）
- **`content_priority`**（11 种）：300（46）、500（22）、1000（21）、1100（20）、700（19）、900（18）、600（17）、200（16）、100（16）、400（15）、800（14）
- **`for`**（3 种）：adm（50）、dip（50）、mil（50）
- **`government`**（5 种）：steppe_horde（22）、monarchy（17）、republic（16）、theocracy（12）、tribe（4）
- **`monthly_legitimacy`**（3 种）：0.1（59）、0.05（10）、0.15（1）
- **`monthly_prestige`**（6 种）：0.1（49）、0.05（11）、0.15（4）、0.10（2）、0.2（1）、0.20（1）
- **`land_morale_modifier`**（6 种）：0.1（43）、0.15（9）、0.05（7）、0.10（6）、0.2（1）、0.20（1）
- **`cultural_tradition_modifier`**（7 种）：0.1（30）、0.2（16）、0.10（6）、0.15（3）、0.33（3）、0.20（3）、0.05（1）
- **`tolerance_own`**（3 种）：1（46）、0.5（8）、2（6）
- **`cultural_influence_modifier`**（7 种）：0.1（33）、0.25（7）、0.15（7）、0.10（4）、0.20（2）、0.05（1）、0.2（1）
- **`global_max_literacy`**（4 种）：5（30）、10（22）、3（2）、2.5（1）
- **`diplomatic_reputation`**（5 种）：diplomatic_reputation_mild_bonus（32）、diplomatic_reputation_weak_bonus（17）、2（2）、1（2）、diplomatic_reputation_severe_bonus（1）
- **`tax_income_efficiency`**（4 种）：medium_tax_income_efficiency_bonus（22）、small_tax_income_efficiency_bonus（17）、large_tax_income_efficiency_bonus（11）、tiny_tax_income_efficiency_bonus（1）
- **`global_defensive`**（6 种）：0.1（27）、0.2（13）、0.20（3）、0.25（2）、0.05（2）、0.15（1）
- **`global_monthly_food_modifier`**（7 种）：0.1（17）、0.2（10）、0.05（6）、0.10（5）、0.15（4）、0.20（2）、0.25（1）
- **`global_manpower_modifier`**（8 种）：0.1（25）、0.15（8）、0.125（4）、0.10（3）、0.075（2）、0.5（1）、0.05（1）、0.20（1）
- **`monthly_republican_tradition`**（2 种）：0.1（38）、0.05（6）
- **`research_speed_modifier`**（6 种）：0.05（23）、0.1（7）、0.2（6）、0.10（3）、0.025（2）、0.03（1）
- **`monthly_army_tradition`**（4 种）：0.05（27）、0.03（8）、0.1（6）、0.025（1）
- **`discipline`**（3 种）：0.05（39）、0.025（2）、0.03（1）
- …另有 489 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 20）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `monthly_legitimacy` | 70 | 61 | 0.1（59）、0.05（10）、0.15（1） |
| `monthly_prestige` | 68 | 54 | 0.1（49）、0.05（11）、0.15（4）、0.10（2）、0.2（1） |
| `land_morale_modifier` | 67 | 57 | 0.1（43）、0.15（9）、0.05（7）、0.10（6）、0.2（1） |
| `cultural_tradition_modifier` | 62 | 58 | 0.1（30）、0.2（16）、0.10（6）、0.15（3）、0.33（3） |
| `tolerance_own` | 60 | 54 | 1（46）、0.5（8）、2（6） |
| `global_max_literacy` | 55 | 40 | 5（30）、10（22）、3（2）、2.5（1） |
| `cultural_influence_modifier` | 55 | 47 | 0.1（33）、0.25（7）、0.15（7）、0.10（4）、0.20（2） |
| `diplomatic_reputation` | 54 | 45 | diplomatic_reputation_mild_bonus（32）、diplomatic_reputation_weak_bonus（17）、2（2）、1（2）、diplomatic_reputation_severe_bonus（1） |
| `tax_income_efficiency` | 51 | 40 | medium_tax_income_efficiency_bonus（22）、small_tax_income_efficiency_bonus（17）、large_tax_income_efficiency_bonus（11）、tiny_tax_income_efficiency_bonus（1） |
| `global_defensive` | 48 | 41 | 0.1（27）、0.2（13）、0.20（3）、0.25（2）、0.05（2） |
| `global_monthly_food_modifier` | 45 | 39 | 0.1（17）、0.2（10）、0.05（6）、0.10（5）、0.15（4） |
| `global_manpower_modifier` | 45 | 40 | 0.1（25）、0.15（8）、0.125（4）、0.10（3）、0.075（2） |
| `monthly_republican_tradition` | 44 | 38 | 0.1（38）、0.05（6） |
| `research_speed_modifier` | 42 | 38 | 0.05（23）、0.1（7）、0.2（6）、0.10（3）、0.025（2） |
| `monthly_army_tradition` | 42 | 36 | 0.05（27）、0.03（8）、0.1（6）、0.025（1） |
| `discipline` | 42 | 41 | 0.05（39）、0.025（2）、0.03（1） |
| `monthly_devotion` | 40 | 34 | 0.1（36）、0.05（4） |
| `country_cabinet_efficiency` | 39 | 35 | 0.1（16）、0.05（12）、0.10（8）、0.20（2）、0.15（1） |
| `diplomatic_capacity` | 39 | 37 | 1（38）、2（1） |
| `global_crown_estate_power` | 38 | 37 | 0.1（20）、0.10（7）、0.25（6）、0.50（2）、0.05（1） |
| … | 另有 10 个修正名 | | |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `potential_trigger` | 0 次（214 档） | **有**（写在别的类目） |
| `scale` | 0 次（214 档） | **有**（写在别的类目） |
| `unlock_relation_type` | 0 次（214 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_weight` | 149 | 79 |
| `ai_preference_tags` | 29 | 7 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `has_or_had_tag` | 1613 | AND、potential、OR、NOT |
| `culture` | 770 | OR、potential、NOR、AND |
| `OR` | 548 | AND、allow、market、any_owned_location |
| `religion` | 156 | OR、potential、NOR |
| `original_capital.region` | 74 | OR |
| `sub_continent` | 67 | OR、potential |
| `religion.group` | 58 | OR、potential |
| `NOT` | 44 | settle_the_frontier_advance、OR、potential、allow |
| `region` | 28 | OR |
| `original_tag` | 24 | potential |
| `exists` | 18 | potential |
| `original_capital.area` | 18 | NOR |
| `is_basque_spain` | 18 | OR |
| `add` | 17 | ai_weight、if |
| `AND` | 16 | OR、potential |
