# common/character_interactions（角色交互）

> **一句话**：角色交互字段：对本国与他国角色的可用开关、价格与支付方、目标选择、AI 检查频率与冷却，根是角色而国家走另一作用域。
> **什么时候看**：写角色交互，或排查根作用域与国家级触发写成角色时看。
> **体量**：130 行 · 约 6 分钟通读

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

> **数据源**：`in_game\common\character_interactions\` 全量 **30 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、取值白名单（本体出现过的值 + 次数）

- **`message`**（2 种）：yes（31）、no（1）
- **`ai_tick`**（3 种）：daily（20）、never（8）、monthly（3）
- **`on_own_nation`**（1 种）：yes（30）
- **`ai_tick_frequency`**（11 种）：90（4）、180（4）、120（3）、30（3）、10（2）、45（2）、6（2）、360（1）、100（1）、365（1）、383（1）
- **`is_consort_action`**（2 种）：no（10）、yes（1）
- **`price`**（11 种）：price:pardon_price（1）、price:commission_art_price（1）、price:abdicate_price（1）、price:compose_strategikon_price（1）、price:blind_character_price（1）、price:appoint_as_heir_price（1）、price:castrate_character_price（1）、price:ennoble_price（1）、price:head_of_cabinet_promotion（1）、price:grant_cabinet_right_price（1）、price:assign_despot_price（1）
- **`sound`**（5 种）："event:/SFX/UI/Character/Unique/sfx_ui_character_arrange_marriage"（1）、UI_action_character_ennoble（1）、PROMOTE_TO_HEAD_OF_CABINET（1）、UI_action_character_grant_cabinet_rights（1）、UI_action_character_marry_lowborn（1）
- **`on_other_nation`**（1 种）：yes（1）

### 二、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_interaction_source_list` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `allow_null` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `allow_null_trigger` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `allow_self` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `bottom_widget` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `cache_interaction_source_list` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `cache_order` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `cache_targets` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `column` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `cooldown` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `default` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `default_sort` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `enabled` | 0 次（30 档） | **有**（出现在 12 个类目） |
| `format` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `interaction_source_list` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `looking_for_a` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `map_color` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `map_mode` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `max` | 0 次（30 档） | **有**（出现在 25 个类目） |
| `max_targets_for_ui` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（30 档） | **有**（出现在 19 个类目） |
| `name` | 0 次（30 档） | **有**（出现在 30 个类目） |
| `none_available_msg_key` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `only_color_selectable` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `payee` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `payer` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（30 档） | **有**（出现在 6 个类目） |
| `pre_evaluation_sort_value` | 0 次（30 档） | **有**（出现在 6 个类目） |
| `secondary_map_color` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `selected` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `should_execute_price` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `show_if` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `show_in_gui_list` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `show_message` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `show_message_to_target` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `show_why_not_enabled` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `show_why_not_visible` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `source` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `source_ai_override` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `source_flags` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `source_flags_ai_override` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `source_global_list` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `step` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `target_flag` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `top_widget` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `visible` | 0 次（30 档） | **有**（出现在 12 个类目） |

### 三、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 117 | ordered_location_in_area、trigger_if、random_country、every_international_organization_member |
| `scope:actor` | 107 | visible、and、interaction_source_list、hidden_effect |
| `if` | 91 | create_country_from_location、owner、price_modifier、ai_will_do |
| `value` | 74 | add_reform_desire、subtract、scope:actor、ai_will_do |
| `add` | 70 | scope:recipient、price_modifier、size、? |
| `scope:recipient` | 59 | limit、show_as_tooltip、subtract、visible |
| `NOT` | 49 | custom_tooltip、limit、enabled、any_current_war |
| `column` | 47 | select_trigger |
| `name` | 45 | set_variable、select_trigger |
| `target_flag` | 42 | select_trigger |
| `looking_for_a` | 42 | select_trigger |
| `visible` | 41 | select_trigger |
| `data` | 39 | column |
| `source` | 37 | select_trigger |
| `or` | 37 | scope:actor、any_location_in_area、trigger_if、scope:recipient |
