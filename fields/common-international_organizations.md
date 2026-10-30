# common/international_organizations（国际组织系列）

> **一句话**：国际组织系列的压缩字段表：组织本体、土地所有权规则、付款与特殊地位四档，另附国际组织专用的触发器、效果与链接清单。
> **什么时候看**：做国际组织内容、查国际组织脚本接口，或核对唯一类型链接限制时看。
> **体量**：259 行 · 约 12 分钟通读

覆盖 readme：`in_game\common\international_organizations\readme.txt`、`international_organization_land_ownership_rules\readme.txt`、`international_organization_payments\readme.txt`、`international_organization_special_statuses\readme.txt`

## international_organizations 字段（压缩表）

```
<io_type> = {
  # 外观/UI
  should_show_ruler_history=<yes/no>; background_texture=<path>; show_strength_comparison_with_target=<yes/no>
  show_on_diplomatic_map=<yes/no>; show_as_overlord_on_map_trigger=<trigger>(root=member,scope:recipient=IO)
  override_ruler_title=<yes/no>; leader_title_key=<string>(character 领导自动加 _MALE/_FEMALE); title_is_suffix=<yes/no>
  leader_color/member_color/target_color=<color>; map_color_override/secondary_map_color_override=<script color>; tooltip=<loc 键>(均 root=location,scope:recipient=IO)
  # 目标/敌人
  declare_war_on_target_casus_belli=<cb tag>; has_target=<yes/no>; potential_target_trigger/can_target_trigger=<trigger>(root=enemy)
  has_enemies=<yes/no>; can_be_enemy_trigger=<trigger>(root=enemy,recipient=org)
  # 创建/加入
  create_visible/enabled_trigger=<trigger>(root=creator,默认yes); invite_/join_visible/enabled=<trigger>(root=inviter,scope:recipient=IO,scope:target)
  can_invite_countries=<yes/no>(默认yes); subject_limited=<yes/no>(默认yes)
  can_join_trigger(root=joiner,actor/recipient/target); can_leave_trigger/auto_leave_trigger
  auto_disband_trigger(root=IO,scope:target); disband_if_no_leader=<yes/no>(默认yes); disband_message_trigger(root=IO,actor)
  # 领导
  has_leader_country=<yes/no>(默认no); leader=<effect>(root=org,add_to_list=leaders)
  leader_type=<none/country/character>(默认country); use_regnal_number=<yes/no>
  leader_change_trigger_type=<none/rulerchange/timed>; leader_change_method=<rotation/vote/lottery/score/none>
  leadership_election_resolution=<resolution key>; months_between_leader_changes=<int>
  can_lead_trigger(root=leader); leader_score=<script value>(method=score 时)
  # 议会/决议
  max_active_resolutions=<int>; has_parliament=<yes/no>(默认no); parliament_type=<parliament_type>(须为 IO 定义)
  resolution_widget=<widget>; can_initiate_policy_votes=<trigger>(root=country,recipient=IO); ai_issue_voting_bias=<script value>
  # 战争
  join_defensive/offensive_wars_always/auto_call/can_call=<trigger>(root=IO,scope:actor=caller,scope:recipient=callee,scope:target)
  only_leader_country_joins_defensive/offensive_wars=<yes/no>(默认no); takes_over_wars_when_called=<yes/no>(默认no)
  joins_defensive/offensive_wars_as_co_belligerent=<yes/no>(默认no)
  can_declare_war=<trigger>(attacker/defender/recipient=IO); has_military_access/fleet_basing_rights=<trigger>(root=org,actor/recipient/war)
  gives_military_access_to_all_when_at_war=<yes/no>
  # 成员建筑权限
  can_recruit_regiments_in_members/can_build_ships_in_members/can_build_roads_in_members/can_build_buildings_in_members/can_build_rgos_in_members=<yes/no>
  has_buildings=<yes/no>(链接建筑恒归 IO 领袖、IO 被毁则销毁、IO 有钱则代付)
  # 关系
  opinion_bonus/opinion_trust=<float>; min_opinion/min_trust=<float>(不满足 IO 破裂,大 IO 昂贵慎用)
  antagonism_towards_leader_modifier/antagonism_modifier_for_taking_land_from_fellow_member/antagonism_modifier_for_taking_land_from_member_as_outsider/no_cb_price_modifier_for_fellow_member=<float>
  # 外交容量/货币/吞并
  diplomatic_capacity_cost=<script value>(root=country,scope:recipient=IO); <currency type>=<yes/no>(IO 国库含该货币)
  allow_member_annexation=<yes/no>; annexation_min_years_before=<int script value>; can_annex_members/can_annex_visible=<trigger>
  annexation_speed=<float script value>(tooltip 无作用域,用 if={limit={exists=root}} else={} 形式)
  # 驱逐/条约
  expel_members_who_are_targets_of_other_members/_who_target_the_leader/_who_are_attackers_at_war_with_other_members/_who_are_defenders_at_war_with_other_members=<yes/no>
  annulled_by_peace_treaty=<yes/no>; annullment_favours_required=<int>
  # 其他
  unique=<yes/no>(全世界唯一); custom_name=<customizable_localization 键>; land_ownership_rule=<规则 tag>
  gives_food_access_to_members=<yes/no>(默认no); use_laws_as_join_reason=<yes/no>(默认yes); show_leave_message=<yes/no>(默认yes)
  has_dynastic_power=<yes/no>; on_creation/on_disband/monthly_effect=<effect>(root=IO)
  on_joined/on_left=<effect>(root=country,scope:recipient=org,scope:target)
  variables={<var>={format/change_format=<string> monthly_change=<scripted value> start=<script> min/max=<float> hidden/monthly_change_hidden=<yes/no>}}
  special_statuses_implemented={<tags>}; payments_implemented={<tags>}(初始实施,可经法律/政策增删)
  ai_desire_to_join=<script value>(root=joiner,actor=leader,recipient,target); ai_desire_to_allow_new_member(root=IO,actor,target); ai_desire_to_attack_other_members(root=attacker,defender,recipient,target)
  modifier/leader_modifier/non_leader_modifier/target_modifier=<scaled modifier>(scale:root=country,scope:recipient=org)
  owned_location_modifier=<modifier>(不缩放); international_organization_modifier=<modifier>(root=org)
}
```

## 可用脚本

- IO 触发器：total_members、total_enemies、any_international_organization_member/enemy/owned_location(首行可 count=<x> 或 percent=<x>)、is_international_organization_unique、location_can_be_added_to/removed_from_international_organization、law_/policy_visible/enabled_to_international_organization、international_organization_can_own_land、international_organization_type、has_international_organization_modifier、has_elections、has_special_status_available、country_has_special_status{type=special_status:<> country=<>}、leader_type、leader_change_method、leader_change_trigger_type、months_between_leader_changes
- IO 效果：international_organization_member/owned_location(every/random/ordered)、add/remove_country/enemy/location/policy_to_international_organization、international_organization_chooses_new_leader、international_organization_add/remove_special_status{type=special_status:<> country=<>}、add_international_organization_modifier{name=... days/months/years=x mode=add/extend/replace/add_and_extend <size=x>}、remove_international_organization_modifier
- IO 链接：target、leader、leadership_election_resolution、international_organization:<type_tag>(**仅 unique 类型**)
- 国家触发器：international_organizations_member_of/_target_of(all/any)、is_member_of_international_organization_of_type、is_member_of/enemy_of_international_organization、has_special_status_in_international_organization{type=special_status:<> international_organization=<>}、can_lead_international_organization、country_can_join_international_organization
- 国家效果：international_organizations_member_of/_target_of(every/random)、create_international_organization(创建并进入该作用域)
- location 触发器：is_owned_by_international_organization

## land_ownership_rules

```
<rule> = { modifier=<modifier>(IO 拥有的 location); can_add_trigger/can_remove_trigger=<trigger>(root=IO)
  can_add_location_trigger/can_remove_location_trigger=<trigger>(root=location,recipient=IO); on_added/on_removed=<effect>(root=location,recipient=IO)
  ai_desire_to_add=<script value>(root=location,recipient=IO); owned_location_color=<color>(条纹)
  removed_by_peace_treaty=<yes/no>(默认no,仅能经和平条约移除); remove_war_score_modifier=<double>(仅前者生效时) }
```

## payments

```
<payment> = { get_payer_list/get_payee_list=<effect>(root=IO,add_to_local_variable_list=payers/payees 填表,可为国家或阶层)
  price=<base>; price_multiplier=<script value>(root=IO); uses_maintenance=<yes/no>; maintenance_modifier=<modifier>
  proportion_for_payer/proportion_for_payee=<script value>(root=potential country,scope:recipient=IO)
  min_slider_value=<float>(0..1,默认0.5); ai_maintenance_value=<script value>(0..1,留空用 AI_DEFAULT_IO_MAINETANCE_VALUE define)
  ai_maintenance_ignore_saving=<yes/no>(默认no) }
```

## special_statuses

```
<tag> = { priority=<int>(默认1,0=等同普通成员); can_bestow_trigger/auto_bestowal_trigger/auto_dismissal_trigger=<trigger>(root=country,scope:recipient=org,scope:source)
  max_countries=<script value>(root=IO,scope:source); on_bestowed_effect/on_rescinded_effect=<effect>(root=country,scope:recipient,scope:source)
  modifier=<modifier>; leader_modifier=<modifier>(×持状态国家数); map_color=<script color>; special_status_power=<script value>(io 议会议题用) }
```

## 审查要点

- `international_organization:<type_tag>` 链接仅对 unique 类型有效。
- 自动意见/信任：`io_opinion_<key>`、`io_trust_<key>` 自动施加于成员。
- 未在 readme 中说明：各类型本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\international_organizations\` 全量 **34 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `disband_minimum_member_count` | 13 | 13 | 0（8）、1（5） |
| `situation_relevant` | 3 | 3 | 100（3） |
| `alert_view_tab` | 2 | 2 | parliament（1）、overview（1） |
| `join_defensive_wars` | 2 | 2 | Never（2） |
| `join_offensive_wars` | 2 | 2 | Never（2） |
| `fog_of_war_lifted` | 2 | 2 | yes（2） |
| `max_circles_at_formation` | 1 | 1 | 10（1） |
| `promote_strongest_member_to_war_leader` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`leader_type`**（3 种）：country（21）、character（11）、none（3）
- **`expel_members_who_are_targets_of_other_members`**（2 种）：no（23）、yes（12）
- **`has_leader_country`**（2 种）：yes（32）、no（1）
- **`has_target`**（2 种）：no（27）、yes（4）
- **`unique`**（2 种）：yes（23）、no（8）
- **`disband_if_no_leader`**（2 种）：no（15）、yes（9）
- **`show_on_diplomatic_map`**（1 种）：yes（22）
- **`leader_color`**（9 种）：define:NMapColors|INTERNATIONAL_ORGANIZATION_LEADER_COLOR（11）、map_iw_italian_2（1）、map_iw_hre（1）、map_iw_balkan（1）、map_iw_iberia（1）、map_iw_italian_1（1）、map_iw_france（1）、map_iw_italian_3（1）、war_of_religions_catholic_league_leader_color（1）
- **`land_ownership_rule`**（8 种）：italian_wars_land_ownership（7）、hre_land_ownership（1）、swiss_confederation_land_ownership（1）、middle_kingdom_land_ownership（1）、sect_land_ownership（1）、japanese_shogunate_land_ownership（1）、high_kingship_land_ownership（1）、lordship_of_ireland_land_ownership（1）
- **`disband_minimum_member_count`**（2 种）：0（8）、1（5）
- **`leader_change_trigger_type`**（3 种）：none（6）、rulerchange（4）、timed（3）
- **`expel_members_who_are_attackers_at_war_with_other_members`**（1 种）：yes（12）
- **`custom_name`**（12 种）：sect_name（1）、independence_movement_name（1）、colonial_federation_name（1）、marriage_union_name（1）、hindu_branch_name（1）、union_name（1）、crusade_name（1）、high_kingship_name（1）、tribal_confederation_name（1）、autocephalous_patriarchate_name（1）、lordship_of_ireland_name（1）、jurchen_confederation_name（1）
- **`leader_change_method`**（3 种）：none（5）、score（4）、vote（2）
- **`expel_members_who_are_defenders_at_war_with_other_members`**（2 种）：no（7）、yes（3）
- **`leader_title_key`**（10 种）："JAPANESE_SHOGUNATE_LEADER"（1）、"TATAR_YOKE_LEADER"（1）、"EMPEROR_CHINA"（1）、"high_kingship_LEADER"（1）、"lordship_of_ireland_LEADER"（1）、"SWISS_CONFEDERATION_LEADER"（1）、"HRE_LEADER"（1）、"ILKHANATE_LEADER"（1）、"SIKHISM_LEADER"（1）、"CATHOLIC_CHURCH_LEADER"（1）
- **`gives_food_access_to_members`**（1 种）：yes（9）
- **`member_color`**（8 种）：map_iw_france_member（1）、map_iw_italian_1_member（1）、war_of_religions_catholic_league_color（1）、map_iw_italian_3_member（1）、map_iw_iberia_member（1）、map_iw_hre_member（1）、map_iw_italian_2_member（1）、map_iw_balkan_member（1）
- **`can_recruit_regiments_in_members`**（1 种）：yes（7）
- **`antagonism_modifier_for_taking_land_from_fellow_member`**（3 种）：0.5（5）、1.25（1）、0.1（1）
- …另有 40 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `gold` | 3 | 5 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `add_country_to_international_organization` | 0 次（34 档） | **有**（出现在 8 个类目） |
| `add_enemy_to_international_organization` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `add_international_organization_modifier` | 0 次（34 档） | **有**（出现在 3 个类目） |
| `add_location_to_international_organization` | 0 次（34 档） | **有**（出现在 7 个类目） |
| `add_policy_to_international_organization` | 0 次（34 档） | **有**（出现在 8 个类目） |
| `allow_member_annexation` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `annexation_speed` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `antagonism_towards_leader_modifier` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `any_international_organization_enemy` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `any_international_organization_member` | 0 次（34 档） | **有**（出现在 11 个类目） |
| `any_international_organization_owned_location` | 0 次（34 档） | **有**（出现在 4 个类目） |
| `background_texture` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `can_build_buildings_in_members` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `can_build_rgos_in_members` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `can_build_roads_in_members` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `can_invite_countries` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `can_lead_international_organization` | 0 次（34 档） | **有**（出现在 4 个类目） |
| `change_format` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `country_can_join_international_organization` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `country_has_special_status` | 0 次（34 档） | **有**（出现在 19 个类目） |
| `create_international_organization` | 0 次（34 档） | **有**（出现在 6 个类目） |
| `format` | 0 次（34 档） | **有**（出现在 3 个类目） |
| `has_elections` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `has_international_organization_modifier` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `has_special_status_available` | 0 次（34 档） | **有**（出现在 3 个类目） |
| `has_special_status_in_international_organization` | 0 次（34 档） | **有**（出现在 18 个类目） |
| `hidden` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `international_organization` | 0 次（34 档） | **有**（出现在 18 个类目） |
| `international_organization_add_special_status` | 0 次（34 档） | **有**（出现在 7 个类目） |
| `international_organization_can_own_land` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `international_organization_chooses_new_leader` | 0 次（34 档） | **有**（出现在 6 个类目） |
| `international_organization_member` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `international_organization_owned_location` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `international_organization_remove_special_status` | 0 次（34 档） | **有**（出现在 8 个类目） |
| `international_organization_type` | 0 次（34 档） | **有**（出现在 21 个类目） |
| `international_organizations_member_of` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `international_organizations_target_of` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `io_opinion_` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `io_trust_` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `is_enemy_of_international_organization` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `is_international_organization_unique` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `is_member_of_international_organization` | 0 次（34 档） | **有**（出现在 29 个类目） |
| `is_member_of_international_organization_of_type` | 0 次（34 档） | **有**（出现在 11 个类目） |
| `is_owned_by_international_organization` | 0 次（34 档） | **有**（出现在 11 个类目） |
| `join_defensive_wars_can_call` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `join_enabled_trigger` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `join_offensive_wars_can_call` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `joins_offensive_wars_as_co_belligerent` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `law_enabled_to_international_organization` | 0 次（34 档） | **有**（出现在 3 个类目） |
| `law_visible_to_international_organization` | 0 次（34 档） | **有**（出现在 3 个类目） |
| `location_can_be_added_to_international_organization` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `location_can_be_removed_from_international_organization` | 0 次（34 档） | **有**（出现在 2 个类目） |
| `map_color_override` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `max` | 0 次（34 档） | **有**（出现在 25 个类目） |
| `min` | 0 次（34 档） | **有**（出现在 19 个类目） |
| `min_opinion` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `min_trust` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| `monthly_change` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `monthly_change_hidden` | 0 次（34 档） | **有**（出现在 1 个类目） |
| `only_leader_country_joins_offensive_wars` | 0 次（34 档） | 全库也没有 → 疑似废弃字段 |
| … | 另有 15 个 | |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `laws` | 5 | 5 |
| `join_diplo_chance` | 1 | 1 |
| `can_lead_tooltip_trigger` | 1 | 1 |
| `can_vote_in_parliament` | 1 | 1 |
| `imperial_circle_leader_modifier` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 545 | every_country_with_special_status_of_type、every_foreign_building_countries_in_location、random_avatar_for_god、every_country_with_coalition_grade_antagonism_against_us |
| `value` | 486 | scope:target_country、scope:recipient、subtract、root |
| `desc` | 370 | multiply、add、subtract |
| `add` | 353 | scale、every_international_organization_owned_location、value、ai_desire_to_allow_new_member |
| `if` | 340 | hidden_effect、on_joined、scale、every_country |
| `not` | 259 | can_lead_trigger、can_target_trigger、OR、var:union_heir_character |
| `multiply` | 203 | scope:actor、subtract、every_international_organization_enemy、root |
| `always` | 145 | can_join_trigger、join_defensive_wars_always、auto_leave_trigger、create_visible_trigger |
| `scope:recipient` | 139 | can_join_trigger、value、limit、on_joined |
| `OR` | 137 | any_owned_location、or、random_international_organization_owned_location、NOT |
| `subtract` | 122 | scope:actor、subtract、every_country_with_coalition_grade_antagonism_against_us、value |
| `root` | 94 | random_international_organization_owned_location、scope:actor、scope:league_leader_current、limit |
| `trigger_if` | 93 | can_lead_tooltip_trigger、join_defensive_wars_always、auto_leave_trigger、create_visible_trigger |
| `remove_country_from_international_organization` | 85 | international_organization:italian_league_1、international_organization:ghibellines_io、international_organization:foreign_league_france、international_organization:foreign_league_balkan |
| `exists` | 81 | AND、NOT、can_join_trigger、scope:recipient |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `gold` | 3 | 5 |
