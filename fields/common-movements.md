# common/movements（思潮/运动）

来源：`in_game\common\movements\readme.txt`

## 字段

```
<movement_id> = {
    potential = <trigger>                 # 是否在 UI 出现（也因此可生成）；基本只用于 has_dlc 门槛（scope:movement_definition）
    allow = <trigger>                     # 失败则不尝试每月生成但仍显示在 UI；"世界是否准备好"检查
    monthly_spawn_chance = <script value> # 每月生成概率（0..1）
    spawn = <effect>                      # 须含 spawn_movement effect
    r0 = <script value>                   # root = location, scope:movement_definition
    environmental_infection = <script value>  # root = location
    calc_interval_days = <script value>   # 计算间隔；无作用域
    location_spread_threshold = <script value>  # (0..1) root = character's location
    on_spread_to_country = <event>        # root = country, scope:movement_definition
    on_calc_effect = <effect>             # 计算日调用；root = movement
    map_color / secondary_map_color = <script color>  # root = location
    custom_name = <customizable_localization 键>     # 根作用域 = movement
    # --- 受影响 pop 门槛（为空则忽略）---
    required_languages / required_language_families   # 两者都写时匹配其一即可
    required_tags                                # has_or_had_tag 逻辑的国家
    required_religions / required_religion_groups # 两者都写时匹配其一即可
    required_pop_types
    required_cultures
    # --- 传播对象 ---
    specific_pop_type_effect = { pop_type = <pop_type> multiplier = <float> }  # 默认不传播；可写 culture/religion/religion_group/language/language_family；multiplier = 1 表示全体正常
    location_modifier = <modifier>        # ×presence %
    religion = <religion>                 # 二选一（必须指定 religion 或 culture）
    culture = <culture>                   # 二选一
    # --- 传播乘数方向 ---
    development / literacy / local_control / pop_satisfaction = <neutral/positive/negative>
}
```

## 配套 modifier 命名约定

- `local_<tag>_resistance_modifier` / `national_<tag>_resistance_modifier` / `global_<tag>_resistance_modifier`
- `local_<tag>_growth_modifier` / `national_<tag>_growth_modifier` / `global_<tag>_growth_modifier`

## 传播规则（同 diseases）

- 目标无人口 / 两地主国禁运 / 目标已有 ≥50% presence / 目标停滞 → 不传播；目标：邻居、市场中心、市场中心贸易伙伴、所有者首都。

## 审查要点

- 必须指定 `religion` 或 `culture` 之一。
- `specific_pop_type_effect` 默认"不传播"——想全传播写 `multiplier = 1`。
- 未在 readme 中说明：本地化键格式。
