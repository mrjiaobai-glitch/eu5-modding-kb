# common/laws（法律与政策）

> **一句话**：法律与政策两档：法律是容器、内含互斥政策，政策的费用、生效时长与三类缩放修正，以及国际组织政策对组织字段的覆盖语义。
> **什么时候看**：写法律与政策、做国际组织政策，或排查修正被替换而非叠加时看。
> **体量**：201 行 · 约 10 分钟通读

来源：`in_game\common\laws\readme.txt`

## 结构语义

- **法律（law）是容器**，含一个或多个可选政策（policy）；同一时间只能选一个政策。

## Laws 字段

```
<law_id> = {
    type = <实体类型>            # 通常 country 或 international_organization
    potential = <trigger>        # 能否显示
    allow = <trigger>            # 能否更改
    locked = <trigger>           # 是否锁定不可交互
    requires_vote = <yes/no>     # 该法律的政策通过是否需要投票（国际组织内）
    law_religion_group = <catholic/sunni/...>   # 特定宗教专属法律
    law_gov_group = <monarchy/republic/theocracy>  # 特定政体专属
    law_country_goup = <country tag>            # 特定国家专属（原文即拼写 law_country_goup）
    unique = <yes/no>            # 默认 no
    custom_tags = { <strings> }
    show_tags_in_ui = <yes/no>
    # 其余键 = 可选政策 tag
}
```

## Policies 字段

```
<policy_id> = {
    price = <price>
    potential = <trigger>        # root = country（或 IO）
    allow = <trigger>
    custom_tags = { <strings> }
    show_tags_in_ui = <yes/no>
    years / months / weeks / days = <int>
    on_pay_price = <effect>      # root = country/IO
    on_activate / on_fully_activated / on_deactivate = <effect>  # root = country/IO
    international_organization_modifier = <scaled & triggered modifier>  # 施加于整个组织
    country_modifier / province_modifier / location_modifier = <scaled & triggered modifier>
    wants_this_policy_bias = <scripted maths>  # 政府: root = country；IO: root = country, scope:actor, scope:recipient = IO, scope:target, scope:policy
    wants_propose_policy = <scripted maths>    # root = country, scope:actor（IO 不存在时）, scope:recipient, scope:target, scope:policy
    wants_keep_policy = <scripted math>
    reasons_to_join = <scripted math>          # 对加入 IO 意愿的影响
    diplomatic_capacity_cost = <scripted maths>  # 仅 IO；每个成员的外交容量成本；root = country, scope:recipient = IO
}
```

## 政策为 IO 定义时可覆盖 IO 的字段（新政策取代旧政策）

- `modifier`（替换成员修正）、`leader_modifier`、`non_leader_modifier`、`owned_location_modifier`、`international_organization_modifier`
- `can_join_trigger` / `can_leave_trigger` / `auto_leave_trigger` / `auto_disband_trigger`
- `join_defensive_wars_always/auto_call/can_call`、`join_offensive_wars_always/auto_call/can_call`（root = IO, scope:actor/scope:recipient/scope:target）
- `can_declare_war`（attacker/defender/recipient = IO）、`has_military_access`（root = int org, actor/recipient/war）
- `<currency type> = <yes/no>`、`min_opinion` / `min_trust` = <float>、`antagonism_towards_leader_modifier`、`antagonism_modifier_for_taking_land_from_fellow_member`、`no_cb_price_modifier_for_fellow_member`
- `payments_implemented` / `payments_repealed` = { <payment tags> }
- `special_statuses_implemented` / `special_statuses_repealed` = { <status tags> }
- `leader_title_key` / `title_is_suffix` / `leader` / `leader_type` / `leader_change_trigger_type` / `leader_change_method` / `leadership_election_resolution` / `months_between_leader_changes` / `has_leader_country` / `has_parliament` / `can_invite_countries` / `gives_food_access_to_members` / `has_dynastic_power`

## 审查要点

- 覆盖语义：IO 政策的 `modifier`/`leader_modifier`/`non_leader_modifier` 是**替换**先前修正；要叠加用 country/province/location_modifier。
- 政策引用的 payment/special status/parliament_type 须在对应类目存在。
- `law_country_goup` 为原版拼写（可能是笔误但原版如此）。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\laws\` 全量 **30 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `law_category` | 196 | 30 | socioeconomic（50）、administrative（49）、religious（40）、military（22）、estates（11） |
| `has_levels` | 9 | 1 | yes（9） |
| `law_country_group` | 2 | 1 | ENG（1）、NAV（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`law_category`**（13 种）：socioeconomic（50）、administrative（49）、religious（40）、military（22）、estates（11）、centralization（8）、election（5）、elector（4）、common_laws（2）、free_city（2）、foundation（1）、foreign_policy（1）、leadership（1）
- **`type`**（1 种）：international_organization（93）
- **`requires_vote`**（2 种）：no（25）、yes（24）
- **`unique`**（1 种）：yes（42）
- **`law_gov_group`**（3 种）：republic（5）、theocracy（4）、monarchy（3）
- **`has_levels`**（1 种）：yes（9）
- **`law_country_group`**（2 种）：ENG（1）、NAV（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `antagonism_modifier_for_taking_land_from_fellow_member` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `antagonism_towards_leader_modifier` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `auto_disband_trigger` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `auto_leave_trigger` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `can_declare_war` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `can_invite_countries` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `can_join_trigger` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `can_leave_trigger` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `country_modifier` | 0 次（30 档） | **有**（出现在 14 个类目） |
| `days` | 0 次（30 档） | **有**（出现在 11 个类目） |
| `diplomatic_capacity_cost` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `gives_food_access_to_members` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `has_dynastic_power` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `has_leader_country` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `has_military_access` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `has_parliament` | 0 次（30 档） | **有**（出现在 8 个类目） |
| `international_organization_modifier` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `join_defensive_wars_always` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `join_defensive_wars_auto_call` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `join_defensive_wars_can_call` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `join_offensive_wars_always` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `join_offensive_wars_auto_call` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `join_offensive_wars_can_call` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `law_country_goup` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `Laws` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `leader` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `leader_change_method` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `leader_change_trigger_type` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `leader_modifier` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `leader_title_key` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `leader_type` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `leadership_election_resolution` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `location_modifier` | 0 次（30 档） | **有**（出现在 14 个类目） |
| `min_opinion` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `min_trust` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `modifier` | 0 次（30 档） | **有**（出现在 39 个类目） |
| `months` | 0 次（30 档） | **有**（出现在 15 个类目） |
| `months_between_leader_changes` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `no_cb_price_modifier_for_fellow_member` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `non_leader_modifier` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `on_activate` | 0 次（30 档） | **有**（出现在 4 个类目） |
| `on_deactivate` | 0 次（30 档） | **有**（出现在 5 个类目） |
| `on_fully_activated` | 0 次（30 档） | **有**（出现在 3 个类目） |
| `on_pay_price` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `owned_location_modifier` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `payments_implemented` | 0 次（30 档） | **有**（出现在 2 个类目） |
| `payments_repealed` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `Policies` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `price` | 0 次（30 档） | **有**（出现在 10 个类目） |
| `province_modifier` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `reasons_to_join` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `show_tags_in_ui` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `special_statuses_implemented` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `special_statuses_repealed` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `title_is_suffix` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `wants_keep_policy` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `wants_propose_policy` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `wants_this_policy_bias` | 0 次（30 档） | **有**（出现在 1 个类目） |
| `weeks` | 0 次（30 档） | 全库也没有 → 疑似废弃字段 |
| `years` | 0 次（30 档） | **有**（出现在 23 个类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `curtail_peasants_policy` | 1 | 1 |
| `enclosure_allowed` | 1 | 1 |
| `sankin_kotai_policy` | 1 | 1 |
| `revert_to_the_crown` | 1 | 1 |
| `society_of_jesus_allowed` | 1 | 1 |
| `priest_marriage_not_allowed` | 1 | 1 |
| `commercial_mission_policy` | 1 | 1 |
| `zimbabwe_mining_law` | 1 | 1 |
| `landfriede_rank_4_policy` | 1 | 1 |
| `flower_garland_school_policy` | 1 | 1 |
| `monogamous_marriage` | 1 | 1 |
| `military_settlements_policy` | 1 | 1 |
| `continue_the_imperial_examination` | 1 | 1 |
| `the_liberty_of_ancients_vs_moderns_policy` | 1 | 1 |
| `russkaya_pravda_policy` | 1 | 1 |
| … | 另有 25 种 | |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `country_modifier` | 917 | curtail_peasants_policy、reformed_hussite_armies_policy、commercialist_principle、doktrinalnaya_ohrana |
| `value` | 868 | ai_will_do、scope:actor、wants_propose_policy、save_temporary_scope_value_as |
| `add` | 849 | else_if、else、wants_propose_policy、every_international_organization_member |
| `desc` | 685 | add、multiply、subtract |
| `years` | 645 | eng_order_of_the_garter_nobility_policy、couteume_de_normaundie、allow_christological_debates_tenet、decentralized_farming_villages |
| `estate_preferences` | 494 | eng_order_of_the_garter_nobility_policy、inherited_clan_holdings、novgorod_judicial_charter_policy、mansabdari_system_policy |
| `limit` | 345 | if、every_known_country、trigger_if、else_if |
| `if` | 300 | else_if、on_deactivate、wants_propose_policy、ordered_country_with_special_status_of_type |
| `potential_trigger` | 282 | location_modifier、international_organization_modifier、country_modifier |
| `NOT` | 279 | locked、scope:recipient、potential、international_organization:middle_kingdom |
| `potential` | 251 | secret_police_policy、rajya_policy、theocratic_education、dop_law_assemblee_nationale |
| `multiply` | 234 | if、add_gold、allow、subtract |
| `wants_this_policy_bias` | 231 | sc_remain_in_the_hre、shaktism、union_sovereign_succession_law、dasvandh_tax |
| `on_activate` | 217 | sc_remain_in_the_hre、union_sovereign_succession_law、westernizers、imperial_armory_implementation |
| `custom_tooltip` | 210 | locked、on_deactivate、else_if、if |
