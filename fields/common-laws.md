# common/laws（法律与政策）

> **一句话**：法律与政策两档：法律是容器、内含互斥政策，政策的费用、生效时长与三类缩放修正，以及国际组织政策对组织字段的覆盖语义。
> **什么时候看**：写法律与政策、做国际组织政策，或排查修正被替换而非叠加时看。
> **体量**：202 行 · 约 10 分钟通读

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

> **数据源**：`in_game\common\laws\` 全量 **29 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `law_category` | 192 | 29 | socioeconomic（50）、administrative（49）、religious（38）、military（21）、estates（11） |
| `has_levels` | 9 | 1 | yes（9） |
| `law_country_group` | 2 | 1 | NAV（1）、ENG（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`law_category`**（13 种）：socioeconomic（50）、administrative（49）、religious（38）、military（21）、estates（11）、centralization（8）、election（5）、elector（4）、free_city（2）、leadership（1）、foreign_policy（1）、federal（1）、foundation（1）
- **`type`**（1 种）：international_organization（92）
- **`requires_vote`**（2 种）：no（25）、yes（24）
- **`unique`**（1 种）：yes（42）
- **`law_gov_group`**（3 种）：republic（5）、theocracy（4）、monarchy（3）
- **`has_levels`**（1 种）：yes（9）
- **`law_country_group`**（2 种）：NAV（1）、ENG（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `antagonism_modifier_for_taking_land_from_fellow_member` | 0 次（29 档） | **有**（写在别的类目） |
| `antagonism_towards_leader_modifier` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `auto_disband_trigger` | 0 次（29 档） | **有**（写在别的类目） |
| `auto_leave_trigger` | 0 次（29 档） | **有**（写在别的类目） |
| `can_declare_war` | 0 次（29 档） | **有**（写在别的类目） |
| `can_invite_countries` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `can_join_trigger` | 0 次（29 档） | **有**（写在别的类目） |
| `can_leave_trigger` | 0 次（29 档） | **有**（写在别的类目） |
| `country_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `days` | 0 次（29 档） | **有**（写在别的类目） |
| `diplomatic_capacity_cost` | 0 次（29 档） | **有**（写在别的类目） |
| `gives_food_access_to_members` | 0 次（29 档） | **有**（写在别的类目） |
| `has_dynastic_power` | 0 次（29 档） | **有**（写在别的类目） |
| `has_leader_country` | 0 次（29 档） | **有**（写在别的类目） |
| `has_military_access` | 0 次（29 档） | **有**（写在别的类目） |
| `has_parliament` | 0 次（29 档） | **有**（写在别的类目） |
| `international_organization_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `join_defensive_wars_always` | 0 次（29 档） | **有**（写在别的类目） |
| `join_defensive_wars_auto_call` | 0 次（29 档） | **有**（写在别的类目） |
| `join_defensive_wars_can_call` | 0 次（29 档） | **有**（写在别的类目） |
| `join_offensive_wars_always` | 0 次（29 档） | **有**（写在别的类目） |
| `join_offensive_wars_auto_call` | 0 次（29 档） | **有**（写在别的类目） |
| `join_offensive_wars_can_call` | 0 次（29 档） | **有**（写在别的类目） |
| `law_country_goup` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `Laws` | 0 次（29 档） | **有**（写在别的类目） |
| `leader` | 0 次（29 档） | **有**（写在别的类目） |
| `leader_change_method` | 0 次（29 档） | **有**（写在别的类目） |
| `leader_change_trigger_type` | 0 次（29 档） | **有**（写在别的类目） |
| `leader_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `leader_title_key` | 0 次（29 档） | **有**（写在别的类目） |
| `leader_type` | 0 次（29 档） | **有**（写在别的类目） |
| `leadership_election_resolution` | 0 次（29 档） | **有**（写在别的类目） |
| `location_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `min_opinion` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `min_trust` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `months` | 0 次（29 档） | **有**（写在别的类目） |
| `months_between_leader_changes` | 0 次（29 档） | **有**（写在别的类目） |
| `no_cb_price_modifier_for_fellow_member` | 0 次（29 档） | **有**（写在别的类目） |
| `non_leader_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `on_activate` | 0 次（29 档） | **有**（写在别的类目） |
| `on_deactivate` | 0 次（29 档） | **有**（写在别的类目） |
| `on_fully_activated` | 0 次（29 档） | **有**（写在别的类目） |
| `on_pay_price` | 0 次（29 档） | **有**（写在别的类目） |
| `owned_location_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `payments_implemented` | 0 次（29 档） | **有**（写在别的类目） |
| `payments_repealed` | 0 次（29 档） | **有**（写在别的类目） |
| `Policies` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `price` | 0 次（29 档） | **有**（写在别的类目） |
| `province_modifier` | 0 次（29 档） | **有**（写在别的类目） |
| `reasons_to_join` | 0 次（29 档） | **有**（写在别的类目） |
| `show_tags_in_ui` | 0 次（29 档） | **有**（写在别的类目） |
| `special_statuses_implemented` | 0 次（29 档） | **有**（写在别的类目） |
| `special_statuses_repealed` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `title_is_suffix` | 0 次（29 档） | **有**（写在别的类目） |
| `wants_keep_policy` | 0 次（29 档） | **有**（写在别的类目） |
| `wants_propose_policy` | 0 次（29 档） | **有**（写在别的类目） |
| `wants_this_policy_bias` | 0 次（29 档） | **有**（写在别的类目） |
| `weeks` | 0 次（29 档） | 全库也没有 → 疑似废弃字段 |
| `years` | 0 次（29 档） | **有**（写在别的类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `union_cooperative_investment_policy` | 1 | 1 |
| `dop_law_assemblee_nationale` | 1 | 1 |
| `byz_tagmata_policy_upgraded` | 1 | 1 |
| `sustained_discipline_policy` | 1 | 1 |
| `sump_law_warrior_culture` | 1 | 1 |
| `allow_christological_debates_tenet` | 1 | 1 |
| `burghers_mining_law` | 1 | 1 |
| `harem_policy` | 1 | 1 |
| `ghalla_bakshi` | 1 | 1 |
| `council_of_three_lands_policy` | 1 | 1 |
| `society_of_jesus_not_allowed` | 1 | 1 |
| `no_veneration` | 1 | 1 |
| `byz_porphyrogennetos_court_policy` | 1 | 1 |
| `jesuits_not_allowed` | 1 | 1 |
| `black_army_policy` | 1 | 1 |
| … | 另有 25 种 | |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `country_modifier` | 905 | protection_of_the_poor、dop_law_castilian_courts_reinforced、al_mamalik_al_sultaniyya、complete_religious_autonomy_policy |
| `value` | 865 | subtract、divide、scale、value |
| `add` | 854 | every_international_organization_member、?、scale、value |
| `desc` | 685 | add、multiply、subtract、value |
| `years` | 634 | bullion_coins、court_of_culture_policy、copper_coins、exceptional_only |
| `estate_preferences` | 488 | bullion_coins、court_of_culture_policy、copper_coins、exceptional_only |
| `limit` | 302 | every_international_organization_member、if、every_known_country、trigger_if |
| `potential_trigger` | 276 | location_modifier、international_organization_modifier、country_modifier |
| `if` | 266 | every_international_organization_member、on_deactivate、ordered_country_with_special_status_of_type、? |
| `potential` | 249 | french_east_india_company、epic_tradition_education、nobles_electorate_policy、zaidi_policy |
| `NOT` | 238 | OR、custom_tooltip、?、has_limited_diplomacy |
| `wants_this_policy_bias` | 231 | sc_defensive_pact、no_monetary_contribution、sinicized_administration_policy、power_of_the_emperor_law |
| `on_activate` | 215 | ministry_of_revenue、no_monetary_contribution、nobles_electorate_policy、only_imperial_religion_group_policy |
| `custom_tooltip` | 205 | OR、on_deactivate、join_offensive_wars_always、join_offensive_wars_can_call |
| `multiply` | 197 | if、add、subtract、scope:actor |
