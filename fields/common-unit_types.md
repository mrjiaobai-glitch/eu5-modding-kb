# common/unit_types（单位类型）

> **一句话**：单位类型的字段：时代、建造、升级路径、征召与强攻轰击开关，以及地形战斗与移动修正。
> **什么时候看**：加兵种、调地形加成，或要设置单位数量上限与招募门槛时翻这篇。
> **体量**：156 行 · 约 8 分钟通读

来源：`in_game\common\unit_types\readme.txt`

## 字段

```
<unit_type_id> = {
    age = <age>                          # 启用时代
    build_time = <int>
    upgrades_to = <another unit type>    # 可选；升级路径
    buildable = <yes/no>
    levy = <yes/no>                      # 能否用于征召
    default = <...>                      # readme 未说明取值
    construction_demand = <goods demand>
    maintenance_demand = <goods demand>
    use_ship_names = <yes/no>
    assault = <yes/no>                   # 能否强攻要塞
    bombard = <yes/no>                   # 能否轰击要塞
    auxiliary = <yes/no>                 # 视为辅助单位
    category = <unit_category>           # 所属类别
    location_trigger = { <location_triggers> }   # location 满足才可招募
    location_potential = { <location_triggers> } # location 满足才显示
    country_potential = { <country_triggers> }   # 国家满足才显示
    mercenaries_per_location = { pop_type = <pop type> multiply = <proportion> }
    limit = { <value calculations> } / <default_value>  # 该类型单位数量上限
    combat = { <topography/vegetation/climate/coastal/inland/river> }  # 特定地形伤害修正
    impact = { <topography/vegetation/climate/coastal/inland/river> }  # 特定地形移动速度修正
    copy_from = <unit_type>              # 复制模板全部数值，可后续再改
    gfx_tags = {}
    color = <color>                      # 覆盖视觉主色
    <combat modifiers>                   # 同 unit_categories 的三组修正
}
```

## 审查要点

- `category` 引用须在 common/unit_categories 存在；`copy_from`/`upgrades_to` 引用须存在。
- `combat`/`impact` 的键是地形枚举（topography/vegetation/climate/coastal/inland/river）。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\unit_types\` 全量 **31 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `is_special` | 206 | 24 | yes（206） |
| `movement_speed` | 85 | 19 | 0.20（13）、1（8）、-0.25（7）、1.0（6）、4（4） |
| `strength_damage_taken` | 75 | 14 | -0.10（22）、-0.1（14）、-0.05（13）、-0.25（12）、0.1（5） |
| `initiative` | 71 | 11 | 2（36）、-0.5（8）、1（8）、5（8）、3（7） |
| `morale_damage_done` | 60 | 12 | 0.2（16）、0.05（14）、0.1（12）、0.33（6）、0.10（5） |
| `hull_size` | 51 | 9 | 2（8）、20（4）、8（4）、10（4）、-2（3） |
| `max_strength` | 46 | 3 | 0.1（10）、1.0（5）、0.2（4）、0.4（4）、1.2（3） |
| `crew_size` | 46 | 9 | 0.010（6）、0.03（3）、0.100（3）、-0.010（3）、0.050（3） |
| `morale_damage_taken` | 44 | 10 | -0.10（16）、-0.25（12）、-0.1（8）、-0.2（3）、-0.15（2） |
| `maritime_presence` | 41 | 8 | ship_neglible_maritime（5）、ship_very_small_maritime（5）、ship_small_maritime（4）、ship_tiny_maritime（4）、ship_small_high_maritime（3） |
| `hide` | 40 | 1 | yes（40） |
| `combat_power` | 38 | 3 | 0.25（7）、2.6（6）、1（6）、3.25（4）、3（3） |
| `strength_damage_done` | 34 | 9 | 0.2（12）、0.1（12）、-0.1（5）、0.10（2）、0.05（1） |
| `family` | 31 | 5 | cawa（6）、varangians（6）、byzantine_cataphracts（6）、legionaries（6）、janissaries（5） |
| `blockade_capacity` | 26 | 4 | 0.1（6）、0.5（3）、0.2（2）、1.5（2）、3.0（2） |
| `combat_speed` | 23 | 6 | -1（6）、1（5）、0.05（2）、-0.05（2）、0.25（2） |
| `build_time_modifier` | 23 | 7 | 0.50（8）、0.5（7）、0.33（3）、0.1（2）、0.20（1） |
| `upgrades_to_only` | 20 | 3 | a_varangians_3（1）、a_legionaries_4（1）、a_reformation_janissaries（1）、a_byzantine_cataphracts_6（1）、a_byzantine_cataphracts_2（1） |
| `transport_capacity` | 13 | 3 | 0.010（2）、0.25（2）、0.05（1）、0.10（1）、0.030（1） |
| `use_religious_order_coa` | 12 | 3 | yes（12） |
| `food_storage_per_strength` | 9 | 3 | 10（2）、60（1）、1500（1）、120（1）、960（1） |
| `bombard_efficiency` | 8 | 3 | 0.1（3）、0.30（1）、0.20（1）、0.125（1）、0.15（1） |
| `artillery_barrage` | 7 | 2 | 1（2）、3（1）、9（1）、4（1）、5（1） |
| `attrition_loss` | 7 | 2 | -0.50（6）、-0.25（1） |
| `supply_weight` | 7 | 4 | -0.25（5）、-0.15（1）、0.5（1） |
| `flanking_ability` | 6 | 5 | 0.25（2）、0.2（2）、-0.1（1）、0.3（1） |
| `empty` | 4 | 1 | yes（4） |
| `secure_flanks_defense` | 1 | 1 | 0.025（1） |
| `frontage` | 1 | 1 | -0.25（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`buildable`**（2 种）：no（133）、yes（114）
- **`is_special`**（1 种）：yes（206）
- **`category`**（10 种）：navy_galley（32）、navy_light_ship（20）、navy_transport（16）、navy_heavy_ship（16）、army_light_cavalry（14）、army_heavy_cavalry（13）、army_artillery（6）、army_auxiliary（6）、army_heavy_infantry（1）、army_light_infantry（1）
- **`age`**（6 种）："age_1_traditions"（18）、"age_6_revolutions"（16）、"age_3_discovery"（16）、"age_5_absolutism"（14）、"age_4_reformation"（14）、"age_2_renaissance"（14）
- **`strength_damage_taken`**（12 种）：-0.10（22）、-0.1（14）、-0.05（13）、-0.25（12）、0.1（5）、0.10（2）、-0.15（2）、0.2（1）、-0.20（1）、-0.5（1）、-0.2（1）、-0.125（1）
- **`initiative`**（8 种）：2（36）、-0.5（8）、1（8）、5（8）、3（7）、4（2）、-4（1）、0.1（1）
- **`levy`**（1 种）：yes（61）
- **`morale_damage_done`**（8 种）：0.2（16）、0.05（14）、0.1（12）、0.33（6）、0.10（5）、0.25（4）、0.15（2）、0.20（1）
- **`morale_damage_taken`**（7 种）：-0.10（16）、-0.25（12）、-0.1（8）、-0.2（3）、-0.15（2）、-0.05（2）、0.2（1）
- **`hide`**（1 种）：yes（40）
- **`combat_power`**（13 种）：0.25（7）、2.6（6）、1（6）、3.25（4）、3（3）、1.5（2）、2.25（2）、2（2）、2.85（2）、4（1）、5.5（1）、7.5（1）、5（1）
- **`strength_damage_done`**（7 种）：0.2（12）、0.1（12）、-0.1（5）、0.10（2）、0.05（1）、0.15（1）、-0.15（1）
- **`family`**（6 种）：cawa（6）、varangians（6）、byzantine_cataphracts（6）、legionaries（6）、janissaries（5）、qizilbash（2）
- **`use_ship_names`**（2 种）：yes（27）、no（1）
- **`blockade_capacity`**（14 种）：0.1（6）、0.5（3）、0.2（2）、1.5（2）、3.0（2）、0.25（2）、1.0（2）、2.0（1）、2.5（1）、0.75（1）、0.6（1）、0.3（1）、0.4（1）、1.25（1）
- **`combat_speed`**（11 种）：-1（6）、1（5）、0.05（2）、-0.05（2）、0.25（2）、3（1）、-0.5（1）、-1.5（1）、0.1（1）、0.5（1）、-0.25（1）
- **`build_time_modifier`**（7 种）：0.50（8）、0.5（7）、0.33（3）、0.1（2）、0.20（1）、0.2（1）、0.25（1）
- **`transport_capacity`**（11 种）：0.010（2）、0.25（2）、0.05（1）、0.10（1）、0.030（1）、1.5（1）、0.020（1）、0.5（1）、1.25（1）、0.75（1）、1.0（1）
- **`use_religious_order_coa`**（1 种）：yes（12）
- **`food_storage_per_strength`**（8 种）：10（2）、60（1）、1500（1）、120（1）、960（1）、480（1）、30（1）、240（1）
- …另有 12 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 2）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `cannons` | 54 | 5 |
| `content_priority` | 2 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `assault` | 0 次（31 档） | **有**（出现在 1 个类目） |
| `auxiliary` | 0 次（31 档） | **有**（出现在 1 个类目） |
| `bombard` | 0 次（31 档） | **有**（出现在 1 个类目） |
| `build_time` | 0 次（31 档） | **有**（出现在 4 个类目） |
| `force_culture` | 0 次（31 档） | 全库也没有 → 疑似废弃字段 |
| `force_culture_gfx` | 0 次（31 档） | 全库也没有 → 疑似废弃字段 |
| `force_religion` | 0 次（31 档） | 全库也没有 → 疑似废弃字段 |
| `modifiers` | 0 次（31 档） | 全库也没有 → 疑似废弃字段 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `multiply` | 118 | multiply、limit、mercenaries_per_location |
| `pop_type` | 115 | mercenaries_per_location |
| `mountains` | 29 | impact、combat |
| `desert` | 27 | impact、combat |
| `arid` | 26 | impact、combat |
| `OR` | 25 | any_pop、location_potential、country_potential、location_trigger |
| `jungle` | 23 | impact、combat |
| `wetlands` | 23 | impact、combat |
| `hills` | 23 | impact、combat |
| `culture` | 22 | OR、country_potential |
| `has_or_had_tag` | 18 | OR、country_potential |
| `grasslands` | 17 | combat |
| `has_dlc` | 17 | country_potential |
| `plateau` | 15 | impact、combat |
| `arctic` | 14 | impact |

### 六、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `cannons` | 54 | 5 |
| `content_priority` | 2 | 9 |
