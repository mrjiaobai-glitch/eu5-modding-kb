# common/cabinet_actions（内阁行动）

> **一句话**：内阁行动的顶层字段与目标选择块格式：完成条件、进度、三类缩放修正、AI 评估与预筛，并附技能系数不一致的提示。
> **什么时候看**：做内阁行动、写目标选择块，或核对技能缩放系数时翻这篇。
> **体量**：181 行 · 约 9 分钟通读

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
  ⚠️ **readme 写 0.05，本体 defines 写 0.005**（`loading_screen\common\defines\00_defines.txt:215`），**差 10 倍，以 defines 为准**。
- `select_trigger` 中 `target_flag` 自定义名字后，后续 effect 用该名字引用（scope:target_province 等）。
- `column` 可省略（默认见 \common\attribute_columns）。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\cabinet_actions\` 全量 **73 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `require_matching_character_religion` | 3 | 2 | yes（3） |
| `forbid_for_automation` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`ability`**（3 种）：adm（36）、mil（22）、dip（18）
- **`allow_multiple`**（2 种）：no（21）、yes（18）
- **`icon`**（8 种）：pop_promote_action（2）、diplomatic_endeavors（2）、develop_province（1）、byz_extensive_conscription（1）、turmoil_in_brandenburg（1）、increase_control_province（1）、integrate_area（1）、taluqdar_tax_collection（1）
- **`years`**（3 种）：10（3）、2（2）、4（1）
- **`helps_with`**（5 种）："charter"（2）、"location"（1）、"charter|location"（1）、"subject"（1）、"location|subject"（1）
- **`require_matching_character_religion`**（1 种）：yes（3）
- **`min`**（1 种）：0（2）
- **`days`**（1 种）：1825（1）
- **`forbid_for_automation`**（1 种）：yes（1）
- **`societal_values`**（1 种）：0.1（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `icon` | 10 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_interaction_source_list` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `allow_null` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `allow_self` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `bottom_widget` | 0 次（73 档） | **有**（出现在 1 个类目） |
| `cache_interaction_source_list` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `cache_order` | 0 次（73 档） | **有**（出现在 1 个类目） |
| `cache_targets` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `column` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `default` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `default_sort` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `enabled` | 0 次（73 档） | **有**（出现在 12 个类目） |
| `format` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `interaction_source_list` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `looking_for_a` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `map_color` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `map_mode` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `max` | 0 次（73 档） | **有**（出现在 25 个类目） |
| `max_targets_for_ui` | 0 次（73 档） | 全库也没有 → 疑似废弃字段 |
| `months` | 0 次（73 档） | **有**（出现在 15 个类目） |
| `name` | 0 次（73 档） | **有**（出现在 30 个类目） |
| `none_available_msg_key` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `only_color_selectable` | 0 次（73 档） | **有**（出现在 1 个类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（73 档） | **有**（出现在 6 个类目） |
| `pre_evaluation_sort_value` | 0 次（73 档） | **有**（出现在 6 个类目） |
| `secondary_map_color` | 0 次（73 档） | **有**（出现在 2 个类目） |
| `selected` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `show_if` | 0 次（73 档） | **有**（出现在 2 个类目） |
| `show_why_not_enabled` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `show_why_not_visible` | 0 次（73 档） | 全库也没有 → 疑似废弃字段 |
| `source` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `source_ai_override` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `source_flags` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `source_flags_ai_override` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `source_global_list` | 0 次（73 档） | **有**（出现在 3 个类目） |
| `step` | 0 次（73 档） | **有**（出现在 2 个类目） |
| `target_flag` | 0 次（73 档） | **有**（出现在 8 个类目） |
| `top_widget` | 0 次（73 档） | **有**（出现在 4 个类目） |
| `visible` | 0 次（73 档） | **有**（出现在 12 个类目） |
| `weeks` | 0 次（73 档） | 全库也没有 → 疑似废弃字段 |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `map_marker` | 29 | 28 |
| `rebel_modifier` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `column` | 131 | select_trigger |
| `data` | 128 | column |
| `NOT` | 71 | scope:target、any_location_in_province、AND、trigger_if |
| `limit` | 69 | while、else_if、every_accepted_culture、trigger_if |
| `exists` | 68 | limit、display_trigger、any_location_in_province、any_location_in_area |
| `value` | 60 | scope:selected_province、scope:actor、ai_will_do、scope:target |
| `name` | 55 | select_trigger、set_variable |
| `looking_for_a` | 50 | select_trigger |
| `target_flag` | 50 | select_trigger |
| `none_available_msg_key` | 48 | select_trigger |
| `visible` | 47 | select_trigger |
| `OR` | 45 | any_location_in_area、scope:actor、trigger_if、any_location_in_province_definition |
| `source` | 43 | select_trigger |
| `if` | 41 | scope:actor、else、on_deactivate、ai_will_do |
| `map_color` | 37 | reduced_paperwork、select_trigger、assimilate_area、soldiers_as_workforce |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `icon` | 10 | 9 |
