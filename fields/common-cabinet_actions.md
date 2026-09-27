# common/cabinet_actions（内阁行动）

来源：`in_game\common\cabinet_actions\readme.txt`

## 顶层字段

```
<cabinet_action_id> = {
    ability = <adm/dip/mil>                  # 使用的君主属性
    is_finished = <trigger>                  # 是否完成（root = country, scope:target = province）
    select_trigger = { ... }                 # 可多个；选择目标/参数，存入 scope:target, scope:target_1...
    allow_multiple = <yes/no>                # 是否允许多个同时进行
    societal_values = <amount>               # 社会价值观改变量
    potential = <trigger>                    # root = country
    allow = <trigger>                        # root = country
    years / months / weeks / days = <int>    # 完全生效时间；修正按比例缩放
    progress = <script value>                # 返回当前进度
    on_activate = <effect>                   # root = cabinet, scope:actor = country, scope:target...
    on_fully_activated = <effect>            # root = cabinet, scope:actor...
    on_deactivate = <effect>                 # root = cabinet, scope:actor...
    country_modifier / province_modifier / location_modifier = <scaled & triggered modifier>
    ai_will_do = <effect script>             # scope:actor = country, scope:recipient, scope:target...
}
```

## select_trigger 内部格式

```
{
    looking_for_a = <character/location/province/area/region/country/value/boolean 等>
    target_flag = <scope 名>          # 存入选中的名字（默认 target, target_1, target_2...）
    source = <actor/recipient/target/target_1/.../target_4>
    source_ai_override = <同上>        # 仅 AI
    source_flags = <性能优化选项>       # neighbor/possible_colonial_charters/include_dead/include_any_present/possible_exploration_areas/adjacent_locations/vacant_adjacent_locations/adjacent_provinces/border/border_or_recipients_capital_area/provinces_ai_wants_to_give_away/only_actual_locations
    source_flags_ai_override = <同上>
    source_global_list = <全局变量列表名>
    interaction_source_list = <effect>  # scope:actor = country, scope:recipient/target/...；用 add_to_list = source 填表
    ai_interaction_source_list = { ... }  # 仅 AI
    pre_evaluation_sort_value = <script value>  # 预筛排序（配合 number_to_evaluate_fully）
    pre_evaluation_number_to_evaluate_fully = <integer>  # 预筛后进入全量评估的数量（仅 AI）
    max_targets_for_ui = <integer>    # 交给 UI 供玩家选择的数量
    cache_targets / cache_interaction_source_list / cache_order = <yes/no>  # 性能缓存
    name = <本地化键>                  # 选择阶段的标题
    allow_null = <yes/no 或 trigger>
    allow_self = <yes/no>             # 仅对国家有效
    move_to_next_section_on_click = <yes/no>  # 默认 yes
    top_widget / bottom_widget = <gui widget 类型>
    column = { data = <column_id> width = <int> icon = <path> show_icon_in_cells = <yes/no> }
    default_sort = <sort key>         # 默认排序（键在 \common\attribute_columns\）
    none_available_msg_key = <loc 键>
    show_why_not_visible / show_why_not_enabled = <yes/no>
    show_if / visible / enabled / selected = <trigger>  # root = 被测对象, scope:actor/recipient/target...
    min / max / step / default = <script value>   # value 类型用
    map_mode = <map mode tag>
    map_color = <script color>        # root = location, scope:actor/recipient...
    only_color_selectable = <yes/no>
    secondary_map_color = <script color>
}
```

## 审查要点

- 所有修正 × `1 + (effective ability + cabinet efficiency) * CABINET_ACTION_SKILL_MODIFIER`——非字面值。
  ⚠️ **readme 写 0.05，本体 defines 写 0.005**（`loading_screen\common\defines\00_defines.txt:202`），**差 10 倍，以 defines 为准**。
- `select_trigger` 中 `target_flag` 自定义名字后，后续 effect 用该名字引用（scope:target_province 等）。
- `column` 可省略（默认见 \common\attribute_columns）。
- 未在 readme 中说明：本地化键格式。
