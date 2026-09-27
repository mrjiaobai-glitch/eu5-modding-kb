# common/country_interactions（国家交互/外交行动）

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
