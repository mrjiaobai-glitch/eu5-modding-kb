# common/international_organizations（国际组织系列）

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
