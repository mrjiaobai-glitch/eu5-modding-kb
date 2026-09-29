# common/character_interactions（角色交互）

> **一句话**：角色交互字段：对本国与他国角色的可用开关、价格与支付方、目标选择、AI 检查频率与冷却，根是角色而国家走另一作用域。
> **什么时候看**：写角色交互，或排查根作用域与国家级触发写成角色时看。
> **体量**：138 行 · 约 7 分钟通读

来源：`in_game\common\character_interactions\readme.txt`

## 顶层字段

```
<interaction_name> = {
    message = <yes/no>                     # 是否显示消息
    sound = <sound>
    on_other_nation = <yes/no>             # 可否对别国角色使用（默认 no）
    on_own_nation = <yes/no>               # 可否对本国角色使用（默认 no）
    is_consort_action = <yes/no>           # 是否是对配偶的交互
    potential = <trigger>; allow = <trigger>   # scope:actor = 执行国
    price = <price>（引用 \common\prices\）; price_modifier = <script value>
    payer = <script>（默认 actor）; payee = <script>（默认无人）
    select_trigger = { ... }               # 格式同 common-cabinet_actions.md 的 select_trigger；source 此处多 world
    ai_tick = <never/daily/monthly>; ai_tick_frequency = <scripted value>
    show_message = no / show_message_to_target = no / should_execute_price = no / show_in_gui_list = no
    ai_will_do = <effect script>; effect = <effect>
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
}
```

## select_trigger 要点（公共格式详见 common-cabinet_actions.md）

- `looking_for_a`：character/location/province/area/region/country/value/boolean 等
- `target_flag`：自定义 scope 名（默认 target, target_1...）；`source`：actor/recipient/target/.../target_4/**world**
- `source_flags` 性能选项：neighbor/possible_colonial_charters/include_dead/include_any_present/possible_exploration_areas/adjacent_locations/vacant_adjacent_locations/adjacent_provinces/border/border_or_recipients_capital_area/provinces_ai_wants_to_give_away/only_actual_locations
- `interaction_source_list = <effect>`：scope:actor = country，add_to_list = source 填表
- `allow_null` / `allow_null_trigger` / `allow_self`（仅国家）/ `name` / `visible` / `enabled` / `selected`（root = 被测对象）/ `map_color` 等

## 审查要点

- `potential`/`allow` 中 root 是角色，`scope:actor` 才是国家——勿把国家级 trigger 直接放 root。
- price 引用须在 common/prices 存在。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\character_interactions\` 全量 **32 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `context_menu_click_mode` | 3 | 3 | click（3） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`message`**（1 种）：yes（34）
- **`ai_tick`**（3 种）：daily（22）、never（8）、monthly（4）
- **`on_own_nation`**（1 种）：yes（32）
- **`ai_tick_frequency`**（11 种）：120（5）、90（4）、180（4）、45（3）、30（3）、6（2）、10（2）、383（1）、1（1）、100（1）、365（1）
- **`is_consort_action`**（2 种）：no（10）、yes（3）
- **`price`**（11 种）：price:assign_despot_price（1）、price:ennoble_price（1）、price:grant_cabinet_right_price（1）、price:castrate_character_price（1）、price:commission_art_price（1）、price:blind_character_price（1）、price:appoint_as_heir_price（1）、price:abdicate_price（1）、price:pardon_price（1）、price:head_of_cabinet_promotion（1）、price:compose_strategikon_price（1）
- **`sound`**（6 种）：UI_action_character_marry_noble（2）、"event:/SFX/UI/Character/Unique/sfx_ui_character_arrange_marriage"（1）、UI_action_character_marry_lowborn（1）、PROMOTE_TO_HEAD_OF_CABINET（1）、UI_action_character_ennoble（1）、UI_action_character_grant_cabinet_rights（1）
- **`context_menu_click_mode`**（1 种）：click（3）
- **`on_other_nation`**（1 种）：yes（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_interaction_source_list` | 0 次（32 档） | **有**（写在别的类目） |
| `allow_null` | 0 次（32 档） | **有**（写在别的类目） |
| `allow_null_trigger` | 0 次（32 档） | 全库也没有 → 疑似废弃字段 |
| `allow_self` | 0 次（32 档） | **有**（写在别的类目） |
| `bottom_widget` | 0 次（32 档） | **有**（写在别的类目） |
| `cache_interaction_source_list` | 0 次（32 档） | **有**（写在别的类目） |
| `cache_order` | 0 次（32 档） | **有**（写在别的类目） |
| `cache_targets` | 0 次（32 档） | **有**（写在别的类目） |
| `column` | 0 次（32 档） | **有**（写在别的类目） |
| `cooldown` | 0 次（32 档） | **有**（写在别的类目） |
| `default` | 0 次（32 档） | **有**（写在别的类目） |
| `default_sort` | 0 次（32 档） | **有**（写在别的类目） |
| `enabled` | 0 次（32 档） | **有**（写在别的类目） |
| `format` | 0 次（32 档） | **有**（写在别的类目） |
| `interaction_source_list` | 0 次（32 档） | **有**（写在别的类目） |
| `looking_for_a` | 0 次（32 档） | **有**（写在别的类目） |
| `map_color` | 0 次（32 档） | **有**（写在别的类目） |
| `map_mode` | 0 次（32 档） | **有**（写在别的类目） |
| `max` | 0 次（32 档） | **有**（写在别的类目） |
| `max_targets_for_ui` | 0 次（32 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（32 档） | **有**（写在别的类目） |
| `name` | 0 次（32 档） | **有**（写在别的类目） |
| `none_available_msg_key` | 0 次（32 档） | **有**（写在别的类目） |
| `only_color_selectable` | 0 次（32 档） | **有**（写在别的类目） |
| `payee` | 0 次（32 档） | **有**（写在别的类目） |
| `payer` | 0 次（32 档） | **有**（写在别的类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（32 档） | **有**（写在别的类目） |
| `pre_evaluation_sort_value` | 0 次（32 档） | **有**（写在别的类目） |
| `secondary_map_color` | 0 次（32 档） | **有**（写在别的类目） |
| `selected` | 0 次（32 档） | **有**（写在别的类目） |
| `should_execute_price` | 0 次（32 档） | **有**（写在别的类目） |
| `show_if` | 0 次（32 档） | **有**（写在别的类目） |
| `show_in_gui_list` | 0 次（32 档） | **有**（写在别的类目） |
| `show_message` | 0 次（32 档） | **有**（写在别的类目） |
| `show_message_to_target` | 0 次（32 档） | **有**（写在别的类目） |
| `show_why_not_enabled` | 0 次（32 档） | **有**（写在别的类目） |
| `show_why_not_visible` | 0 次（32 档） | 全库也没有 → 疑似废弃字段 |
| `source` | 0 次（32 档） | **有**（写在别的类目） |
| `source_ai_override` | 0 次（32 档） | **有**（写在别的类目） |
| `source_flags` | 0 次（32 档） | **有**（写在别的类目） |
| `source_flags_ai_override` | 0 次（32 档） | **有**（写在别的类目） |
| `source_global_list` | 0 次（32 档） | **有**（写在别的类目） |
| `step` | 0 次（32 档） | **有**（写在别的类目） |
| `target_flag` | 0 次（32 档） | **有**（写在别的类目） |
| `top_widget` | 0 次（32 档） | **有**（写在别的类目） |
| `visible` | 0 次（32 档） | **有**（写在别的类目） |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 121 | every_known_country、every_location_in_area、every_character、every_child |
| `scope:actor` | 113 | else、hidden_effect、OR、allow |
| `if` | 97 | owner、price_modifier、create_country_from_location、scope:recipient |
| `value` | 86 | else、set_variable、scope:recipient、pre_evaluation_sort_value |
| `add` | 76 | ?、price_modifier、scope:recipient、if |
| `scope:recipient` | 66 | limit、hidden_effect、NOT、if |
| `NOT` | 59 | any_current_war、limit、enabled、custom_tooltip |
| `name` | 50 | select_trigger、set_variable、add_to_variable_list |
| `column` | 47 | select_trigger |
| `target_flag` | 43 | select_trigger |
| `looking_for_a` | 43 | select_trigger |
| `visible` | 43 | select_trigger |
| `or` | 42 | limit、scope:recipient、enabled、AND |
| `source` | 39 | select_trigger |
| `data` | 39 | column |
