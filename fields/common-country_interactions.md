# common/country_interactions（国家交互/外交行动）

> **一句话**：外交行动字段表：类型枚举、接受值与接受度覆盖、外交官成本与冷却，并完整列出接受度可用的合法键清单。
> **什么时候看**：写国家交互、调接受度键名，或核对消息与按钮接线时看。
> **体量**：172 行 · 约 8 分钟通读

来源：`in_game\common\country_interactions\readme.txt`

## 顶层字段

```
<interaction_name> = {
    type = <subject/diplomacy/union>
    sound = <sound>
    accept = <script>                    # AI 接受值
    diplo_chance = {}                    # 覆盖默认接受值；键见下方 AI Diplo chances 清单
    block_when_at_war = <yes/no>         # 默认 yes
    use_enroute = <yes/no>               # 是否使用耗时外交官
    diplomatic_cost = <diplomatic_cost_id>; diplomatic_cost_modifier = <script value>  # 未指定 = 1
    potential = <trigger>; allow = <trigger>   # scope:actor = 执行国
    price = <price>（引用 \common\prices\）; price_modifier = <script value>
    payer = <script>（默认 actor）; payee = <script>（默认无人）
    ai_limit_per_check = <int>           # 限制 AI 每月使用该交互的国家数
    select_trigger = { ... }             # 格式同 common-cabinet_actions.md 的 select_trigger；source 另有 knowncountries/inrange/rivals/subjects/atwar；source_flags 另有 same_international_organization/wants_military_access_in
    ai_tick = <never/daily/monthly>; ai_tick_frequency = <scripted value>
    show_message = no / show_message_to_target = no / should_execute_price = no / show_in_gui_list = no
    ai_will_do = <effect script>
    ai_prerequisite = <trigger>          # 仅 scope:actor 可用；AI 是否检查该交互（性能）
    effect = <effect>; reject_effect = <effect>   # reject_effect 仅当该行动有接受值时执行
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
}
```

## AI Diplo chances 清单（diplo_chance 合法键，readme 全列）

at_war, recipient_at_war, actor_at_war, recipient_civil_war, actor_civil_war, multiple_offensive_wars, actor_is_rival, recipient_is_rival, same_religion, different_religion, same_culture, same_court_language, same_common_language, different_culture, diplomatic_reputation, culture_war, opinion, warscore, peaceoffer, peaceoffer_most_of_wanted, months_at_war, planning_demise, conflicting_interests, max_relations, actor_max_relations, capital_distance, yesman, defeat, victory, has_border, same_international_organization, giving_defensive_support, receiving_defensive_support, base, giving_them_access, in_debt, cost, claim, current_strength, potential_strength, relative_strength, capital, location_value, base_location_value, interesting, vital, avoided, stability, positive_stability, negative_stability, war_exhaustion, low_manpower, disloyal_subject, no_action, separate_peace, junior_to, price, desperation, war_balance, war_goal, making_gains, on_retreat, tutorial, target_opinion, lacks_border, another_war, fighting_together, border_distance, province_distance, revolter, rank, rank_difference, common_threat, competing_power, recipient_at_peace, actor_at_peace, best_possible_offer, substantial_land_lost, last_major_battle, few_relations, no_access, positive_opinion, negative_opinion, allied_to_enemy, enforced_demand, ai_setting, has_truce, has_truce_with_target, overlord, my_proposal, heir, interest_rate_too_high, good_interest_rate, existing_loans_from_country, too_many_loans, need_loan, loan_is_insignificant, loan_ends_too_soon, loan_ends_too_late, using_favors, unbalanced_favors, trust_in_actor, positive_trust_in_actor, negative_trust_in_actor, trust_in_recipient, same_government_type, different_government_type, estates_like, estates_dislike, culture_view, religion_view, royal_ties, call_for_peace, war_enthusiam, want_more, want_something_else, tax_base, promised_land, demands_made, belongs_to_international_organization, different_religion_group, conquer_desire, produced_goods, price_percentage_of_treasury_funds, betrayed_ally, too_much_antagonism, antagonism, strategic_interest

## GUI 用法（readme 示例要点）

- `action_button_default` widget：`left_action/right_action/left_click_and_hold_action/right_click_and_hold_action`，含 `action_name`、`action_direction`（request/offer/cancellation/break）、`parameter = { parameter_name = xxx parameter_value = "[GetInternationalOrganizationType('defensive_league')]" }` 等。

## 审查要点

- `diplo_chance` 键必须是上方清单之一（拼错键静默失效）。
- `type` 枚举：subject/diplomacy/union。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\country_interactions\` 全量 **88 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `category` | 136 | 87 | CATEGORY_HRE_ACTIONS（33）、CATEGORY_SUBJECT_ACTIONS（31）、CATEGORY_FRIENDLY_ACTIONS（18）、CATEGORY_ECONOMY_ACTIONS（15）、CATEGORY_UNION_ACTIONS（8） |
| `icon` | 8 | 1 | circle_leader_action（5）、circle_emperor_action（2）、circle_member_action（1） |
| `automation_tick` | 4 | 4 | never（4） |
| `automation_tick_frequency` | 4 | 4 | 12（4） |
| `is_take_over_loan` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`type`**（3 种）：diplomacy（102）、subject（28）、union（8）
- **`category`**（12 种）：CATEGORY_HRE_ACTIONS（33）、CATEGORY_SUBJECT_ACTIONS（31）、CATEGORY_FRIENDLY_ACTIONS（18）、CATEGORY_ECONOMY_ACTIONS（15）、CATEGORY_UNION_ACTIONS（8）、CATEGORY_COVERT_ACTIONS（7）、CATEGORY_PAPAL_ACTIONS（7）、CATEGORY_HOSTILE_ACTIONS（6）、CATEGORY_INFLUENCE_ACTIONS（6）、CATEGORY_ACCESS_ACTIONS（2）、CATEGORY_CHINA_ACTIONS（2）、CATEGORY_MILITARY_ACTIONS（1）
- **`ai_will_do`**（1 种）：-10（2）
- **`ai_tick`**（3 种）：never（17）、daily（12）、monthly（8）
- **`ai_tick_frequency`**（8 种）：360（4）、180（4）、120（3）、6（3）、24（2）、3（2）、12（1）、365（1）
- **`ai_limit_per_check`**（2 种）：1（20）、5（1）
- **`block_when_at_war`**（2 种）：yes（10）、no（9）
- **`price`**（13 种）：price:catholic_country_interaction（5）、price:bribe_voter_for_policy（2）、price:request_divorce_price（1）、price:capital_movement（1）、price:invite_artist（1）、price:sell_work_of_art（1）、price:sell_icon（1）、price:request_work_of_art_purchase（1）、price:merge_colonies_price（1）、price:buy_military_access（1）、price:excommunication_price（1）、price:transfer_subject_price（1）、price:rtr_demand_annexation_price（1）
- **`icon`**（3 种）：circle_leader_action（5）、circle_emperor_action（2）、circle_member_action（1）
- **`payee`**（2 种）：scope:recipient（4）、scope:actor（3）
- **`payer`**（2 种）：scope:recipient（4）、scope:actor（1）
- **`use_enroute`**（2 种）：no（4）、yes（1）
- **`automation_tick`**（1 种）：never（4）
- **`automation_tick_frequency`**（1 种）：12（4）
- **`show_message`**（1 种）：no（2）
- **`show_message_to_target`**（1 种）：no（2）
- **`is_take_over_loan`**（1 种）：yes（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `action_button_default` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `action_direction` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `action_name` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `actor` | 0 次（88 档） | **有**（写在别的类目） |
| `ai_interaction_source_list` | 0 次（88 档） | **有**（写在别的类目） |
| `allow_null` | 0 次（88 档） | **有**（写在别的类目） |
| `allow_self` | 0 次（88 档） | **有**（写在别的类目） |
| `bottom_widget` | 0 次（88 档） | **有**（写在别的类目） |
| `cache_interaction_source_list` | 0 次（88 档） | **有**（写在别的类目） |
| `cache_order` | 0 次（88 档） | **有**（写在别的类目） |
| `cache_targets` | 0 次（88 档） | **有**（写在别的类目） |
| `column` | 0 次（88 档） | **有**（写在别的类目） |
| `default` | 0 次（88 档） | **有**（写在别的类目） |
| `default_sort` | 0 次（88 档） | **有**（写在别的类目） |
| `description` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `enabled` | 0 次（88 档） | **有**（写在别的类目） |
| `format` | 0 次（88 档） | **有**（写在别的类目） |
| `interaction_source_list` | 0 次（88 档） | **有**（写在别的类目） |
| `left_action` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `left_click_and_hold_action` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `looking_for_a` | 0 次（88 档） | **有**（写在别的类目） |
| `map_color` | 0 次（88 档） | **有**（写在别的类目） |
| `map_mode` | 0 次（88 档） | **有**（写在别的类目） |
| `max` | 0 次（88 档） | **有**（写在别的类目） |
| `max_targets_for_ui` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（88 档） | **有**（写在别的类目） |
| `name` | 0 次（88 档） | **有**（写在别的类目） |
| `none_available_msg_key` | 0 次（88 档） | **有**（写在别的类目） |
| `only_color_selectable` | 0 次（88 档） | **有**（写在别的类目） |
| `parameter` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `parameter_name` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `parameter_value` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（88 档） | **有**（写在别的类目） |
| `pre_evaluation_sort_value` | 0 次（88 档） | **有**（写在别的类目） |
| `recipient` | 0 次（88 档） | **有**（写在别的类目） |
| `right_action` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `right_click_and_hold_action` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `secondary_map_color` | 0 次（88 档） | **有**（写在别的类目） |
| `selected` | 0 次（88 档） | **有**（写在别的类目） |
| `should_execute_price` | 0 次（88 档） | **有**（写在别的类目） |
| `show_if` | 0 次（88 档） | **有**（写在别的类目） |
| `show_in_gui_list` | 0 次（88 档） | **有**（写在别的类目） |
| `show_why_not_enabled` | 0 次（88 档） | **有**（写在别的类目） |
| `show_why_not_visible` | 0 次（88 档） | 全库也没有 → 疑似废弃字段 |
| `size` | 0 次（88 档） | **有**（写在别的类目） |
| `sound` | 0 次（88 档） | **有**（写在别的类目） |
| `source` | 0 次（88 档） | **有**（写在别的类目） |
| `source_ai_override` | 0 次（88 档） | **有**（写在别的类目） |
| `source_flags` | 0 次（88 档） | **有**（写在别的类目） |
| `source_flags_ai_override` | 0 次（88 档） | **有**（写在别的类目） |
| `source_global_list` | 0 次（88 档） | **有**（写在别的类目） |
| `step` | 0 次（88 档） | **有**（写在别的类目） |
| `target_flag` | 0 次（88 档） | **有**（写在别的类目） |
| `text` | 0 次（88 档） | **有**（写在别的类目） |
| `title` | 0 次（88 档） | **有**（写在别的类目） |
| `top_widget` | 0 次（88 档） | **有**（写在别的类目） |
| `visible` | 0 次（88 档） | **有**（写在别的类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_prerequisite` | 16 | 12 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 599 | multiply、if、has_gold_org_country_trigger、enabled |
| `limit` | 533 | every_character_in_dynasty、random_core_in_location、every_loan_lent_to_country、every_international_organization_member |
| `add` | 464 | change_variable、add_liberty_desire、scope:target、multiply |
| `if` | 447 | if、international_organization:hre、scope:target、pre_evaluation_sort_value |
| `desc` | 410 | multiply、divide、add、subtract |
| `scope:actor` | 408 | if、potential、hidden_trigger、limit |
| `scope:recipient` | 282 | custom_tooltip、trigger_if、OR、enabled |
| `not` | 247 | select_trigger、enabled、scope:io_recipient、limit |
| `column` | 237 | invite_artist、challenge_circle_leadership、ask_support_curia_proposal、select_trigger |
| `name` | 222 | save_temporary_scope_value_as、change_variable、request_circle_privilege、is_target_in_variable_list |
| `data` | 221 | column |
| `target_flag` | 209 | challenge_circle_leadership、ask_support_curia_proposal、select_trigger、request_circle_privilege |
| `looking_for_a` | 208 | select_trigger |
| `visible` | 201 | invite_artist、challenge_circle_leadership、select_trigger、request_circle_privilege |
| `enabled` | 163 | invite_artist、challenge_circle_leadership、rtr_demand_annexation、select_trigger |
