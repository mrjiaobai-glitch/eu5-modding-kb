# 参考资料：原版可复用脚本库（scripted_effects / scripted_triggers / scripted_lists）

> **用途**：写 mod 时直接看原版有哪些可复用逻辑（效果/触发器/列表），避免重复造轮子；名字可直接在自己脚本里调用，带 `$参数$` 的需传参。
> **来源**：对 `game\in_game\common\scripted_effects|scripted_triggers|scripted_lists` 的静态提取（顶层条目名 + 文件 + 上方注释首行），**EU5 1.4.5**，2026-10-30。
> **体量**：**检索用清单，不必通读**。

## 目录

- [scripted_effects（可复用效果）（566 条）](#scripted_effects可复用效果566-条)
- [scripted_triggers（可复用触发器）（555 条）](#scripted_triggers可复用触发器555-条)
- [scripted_lists（可复用列表）（8 条）](#scripted_lists可复用列表8-条)
- [与其它档的关系](#与其它档的关系)

## scripted_effects（可复用效果）（566 条）

> 调用方式：在效果上下文直接 `名 = yes`（带 `$参数$` 的传参）

| 名称 | 文件 | 注释首行 |
|---|---|---|
| `abstain_vote` | international_organization_effects.txt | Only usable for resolutions which allow a null object target |
| `add_3_noble_privileges` | country_effects.txt |  |
| `add_all_estate_satisfaction` | country_effects.txt |  |
| `add_all_possible_pentarchies` | country_effects.txt |  |
| `add_casus_belli_mutual_effect` | country_effects.txt |  |
| `add_character_to_first_open_cabinet` | country_effects.txt |  |
| `add_character_to_random_open_cabinet` | country_effects.txt |  |
| `add_decree_monthly_celestial_authority_loss` | country_effects.txt |  |
| `add_effect_scaling_with_societal_value` | country_effects.txt | yearly_sailors |
| `add_hafsid_governor_content` | tunis_effects.txt |  |
| `add_head_of_cabinet_modifier` | country_effects.txt | Update the game concepts when you add a new flavor title for the prime minister |
| `add_historical_rival_scripted` | country_effects.txt |  |
| `add_influence_over_granada_towards_trend` | situation_effects.txt | influence > 0 (toward MOR) -> +$value$ ; influence < 0 (toward CAS) -> -$value$. |
| `add_italian_wars_militarization_demands` | situation_effects.txt |  |
| `add_opinion_mutual_effect` | country_effects.txt |  |
| `add_organization_donation_opinion_bonus` | country_effects.txt |  |
| `add_pentarchy_modifier` | country_effects.txt |  |
| `add_random_dip_regent_trait` | character_effects.txt |  |
| `add_random_dip_ruler_trait` | character_effects.txt |  |
| `add_random_positive_cabinet_trait` | character_effects.txt | so the effect is safe on any character and the eight unconditional traits keep the list no… |
| `add_religious_currency_pilgrimage` | religious_effects.txt |  |
| `add_religious_influence_if_valid` | religious_effects.txt |  |
| `add_replacing_gov_reform` | country_effects.txt |  |
| `add_to_revolutionary_list` | country_effects.txt |  |
| `add_trust_equilibrium_mutual_effect` | country_effects.txt |  |
| `adopt_compatible_heir_religion_law` | country_effects.txt |  |
| `ai_international_organization_diplo_action_cooldown` | country_effects.txt |  |
| `align_subject_curia_votes_effect` | religious_effects.txt |  |
| `annex_country_diplomatically` | country_effects.txt |  |
| `apply_all_union_policy_effects` | international_organization_effects.txt |  |
| `apply_independent_patriarchate_default_policies` | international_organization_effects.txt |  |
| `apply_modifier_on_all_members` | international_organization_effects.txt |  |
| `apply_modifier_on_all_members_excluding_leader` | international_organization_effects.txt |  |
| `apply_parliament_bribe` | international_organization_effects.txt |  |
| `apply_policy_voting_cooldown_effect` | international_organization_effects.txt |  |
| `apply_pu_estate_satisfaction_hit` | international_organization_effects.txt |  |
| `apply_pu_opponents_estate_satisfaction_hit` | international_organization_effects.txt |  |
| `apply_pu_supporters_estate_satisfaction_hit` | international_organization_effects.txt | ################################################ |
| `apply_same_inheritance_policy_effect` | international_organization_effects.txt |  |
| `apply_same_laws_policy_effect` | international_organization_effects.txt |  |
| `apply_societal_value_push_modifiers` | country_effects.txt |  |
| `apply_union_levels` | international_organization_effects.txt |  |
| `assimilate_group_into_merged_culture` | culture_unification.txt |  |
| `assimilate_location` | location_effects.txt |  |
| `assimilate_location_with_randomness` | location_effects.txt |  |
| `banish_character` | character_effects.txt | This effect give your character 15/40/30 at worst and a 30/60/80 at best. |
| `banish_character_to_country_effect` | character_effects.txt |  |
| `block_disaster_effect` | country_effects.txt |  |
| `block_disaster_temporarily_effect` | country_effects.txt |  |
| `break_alliance` | country_effects.txt |  |
| `break_union` | country_effects.txt |  |
| `byz_set_up` | on_action_effects.txt |  |
| `cache_distance_map` | tunis_effects.txt | Create a local variable map "preferred_city" that will point the location to the closest e… |
| `call_io_parliament` | international_organization_effects.txt |  |
| `call_parliament_for_law_change` | international_organization_effects.txt |  |
| `call_recursive_scripted_effect` | global_effects.txt |  |
| `cancel_bfts_gold_trade` | country_effects.txt |  |
| `cap_parliament_support` | international_organization_effects.txt |  |
| `cas_create_cultural_revolt` | country_effects.txt |  |
| `cast_vote` | international_organization_effects.txt |  |
| `catastrophic_destroy_building_levels_in_location` | location_effects.txt |  |
| `cc_setup_new_town` | country_effects.txt |  |
| `change_all_pop_satisfaction` | location_effects.txt |  |
| `change_celestial_authority` | international_organization_effects.txt | ############################################# |
| `change_culture_for_ruler_and_family` | country_effects.txt |  |
| `change_estate_gold_effect` | country_gold_effects.txt |  |
| `change_gold_effect` | country_gold_effects.txt |  |
| `change_gold_effect_bounded` | country_gold_effects.txt | Example: change_gold_effect_bounded = { scale = -2 min = 2 max = 1000 } |
| `change_granadan_sovereignty_effect` | situation_effects.txt |  |
| `change_imperial_authority` | international_organization_effects.txt |  |
| `change_influence_over_granada_effect` | situation_effects.txt |  |
| `change_location_rank_effect` | location_effects.txt | Megalopolis target is intentionally not handled here; megalopolis upgrades go through the … |
| `change_mutual_religious_view` | religious_effects.txt |  |
| `change_opinion_of_expedition` | country_effects.txt |  |
| `change_papal_authority` | international_organization_effects.txt | ############################################# |
| `change_protestant_view` | religious_effects.txt |  |
| `change_religion_for_ruler_and_family` | country_effects.txt |  |
| `change_tag_cosmetic` | country_effects.txt |  |
| `change_union_integration_level` | international_organization_effects.txt |  |
| `check_situation_and_change_granadan_sovereignty` | situation_effects.txt |  |
| `chi_adjust_pre_rtr_modifiers` | country_effects.txt |  |
| `chi_expedition_resolution_negative` | country_effects.txt |  |
| `chi_expedition_resolution_positive` | country_effects.txt |  |
| `chi_expedition_resolution_very_negative` | country_effects.txt |  |
| `chi_expedition_resolution_very_positive` | country_effects.txt |  |
| `china_set_up` | on_action_effects.txt |  |
| `chinese_expedition_calculate_gift_for_this` | country_effects.txt |  |
| `chinese_treasure_depart_from_port` | country_effects.txt |  |
| `claim_non_conflicting_owned_areas` | situation_effects.txt |  |
| `cleanup_chinese_treasure_voyage_state` | country_effects.txt |  |
| `clear_crisis_of_faith_heresiarch_effect` | crisis_of_faith_effects.txt |  |
| `clear_gui_variable` | global_effects.txt |  |
| `clear_individual_integration_levels` | international_organization_effects.txt |  |
| `clr_government_name` | country_effects.txt |  |
| `compare_and_get_highest_variable_from_4` | global_effects.txt |  |
| `construct_latest_fort_building` | location_effects.txt |  |
| `convert_location` | location_effects.txt |  |
| `convert_location_with_randomness` | location_effects.txt |  |
| `convert_pops_in_location_to_catholic` | location_effects.txt |  |
| `council_of_trent_abstain_effect` | situation_effects.txt |  |
| `council_of_trent_force_end_effect` | situation_effects.txt | #################################### |
| `council_of_trent_pass_effect` | situation_effects.txt |  |
| `council_of_trent_proposal_effect` | situation_effects.txt |  |
| `council_of_trent_reject_effect` | situation_effects.txt |  |
| `council_of_trent_vote_effect` | situation_effects.txt |  |
| `create_alliance` | country_effects.txt |  |
| `create_familial_governor_in_location_effect` | tunis_effects.txt |  |
| `create_gui_variable` | global_effects.txt |  |
| `create_instant_rebellion` | global_effects.txt | Make absolutely sure that it has a guaranteed end and does not run into a potential infini… |
| `create_newborn_female_ruler_child` | country_effects.txt |  |
| `create_newborn_ruler_child` | country_effects.txt |  |
| `create_some_heretics` | location_effects.txt | Create some heretics in catholic locations |
| `create_union_of_integration_level` | country_effects.txt |  |
| `create_union_of_level` | international_organization_effects.txt |  |
| `create_work_of_art_with_artist_input` | location_effects.txt |  |
| `crusade_effect` | religious_effects.txt |  |
| `crusade_track_contribution` | holy_war_effects.txt | Holy-war (Crusade / Jihad) shared scripted effects. |
| `damage_regiment` | ___test_effects.txt |  |
| `decrease_opinion_of_expedition` | country_effects.txt |  |
| `decrease_opinion_of_expedition_greatly` | country_effects.txt |  |
| `demote_to_destroyed_canal` | location_effects.txt |  |
| `destroy_all_italian_leagues` | international_organization_effects.txt | ################################################ |
| `destroy_every_sub_unit_of_type` | unit_effects.txt |  |
| `disable_disaster_effect` | country_effects.txt |  |
| `disable_mission_effect` | country_effects.txt |  |
| `disable_situation_effect` | situation_effects.txt |  |
| `disable_treasure_voyages` | country_effects.txt |  |
| `dispatch_bad_mining_event` | expedition_effects.txt | Called from mining_survey_expedition on_arrive_to_waypoint when the bad-outcome roll wins. |
| `dispatch_good_mining_event` | expedition_effects.txt | Country scope; scope:location must point at the surveyed location. |
| `distribute_world` | situation_effects.txt | #################################### |
| `donate_gold_to_international_organization` | country_effects.txt |  |
| `downgrade_pentarchy_modifier` | country_effects.txt |  |
| `enable_disaster_effect` | country_effects.txt |  |
| `enable_mission_effect` | country_effects.txt |  |
| `enable_situation_effect` | situation_effects.txt | rise_of_timur |
| `enable_treasure_voyages` | country_effects.txt |  |
| `enforce_culture_effect` | country_effects.txt |  |
| `enforce_religion_effect` | country_effects.txt |  |
| `enslave_pop` | location_effects.txt | root = pop |
| `establish_cape_route_post` | expedition_effects.txt | Usage:  scope:location = { establish_cape_route_post = { gold_scale = -3 feitoria_count = … |
| `event_illustration_culture_effect` | 00_event_illustration_effects.txt | event_illustration_culture_effect = { culture = culture:catalan } |
| `event_illustration_estate_background_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_estate_effect` | 00_event_illustration_effects.txt | event_illustration_estate_effect = { foreground = estate_type:nobles_estate   background =… |
| `event_illustration_estate_foreground_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_estate_in_location_effect` | 00_event_illustration_effects.txt | location = { event_illustration_estate_in_location_effect = { estate_type = estate_type:bu… |
| `event_illustration_government_estate_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_nobility_estate_of_neighbor_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_nobility_estate_of_rival_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_peasants_foreign_religion_pop_type_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_poptype_background_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_poptype_effect` | 00_event_illustration_effects.txt | event_illustration_poptype_effect = { foreground = pop_type:nobles   background = pop_type… |
| `event_illustration_poptype_foreground_effect` | 00_event_illustration_effects.txt |  |
| `event_illustration_religion_effect` | 00_event_illustration_effects.txt | event_illustration_religion_effect = { religion = religion:orthodox } |
| `excommunicate_effect` | religious_effects.txt |  |
| `execute_prisoners_effect` | prisoner_effects.txt | root = unit |
| `fail_vote` | international_organization_effects.txt |  |
| `fix_grand_canal_repair` | location_effects.txt |  |
| `flag_situation_event` | situation_effects.txt |  |
| `force_convert_religion` | religious_effects.txt |  |
| `four_yearly_country_pulse_50_percent_effect` | on_action_effects.txt |  |
| `gain_special_status_parliament_issue_support` | international_organization_effects.txt |  |
| `get_ottomans_equivalent_scope` | global_effects.txt |  |
| `get_pope_character_scope` | global_effects.txt | Or to manipulate variables |
| `grant_benefits_to_estate` | rebel_negotiate_effects.txt |  |
| `grant_effects_of_opinion` | country_effects.txt |  |
| `grant_seven_cities_expedition_loot_effect` | seven_cities_effects.txt |  |
| `grant_special_unit_experience` | unit_effects.txt | root = unit |
| `gt_connections_courtly_effect` | expedition_effects.txt |  |
| `gt_experience_handson_effect` | expedition_effects.txt |  |
| `gt_friend_encounter_roll_effect` | expedition_effects.txt |  |
| `gt_knowledge_study_effect` | expedition_effects.txt |  |
| `gt_opportunity_chain_roll_effect` | expedition_effects.txt |  |
| `gt_party_effect` | expedition_effects.txt |  |
| `gt_party_trait_roll_effect` | expedition_effects.txt |  |
| `gt_prodigy_connections_reward_effect` | expedition_effects.txt |  |
| `gt_prodigy_experience_reward_effect` | expedition_effects.txt |  |
| `gt_prodigy_knowledge_reward_effect` | expedition_effects.txt |  |
| `gt_roll_hidden_path_effect` | expedition_effects.txt |  |
| `hafsid_caliphate_effect` | tunis_effects.txt |  |
| `hafsid_succession_effect` | tunis_effects.txt |  |
| `hafsid_succession_unite_two_countries` | tunis_effects.txt | If TUN tag is involved, make sure the annexing country is TUN or that it becomes TUN |
| `hajj_display_of_wealth_effect` | hajj_caravan_effects.txt |  |
| `harmony_mild_towards_equilibrium` | religious_effects.txt |  |
| `harmony_weak_towards_equilibrium` | religious_effects.txt |  |
| `hire_prisoners_as_mercenaries_effect` | prisoner_effects.txt | root = unit |
| `holy_war_desertion_penalty_effect` | holy_war_effects.txt |  |
| `holy_war_discipline_penalty_effect` | holy_war_effects.txt |  |
| `holy_war_levy_payment_effect` | holy_war_effects.txt |  |
| `holy_war_payout_equal_share` | holy_war_effects.txt |  |
| `holy_war_payout_largesse` | holy_war_effects.txt |  |
| `holy_war_payout_war_end` | holy_war_effects.txt |  |
| `holy_war_plunder_to_treasury_effect` | holy_war_effects.txt |  |
| `holy_war_scaled_gold_transfer_effect` | holy_war_effects.txt |  |
| `holy_war_snapshot_participation` | holy_war_effects.txt |  |
| `holy_war_victory_credit_effect` | holy_war_effects.txt |  |
| `horde_raid_location` | location_effects.txt |  |
| `hre_allow_emperor_reform_proposals` | international_organization_effects.txt |  |
| `hre_bind_electorship_to_religion` | international_organization_effects.txt |  |
| `hre_bind_electorship_to_religion_group` | international_organization_effects.txt |  |
| `hre_bind_voting_to_religion` | international_organization_effects.txt |  |
| `hre_bind_voting_to_religion_group` | international_organization_effects.txt |  |
| `hre_break_free_city_emperor_subject` | international_organization_effects.txt |  |
| `hre_clear_emperor_from_electorship` | international_organization_effects.txt |  |
| `hre_convert_free_city_to_emperor_subject` | international_organization_effects.txt |  |
| `hre_disable_all_imperial_laws` | international_organization_effects.txt |  |
| `hre_disable_direct_free_city_subjects` | international_organization_effects.txt |  |
| `hre_disable_free_cities` | international_organization_effects.txt |  |
| `hre_disable_free_cities_protection` | international_organization_effects.txt |  |
| `hre_disable_free_city_subjects` | international_organization_effects.txt |  |
| `hre_disable_imperial_privileges` | international_organization_effects.txt |  |
| `hre_disallow_emperor_reform_proposals` | international_organization_effects.txt |  |
| `hre_enable_all_imperial_laws` | international_organization_effects.txt |  |
| `hre_enable_direct_free_city_subjects` | international_organization_effects.txt |  |
| `hre_enable_free_cities` | international_organization_effects.txt |  |
| `hre_enable_free_cities_protection` | international_organization_effects.txt |  |
| `hre_enable_free_city_subjects` | international_organization_effects.txt |  |
| `hre_enable_imperial_privileges` | international_organization_effects.txt |  |
| `hre_enact_fixed_succession` | international_organization_effects.txt |  |
| `hre_grant_elector_role` | international_organization_effects.txt |  |
| `hre_imperial_circle_set_ai_diplo_action_cooldown` | international_organization_effects.txt |  |
| `hre_join_imperial_circle_effect` | hre_circle_formation_effects.txt | Usage: hre_join_imperial_circle_effect = { circle = <circle_scope> } |
| `hre_lock_law` | international_organization_effects.txt | ################################################ |
| `hre_on_reform_level_passed_effect` | international_organization_effects.txt |  |
| `hre_on_reform_level_passed_opinion_effect_all_members` | international_organization_effects.txt |  |
| `hre_on_reform_level_passed_opinion_effect_electors` | international_organization_effects.txt |  |
| `hre_on_reform_level_passed_opinion_effect_free_cities` | international_organization_effects.txt |  |
| `hre_on_reform_passed_effect` | international_organization_effects.txt |  |
| `hre_parliament_debate_failed_effect` | international_organization_effects.txt |  |
| `hre_repeal_fixed_succession` | international_organization_effects.txt |  |
| `hre_set_ai_diplo_action_cooldown` | international_organization_effects.txt |  |
| `hre_set_imperial_religion` | international_organization_effects.txt | christian |
| `hre_set_imperial_religion_group_value` | international_organization_effects.txt |  |
| `hre_set_imperial_religion_value` | international_organization_effects.txt |  |
| `hre_setup_hre_variables` | international_organization_effects.txt |  |
| `hre_start_circle_formation` | hre_circle_formation_effects.txt | Kicks off the one-time dynamic formation process when the Imperial Circles law passes. |
| `hre_unbind_electorship_from_religion` | international_organization_effects.txt |  |
| `hre_unbind_voting_from_religion` | international_organization_effects.txt |  |
| `hre_unlock_law` | international_organization_effects.txt |  |
| `hre_update_kingdom_title` | international_organization_effects.txt |  |
| `hussite_wars_add_country_to_side_effect` | situation_effects.txt |  |
| `hussite_wars_add_war_joiner_to_side_effect` | situation_effects.txt |  |
| `hussite_wars_clear_sides_effect` | situation_effects.txt |  |
| `hussite_wars_recompute_relative_strength_effect` | situation_effects.txt |  |
| `hussite_wars_resolve_current_war_effect` | situation_effects.txt |  |
| `hussite_wars_update_sides_effect` | situation_effects.txt |  |
| `ifriqiya_reunited_decision_effect` | tunis_effects.txt |  |
| `ignore_succesion_effect` | country_effects.txt |  |
| `imprison_character_effect` | character_effects.txt |  |
| `imprison_character_in_country_effect` | character_effects.txt |  |
| `increase_opinion_of_expedition` | country_effects.txt |  |
| `increase_opinion_of_expedition_greatly` | country_effects.txt |  |
| `interdict_effect` | religious_effects.txt |  |
| `international_organization_lock_global_law_effect` | global_effects.txt |  |
| `international_organization_lock_law_effect` | international_organization_effects.txt |  |
| `international_organization_lock_parliament_agenda_effect` | international_organization_effects.txt |  |
| `international_organization_lock_parliament_issue_effect` | international_organization_effects.txt |  |
| `international_organization_lock_parliament_type_effect` | international_organization_effects.txt |  |
| `international_organization_lock_policy_effect` | international_organization_effects.txt |  |
| `international_organization_unlock_global_law_effect` | global_effects.txt |  |
| `international_organization_unlock_law_effect` | international_organization_effects.txt |  |
| `international_organization_unlock_parliament_agenda_effect` | international_organization_effects.txt |  |
| `international_organization_unlock_parliament_issue_effect` | international_organization_effects.txt |  |
| `international_organization_unlock_parliament_type_effect` | international_organization_effects.txt |  |
| `international_organization_unlock_policy_effect` | international_organization_effects.txt |  |
| `iw_foreign_league_triumph_events` | international_organization_effects.txt |  |
| `iw_increase_tension_effect` | situation_effects.txt |  |
| `iw_italian_league_triumph_events` | international_organization_effects.txt |  |
| `iw_reset_tension_effect` | situation_effects.txt |  |
| `japan_set_up` | on_action_effects.txt |  |
| `jihad_track_contribution` | holy_war_effects.txt |  |
| `join_situation_faction` | situation_effects.txt |  |
| `leave_situation_faction` | situation_effects.txt |  |
| `lift_excommunication_effect` | religious_effects.txt |  |
| `lift_interdict_effect` | religious_effects.txt |  |
| `limit_variable` | global_effects.txt |  |
| `limit_variable_to_max` | global_effects.txt | Do not use it with timed variables though |
| `limit_variable_to_min` | global_effects.txt |  |
| `lock_advance_effect` | country_effects.txt |  |
| `lock_artist_type_effect` | country_effects.txt |  |
| `lock_building_effect` | country_effects.txt |  |
| `lock_cabinet_action_effect` | country_effects.txt |  |
| `lock_casus_belli_effect` | country_effects.txt |  |
| `lock_character_interaction_effect` | country_effects.txt |  |
| `lock_child_education_effect` | country_effects.txt |  |
| `lock_country_interaction_effect` | country_effects.txt |  |
| `lock_current_government_reforms` | country_effects.txt |  |
| `lock_current_heir_selection` | country_effects.txt |  |
| `lock_current_law` | country_effects.txt |  |
| `lock_current_policy` | country_effects.txt |  |
| `lock_current_subject_type` | country_effects.txt |  |
| `lock_estate_privilege_effect` | country_effects.txt |  |
| `lock_generic_action_effect` | country_effects.txt |  |
| `lock_global_law_effect` | global_effects.txt |  |
| `lock_global_policy_effect` | global_effects.txt |  |
| `lock_government_reform_effect` | country_effects.txt |  |
| `lock_heir_selection_effect` | country_effects.txt |  |
| `lock_law_effect` | country_effects.txt |  |
| `lock_levy_effect` | country_effects.txt |  |
| `lock_mission_effect` | country_effects.txt |  |
| `lock_mission_task_effect` | country_effects.txt |  |
| `lock_parliament_agenda_effect` | country_effects.txt |  |
| `lock_parliament_issue_effect` | country_effects.txt |  |
| `lock_parliament_type_effect` | country_effects.txt |  |
| `lock_peace_treaty_effect` | country_effects.txt |  |
| `lock_policy_effect` | country_effects.txt |  |
| `lock_production_method_effect` | country_effects.txt |  |
| `lock_recruitment_method_effect` | country_effects.txt |  |
| `lock_regency_type_effect` | country_effects.txt |  |
| `lock_relation_type_effect` | country_effects.txt |  |
| `lock_religious_aspect_effect` | religious_effects.txt |  |
| `lock_resolution_effect` | international_organization_effects.txt |  |
| `lock_societal_value_effect` | country_effects.txt |  |
| `lock_unit_ability_effect` | country_effects.txt |  |
| `lock_unit_effect` | country_effects.txt |  |
| `lock_work_of_art_definition_effect` | country_effects.txt |  |
| `lots_of_religion` | country_effects.txt |  |
| `maghreb_set_up` | on_action_effects.txt |  |
| `majapahit_set_up` | on_action_effects.txt |  |
| `major_destroy_building_levels_in_location` | location_effects.txt |  |
| `make_into_expedition_leader` | country_effects.txt |  |
| `make_regent_to_ruler` | country_effects.txt |  |
| `make_the_great_scope` | country_effects.txt |  |
| `minor_destroy_building_levels_in_location` | location_effects.txt | Prev = building |
| `move_art_and_owner` | country_effects.txt |  |
| `move_building` | location_effects.txt | root = location |
| `move_capital_event_effect` | country_effects.txt |  |
| `move_country_with_family` | character_effects.txt |  |
| `move_imperial_work_of_art` | country_effects.txt |  |
| `move_to_expedition` | character_effects.txt |  |
| `nanbokuchou_change_sides_effect` | situation_effects.txt | #################################### |
| `nanbokuchou_declare_neutrality_effect` | situation_effects.txt |  |
| `nudge_towards_isolationism` | country_effects.txt |  |
| `nudge_towards_openness` | country_effects.txt |  |
| `on_bribe_default_effect` | country_effects.txt |  |
| `on_country_formation_effect` | on_action_effects.txt |  |
| `on_ibadi_ruler_change` | on_action_effects.txt | has_policy = yes/no |
| `on_timurid_occupy_location_effect` | situation_effects.txt |  |
| `on_union_policy_change_effect` | international_organization_effects.txt |  |
| `pacify_rebel_pops` | rebel_negotiate_effects.txt | root = rebel |
| `parliament_debate_failed_effect` | country_effects.txt |  |
| `participate_in_parliament` | international_organization_effects.txt |  |
| `pay_gold_org_country_effect` | country_gold_effects.txt |  |
| `pilgrimage_holy_site_arrival_effect` | religious_effects.txt |  |
| `promote_to_head_of_cabinet_effect` | country_effects.txt |  |
| `promote_to_head_of_cabinet_effect_apply_country_modifier` | country_effects.txt |  |
| `promote_to_head_of_cabinet_effect_base` | country_effects.txt |  |
| `propose_parliament_issue` | international_organization_effects.txt |  |
| `raid_set_up_effect` | on_action_effects.txt |  |
| `ransom_prisoners_effect` | prisoner_effects.txt | root = unit |
| `recalculate_parliament_vote_for_law_change` | international_organization_effects.txt |  |
| `recalculate_parliament_vote_for_law_change_for_agenda` | international_organization_effects.txt |  |
| `recalculate_union_parliament_issue_support` | international_organization_effects.txt |  |
| `reconcile_hre_league_leadership` | situation_effects.txt |  |
| `reconcile_italian_wars_league_land` | situation_effects.txt |  |
| `reconcile_single_italian_wars_league_land` | situation_effects.txt |  |
| `reinstate_the_pope` | religious_effects.txt |  |
| `reject_hegemon_ultimatum_effect` | country_effects.txt |  |
| `release_character_from_prison_effect` | character_effects.txt |  |
| `relic_expedition_announce_holy_lance_effect` | expedition_effects.txt |  |
| `relic_expedition_announce_holy_shroud_effect` | expedition_effects.txt |  |
| `relic_expedition_announce_true_cross_effect` | expedition_effects.txt |  |
| `relic_expedition_grant_minor_relic_effect` | expedition_effects.txt |  |
| `relic_expedition_grant_relic_effect` | expedition_effects.txt |  |
| `relic_expedition_grant_relic_effect_procedural` | expedition_effects.txt |  |
| `remove_all_pentarchy_modifiers` | country_effects.txt |  |
| `remove_cardinal_variables` | on_action_effects.txt | character scope |
| `remove_casus_belli_mutual_effect` | country_effects.txt |  |
| `remove_current_major_government_reform` | country_effects.txt |  |
| `remove_decree_monthly_celestial_authority_loss` | country_effects.txt |  |
| `remove_from_revolutionary_list` | country_effects.txt |  |
| `remove_head_of_cabinet_country_effect` | country_effects.txt | Character effect called in the on_cabinet_assigned on_action |
| `remove_head_of_cabinet_effect` | country_effects.txt |  |
| `remove_high_kingship_reclamation_cb_effect` | international_organization_effects.txt |  |
| `remove_historical_rival_scripted` | country_effects.txt |  |
| `remove_italian_wars_militarization_demands` | situation_effects.txt |  |
| `remove_opinion_mutual_effect` | country_effects.txt |  |
| `remove_pentarchy_modifier` | country_effects.txt |  |
| `remove_revolution` | country_effects.txt |  |
| `remove_trust_equilibrium_mutual_effect` | country_effects.txt |  |
| `replaceCountryInTordesillas` | situation_effects.txt |  |
| `reset_all_societal_values` | country_effects.txt |  |
| `reset_influence_over_granada_effect` | situation_effects.txt |  |
| `reset_societal_value` | country_effects.txt |  |
| `rise_of_the_ottomans_recalculate_top_3` | situation_effects.txt | #################################### |
| `rot_increase_kill_counter` | situation_effects.txt | #################################### |
| `rtr_change_allegiance_points` | situation_effects.txt | #################################### |
| `rtr_core_all_of_china_effect` | situation_effects.txt |  |
| `rtr_grant_annexation_cb_in` | situation_effects.txt |  |
| `rtr_recreate_middle_kingdom` | situation_effects.txt |  |
| `rustamid_legacy_effect` | country_effects.txt |  |
| `sacrifice_prisoners_effect` | prisoner_effects.txt | root = unit |
| `save_arrival_holy_site_effect` | expedition_effects.txt | Scripted effects for expedition events |
| `save_route_to_the_target_effect` | location_effects.txt |  |
| `save_ruler_regent_as_target_character` | country_effects.txt |  |
| `seed_crisis_of_faith_heresiarch_effect` | crisis_of_faith_effects.txt |  |
| `select_insignificant_gov_reform` | country_effects.txt |  |
| `select_or_create_braganca_ruler` | country_effects.txt |  |
| `select_suitable_patriarch` | on_action_effects.txt |  |
| `sell_prisoners_into_slavery_effect` | prisoner_effects.txt | root = unit |
| `sengoku_appoint_new_shogun` | situation_effects.txt |  |
| `sengoku_reevaluate_strongest_daimyos` | situation_effects.txt |  |
| `set_as_designated_heir_effect` | country_effects.txt |  |
| `set_as_designated_heir_with_reason_effect` | country_effects.txt |  |
| `set_as_part_of_grand_canal` | location_effects.txt |  |
| `set_character_as_the_great` | country_effects.txt |  |
| `set_character_erratic_behavior` | character_effects.txt |  |
| `set_character_siblings_scope` | global_effects.txt | Used for localization's sake |
| `set_chinese_expedition_scopes` | country_effects.txt |  |
| `set_country_rank_effect` | country_effects.txt |  |
| `set_ended_disaster_flag_effect` | country_effects.txt |  |
| `set_government_name` | country_effects.txt |  |
| `set_individual_integration_level` | international_organization_effects.txt |  |
| `set_mutual_religious_view` | religious_effects.txt |  |
| `set_new_ruler_with_union` | country_effects.txt |  |
| `set_new_ruler_with_union_fratricide` | country_effects.txt |  |
| `set_new_ruler_with_union_if_senior` | country_effects.txt |  |
| `set_nickname_if_none_present` | character_effects.txt |  |
| `set_papacy_active` | religious_effects.txt |  |
| `set_papacy_inactive` | religious_effects.txt |  |
| `set_papal_states_schism_cardinal` | religious_effects.txt |  |
| `set_patriarch_variables` | on_action_effects.txt |  |
| `set_pentarch_variable` | on_action_effects.txt |  |
| `set_revolution_effect` | country_effects.txt |  |
| `set_revolution_target_effect` | country_effects.txt |  |
| `set_schism_opponent_country` | religious_effects.txt |  |
| `set_schism_opponent_country_schism_cardinal` | religious_effects.txt |  |
| `set_senior_partner` | international_organization_effects.txt |  |
| `set_situation_end_flag_effect` | global_effects.txt |  |
| `set_to_default_heir_selection` | country_effects.txt |  |
| `set_to_limited_random_artist_skill` | character_effects.txt |  |
| `set_to_limited_random_stats` | character_effects.txt | For character scope |
| `set_up_cardinal` | on_action_effects.txt | location scope |
| `set_up_patriarch` | on_action_effects.txt | $organization$ = autocephalous patriarchate IO, $leader$ = leader_country |
| `set_up_patriarch_for_io` | on_action_effects.txt |  |
| `set_up_proper_china_for_middle_kingdom` | international_organization_effects.txt |  |
| `setup_area_preferences` | on_action_effects.txt |  |
| `setup_colonial_nation` | country_effects.txt |  |
| `setup_societal_values` | country_effects.txt |  |
| `seven_cities_expedition_calamity_effect` | seven_cities_effects.txt |  |
| `seven_cities_expedition_stall_effect` | seven_cities_effects.txt |  |
| `show_all_event_targets` | ___test_effects.txt |  |
| `show_broken_event_targets` | ___test_effects.txt |  |
| `show_event_target_advance_type` | ___test_effects.txt |  |
| `show_event_target_artist_type` | ___test_effects.txt |  |
| `show_event_target_avatar` | ___test_effects.txt |  |
| `show_event_target_building_type` | ___test_effects.txt |  |
| `show_event_target_cabinet_action` | ___test_effects.txt |  |
| `show_event_target_casus_belli` | ___test_effects.txt |  |
| `show_event_target_character_interaction` | ___test_effects.txt |  |
| `show_event_target_child_education` | ___test_effects.txt |  |
| `show_event_target_climate` | ___test_effects.txt |  |
| `show_event_target_country_interaction` | ___test_effects.txt |  |
| `show_event_target_country_rank` | ___test_effects.txt |  |
| `show_event_target_culture` | ___test_effects.txt |  |
| `show_event_target_culture_group` | ___test_effects.txt |  |
| `show_event_target_demand` | ___test_effects.txt |  |
| `show_event_target_disaster_type` | ___test_effects.txt |  |
| `show_event_target_disease` | ___test_effects.txt |  |
| `show_event_target_estate_privilege` | ___test_effects.txt |  |
| `show_event_target_estate_type` | ___test_effects.txt |  |
| `show_event_target_formable_country` | ___test_effects.txt |  |
| `show_event_target_generic_action` | ___test_effects.txt |  |
| `show_event_target_god` | ___test_effects.txt |  |
| `show_event_target_goods` | ___test_effects.txt |  |
| `show_event_target_government_reform` | ___test_effects.txt |  |
| `show_event_target_government_type` | ___test_effects.txt |  |
| `show_event_target_hegemony` | ___test_effects.txt |  |
| `show_event_target_heir_selection` | ___test_effects.txt |  |
| `show_event_target_holy_site_definition` | ___test_effects.txt |  |
| `show_event_target_holy_site_type` | ___test_effects.txt |  |
| `show_event_target_institution` | ___test_effects.txt |  |
| `show_event_target_international_organization` | ___test_effects.txt |  |
| `show_event_target_language` | ___test_effects.txt |  |
| `show_event_target_language_family` | ___test_effects.txt |  |
| `show_event_target_law` | ___test_effects.txt |  |
| `show_event_target_levy_setup` | ___test_effects.txt |  |
| `show_event_target_location_rank` | ___test_effects.txt |  |
| `show_event_target_mission` | ___test_effects.txt |  |
| `show_event_target_mission_task` | ___test_effects.txt |  |
| `show_event_target_parliament_agenda` | ___test_effects.txt |  |
| `show_event_target_parliament_issue` | ___test_effects.txt |  |
| `show_event_target_peace_treaty` | ___test_effects.txt |  |
| `show_event_target_policy` | ___test_effects.txt |  |
| `show_event_target_pop_type` | ___test_effects.txt |  |
| `show_event_target_production_method` | ___test_effects.txt |  |
| `show_event_target_recruitment_method` | ___test_effects.txt |  |
| `show_event_target_regency_type` | ___test_effects.txt |  |
| `show_event_target_relation_type` | ___test_effects.txt |  |
| `show_event_target_religion` | ___test_effects.txt |  |
| `show_event_target_religion_group` | ___test_effects.txt |  |
| `show_event_target_religious_aspect` | ___test_effects.txt |  |
| `show_event_target_religious_faction` | ___test_effects.txt |  |
| `show_event_target_religious_focus` | ___test_effects.txt |  |
| `show_event_target_religious_school` | ___test_effects.txt |  |
| `show_event_target_resolution` | ___test_effects.txt |  |
| `show_event_target_societal_value_type` | ___test_effects.txt |  |
| `show_event_target_sub_unit_category` | ___test_effects.txt |  |
| `show_event_target_subject_type` | ___test_effects.txt |  |
| `show_event_target_topography` | ___test_effects.txt |  |
| `show_event_target_trait` | ___test_effects.txt |  |
| `show_event_target_unit_ability` | ___test_effects.txt |  |
| `show_event_target_unit_type` | ___test_effects.txt |  |
| `show_event_target_vegetation` | ___test_effects.txt |  |
| `show_event_target_work_of_art_definition` | ___test_effects.txt |  |
| `start_parliament_effect` | country_effects.txt |  |
| `start_western_schism_resolution` | religious_effects.txt | situation effect |
| `subtract_imperial_authority` | international_organization_effects.txt |  |
| `subtract_influence_over_granada_trend` | situation_effects.txt | influence < 0 (toward CAS) -> +$value$ ; influence > 0 (toward MOR) -> -$value$. |
| `subtract_papal_authority` | international_organization_effects.txt |  |
| `transfer_claims` | situation_effects.txt |  |
| `transfer_gold_effect_bounded` | country_gold_effects.txt |  |
| `transfer_gold_to_org_country_effect` | country_gold_effects.txt |  |
| `transfer_leadership_to_union_senior_partner` | international_organization_effects.txt |  |
| `transfer_pop` | location_effects.txt | root = pop |
| `transfer_yearly_gold_from_all_members` | international_organization_effects.txt |  |
| `transfer_yearly_manpower_from_all_members` | international_organization_effects.txt |  |
| `tumu_crisis_restore_emperor` | character_effects.txt |  |
| `turn_japanese_clan_into_building_based` | country_effects.txt |  |
| `turn_japanese_clan_into_normal_tag` | country_effects.txt |  |
| `unlock_advance_effect` | country_effects.txt |  |
| `unlock_artist_type_effect` | country_effects.txt |  |
| `unlock_building_effect` | country_effects.txt |  |
| `unlock_cabinet_action_effect` | country_effects.txt |  |
| `unlock_casus_belli_effect` | country_effects.txt |  |
| `unlock_character_interaction_effect` | country_effects.txt |  |
| `unlock_child_education_effect` | country_effects.txt |  |
| `unlock_country_interaction_effect` | country_effects.txt |  |
| `unlock_current_government_reforms` | country_effects.txt |  |
| `unlock_current_heir_selection` | country_effects.txt |  |
| `unlock_current_law` | country_effects.txt |  |
| `unlock_current_policy` | country_effects.txt |  |
| `unlock_current_subject_type` | country_effects.txt |  |
| `unlock_estate_privilege_effect` | country_effects.txt |  |
| `unlock_generic_action_effect` | country_effects.txt |  |
| `unlock_global_law_effect` | global_effects.txt |  |
| `unlock_global_policy_effect` | global_effects.txt |  |
| `unlock_government_reform_effect` | country_effects.txt |  |
| `unlock_heir_selection_effect` | country_effects.txt |  |
| `unlock_law_effect` | country_effects.txt |  |
| `unlock_levy_effect` | country_effects.txt |  |
| `unlock_mission_effect` | country_effects.txt |  |
| `unlock_mission_task_effect` | country_effects.txt |  |
| `unlock_parliament_agenda_effect` | country_effects.txt |  |
| `unlock_parliament_issue_effect` | country_effects.txt |  |
| `unlock_parliament_type_effect` | country_effects.txt |  |
| `unlock_peace_treaty_effect` | country_effects.txt |  |
| `unlock_policy_effect` | country_effects.txt |  |
| `unlock_production_method_effect` | country_effects.txt |  |
| `unlock_recruitment_method_effect` | country_effects.txt |  |
| `unlock_regency_type_effect` | country_effects.txt |  |
| `unlock_relation_type_effect` | country_effects.txt |  |
| `unlock_religious_aspect_effect` | religious_effects.txt |  |
| `unlock_resolution_effect` | international_organization_effects.txt | ################################################ |
| `unlock_societal_value_effect` | country_effects.txt |  |
| `unlock_unit_ability_effect` | country_effects.txt |  |
| `unlock_unit_effect` | country_effects.txt |  |
| `unlock_work_of_art_definition_effect` | country_effects.txt |  |
| `update_chinese_treasure_leg_progress` | country_effects.txt |  |
| `update_cloth_silk_guild_industry_variable` | on_action_effects.txt |  |
| `update_leader_country` | international_organization_effects.txt |  |
| `upgrade_location_rank` | location_effects.txt | root - location |
| `upgrade_pentarchy_modifier` | country_effects.txt |  |
| `upset_countries_of_religion` | country_effects.txt |  |
| `ver_set_up` | on_action_effects.txt |  |
| `wardens_raid_loss_effect` | religious_effects.txt |  |
| `yua_appoint_as_dictator` | country_effects.txt |  |

## scripted_triggers（可复用触发器）（555 条）

> 调用方式：在触发器上下文直接 `名 = yes`（带 `$参数$` 的传参）

| 名称 | 文件 | 注释首行 |
|---|---|---|
| `actor_has_order_of_calling_trigger` | religious_order_triggers.txt |  |
| `ai_international_organization_diplo_action_cooldown` | international_organization_triggers.txt |  |
| `alliance_can_exist` | country_triggers.txt |  |
| `allows_scaligeri_peace_treaty_for_country` | war_triggers.txt |  |
| `ambrosian_republic_disaster_end_trigger` | disaster_triggers.txt |  |
| `artillery_institution_plausible_location` | institution_triggers.txt |  |
| `aspiration_for_liberty_end_trigger` | disaster_triggers.txt |  |
| `banking_plausible_location` | institution_triggers.txt |  |
| `basic_checks_for_calculating_foreign_league_leaders` | country_triggers.txt |  |
| `battle_for_the_strait_castile_holds_the_strait` | situation_triggers.txt |  |
| `battle_for_the_strait_castile_requirements_met` | situation_triggers.txt |  |
| `battle_for_the_strait_contender_eliminated` | situation_triggers.txt |  |
| `battle_for_the_strait_granada_holds_the_strait` | situation_triggers.txt |  |
| `battle_for_the_strait_granada_requirements_met` | situation_triggers.txt |  |
| `battle_for_the_strait_morocco_holds_the_strait` | situation_triggers.txt |  |
| `battle_for_the_strait_morocco_requirements_met` | situation_triggers.txt |  |
| `byzantine_succession_crisis_end_trigger` | disaster_triggers.txt |  |
| `cabinet_already_performing_same_task_on_target` | cabinet_triggers.txt |  |
| `cabinet_control_area_action_covers_province` | cabinet_triggers.txt |  |
| `cabinet_control_province_action_in_area` | cabinet_triggers.txt |  |
| `can_adopt_compatible_religion_law` | country_triggers.txt |  |
| `can_be_changed_to_via_generic_action` | ibadi_content_triggers.txt |  |
| `can_be_targetted_by_tun_tax_raid` | tunis_triggers.txt |  |
| `can_become_a_leader` | character_triggers.txt |  |
| `can_become_a_regent` | character_triggers.txt |  |
| `can_become_a_ruler` | character_triggers.txt |  |
| `can_become_familial_governor_trigger` | tunis_triggers.txt |  |
| `can_become_strong_imam` | ibadi_content_triggers.txt |  |
| `can_become_weak_imam` | ibadi_content_triggers.txt |  |
| `can_bribe_default_trigger` | country_triggers.txt |  |
| `can_build_satellite_trade_building` | building_triggers.txt | Scope into the TC that owns root.region so the cap is checked on the actual building owner… |
| `can_call_crusade_trigger` | religion_triggers.txt |  |
| `can_change_government_type` | country_triggers.txt |  |
| `can_change_primary_culture` | country_triggers.txt |  |
| `can_claim_ilkhanate` | country_triggers.txt |  |
| `can_clan_get_this_location_when_turning_into_normal_tag` | location_triggers.txt |  |
| `can_country_have_tusi` | country_triggers.txt |  |
| `can_create_subject_alliance` | country_triggers.txt |  |
| `can_declare_legal_war_on` | country_triggers.txt |  |
| `can_fire_situation_event` | situation_triggers.txt |  |
| `can_give_land_to_pope_trigger` | religion_triggers.txt |  |
| `can_have_parliament_called_by` | international_organization_triggers.txt |  |
| `can_outmatch_defending_coalition` | situation_triggers.txt |  |
| `can_potentially_become_familial_governor_trigger` | tunis_triggers.txt |  |
| `can_revoke_tle_support_banu_al_wad_reform` | country_triggers.txt |  |
| `can_select_location_for_familial_governor` | tunis_triggers.txt |  |
| `can_upgrade_country_rank` | country_triggers.txt |  |
| `can_upgrade_country_rank_duchy` | country_triggers.txt |  |
| `can_upgrade_country_rank_empire` | country_triggers.txt |  |
| `can_upgrade_country_rank_kingdom` | country_triggers.txt |  |
| `capital_in_old_world_trigger` | exploration_triggers.txt |  |
| `casus_belli_forbids_location_conquest` | war_triggers.txt |  |
| `casus_belli_forbids_normal_subjugation_tributary` | war_triggers.txt |  |
| `character_address_imperial_ruler_trigger` | character_triggers.txt |  |
| `character_address_royal_ruler_trigger` | character_triggers.txt |  |
| `character_age_filter_trigger` | character_triggers.txt |  |
| `character_bg_use_admiral` | character_background_triggers.txt |  |
| `character_bg_use_artist` | character_background_triggers.txt |  |
| `character_bg_use_burghers` | character_background_triggers.txt |  |
| `character_bg_use_clergy` | character_background_triggers.txt |  |
| `character_bg_use_general` | character_background_triggers.txt |  |
| `character_bg_use_nobles` | character_background_triggers.txt |  |
| `character_bg_use_peasants` | character_background_triggers.txt |  |
| `character_bg_use_royalty` | character_background_triggers.txt |  |
| `character_can_marry_recipient_trigger` | character_triggers.txt |  |
| `character_can_marry_trigger` | character_triggers.txt |  |
| `character_in_combat` | character_triggers.txt |  |
| `character_is_co_ruler_trigger` | character_triggers.txt |  |
| `character_is_the_great_ruler` | country_triggers.txt |  |
| `ciompi_revolt_end_trigger` | disaster_triggers.txt |  |
| `city_illustration_trigger` | location_triggers.txt |  |
| `clan_has_valid_landable_location` | situation_triggers.txt |  |
| `colonial_nation_can_declare_wars_trigger` | country_triggers.txt | scope:defender = country they are trying to declare war on |
| `columbian_exchange_end_trigger` | situation_triggers.txt |  |
| `confessionalism_plausible_location` | institution_triggers.txt |  |
| `controls_and_owns_pentarchy` | religion_triggers.txt |  |
| `country_allows_a_weak_imam` | ibadi_content_triggers.txt |  |
| `country_can_build_in_location` | country_triggers.txt |  |
| `country_can_ennoble_trigger` | country_triggers.txt |  |
| `country_can_pay_tithe` | religion_triggers.txt |  |
| `country_can_promote_head_of_cabinet` | country_triggers.txt |  |
| `country_can_start_exploration_trigger` | exploration_triggers.txt |  |
| `country_has_currently_ruling_dynasty` | country_triggers.txt |  |
| `country_has_dynasty` | country_triggers.txt |  |
| `country_has_gallowglass_trigger` | unit_triggers.txt |  |
| `country_has_no_other_copy_of_building` | building_triggers.txt |  |
| `country_has_recently_joined_situation_faction` | situation_triggers.txt |  |
| `country_has_recently_left_situation_faction` | situation_triggers.txt |  |
| `country_has_sufism` | religion_triggers.txt |  |
| `country_is_senior_partner` | international_organization_triggers.txt |  |
| `country_rank_is_county` | country_triggers.txt |  |
| `country_rank_is_duchy` | country_triggers.txt |  |
| `country_rank_is_empire` | country_triggers.txt |  |
| `country_rank_is_kingdom` | country_triggers.txt |  |
| `country_rank_is_what` | country_triggers.txt |  |
| `coup_attempt_end_trigger` | disaster_triggers.txt |  |
| `court_and_country_end_trigger` | disaster_triggers.txt |  |
| `crisis_of_faith_end_trigger` | disaster_triggers.txt |  |
| `crisis_of_faith_qualifies_trigger` | disaster_triggers.txt |  |
| `crisis_of_the_chinese_dynasty_end_trigger` | disaster_triggers.txt |  |
| `crisis_of_the_sayfawa_dynasty_end_trigger` | disaster_triggers.txt |  |
| `crusade_allow_trigger` | religion_triggers.txt |  |
| `crusade_potential_trigger` | religion_triggers.txt |  |
| `crusade_target_relations_valid_trigger` | religion_triggers.txt |  |
| `current_age_or_later` | game_triggers.txt |  |
| `current_celestial_authority` | international_organization_triggers.txt | ############################################# |
| `current_imperial_authority` | international_organization_triggers.txt |  |
| `current_later_age_1_traditions_trigger` | game_triggers.txt |  |
| `current_later_age_2_renaissance_trigger` | game_triggers.txt |  |
| `current_later_age_3_discovery_trigger` | game_triggers.txt |  |
| `current_later_age_4_reformation_trigger` | game_triggers.txt |  |
| `current_later_age_5_absolutism_trigger` | game_triggers.txt |  |
| `current_later_age_6_revolutions_trigger` | game_triggers.txt |  |
| `current_papal_authority` | international_organization_triggers.txt |  |
| `curse_of_stefan_uros_iii_end_trigger` | disaster_triggers.txt |  |
| `daimyo_is_big_enough_to_be_landed` | situation_triggers.txt |  |
| `death_of_hayan_wuruk_end_trigger` | disaster_triggers.txt |  |
| `decline_of_empire_end_trigger` | disaster_triggers.txt |  |
| `decline_of_majapahit_trigger` | disaster_triggers.txt |  |
| `decline_of_mali_end_trigger` | disaster_triggers.txt |  |
| `discovered_any_in_americas_trigger` | exploration_triggers.txt |  |
| `discovered_any_in_moluccas_trigger` | exploration_triggers.txt |  |
| `discovered_route_to_india` | exploration_triggers.txt |  |
| `dissolution_of_delhi_end_trigger` | disaster_triggers.txt |  |
| `does_not_own_any_copy_of_building_type` | country_triggers.txt |  |
| `emperor_exists` | international_organization_triggers.txt | ############################################### |
| `english_civil_war_end_trigger` | disaster_triggers.txt |  |
| `enlightenment_plausible_location` | institution_triggers.txt |  |
| `event_illustration_background_trigger` | 00_event_illustration_triggers.txt |  |
| `event_illustration_foreground_trigger` | 00_event_illustration_triggers.txt |  |
| `excommunicate_allow_trigger` | religion_triggers.txt |  |
| `excommunicate_target_trigger` | religion_triggers.txt |  |
| `fate_of_the_phoenix_end_trigger` | disaster_triggers.txt |  |
| `frankokratia_vassal_state_can_declare_wars_trigger` | country_triggers.txt |  |
| `french_wars_religion_end_trigger` | disaster_triggers.txt |  |
| `game_has_mission_rewards_enabled` | game_triggers.txt |  |
| `game_has_mission_task_rewards_enabled` | game_triggers.txt |  |
| `game_has_missions_enabled` | game_triggers.txt |  |
| `game_has_victory_cards_enabled` | game_triggers.txt |  |
| `game_is_initialized` | game_triggers.txt |  |
| `game_is_playing_on_easy_mode` | game_triggers.txt |  |
| `game_is_playing_on_hard_mode` | game_triggers.txt |  |
| `get_most_likely_candidate_for_seniority` | international_organization_triggers.txt |  |
| `get_senior_partner` | international_organization_triggers.txt |  |
| `global_trade_plausible_location` | institution_triggers.txt |  |
| `government_reforms_is_not_locked` | country_triggers.txt |  |
| `grand_embassy_host_knows_ruler` | country_triggers.txt |  |
| `guelphs_and_ghibellines_ghibellines_end_trigger` | situation_triggers.txt |  |
| `guelphs_and_ghibellines_guelphs_end_trigger` | situation_triggers.txt |  |
| `guelphs_and_ghibellines_guelphs_stalemate_trigger` | situation_triggers.txt |  |
| `had_disaster_trigger` | disaster_triggers.txt |  |
| `had_for_elective_heir_selection` | country_triggers.txt |  |
| `had_situation_trigger` | situation_triggers.txt |  |
| `hajj_caravan_capital_has_route_to_mecca` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_arabia` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_bab_el_mandeb` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_baghdad` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_cairo` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_damascus` | hajj_caravan_triggers.txt |  |
| `hajj_caravan_routes_via_jeddah` | hajj_caravan_triggers.txt |  |
| `has_abstained_vote` | international_organization_triggers.txt | Only usable for resolutions which allow a null object target |
| `has_access_to_god_holy_site` | religion_triggers.txt |  |
| `has_active_senior_partner` | international_organization_triggers.txt |  |
| `has_all_pentarchy` | country_triggers.txt |  |
| `has_all_pentarchy_except` | country_triggers.txt |  |
| `has_any_disease_present` | location_triggers.txt |  |
| `has_any_guild_building` | building_triggers.txt |  |
| `has_any_pentarchy_modifier` | country_triggers.txt |  |
| `has_blocked_disaster_trigger` | country_triggers.txt |  |
| `has_compatible_heir_religion_law_with` | country_triggers.txt |  |
| `has_consistent_integration_level` | international_organization_triggers.txt |  |
| `has_currency_to_vote_for_law` | international_organization_triggers.txt |  |
| `has_desirable_order_of_chivalry_for_target_country` | country_triggers.txt |  |
| `has_dock_infrastructure` | location_triggers.txt |  |
| `has_enabled_disaster_trigger` | country_triggers.txt |  |
| `has_enabled_mission_trigger` | country_triggers.txt |  |
| `has_enabled_situation_trigger` | situation_triggers.txt | Generic Situation triggers |
| `has_expensive_procure_remedies_goods` | situation_triggers.txt | Black Death Situation Triggers |
| `has_fired_situation_event` | situation_triggers.txt |  |
| `has_for_elective_heir_selection` | country_triggers.txt |  |
| `has_foreign_league_won_the_italian_wars` | situation_triggers.txt |  |
| `has_gold_org_country_trigger` | country_triggers.txt |  |
| `has_good_mining_raw_material` | goods_triggers.txt |  |
| `has_government_name` | country_triggers.txt |  |
| `has_great_or_unique_palace` | building_triggers.txt |  |
| `has_had_situation` | situation_triggers.txt |  |
| `has_imperial_examinations` | country_triggers.txt |  |
| `has_industrial_raw_material` | goods_triggers.txt |  |
| `has_italian_league_won_the_italian_wars` | situation_triggers.txt |  |
| `has_land_military_building` | building_triggers.txt |  |
| `has_locked_religious_aspect_trigger` | religion_triggers.txt |  |
| `has_meaningful_holy_war_participation` | religion_triggers.txt |  |
| `has_midgame_manufactory_building` | building_triggers.txt |  |
| `has_military_producing_building` | building_triggers.txt |  |
| `has_not_consistent_integration_level` | international_organization_triggers.txt |  |
| `has_not_union_integration_level_as_union` | international_organization_triggers.txt |  |
| `has_or_had_great_ruler` | country_triggers.txt |  |
| `has_pentarchy_modifier` | country_triggers.txt |  |
| `has_proposed_parliament_issue_in` | international_organization_triggers.txt |  |
| `has_religion_of_country_heir_law` | character_triggers.txt |  |
| `has_ruler_in_country` | country_triggers.txt |  |
| `has_slavery` | country_triggers.txt |  |
| `has_societal_value_push` | country_triggers.txt |  |
| `has_subjugated_timurids` | situation_triggers.txt |  |
| `has_timurids_stagnate` | situation_triggers.txt |  |
| `has_tribal_government` | country_triggers.txt |  |
| `has_union_integration_level_as_union` | international_organization_triggers.txt |  |
| `has_unlocked_advance_trigger` | country_triggers.txt |  |
| `has_unlocked_artist_type_trigger` | country_triggers.txt |  |
| `has_unlocked_building_trigger` | country_triggers.txt |  |
| `has_unlocked_cabinet_action_trigger` | country_triggers.txt |  |
| `has_unlocked_casus_belli_trigger` | country_triggers.txt |  |
| `has_unlocked_character_interaction_trigger` | country_triggers.txt |  |
| `has_unlocked_child_education_trigger` | country_triggers.txt |  |
| `has_unlocked_country_interaction_trigger` | country_triggers.txt |  |
| `has_unlocked_estate_privilege_trigger` | country_triggers.txt |  |
| `has_unlocked_generic_action_trigger` | country_triggers.txt |  |
| `has_unlocked_global_law_trigger` | country_triggers.txt |  |
| `has_unlocked_global_policy_trigger` | country_triggers.txt |  |
| `has_unlocked_government_reform_trigger` | country_triggers.txt |  |
| `has_unlocked_heir_selection_trigger` | country_triggers.txt |  |
| `has_unlocked_law_trigger` | country_triggers.txt |  |
| `has_unlocked_levy_trigger` | country_triggers.txt |  |
| `has_unlocked_mission_task_trigger` | country_triggers.txt |  |
| `has_unlocked_mission_trigger` | country_triggers.txt |  |
| `has_unlocked_parliament_agenda_trigger` | country_triggers.txt |  |
| `has_unlocked_parliament_issue_trigger` | country_triggers.txt |  |
| `has_unlocked_parliament_type_trigger` | country_triggers.txt |  |
| `has_unlocked_peace_treaty_trigger` | country_triggers.txt |  |
| `has_unlocked_policy_trigger` | country_triggers.txt |  |
| `has_unlocked_production_method_trigger` | country_triggers.txt |  |
| `has_unlocked_recruitment_method_trigger` | country_triggers.txt |  |
| `has_unlocked_regency_type_trigger` | country_triggers.txt |  |
| `has_unlocked_relation_type_trigger` | country_triggers.txt |  |
| `has_unlocked_religious_aspect_trigger` | religion_triggers.txt |  |
| `has_unlocked_resolution_trigger` | international_organization_triggers.txt | ############################################ |
| `has_unlocked_societal_value_trigger` | country_triggers.txt |  |
| `has_unlocked_unit_ability_trigger` | country_triggers.txt |  |
| `has_unlocked_unit_trigger` | country_triggers.txt |  |
| `has_unlocked_work_of_art_definition_trigger` | country_triggers.txt |  |
| `has_valid_policy_vote_proposer` | international_organization_triggers.txt |  |
| `heir_selection_is_not_locked` | country_triggers.txt |  |
| `hellenism_religion_movement_location_trigger` | culture_triggers.txt |  |
| `hobelars_location_trigger` | unit_triggers.txt |  |
| `holy_war_overwhelms_target` | religion_triggers.txt |  |
| `hook_and_cod_wars_end_trigger` | disaster_triggers.txt |  |
| `horde_civil_war_end_trigger` | disaster_triggers.txt |  |
| `hre_allowed_emperor_reform_proposal` | international_organization_triggers.txt |  |
| `hre_can_select_adjacent_reform_level` | international_organization_triggers.txt |  |
| `hre_country_breaks_imperial_religion` | international_organization_triggers.txt |  |
| `hre_country_fulfills_imperial_religion_requirement` | international_organization_triggers.txt |  |
| `hre_country_is_not_of_imperial_religion` | international_organization_triggers.txt |  |
| `hre_country_is_not_of_imperial_religion_group` | international_organization_triggers.txt |  |
| `hre_country_is_of_imperial_religion` | international_organization_triggers.txt |  |
| `hre_country_is_of_imperial_religion_group` | international_organization_triggers.txt |  |
| `hre_disabled_free_cities` | international_organization_triggers.txt |  |
| `hre_disabled_free_cities_protection` | international_organization_triggers.txt |  |
| `hre_disallowed_emperor_reform_proposal` | international_organization_triggers.txt |  |
| `hre_electorship_bound_to_religion` | international_organization_triggers.txt |  |
| `hre_electorship_bound_to_religion_group` | international_organization_triggers.txt |  |
| `hre_electorship_not_bound_to_religion` | international_organization_triggers.txt |  |
| `hre_electorship_not_bound_to_religion_group` | international_organization_triggers.txt |  |
| `hre_electorship_not_unbound_from_religion` | international_organization_triggers.txt |  |
| `hre_electorship_unbound_from_religion` | international_organization_triggers.txt |  |
| `hre_enabled_free_cities` | international_organization_triggers.txt |  |
| `hre_enabled_free_cities_protection` | international_organization_triggers.txt |  |
| `hre_general_bound_to_religion` | international_organization_triggers.txt |  |
| `hre_general_bound_to_religion_group` | international_organization_triggers.txt |  |
| `hre_has_direct_free_city_subjects` | international_organization_triggers.txt |  |
| `hre_has_disabled_all_imperial_laws` | international_organization_triggers.txt |  |
| `hre_has_disabled_imperial_privileges` | international_organization_triggers.txt |  |
| `hre_has_eligible_leader_candidate` | international_organization_triggers.txt |  |
| `hre_has_enabled_all_imperial_laws` | international_organization_triggers.txt |  |
| `hre_has_enabled_imperial_privileges` | international_organization_triggers.txt |  |
| `hre_has_fixed_succession` | international_organization_triggers.txt |  |
| `hre_has_free_city_subjects` | international_organization_triggers.txt |  |
| `hre_has_locked_law` | international_organization_triggers.txt |  |
| `hre_has_not_direct_free_city_subjects` | international_organization_triggers.txt |  |
| `hre_has_not_fixed_succession` | international_organization_triggers.txt |  |
| `hre_has_not_free_city_subjects` | international_organization_triggers.txt |  |
| `hre_imperial_religion` | international_organization_triggers.txt |  |
| `hre_is_in_formation_period` | hre_circle_formation_triggers.txt | True while the HRE Imperial Circles law has been passed but circles are not yet finalized. |
| `hre_is_perpetual_diet_location` | international_organization_triggers.txt |  |
| `hre_num_of_emperor_comfort_policies_active` | international_organization_triggers.txt |  |
| `hre_voting_bound_to_religion` | international_organization_triggers.txt |  |
| `hre_voting_bound_to_religion_group` | international_organization_triggers.txt |  |
| `hre_voting_not_bound_to_religion` | international_organization_triggers.txt |  |
| `hre_voting_not_bound_to_religion_group` | international_organization_triggers.txt |  |
| `hre_voting_not_unbound_from_religion` | international_organization_triggers.txt |  |
| `hre_voting_unbound_from_religion` | international_organization_triggers.txt |  |
| `hundred_years_war_unified_requirements` | situation_triggers.txt |  |
| `hussite_wars_on_opposing_sides` | situation_triggers.txt |  |
| `imperial_work_of_art_can_be_moved` | work_of_art_triggers.txt |  |
| `industrialization_plausible_location` | institution_triggers.txt |  |
| `international_organization_has_locked_law` | international_organization_triggers.txt |  |
| `international_organization_has_unlocked_law_trigger` | international_organization_triggers.txt |  |
| `international_organization_has_unlocked_parliament_agenda_trigger` | international_organization_triggers.txt |  |
| `international_organization_has_unlocked_parliament_issue_trigger` | international_organization_triggers.txt |  |
| `international_organization_has_unlocked_parliament_type_trigger` | international_organization_triggers.txt |  |
| `international_organization_has_unlocked_policy_trigger` | international_organization_triggers.txt |  |
| `invite_prince_candidate_trigger` | character_triggers.txt |  |
| `is_a_praefecta` | character_triggers.txt |  |
| `is_abrahamic` | religion_triggers.txt |  |
| `is_adjacent_to_owned_or_subject_owns` | building_triggers.txt |  |
| `is_allied_with` | country_triggers.txt |  |
| `is_amazigh` | pop_triggers.txt |  |
| `is_aragonese_catalan_spain` | country_triggers.txt |  |
| `is_basque_spain` | country_triggers.txt |  |
| `is_candidate_for_NW_good` | situation_triggers.txt | #################################### |
| `is_candidate_for_OW_good` | situation_triggers.txt |  |
| `is_capital_in_south_america` | location_triggers.txt |  |
| `is_capital_in_south_america_or_carribean` | location_triggers.txt |  |
| `is_capital_mesoamerica` | location_triggers.txt |  |
| `is_capital_of` | location_triggers.txt |  |
| `is_castilian_spain` | country_triggers.txt |  |
| `is_coastal_within_range` | building_triggers.txt |  |
| `is_construction_material` | goods_triggers.txt |  |
| `is_controlled_by_target_or_subject` | location_triggers.txt |  |
| `is_cossacks` | pop_triggers.txt |  |
| `is_country_buddhist` | religion_triggers.txt |  |
| `is_country_religion_folk_american` | religion_triggers.txt |  |
| `is_country_valid_for_tusi_subject` | country_triggers.txt |  |
| `is_culture_native_american` | culture_triggers.txt |  |
| `is_current_treasure_fleet_host` | treasure_fleet_triggers.txt |  |
| `is_dhimmi` | pop_triggers.txt |  |
| `is_elector` | country_triggers.txt |  |
| `is_eligible_for_the_great_ruler_event` | country_triggers.txt |  |
| `is_emperor` | country_triggers.txt |  |
| `is_endemic_bubonic_plague_location` | location_triggers.txt |  |
| `is_endemic_typhus_location` | location_triggers.txt | Diseases |
| `is_excommunicated` | religion_triggers.txt | Excommunication Triggers |
| `is_favoring_god` | religion_triggers.txt |  |
| `is_finalising` | resolution_triggers.txt |  |
| `is_forced_into_kitman_state` | ibadi_content_triggers.txt |  |
| `is_former_or_current_nomadic_steppe_culture` | culture_triggers.txt |  |
| `is_frankokratia_state` | country_triggers.txt |  |
| `is_free_or_tributary_trigger` | country_triggers.txt |  |
| `is_gaelic_clans` | pop_triggers.txt |  |
| `is_galician_spain` | country_triggers.txt |  |
| `is_garrison_strong_enough_to_come_out` | location_triggers.txt |  |
| `is_geographically_correct` | situation_triggers.txt | #################################### |
| `is_hafsid_governor_trigger` | tunis_triggers.txt |  |
| `is_heathen` | religion_triggers.txt |  |
| `is_heretic` | religion_triggers.txt |  |
| `is_historical_route_of_great_wall` | building_triggers.txt |  |
| `is_in_defensive_war` | country_triggers.txt |  |
| `is_in_offensive_war` | country_triggers.txt |  |
| `is_in_revolutionary_list` | country_triggers.txt |  |
| `is_in_union` | country_triggers.txt |  |
| `is_independent_or_autonomous_subject` | country_triggers.txt |  |
| `is_interested_in_news_from_root_trigger` | country_triggers.txt | Generic trigger to check if a country is interested in major political events from ROOT |
| `is_japanese_emperor_or_shogun` | situation_triggers.txt |  |
| `is_junior_partner` | country_triggers.txt |  |
| `is_jurisprudence_school` | religion_triggers.txt |  |
| `is_leader_of_any_league` | country_triggers.txt |  |
| `is_leader_of_international_organization_of_type` | international_organization_triggers.txt |  |
| `is_legally_male` | character_triggers.txt |  |
| `is_leonese_spain` | country_triggers.txt |  |
| `is_location_buddhist` | religion_triggers.txt |  |
| `is_location_claimable` | situation_triggers.txt |  |
| `is_location_claimed_or_owned_by_country` | situation_triggers.txt | Location has a claim or is directly controlled by a Main country |
| `is_location_colonial` | situation_triggers.txt |  |
| `is_location_dominant_religion_folk_american` | religion_triggers.txt |  |
| `is_location_dominant_religion_pagan` | religion_triggers.txt |  |
| `is_locked_government_reform` | country_triggers.txt |  |
| `is_locked_heir_selection` | country_triggers.txt |  |
| `is_locked_law` | country_triggers.txt |  |
| `is_locked_mechanic` | country_triggers.txt |  |
| `is_locked_parliament_type` | country_triggers.txt |  |
| `is_locked_policy` | country_triggers.txt |  |
| `is_member_of_any_italian_wars_league` | country_triggers.txt |  |
| `is_member_of_any_league` | situation_triggers.txt |  |
| `is_member_of_no_foreign_or_italian_league` | country_triggers.txt |  |
| `is_merchant_republic` | country_triggers.txt |  |
| `is_most_likely_candidate_for_seniority` | international_organization_triggers.txt |  |
| `is_no_longer_capital` | building_triggers.txt |  |
| `is_no_longer_current_capital` | building_triggers.txt |  |
| `is_not_an_autonomous_subject` | country_triggers.txt |  |
| `is_not_protected_from_conversion` | pop_triggers.txt |  |
| `is_owned_by_target_or_subject` | location_triggers.txt |  |
| `is_part_of_chinese_grand_canal` | location_triggers.txt |  |
| `is_peacetime_shakti_avatar` | religion_triggers.txt |  |
| `is_pentarchy_seat` | location_triggers.txt |  |
| `is_port_valid_for_stay` | location_triggers.txt |  |
| `is_portuguese_spain` | country_triggers.txt |  |
| `is_proposal` | resolution_triggers.txt |  |
| `is_protestant` | religion_triggers.txt |  |
| `is_protestant_religion` | religion_triggers.txt |  |
| `is_religion_folk_american` | religion_triggers.txt |  |
| `is_religious_war` | war_triggers.txt |  |
| `is_revolutionary_trigger` | country_triggers.txt |  |
| `is_russian_principality` | country_triggers.txt |  |
| `is_same_ruler_personal_union` | international_organization_triggers.txt | ############################################# |
| `is_subject_which_can_ally_target` | country_triggers.txt |  |
| `is_sufi_school` | religion_triggers.txt |  |
| `is_swiss_for_advances` | culture_triggers.txt |  |
| `is_valid_embassy_country` | country_triggers.txt | and expedition_types load before the events directory. |
| `is_valid_for_exploration_ui_only` | character_triggers.txt | This trigger is UI only |
| `is_valid_landable_daimyo` | situation_triggers.txt |  |
| `is_valid_nanbokuchou_cb_target` | situation_triggers.txt |  |
| `is_valid_nanbokuchou_war_target` | situation_triggers.txt |  |
| `is_valid_target_for_crusade` | religion_triggers.txt |  |
| `is_valid_target_for_jihad` | religion_triggers.txt |  |
| `is_vote` | resolution_triggers.txt |  |
| `is_wartime_shakti_avatar` | religion_triggers.txt |  |
| `is_wokou_nation` | country_triggers.txt |  |
| `junior_union_with` | country_triggers.txt |  |
| `kor_has_mongol_aristocrats` | flavor_kor_triggers.txt |  |
| `law_can_be_initiated_by_members` | international_organization_triggers.txt | Law scope |
| `law_is_not_locked` | country_triggers.txt |  |
| `levee_en_masse_plausible_location` | institution_triggers.txt |  |
| `little_ice_age_end_trigger` | situation_triggers.txt |  |
| `location_and_owner_can_build` | location_triggers.txt |  |
| `location_can_be_settled` | religion_triggers.txt |  |
| `location_can_host_order_holding_trigger` | religious_order_triggers.txt |  |
| `location_in_any_relation_to_target_country` | location_triggers.txt |  |
| `location_wants_camels_trigger` | goods_triggers.txt |  |
| `lordship_of_ireland_can_declare_wars_trigger` | country_triggers.txt |  |
| `manufactories_plausible_location` | institution_triggers.txt |  |
| `military_order_can_declare_wars_trigger` | country_triggers.txt |  |
| `military_revolution_plausible_location` | institution_triggers.txt |  |
| `muscovite_succession_war_end_trigger` | disaster_triggers.txt |  |
| `new_world_plausible_location` | institution_triggers.txt |  |
| `no_papal_bull_debate_active` | religion_triggers.txt |  |
| `oligarchic_capture_end_trigger` | disaster_triggers.txt |  |
| `oligarchic_capture_qualifies_trigger` | disaster_triggers.txt |  |
| `owner_from_old_world` | situation_triggers.txt | Triggers specific for situations |
| `owner_owns_no_copies_of_building` | building_triggers.txt |  |
| `papacy_active` | religion_triggers.txt |  |
| `papal_authority_is_high` | international_organization_triggers.txt |  |
| `papal_authority_is_low` | international_organization_triggers.txt |  |
| `peaceful_and_rich` | country_triggers.txt | root = country |
| `peasants_war_end_trigger` | disaster_triggers.txt |  |
| `pike_and_shot_plausible_location` | institution_triggers.txt |  |
| `policy_is_not_locked` | country_triggers.txt |  |
| `policy_is_voting_target` | international_organization_triggers.txt |  |
| `portrait_abrahamic_african_trigger` | 00_clothing_triggers.txt |  |
| `portrait_bubonic_plague_trigger` | 00_clothing_triggers.txt | # DISEASES |
| `portrait_cannot_wear_armor_trigger` | 00_clothing_triggers.txt |  |
| `portrait_clergy_trigger` | 00_clothing_triggers.txt |  |
| `portrait_clothing_trigger` | 00_clothing_triggers.txt | CORE TRIGGERS |
| `portrait_commoner_trigger` | 00_clothing_triggers.txt | # SOCIAL RANKS |
| `portrait_country_clothing_trigger` | 00_clothing_triggers.txt |  |
| `portrait_disfigurement_trigger` | 00_clothing_triggers.txt |  |
| `portrait_early_trigger` | 00_clothing_triggers.txt | AGE TRIGGERS |
| `portrait_healthy_trigger` | 00_clothing_triggers.txt |  |
| `portrait_high_nobility_trigger` | 00_clothing_triggers.txt |  |
| `portrait_hunchback_trigger` | 00_clothing_triggers.txt |  |
| `portrait_illness_trigger` | 00_clothing_triggers.txt |  |
| `portrait_imperial_trigger` | 00_clothing_triggers.txt |  |
| `portrait_is_hre_elector_trigger` | 00_clothing_triggers.txt | SITUATIONAL TRIGGERS |
| `portrait_is_patriarch_trigger` | 00_clothing_triggers.txt | # SPECIFIC TRIGGERS |
| `portrait_is_ruler_of_trigger` | 00_clothing_triggers.txt |  |
| `portrait_late_trigger` | 00_clothing_triggers.txt |  |
| `portrait_low_nobility_trigger` | 00_clothing_triggers.txt |  |
| `portrait_mid_trigger` | 00_clothing_triggers.txt |  |
| `portrait_military_order_trigger` | 00_clothing_triggers.txt |  |
| `portrait_muslim_trigger` | 00_clothing_triggers.txt |  |
| `portrait_pox_scars_trigger` | 00_clothing_triggers.txt |  |
| `portrait_religion_clothing_trigger` | 00_clothing_triggers.txt |  |
| `portrait_royalty_trigger` | 00_clothing_triggers.txt |  |
| `portrait_scarred_trigger` | 00_clothing_triggers.txt |  |
| `portrait_smallpox_trigger` | 00_clothing_triggers.txt |  |
| `portrait_wear_armor_trigger` | 00_clothing_triggers.txt |  |
| `portrait_wear_crowned_helmet_trigger` | 00_clothing_triggers.txt |  |
| `portrait_wear_helmet_trigger` | 00_clothing_triggers.txt |  |
| `presence_in_eligible_regions_or_member_of_any_league` | country_triggers.txt |  |
| `printing_press_plausible_location` | institution_triggers.txt |  |
| `privilege_events_100_is_in_bad_shape` | province_triggers.txt |  |
| `produces_construction_material` | goods_triggers.txt |  |
| `professional_armies_plausible_location` | institution_triggers.txt |  |
| `reform_society_end_trigger` | disaster_triggers.txt |  |
| `reformation_end_trigger` | situation_triggers.txt |  |
| `reformation_is_enabled` | game_triggers.txt |  |
| `religious_turmoil_end_trigger` | disaster_triggers.txt |  |
| `renaissance_plausible_location` | institution_triggers.txt | i.e. the conditions that apply under has_game_rule = institution_spawn_plausible. |
| `revolution_disaster_end_trigger` | disaster_triggers.txt |  |
| `revolutionary_chaos_end_trigger` | disaster_triggers.txt |  |
| `roman_culture_movement_location_trigger` | culture_triggers.txt |  |
| `rural_illustration_trigger` | location_triggers.txt |  |
| `rustamid_legacy_trigger` | ibadi_content_triggers.txt |  |
| `same_dynasty_as` | country_triggers.txt |  |
| `savonarola_disaster_end_trigger` | disaster_triggers.txt |  |
| `scaligeri_peace_treaty_demand_allow` | war_triggers.txt |  |
| `scaligeri_peace_treaty_demand_allow_FLO` | war_triggers.txt |  |
| `scaligeri_peace_treaty_demand_allow_MLO` | war_triggers.txt |  |
| `scaligeri_peace_treaty_demand_allow_VEN` | war_triggers.txt |  |
| `scaligeri_peace_treaty_tooltip_fallback_scopes` | war_triggers.txt |  |
| `scientific_revolution_plausible_location` | institution_triggers.txt |  |
| `scope_country_can_join_coalition_against` | international_organization_triggers.txt | $target$ is the target of the coalition |
| `senior_union_with` | country_triggers.txt |  |
| `should_use_location_scene_trigger` | character_triggers.txt | This trigger key is used in code so don't change it |
| `show_production_building_trigger` | building_triggers.txt |  |
| `sinicization_disaster_end_trigger` | disaster_triggers.txt |  |
| `smartism_balanced_gods` | religion_triggers.txt |  |
| `struggle_for_royal_power_end_trigger` | disaster_triggers.txt |  |
| `sub_unit_called_camelry_trigger` | unit_triggers.txt |  |
| `sub_unit_called_cataphracts_trigger` | unit_triggers.txt |  |
| `sub_unit_called_knights_trigger` | unit_triggers.txt |  |
| `subject_type_is_not_locked` | country_triggers.txt |  |
| `succession_crisis_end_trigger` | disaster_triggers.txt |  |
| `teu_event_6_trigger` | location_triggers.txt |  |
| `the_revolution_end_trigger` | situation_triggers.txt |  |
| `time_of_troubles_end_trigger` | disaster_triggers.txt |  |
| `timur_could_form_mughals_or_mongol_empire` | situation_triggers.txt |  |
| `town_illustration_trigger` | location_triggers.txt |  |
| `turmoil_in_brandenburg_end_trigger` | disaster_triggers.txt |  |
| `twilight_of_the_tsardom_end_trigger` | disaster_triggers.txt |  |
| `union_flavor_is_great_britain` | international_organization_triggers.txt |  |
| `union_flavor_is_habsburg` | international_organization_triggers.txt |  |
| `union_flavor_is_kalmar_union` | international_organization_triggers.txt |  |
| `union_flavor_is_lpc` | international_organization_triggers.txt |  |
| `union_flavor_is_plc` | international_organization_triggers.txt |  |
| `union_flavor_is_spain` | international_organization_triggers.txt |  |
| `unit_akritai_location_trigger` | unit_triggers.txt |  |
| `unit_almogavars_location_trigger` | unit_triggers.txt |  |
| `unit_almogavars_pop_trigger` | unit_triggers.txt |  |
| `unit_byzantine_cataphracts_location_trigger` | unit_triggers.txt |  |
| `unit_camelry_country_trigger` | unit_triggers.txt | Root = country |
| `unit_camelry_location_trigger` | unit_triggers.txt | Root = location |
| `unit_camelry_region_trigger` | unit_triggers.txt |  |
| `unit_catalan_crossbowmen_location_trigger` | unit_triggers.txt |  |
| `unit_catalan_crossbowmen_pop_trigger` | unit_triggers.txt |  |
| `unit_catalan_ship_location_trigger` | unit_triggers.txt |  |
| `unit_caterans_pop_trigger` | unit_triggers.txt |  |
| `unit_cawa_location_trigger` | unit_triggers.txt |  |
| `unit_cawa_pop_trigger` | unit_triggers.txt |  |
| `unit_florentine_citizen_militia_location_trigger` | unit_triggers.txt |  |
| `unit_florentine_citizen_militia_pop_trigger` | unit_triggers.txt |  |
| `unit_gallowglass_pop_trigger` | unit_triggers.txt |  |
| `unit_genoese_crossbowmen_location_trigger` | unit_triggers.txt |  |
| `unit_genoese_crossbowmen_pop_trigger` | unit_triggers.txt |  |
| `unit_genoese_galley_location_trigger` | unit_triggers.txt |  |
| `unit_iberian_ship_location_trigger` | unit_triggers.txt |  |
| `unit_indian_paik_location_levy_trigger` | unit_triggers.txt |  |
| `unit_indian_paik_location_trigger` | unit_triggers.txt |  |
| `unit_is_tribal_pop_trigger` | unit_triggers.txt |  |
| `unit_jurchen_cavalry_location_trigger` | unit_triggers.txt |  |
| `unit_jurchen_cavalry_pop_trigger` | unit_triggers.txt |  |
| `unit_kerns_pop_trigger` | unit_triggers.txt |  |
| `unit_lanzas_de_castilla_location_trigger` | unit_triggers.txt |  |
| `unit_longbowmen_location_trigger` | unit_triggers.txt |  |
| `unit_longbowmen_pop_trigger` | unit_triggers.txt |  |
| `unit_moa_hunter_location_trigger` | unit_triggers.txt |  |
| `unit_moa_hunter_pop_trigger` | unit_triggers.txt |  |
| `unit_navarrese_crossbowmen_location_trigger` | unit_triggers.txt |  |
| `unit_navarrese_crossbowmen_pop_trigger` | unit_triggers.txt |  |
| `unit_ship_of_the_line_location_trigger` | unit_triggers.txt |  |
| `unit_war_galleon_location_trigger` | unit_triggers.txt |  |
| `uses_parliament_for_law_votes_trigger` | international_organization_triggers.txt |  |
| `war_of_the_aragonese_union_disaster_end_trigger` | disaster_triggers.txt |  |
| `war_of_the_roses_end_trigger` | disaster_triggers.txt |  |
| `was_recently_under_interdict` | religion_triggers.txt |  |
| `work_of_art_exists` | work_of_art_triggers.txt |  |
| `work_of_art_is_imperial_regalia` | work_of_art_triggers.txt |  |

## scripted_lists（可复用列表）（8 条）

> 调用方式：在 `any_/every_/random_` 等迭代器里引用

| 名称 | 文件 | 注释首行 |
|---|---|---|
| `admiral` | character_lists.txt |  |
| `american_colonial_country` | country_lists.txt |  |
| `cardinal_country` | country_lists.txt |  |
| `courtier` | character_lists.txt |  |
| `explorer` | character_lists.txt |  |
| `general` | character_lists.txt |  |
| `junior_partner` | country_lists.txt | every_ally = {} / random_ally = {} / any_ally = {} / ordered_ally = {} |
| `protestant_religion` | religion_lists.txt |  |

## 与其它档的关系
- 引擎自带效果/触发器名录（不是脚本库）：`reference\effects.md`（1,662 条）、`reference\triggers.md`（1,966 条）
- 字段与语法：`fields\common-scripted_effects.md`、`fields\common-scripted_triggers.md`、`guides\scripting-core.md`
- 语义/坑：`reference\script-semantics.md`、`reference\name-traps.md`
