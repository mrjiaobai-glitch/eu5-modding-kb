# common/laws（法律与政策）

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
