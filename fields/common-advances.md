# common/advances（科技/进步）

> **一句话**：讲 advances 科技节点的块格式：时代与前置要求、可研究条件、各类解锁引用，以及研究期间与完成后的修正。
> **什么时候看**：写或审查科技节点、给节点挂解锁项与修正时翻这篇。
> **体量**：164 行 · 约 8 分钟通读

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

> **数据源**：`in_game\common\advances\` 全量 **226 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `research_cost` | 58 | 3 | -0.75（23）、-0.5（16）、2.0（14）、2.5（3）、1.0（2） |
| `in_tree_of` | 33 | 4 | global_trade_advance（7）、new_world_advance（5）、scientific_revolution_advance（4）、enlightenment_advance（4）、artillery_institution_advance（3） |
| `depth` | 29 | 8 | 0（28）、1（1） |
| `starting_technology_level` | 25 | 6 | 1（8）、3（7）、2（6）、4（4） |
| `unlock_town_rights` | 25 | 4 | flemish_cloth_industries_right（1）、textile_charter（1）、naval_charter（1）、royal_jewelry_rights（1）、royal_naval_rights（1） |
| `unlock_cabinet_action` | 13 | 2 | send_people_to_the_colonies（1）、counter_espionage_action（1）、settle_the_frontier（1）、merge_culture_group（1）、reduce_inflation（1） |
| `unlock_estate_privilege` | 10 | 4 | cossacks_colonization（1）、cossacks_rada_autonomy（1）、jaysh_armies（1）、cossacks_register（1）、cossacks_explorers（1） |
| `unlock_policy` | 9 | 8 | couteume_de_normaundie（1）、tur_kanum_i_osmani（1）、humanist_court_policy（1）、hanseatic_coins_policy（1）、custom_union_of_the_rhine_policy（1） |
| `unlock_chivalric_order` | 8 | 3 | society_lion_nassau（1）、society_elephant_tyrol（1）、order_of_lebrel_blanco（1）、society_hubertus_cologne（1）、order_of_the_golden_fleece（1） |
| `unlock_heir_selection` | 6 | 4 | matrilineal_salic_law（1）、veche_selection（1）、semi_salic_law（1）、partition_inheritance（1）、salic_law（1） |
| `unlock_road_type` | 4 | 1 | gravel_road（1）、modern_road（1）、railroad（1）、paved_road（1） |
| `unlock_diplomacy` | 2 | 2 |  |

### 二、取值白名单（本体出现过的值 + 次数）

- **`age`**（6 种）：age_2_renaissance（712）、age_1_traditions（675）、age_3_discovery（596）、age_4_reformation（584）、age_5_absolutism（523）、age_6_revolutions（469）
- **`content_priority`**（11 种）：300（46）、500（22）、1000（22）、700（21）、900（20）、1100（20）、600（18）、100（16）、200（16）、800（15）、400（15）
- **`for`**（3 种）：mil（50）、adm（50）、dip（50）
- **`monthly_legitimacy`**（3 种）：0.1（62）、0.05（29）、0.15（1）
- **`monthly_prestige`**（6 种）：0.1（67）、0.05（12）、0.15（4）、0.10（2）、0.20（1）、0.2（1）
- **`tolerance_own`**（3 种）：1（60）、0.5（8）、2（6）
- **`cultural_influence_modifier`**（7 种）：0.1（49）、0.15（8）、0.25（7）、0.10（4）、0.20（2）、0.2（1）、0.05（1）
- **`government`**（5 种）：steppe_horde（22）、monarchy（17）、republic（16）、theocracy（12）、tribe（4）
- **`tax_income_efficiency`**（4 种）：medium_tax_income_efficiency_bonus（30）、small_tax_income_efficiency_bonus（27）、large_tax_income_efficiency_bonus（11）、tiny_tax_income_efficiency_bonus（1）
- **`global_max_literacy`**（4 种）：5（41）、10（23）、3（2）、2.5（1）
- **`land_morale_modifier`**（6 种）：0.1（43）、0.15（9）、0.05（7）、0.10（6）、0.20（1）、0.2（1）
- **`monthly_republican_tradition`**（2 种）：0.1（41）、0.05（25）
- **`diplomatic_reputation`**（5 种）：diplomatic_reputation_mild_bonus（35）、diplomatic_reputation_weak_bonus（26）、2（2）、1（1）、diplomatic_reputation_severe_bonus（1）
- **`monthly_devotion`**（2 种）：0.1（40）、0.05（23）
- **`country_cabinet_efficiency`**（5 种）：0.1（38）、0.05（12）、0.10（9）、0.20（2）、0.15（2）
- **`cultural_tradition_modifier`**（7 种）：0.1（31）、0.2（16）、0.10（6）、0.20（3）、0.33（3）、0.15（2）、0.05（1）
- **`allow_children`**（1 种）：no（61）
- **`research_cost`**（5 种）：-0.75（23）、-0.5（16）、2.0（14）、2.5（3）、1.0（2）
- **`global_defensive`**（5 种）：0.1（37）、0.2（13）、0.20（3）、0.05（2）、0.25（2）
- **`global_manpower_modifier`**（8 种）：0.1（33）、0.15（8）、0.125（4）、0.10（3）、0.075（2）、0.5（1）、0.05（1）、0.20（1）
- …另有 504 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 20）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `monthly_legitimacy` | 92 | 70 | 0.1（62）、0.05（29）、0.15（1） |
| `monthly_prestige` | 87 | 62 | 0.1（67）、0.05（12）、0.15（4）、0.10（2）、0.20（1） |
| `tolerance_own` | 74 | 62 | 1（60）、0.5（8）、2（6） |
| `cultural_influence_modifier` | 72 | 59 | 0.1（49）、0.15（8）、0.25（7）、0.10（4）、0.20（2） |
| `tax_income_efficiency` | 69 | 51 | medium_tax_income_efficiency_bonus（30）、small_tax_income_efficiency_bonus（27）、large_tax_income_efficiency_bonus（11）、tiny_tax_income_efficiency_bonus（1） |
| `land_morale_modifier` | 67 | 57 | 0.1（43）、0.15（9）、0.05（7）、0.10（6）、0.20（1） |
| `global_max_literacy` | 67 | 49 | 5（41）、10（23）、3（2）、2.5（1） |
| `monthly_republican_tradition` | 66 | 47 | 0.1（41）、0.05（25） |
| `diplomatic_reputation` | 65 | 52 | diplomatic_reputation_mild_bonus（35）、diplomatic_reputation_weak_bonus（26）、2（2）、1（1）、diplomatic_reputation_severe_bonus（1） |
| `monthly_devotion` | 63 | 44 | 0.1（40）、0.05（23） |
| `country_cabinet_efficiency` | 63 | 47 | 0.1（38）、0.05（12）、0.10（9）、0.20（2）、0.15（2） |
| `cultural_tradition_modifier` | 62 | 59 | 0.1（31）、0.2（16）、0.10（6）、0.20（3）、0.33（3） |
| `global_defensive` | 57 | 48 | 0.1（37）、0.2（13）、0.20（3）、0.05（2）、0.25（2） |
| `global_manpower_modifier` | 53 | 46 | 0.1（33）、0.15（8）、0.125（4）、0.10（3）、0.075（2） |
| `monthly_army_tradition` | 51 | 40 | 0.05（35）、0.03（8）、0.1（7）、0.025（1） |
| `global_monthly_food_modifier` | 48 | 42 | monthly_food_productivity_mild_bonus（23）、monthly_food_productivity_extreme_bonus（14）、monthly_food_productivity_weak_bonus（6）、monthly_food_productivity_severe_bonus（4）、monthly_food_productivity_ultimate_bonus（1） |
| `improve_relation_impact` | 47 | 34 | 0.1（32）、0.2（8）、0.10（3）、0.33（2）、0.3（1） |
| `global_pop_conversion_speed_modifier` | 47 | 35 | 0.1（39）、0.2（4）、0.10（2）、0.05（1）、0.15（1） |
| `discipline` | 47 | 46 | 0.05（45）、0.03（1）、0.025（1） |
| `research_speed_modifier` | 44 | 39 | 0.05（24）、0.1（9）、0.2（6）、0.10（3）、0.025（1） |
| … | 另有 12 个修正名 | | |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `potential_trigger` | 0 次（226 档） | **有**（出现在 9 个类目） |
| `scale` | 0 次（226 档） | **有**（出现在 11 个类目） |
| `unlock_relation_type` | 0 次（226 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_weight` | 159 | 80 |
| `ai_preference_tags` | 35 | 8 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `has_or_had_tag` | 1893 | OR、NOR、AND、NOT |
| `culture` | 914 | NOR、potential、OR、AND |
| `OR` | 613 | culture、any_market_present_in_country、limit、allow |
| `religion` | 172 | NOR、potential、OR |
| `sub_continent` | 85 | potential、OR |
| `original_capital.region` | 74 | OR |
| `original_tag` | 73 | potential |
| `religion.group` | 72 | potential、OR |
| `NOT` | 67 | allow、potential、OR、settle_the_frontier_advance |
| `region` | 52 | OR |
| `add` | 22 | ai_weight、if |
| `limit` | 20 | if |
| `if` | 20 | ai_weight |
| `exists` | 18 | potential |
| `original_capital.area` | 18 | NOR |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `content_priority` | 231 | 9 |
| `pure_tooltip_entry` | 6 | 7 |
