# common/unit_abilities、unit_categories、unit_formation_preference（单位能力/类别）

> **一句话**：单位能力、单位类别与阵型偏好三件套的字段，含通用、陆战与海战三组战斗修正清单与作用域。
> **什么时候看**：写单位能力或单位类别、要调战斗与后勤修正时翻这篇。
> **体量**：180 行 · 约 9 分钟通读

覆盖 readme：`in_game\common\unit_abilities\readme.txt`、`in_game\common\unit_categories\readme.txt`、`in_game\common\unit_formation_preference\readme.txt`

## unit_abilities（单位能力）

```
<ability_id> = {
    hidden = <trigger>               # 是否隐藏（unit 作用域）
    allow = <trigger>                # 是否启用（unit 作用域）
    finished_when = <trigger>        # 单位行动何时完成（unit 作用域）
    ai_will_revoke = <trigger>       # unit 作用域
    ai_allow_plan_slowdown = <yes/no>
    duration = <days>                # 持续时间（天）
    toggle = <yes/no>                # 能否开关
    soundeffect = <sound effect>
    army_only = <yes/no> / navy_only = <yes/no>
    cancel_on_combat = <yes/no> / cancel_on_combat_end = <yes/no>
    map = <yes/no>                   # 是否显示在地图上
    start_effect / finish_effect / on_entering_location = <effect>  # unit 作用域
    ai_will_do = <script>            # AI 使用概率
    modifier = <modifier>            # 施加给单位
    idle_entity_state / move_entity_state / available_states = <动画>
    confirm = <yes/no>               # 是否需要确认
    block_reorg = <yes/no>           # 激活时阻止重组
}
```

## unit_categories（单位类别）

```
<unit_category_id> = {
    fallback = <unit_category_id>    # 可选；回退到 1.2 前值（插图等仍用）
    startup_amount = <int>
    build_time = <int>
    assault = <yes/no> / bombard = <yes/no>
    auxiliary = <yes/no>             # 默认 no
    is_garrison = <yes/no>           # 默认 no；**有且仅有一个 army 类别应设 yes**
    construction_demand = <goods demands>
    maintenance_demand = <goods demands>
    <combat modifiers>               # 见下
    is_army = <yes/no>
}
```

- 通用修正：morale_damage_taken、strength_damage_taken、morale_damage_done、strength_damage_done、supply_weight、attrition_loss、food_storage_per_strength、food_consumption_per_strength、movement_speed
- 陆战修正：max_strength、combat_speed、initiative、frontage、combat_power、flanking_ability、secure_flanks_defense
- 海战修正：transport_capacity、maritime_presence、crew_size、blockade_capacity、cannons、hull_size、anti_piracy_warfare

## unit_formation_preference

```
left / center / right = {
}
```

- 该 readme 仅给出块骨架，无字段说明。

## 审查要点

- `is_garrison = yes` 全局只能有一个 army 类别（多设是错误）。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\unit\` 全量 **29 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `initiative` | 10 | 10 | 1（5）、5（2）、2（2）、3（1） |
| `flanking_ability` | 10 | 10 | 1.0（6）、1.6（2）、1.2（1）、1.1（1） |
| `frontage` | 10 | 10 | 1（8）、1.5（1）、0.5（1） |
| `ai_weight` | 10 | 10 | 0.5（5）、0.25（2）、0.2（2）、0.4（1） |
| `combat_speed` | 10 | 10 | 1（3）、2（2）、5（2）、3（1）、0.5（1） |
| `max_strength` | 10 | 10 | 0（6）、1.0（4） |
| `food_storage_per_strength` | 9 | 9 | 5（3）、10（2）、1.0（1）、0.5（1）、3000（1） |
| `damage_taken` | 6 | 6 | 1.0（2）、1.25（2）、0.30（2） |
| `secure_flanks_defense` | 6 | 6 | 0（4）、0.05（2） |
| `movement_speed` | 6 | 6 | 2.0（3）、2.5（2）、3.0（1） |
| `supply_weight` | 6 | 6 | 1.0（5）、0.2（1） |
| `food_consumption_per_strength` | 6 | 6 | 1.32（2）、0.33（2）、0.2（1）、0.66（1） |
| `anti_piracy_warfare` | 4 | 4 | 0.1（1）、1.0（1）、0.05（1）、0.2（1） |
| `transport_capacity` | 3 | 3 | 0.05（2）、0.1（1） |
| `attrition_loss` | 3 | 3 | 0.25（2）、0.5（1） |
| `army` | 3 | 2 | yes（2）、no（1） |
| `morale_damage_taken` | 2 | 2 | 0.10（2） |
| `exclude_from_combined_arms` | 2 | 2 | yes（2） |
| `strength_damage_done` | 2 | 2 | -0.1（2） |
| `transport` | 1 | 1 | yes（1） |
| `animation_gfx_override` | 1 | 1 | 1（1） |
| `cancel_on_move` | 1 | 1 | yes（1） |
| `blockade_capacity` | 1 | 1 | 0.01（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`toggle`**（2 种）：no（13）、yes（4）
- **`map`**（1 种）：yes（16）
- **`build_time`**（8 种）：standard_inf_recruitment_time（2）、standard_cav_recruitment_time（2）、galley_build_time（1）、transport_build_time（1）、standard_aux_recruitment_time（1）、standard_art_recruitment_time（1）、heavy_ship_build_time（1）、light_ship_build_time（1）
- **`ai_weight`**（4 种）：0.5（5）、0.25（2）、0.2（2）、0.4（1）
- **`maintenance_demand`**（8 种）：infantry_maintenance（2）、cavalry_maintenance（2）、heavy_ship_1_maintenance（1）、transport_1_maintenance（1）、light_ship_1_maintenance（1）、galley_1_maintenance（1）、artillery_maintenance（1）、auxuliary_maintenance（1）
- **`frontage`**（3 种）：1（8）、1.5（1）、0.5（1）
- **`max_strength`**（2 种）：0（6）、1.0（4）
- **`combat_speed`**（6 种）：1（3）、2（2）、5（2）、3（1）、0.5（1）、0.1（1）
- **`is_army`**（2 种）：yes（6）、no（4）
- **`initiative`**（4 种）：1（5）、5（2）、2（2）、3（1）
- **`flanking_ability`**（4 种）：1.0（6）、1.6（2）、1.2（1）、1.1（1）
- **`construction_demand`**（8 种）：cavalry_construction（2）、infantry_construction（2）、transport_1_construction（1）、auxuliary_construction（1）、artillery_construction（1）、heavy_ship_1_construction（1）、galley_1_construction（1）、light_ship_1_construction（1）
- **`army_only`**（1 种）：yes（10）
- **`food_storage_per_strength`**（6 种）：5（3）、10（2）、1.0（1）、0.5（1）、3000（1）、2.0（1）
- **`supply_weight`**（2 种）：1.0（5）、0.2（1）
- **`secure_flanks_defense`**（2 种）：0（4）、0.05（2）
- **`duration`**（2 种）：-1（4）、30（2）
- **`food_consumption_per_strength`**（4 种）：1.32（2）、0.33（2）、0.2（1）、0.66（1）
- **`damage_taken`**（3 种）：1.0（2）、1.25（2）、0.30（2）
- **`movement_speed`**（3 种）：2.0（3）、2.5（2）、3.0（1）
- …另有 22 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `default` | 2 | 8 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_allow_plan_slowdown` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `available_states` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `block_reorg` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `idle_entity_state` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `move_entity_state` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `on_entering_location` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `soundeffect` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `reserves` | 3 | 2 |
| `left` | 3 | 2 |
| `right` | 3 | 2 |
| `center` | 3 | 2 |
| `combat` | 2 | 2 |
| `highlight` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `owner` | 26 | allow、while、hidden、custom_tooltip |
| `not` | 20 | allow、hidden、province、OR |
| `limit` | 19 | trigger_if、while、every_location_in_province、ordered_pop |
| `is_exiled` | 18 | finished_when、allow、OR |
| `unit_location` | 17 | scope:ship_unit、start_effect、custom_tooltip、allow |
| `value` | 15 | add_gold、transfer_yearly_gold、add、ai_will_revoke |
| `OR` | 14 | finished_when、limit、hidden、allow |
| `in_combat` | 14 | allow、OR |
| `is_moving` | 12 | allow、OR |
| `add` | 11 | ai_will_do、change_variable、every_location_in_province、if |
| `custom_tooltip` | 10 | start_effect、allow、unit_location |
| `in_siege` | 10 | allow、OR |
| `max_frontage` | 9 | right、center、left |
| `text` | 8 | custom_tooltip |
| `is_army` | 7 | allow |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `default` | 2 | 8 |
