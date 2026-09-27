# common/unit_abilities、unit_categories、unit_formation_preference（单位能力/类别）

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
