# common/generic_actions（通用行动）

> **一句话**：通用行动字段：类型枚举、价格与支付方、目标选择、冷却与自动化分栏，以及四类消息键与按钮接线约定。
> **什么时候看**：写通用行动或自动化行动，或核对消息键与类型拼写时翻这篇。
> **体量**：202 行 · 约 10 分钟通读

来源：`in_game\common\generic_actions\readme.txt`

## 顶层字段

```
<action_tag> = {
    type = <owncountry/religious/religiousfaction/diplomacy/subject/character/location/internationalorganization/situation/internationalorganizationparliament>
    sound = <sound>
    message = <message key>
    potential = <trigger>            # scope:actor = 执行国
    allow = <trigger>                # scope:actor = 执行国
    ai_prerequisite = <trigger>      # 早期阶段无任何 scope/target 可用；root = 国家
    price = <price>                  # 引用 \common\prices\（price:<price_id> 或脚本结果）
    price_modifier = <script value>  # scope:actor/recipient/target...
    payer = <script>                 # 默认 actor
    payee = <script>                 # 默认无人
    select_trigger = { ... }         # 格式同 character_interactions；source_flags 另有 only_defending_sieges/only_attacking_sieges/include_subjects；另有 ai_override_value = <script value>（AI 唯一测试值，性能用）、tooltip_msg_key
    ai_tick = <never/daily/monthly>          # 禁止 AI 用或指定频率
    ai_tick_frequency = <scripted value>
    automation_tick = <never/daily/monthly>  # 自动化进程用
    automation_tick_frequency = <scripted value>
    show_message = no / show_message_to_target = no / should_execute_price = no / show_in_gui_list = no
    ai_will_do = <effect script>
    player_automated_category = <system>  # 玩家开启该系统自动化后按 ai_will_do 执行；取值：finances/research/trade/productionmethods/laws/cabinet/parliament/estates/exploration/colonies/cultureacceptance/religiousdoctrines/buildings/rgo/armybuilder/navybuilder
    effect = <effect>                # scope:actor/recipient/target... 外加 scope:price/scope:price_modifier/scope:payer/scope:payee
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
    maximum_targets_in_one_tick = <int>  # 一次检查执行多次；-1 = 无限；默认 1
    disallowed_duplicates_of_targets_for_ai = {}  # 目标 flags 列表，防止同 tick 重复目标（如 target_location）
    force_click_and_confirm_or_hold = yes  # 总是显示确认对话框（危险的 IO 行动如宣战用）
}
```

## 本地化（readme 声明）

- 行动本身：名称 tag = `<action_tag>`、描述 tag = `<action_tag>_desc`（注意：不是带前缀的形式）。
- 消息类型键（GUI 消息）：
  - 通用兜底：`PERFORM_<Key>_ACTION`
  - 玩家执行：`WE_PERFORM_<Key>_ACTION`
  - 他国执行：`OTHER_PERFORMS_<Key>_ACTION`
  - 他国对我们执行：`ACTION_<Key>_PERFORMED_ON_US`
- 消息字符串可用预设：`$ACTION$`（行动名）、`$EFFECT$`（效果）、`$DESC$`（行动描述）。

## GUI 按钮（action_button_default）

- 字段：`title`/`description`/`effects`/`conditions`（可覆盖默认 tooltip 文案）、`actor`/`recipient`/`target`（GUI 脚本，target 可多个）、`left_action`/`left_click_and_hold_action`/`right_action`/`right_click_and_hold_action`。

## 审查要点

- `type` 是枚举，拼错加载即错。
- `player_automated_category` 取值是固定枚举列表。
- 消息键四形式 + `PERFORM_` 兜底，缺键显示 raw key。
- 未在 readme 中说明：无。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\generic_actions\` 全量 **115 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `exclusive_group` | 23 | 2 | bd_disease_control_actions（10）、bd_social_actions（7）、food_and_trade_actions（6） |
| `estate_interaction_action` | 11 | 2 | yes（11） |
| `required_calling` | 10 | 1 | scholars_calling（1）、almoners_calling（1）、contemplatives_calling（1）、crusaders_calling（1）、confessors_calling（1） |
| `imperial_circle_action` | 7 | 1 | yes（7） |
| `estate_reward_action` | 4 | 1 | burghers_estate（1）、clergy_estate（1）、nobles_estate（1）、peasants_estate（1） |
| `show_in_decision_panel` | 3 | 2 | yes（3） |
| `decision_category` | 3 | 2 | infrastructure_decisions（2）、country_specific_decisions（1） |
| `play_sound` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`type`**（8 种）：situation（155）、religious（96）、owncountry（87）、disaster（58）、internationalorganization（57）、religiousfaction（20）、parliament（11）、internationalorganizationparliament（1）
- **`ai_will_do`**（4 种）：0（2）、10（1）、100（1）、-1000（1）
- **`automation_tick`**（3 种）：never（266）、monthly（141）、daily（43）
- **`ai_tick`**（3 种）：monthly（274）、daily（108）、never（36）
- **`automation_tick_frequency`**（9 种）：12（240）、1（94）、7（31）、6（28）、3（11）、20（6）、30（2）、60（1）、365（1）
- **`sound`**（10 种）：UI_action_religion_generic（117）、PARLIAMENT_REQUEST_MORE_TAXES（2）、PARLIAMENT_ASK_FOR_LARGER_LEVIES（2）、BRIBE_ESTATE（1）、PARLIAMENT_ASK_FOR_LAW_CHANGES（1）、PARLIAMENT_PREPARE_FOR_WAR（1）、UI_action_religion_aspect_change（1）、PARLIAMENT_PROMOTE_MAMLUKS（1）、UI_action_religion_aspect_remove（1）、UI_action_religion_aspect_add（1）
- **`player_automated_category`**（11 种）：religiousdoctrines（85）、parliament（10）、diplomacy（10）、cultureacceptance（6）、governmentreforms（4）、exploration（3）、colonies（2）、privateers（2）、estates（2）、replacegenerals（1）、replaceadmirals（1）
- **`show_message`**（2 种）：no（88）、yes（4）
- **`exclusive_group`**（3 种）：bd_disease_control_actions（10）、bd_social_actions（7）、food_and_trade_actions（6）
- **`show_in_gui_list`**（1 种）：no（16）
- **`message`**（1 种）：national_church_action（14）
- **`estate_interaction_action`**（1 种）：yes（11）
- **`required_calling`**（10 种）：scholars_calling（1）、almoners_calling（1）、contemplatives_calling（1）、crusaders_calling（1）、confessors_calling（1）、cultivators_calling（1）、mariners_calling（1）、confidants_calling（1）、preachers_calling（1）、wardens_calling（1）
- **`show_message_to_target`**（2 种）：yes（7）、no（1）
- **`imperial_circle_action`**（1 种）：yes（7）
- **`estate_reward_action`**（4 种）：burghers_estate（1）、clergy_estate（1）、nobles_estate（1）、peasants_estate（1）
- **`decision_category`**（2 种）：infrastructure_decisions（2）、country_specific_decisions（1）
- **`show_in_decision_panel`**（1 种）：yes（3）
- **`maximum_targets_in_one_tick`**（2 种）：20（2）、10（1）
- **`should_execute_price`**（1 种）：no（3）
- …另有 4 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `icon` | 99 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `action_name` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `actor` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `ai_interaction_source_list` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `ai_override_value` | 0 次（115 档） | **有**（出现在 2 个类目） |
| `allow_null` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `allow_self` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `bottom_widget` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `button` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `button_tooltip_override` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `cache_interaction_source_list` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `cache_order` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `cache_targets` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `click_mode` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `click_type` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `column` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `conditions` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `datacontext` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `default` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `default_sort` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `description` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `effects` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `enabled` | 0 次（115 档） | **有**（出现在 12 个类目） |
| `format` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `interaction_source_list` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `looking_for_a` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `map_color` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `map_mode` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `max` | 0 次（115 档） | **有**（出现在 25 个类目） |
| `max_targets_for_ui` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（115 档） | **有**（出现在 19 个类目） |
| `name` | 0 次（115 档） | **有**（出现在 30 个类目） |
| `none_available_msg_key` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `only_color_selectable` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `parameter` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `payee` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `payer` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（115 档） | **有**（出现在 6 个类目） |
| `pre_evaluation_sort_value` | 0 次（115 档） | **有**（出现在 6 个类目） |
| `scripted_action_tooltip` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `secondary_map_color` | 0 次（115 档） | **有**（出现在 2 个类目） |
| `selected` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `show_if` | 0 次（115 档） | **有**（出现在 2 个类目） |
| `show_why_not_enabled` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `show_why_not_visible` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `source` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `source_ai_override` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `source_flags` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `source_flags_ai_override` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `source_global_list` | 0 次（115 档） | **有**（出现在 3 个类目） |
| `step` | 0 次（115 档） | **有**（出现在 2 个类目） |
| `target_flag` | 0 次（115 档） | **有**（出现在 8 个类目） |
| `title` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `tooltip_msg_key` | 0 次（115 档） | **有**（出现在 1 个类目） |
| `tooltipwidget` | 0 次（115 档） | 全库也没有 → 疑似废弃字段 |
| `top_widget` | 0 次（115 档） | **有**（出现在 4 个类目） |
| `visible` | 0 次（115 档） | **有**（出现在 12 个类目） |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_acceptance` | 5 | 1 |
| `show_message_if` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 1620 | random_international_organizations_member_of、every_policy_in_law、every_current_war、every_descendant |
| `scope:actor` | 1414 | if、add、any_rival、any_country_in_diplomatic_range |
| `value` | 1391 | add_estate_satisfaction、amount、years、supporters |
| `add` | 1354 | join_autocephalous_patriarchate、add、years、if |
| `if` | 1141 | scope:target_1、price、every_current_war、every_location_in_religious_order |
| `column` | 885 | lordship_of_ireland_casus_belli、select_trigger、sc_seek_foreign_aid、? |
| `name` | 777 | set_variable、save_temporary_scope_value_as、remove_list_variable、create_holy_site |
| `target_flag` | 720 | lordship_of_ireland_casus_belli、select_trigger、sc_seek_foreign_aid、? |
| `looking_for_a` | 720 | select_trigger、? |
| `data` | 603 | column |
| `NOT` | 601 | any_owned_location、any_current_war、any_international_organization_member、custom_tooltip |
| `desc` | 550 | subtract、value、add、divide |
| `visible` | 543 | set_colonial_charter_goal、lordship_of_ireland_casus_belli、select_trigger、? |
| `multiply` | 507 | if、add、scope:recipient.leader_country、supporters_percent |
| `custom_tooltip` | 437 | visible、c:FRA、if、establish_treaty_with_kirishitan |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `icon` | 99 | 9 |
