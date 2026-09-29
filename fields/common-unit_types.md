# common/unit_types（单位类型）

> **一句话**：单位类型的字段：时代、建造、升级路径、征召与强攻轰击开关，以及地形战斗与移动修正。
> **什么时候看**：加兵种、调地形加成，或要设置单位数量上限与招募门槛时翻这篇。
> **体量**：142 行 · 约 7 分钟通读

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

> **数据源**：`in_game\common\unit_types\` 全量 **30 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `is_special` | 164 | 23 | yes（164） |
| `strength_damage_taken` | 71 | 14 | -0.10（21）、-0.1（14）、-0.25（13）、-0.05（10）、0.1（5） |
| `movement_speed` | 66 | 18 | 1（8）、-0.25（7）、1.0（4）、4（4）、5（3） |
| `initiative` | 58 | 10 | 2（31）、-0.5（8）、1（7）、3（6）、5（2） |
| `cannons` | 51 | 9 | 2（5）、4（4）、12（4）、10（4）、1（4） |
| `hull_size` | 48 | 9 | 2（7）、20（4）、8（4）、10（4）、6（3） |
| `max_strength` | 45 | 3 | 0.1（9）、1.0（5）、0.2（4）、0.4（4）、0.8（3） |
| `morale_damage_taken` | 44 | 10 | -0.10（16）、-0.25（12）、-0.1（8）、-0.2（3）、-0.15（2） |
| `crew_size` | 44 | 9 | 0.010（5）、0.100（3）、0.050（3）、0.03（3）、0.025（2） |
| `morale_damage_done` | 43 | 11 | 0.2（16）、0.1（11）、0.33（6）、0.25（4）、0.15（2） |
| `hide` | 40 | 1 | yes（40） |
| `combat_power` | 38 | 3 | 4（13）、0.25（7）、1（6）、3（3）、1.5（2） |
| `maritime_presence` | 38 | 8 | ship_neglible_maritime（5）、ship_very_small_maritime（5）、ship_tiny_maritime（3）、ship_small_high_maritime（3）、ship_small_mid_maritime（3） |
| `strength_damage_done` | 31 | 9 | 0.2（12）、0.1（11）、-0.1（5）、0.10（1）、-0.15（1） |
| `blockade_capacity` | 25 | 4 | 0.1（5）、0.5（3）、1.0（2）、3.0（2）、0.2（2） |
| `combat_speed` | 22 | 6 | -1（6）、1（5）、-0.05（2）、0.05（2）、3（1） |
| `build_time_modifier` | 21 | 7 | 0.50（8）、0.5（7）、0.1（2）、0.33（1）、0.25（1） |
| `upgrades_to_only` | 19 | 3 | a_byzantine_cataphracts_5（1）、a_varangians_6（1）、a_byzantine_cataphracts_4（1）、a_legionaries_6（1）、a_byzantine_cataphracts_2（1） |
| `transport_capacity` | 12 | 3 | 0.25（2）、0.030（1）、0.5（1）、1.25（1）、0.10（1） |
| `food_storage_per_strength` | 9 | 3 | 10（2）、960（1）、60（1）、240（1）、120（1） |
| `bombard_efficiency` | 8 | 3 | 0.1（3）、0.125（1）、0.30（1）、0.25（1）、0.15（1） |
| `attrition_loss` | 7 | 2 | -0.50（6）、-0.25（1） |
| `artillery_barrage` | 7 | 2 | 1（2）、7（1）、3（1）、5（1）、4（1） |
| `supply_weight` | 5 | 3 | -0.25（4）、0.5（1） |
| `flanking_ability` | 4 | 4 | 0.25（2）、-0.1（1）、0.2（1） |
| `empty` | 4 | 1 | yes（4） |
| `content_priority` | 2 | 1 | 500（1）、400（1） |
| `secure_flanks_defense` | 1 | 1 | 0.025（1） |
| `frontage` | 1 | 1 | -0.25（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`buildable`**（2 种）：no（114）、yes（92）
- **`is_special`**（1 种）：yes（164）
- **`category`**（10 种）：navy_galley（29）、navy_light_ship（20）、navy_heavy_ship（16）、navy_transport（16）、army_auxiliary（6）、army_artillery（6）、army_heavy_infantry（1）、army_heavy_cavalry（1）、army_light_cavalry（1）、army_light_infantry（1）
- **`age`**（6 种）："age_1_traditions"（17）、"age_3_discovery"（16）、"age_6_revolutions"（16）、"age_2_renaissance"（14）、"age_5_absolutism"（14）、"age_4_reformation"（14）
- **`strength_damage_taken`**（11 种）：-0.10（21）、-0.1（14）、-0.25（13）、-0.05（10）、0.1（5）、0.10（2）、-0.15（2）、-0.125（1）、-0.5（1）、0.2（1）、-0.20（1）
- **`initiative`**（8 种）：2（31）、-0.5（8）、1（7）、3（6）、5（2）、4（2）、-4（1）、0.1（1）
- **`morale_damage_taken`**（7 种）：-0.10（16）、-0.25（12）、-0.1（8）、-0.2（3）、-0.15（2）、-0.05（2）、0.2（1）
- **`levy`**（1 种）：yes（44）
- **`morale_damage_done`**（8 种）：0.2（16）、0.1（11）、0.33（6）、0.25（4）、0.15（2）、0.05（2）、0.20（1）、0.10（1）
- **`hide`**（1 种）：yes（40）
- **`combat_power`**（10 种）：4（13）、0.25（7）、1（6）、3（3）、1.5（2）、2（2）、2.25（2）、5.5（1）、5（1）、7.5（1）
- **`limit`**（7 种）：legionaries_unit_limit（6）、cataphracts_unit_limit（6）、varangians_unit_limit（6）、janissary_unit_limit（5）、qizilbash_unit_limit（2）、ghilman_unit_limit（1）、wagenburg_unit_limit（1）
- **`strength_damage_done`**（6 种）：0.2（12）、0.1（11）、-0.1（5）、0.10（1）、-0.15（1）、0.15（1）
- **`use_ship_names`**（2 种）：yes（26）、no（1）
- **`blockade_capacity`**（14 种）：0.1（5）、0.5（3）、1.0（2）、3.0（2）、0.2（2）、1.5（2）、0.25（2）、0.3（1）、0.6（1）、2.0（1）、0.4（1）、0.75（1）、1.25（1）、2.5（1）
- **`combat_speed`**（11 种）：-1（6）、1（5）、-0.05（2）、0.05（2）、3（1）、0.5（1）、-0.5（1）、-1.5（1）、-0.25（1）、0.25（1）、0.1（1）
- **`build_time_modifier`**（7 种）：0.50（8）、0.5（7）、0.1（2）、0.33（1）、0.25（1）、0.2（1）、0.20（1）
- **`transport_capacity`**（11 种）：0.25（2）、0.030（1）、0.5（1）、1.25（1）、0.10（1）、0.010（1）、1.0（1）、0.75（1）、0.05（1）、1.5（1）、0.020（1）
- **`food_storage_per_strength`**（8 种）：10（2）、960（1）、60（1）、240（1）、120（1）、30（1）、480（1）、1500（1）
- **`bombard_efficiency`**（6 种）：0.1（3）、0.125（1）、0.30（1）、0.25（1）、0.15（1）、0.20（1）
- …另有 10 个枚举字段，见完整普查报告

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `assault` | 0 次（30 档） | **有**（写在别的类目） |
| `auxiliary` | 0 次（30 档） | **有**（写在别的类目） |
| `bombard` | 0 次（30 档） | **有**（写在别的类目） |
| `build_time` | 0 次（30 档） | **有**（写在别的类目） |
| `modifiers` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `multiply` | 102 | limit、mercenaries_per_location、multiply |
| `pop_type` | 99 | mercenaries_per_location |
| `mountains` | 29 | combat、impact |
| `wetlands` | 22 | combat、impact |
| `hills` | 22 | combat、impact |
| `OR` | 21 | any_pop、location_potential、country_potential、location_trigger |
| `grasslands` | 15 | combat |
| `plateau` | 15 | combat、impact |
| `has_dlc` | 15 | country_potential |
| `value` | 14 | value、add、limit、multiply |
| `has_or_had_tag` | 13 | country_potential |
| `coastal` | 12 | combat |
| `culture` | 11 | OR、country_potential |
| `dominant_culture` | 11 | location_potential、OR、location_trigger |
| `has_societal_value` | 11 | country_potential |

| `desc` | 11 | value、multiply、add、min |
