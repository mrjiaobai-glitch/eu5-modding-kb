# 参考资料：on_action 钩子速查

> **用途**：查"有哪些 on_action 钩子 / 各档定义了哪些块 / 某钩子里有没有 trigger·effect·events"**不用翻 27 个 on_action 档**。
> **来源**：`in_game\common\on_action\*.txt`，**生成自 EU5 1.4.5**（2026-10-30）。`_hardcoded.txt` 里的是**引擎内置钩子**（没有可改的 trigger/effect），业务钩子在其它的档。
> **体量**：**检索用清单**；共 27 档 / 255 个顶层钩子。

| 钩子 | 文件:行 | 块内定义 |
|---|---|---|
| `in_battle` | _hardcoded.txt:629 |  |
| `on_accepted_call_to_arms` | _hardcoded.txt:2654 | effect |
| `on_annex` | _hardcoded.txt:794 | effect |
| `on_annexation_cancel` | _hardcoded.txt:1112 | effect |
| `on_annexation_start` | _hardcoded.txt:1092 | effect |
| `on_annexed` | _hardcoded.txt:910 | effect |
| `on_bankruptcy` | _hardcoded.txt:1301 | effect |
| `on_battle_lost` | _hardcoded.txt:3332 | effect |
| `on_battle_lost_character` | _hardcoded.txt:2953 | effect |
| `on_battle_won` | _hardcoded.txt:3206 | effect / events |
| `on_battle_won_character` | _hardcoded.txt:2883 | effect |
| `on_become_revolution_target` | _hardcoded.txt:6229 | events |
| `on_becoming_free` | _hardcoded.txt:3420 | effect / events |
| `on_building_built` | _hardcoded.txt:3626 |  |
| `on_building_construction_ended` | _hardcoded.txt:3621 |  |
| `on_building_construction_started` | _hardcoded.txt:3616 |  |
| `on_building_destroyed` | _hardcoded.txt:3631 |  |
| `on_bureaucracy_added` | _hardcoded.txt:3657 | events |
| `on_bureaucracy_change` | _hardcoded.txt:3651 | events |
| `on_bureaucracy_removed` | _hardcoded.txt:3663 | events |
| `on_cabinet_assigned` | _hardcoded.txt:5757 | effect |
| `on_cabinet_removed` | _hardcoded.txt:5782 | effect |
| `on_capital_moved` | _hardcoded.txt:6324 |  |
| `on_character_betrothal` | _hardcoded.txt:5833 |  |
| `on_character_betrothal_broken` | _hardcoded.txt:5837 |  |
| `on_character_betrothal_broken_by_death` | _hardcoded.txt:5841 |  |
| `on_character_betrothal_confirmed` | _hardcoded.txt:5845 |  |
| `on_character_changed_religion` | _hardcoded.txt:6031 | effect |
| `on_character_divorce` | _hardcoded.txt:5829 |  |
| `on_character_estate_change` | _hardcoded.txt:4190 | effect |
| `on_character_marriage` | _hardcoded.txt:5817 | effect |
| `on_character_moved_country` | _hardcoded.txt:5976 | effect |
| `on_civil_war_annex` | _hardcoded.txt:757 | effect |
| `on_civil_war_annexed` | _hardcoded.txt:907 |  |
| `on_civil_war_lost` | _hardcoded.txt:1326 | effect / events |
| `on_civil_war_start` | _hardcoded.txt:1124 | effect |
| `on_civil_war_won` | _hardcoded.txt:1477 | effect / events |
| `on_colonial_charter_failed` | _hardcoded.txt:3400 | events |
| `on_colonial_charter_finished` | _hardcoded.txt:3393 | events |
| `on_colonize_annex` | _hardcoded.txt:789 |  |
| `on_colonize_annexed` | _hardcoded.txt:1088 |  |
| `on_command_gained` | _hardcoded.txt:670 |  |
| `on_command_lost` | _hardcoded.txt:674 |  |
| `on_country_rank_change` | _hardcoded.txt:6412 | effect |
| `on_culture_changed` | _hardcoded.txt:4577 |  |
| `on_dependency_gained` | _hardcoded.txt:3510 | effect |
| `on_diplomatic_annex` | _hardcoded.txt:715 | effect / events |
| `on_diplomatic_annexed` | _hardcoded.txt:904 |  |
| `on_election` | _hardcoded.txt:3409 | events |
| `on_embrace_revolution` | _hardcoded.txt:6175 | effect / events |
| `on_ending_war` | _hardcoded.txt:2039 | effect |
| `on_enforce_peace_accepted` | _hardcoded.txt:6436 | effect |
| `on_enforce_peace_declined` | _hardcoded.txt:6445 | effect |
| `on_gain_great_power_status` | _hardcoded.txt:6167 |  |
| `on_gain_hegemon_status` | _hardcoded.txt:6428 |  |
| `on_game_start` | _hardcoded.txt:1 | effect |
| `on_game_started_after_lobby` | _hardcoded.txt:316 |  |
| `on_gift_sent` | _hardcoded.txt:6100 |  |
| `on_government_type_change` | _hardcoded.txt:3672 | effect / events |
| `on_great_battle_lost` | _hardcoded.txt:3140 | effect |
| `on_great_battle_won` | _hardcoded.txt:2980 | effect / events |
| `on_heir_selection_changed` | _hardcoded.txt:6328 | effect |
| `on_institution_embraced` | _hardcoded.txt:5798 | effect |
| `on_insult` | _hardcoded.txt:6104 |  |
| `on_international_organization_changed_leader` | _hardcoded.txt:5435 | effect |
| `on_international_organization_creation` | _hardcoded.txt:4581 |  |
| `on_international_organization_disbanded` | _hardcoded.txt:4646 | effect |
| `on_international_organization_disbanding` | _hardcoded.txt:4586 | effect |
| `on_international_organization_parliament_agenda_accepted` | _hardcoded.txt:6383 | effect |
| `on_international_organization_policy_changed` | _hardcoded.txt:6408 |  |
| `on_io_parliament_failed` | _hardcoded.txt:6096 |  |
| `on_io_parliament_passed` | _hardcoded.txt:6092 |  |
| `on_io_parliament_started` | _hardcoded.txt:6088 |  |
| `on_join_war` | _hardcoded.txt:1875 | effect |
| `on_loan_renewed` | _hardcoded.txt:1297 |  |
| `on_loan_repaid` | _hardcoded.txt:1277 | effect |
| `on_loan_taken` | _hardcoded.txt:1293 |  |
| `on_location_changed_owner` | _hardcoded.txt:4851 | effect / events / on_actions |
| `on_location_changed_rank` | _hardcoded.txt:4841 |  |
| `on_location_lost` | _hardcoded.txt:620 | events |
| `on_location_occupied` | _hardcoded.txt:494 | effect / events / on_actions |
| `on_lose_great_power_status` | _hardcoded.txt:6171 |  |
| `on_lose_hegemon_status` | _hardcoded.txt:6432 |  |
| `on_lose_revolution` | _hardcoded.txt:6210 | effect |
| `on_lose_revolution_target` | _hardcoded.txt:6234 |  |
| `on_losing_war` | _hardcoded.txt:1930 | effect |
| `on_made_saint` | _hardcoded.txt:4169 | effect |
| `on_marriage_union_formation` | _hardcoded.txt:4810 | effect |
| `on_max_doom_reached` | _hardcoded.txt:3386 | events |
| `on_military_annex` | _hardcoded.txt:678 | effect |
| `on_military_annexed` | _hardcoded.txt:901 |  |
| `on_mission_abort` | _hardcoded.txt:6302 | effect |
| `on_mission_completion` | _hardcoded.txt:6299 |  |
| `on_mission_start` | _hardcoded.txt:6296 |  |
| `on_mission_task_bypass` | _hardcoded.txt:6314 |  |
| `on_mission_task_completion` | _hardcoded.txt:6311 |  |
| `on_mission_task_start` | _hardcoded.txt:6308 |  |
| `on_nation_changing` | _hardcoded.txt:5713 | events |
| `on_new_age` | _hardcoded.txt:640 | effect / events |
| `on_new_age_global` | _hardcoded.txt:633 | effect |
| `on_new_country_formed` | _hardcoded.txt:5344 | effect |
| `on_new_ruler` | _hardcoded.txt:3722 | effect / events |
| `on_noble_marriage` | _hardcoded.txt:6135 | effect |
| `on_omen_god_selected` | _hardcoded.txt:663 | events |
| `on_overrun_imprisoned` | _hardcoded.txt:3379 | events |
| `on_parliament_abolished` | _hardcoded.txt:6080 |  |
| `on_parliament_established` | _hardcoded.txt:6084 |  |
| `on_parliament_failed` | _hardcoded.txt:6076 |  |
| `on_parliament_passed` | _hardcoded.txt:6059 | effect |
| `on_parliament_started` | _hardcoded.txt:6055 |  |
| `on_policy_changed` | _hardcoded.txt:6390 | effect |
| `on_pre_ending_war` | _hardcoded.txt:1955 |  |
| `on_pre_location_changed_rank` | _hardcoded.txt:4846 |  |
| `on_pre_losing_war` | _hardcoded.txt:2035 |  |
| `on_pre_war_declared` | _hardcoded.txt:2348 | effect |
| `on_pre_winning_war` | _hardcoded.txt:1959 | effect |
| `on_raw_material_changed` | _hardcoded.txt:5340 |  |
| `on_reform_change` | _hardcoded.txt:3610 | events |
| `on_regency_end` | _hardcoded.txt:5753 |  |
| `on_regency_start` | _hardcoded.txt:5720 | effect |
| `on_rejected_call_to_arms` | _hardcoded.txt:2683 | effect |
| `on_released_country` | _hardcoded.txt:5387 | effect |
| `on_religion_changed` | _hardcoded.txt:4203 | effect / events |
| `on_revolt_start` | _hardcoded.txt:1264 | effect |
| `on_road_built` | _hardcoded.txt:3646 |  |
| `on_road_construction_ended` | _hardcoded.txt:3641 |  |
| `on_road_construction_started` | _hardcoded.txt:3636 |  |
| `on_royal_marriage` | _hardcoded.txt:6128 | events |
| `on_ruler_death` | _hardcoded.txt:3972 | effect |
| `on_ruler_death_delayed` | _hardcoded.txt:3998 | effect / events / on_actions |
| `on_shatter_country` | _hardcoded.txt:6239 | effect |
| `on_shattered_country` | _hardcoded.txt:6265 | effect |
| `on_siege_lost` | _hardcoded.txt:447 | effect / events |
| `on_siege_won` | _hardcoded.txt:321 | effect / events / random_events / on_actions |
| `on_storm_reached_location` | _hardcoded.txt:6318 | effect |
| `on_subject_created` | _hardcoded.txt:3575 | effect |
| `on_subject_type_changed` | _hardcoded.txt:6350 | effect |
| `on_took_location_in_peace_treaty` | _hardcoded.txt:2713 | effect |
| `on_transfer_subject` | _hardcoded.txt:6455 | effect |
| `on_truce_broken` | _hardcoded.txt:6472 |  |
| `on_unconditional_surrender` | _hardcoded.txt:6108 | effect |
| `on_union_formation` | _hardcoded.txt:4695 | effect |
| `on_union_merging` | _hardcoded.txt:4678 | effect |
| `on_union_split` | _hardcoded.txt:4806 |  |
| `on_war_declared` | _hardcoded.txt:2392 | effect |
| `on_winning_war` | _hardcoded.txt:1534 | effect |
| `on_work_of_art_created` | _hardcoded.txt:5849 | effect |
| `on_work_of_art_destroyed` | _hardcoded.txt:5872 |  |
| `on_work_of_art_looted` | _hardcoded.txt:5876 | effect |
| `on_work_of_art_moved` | _hardcoded.txt:5972 |  |
| `on_ai_personalities_game_start` | ai_personalities_setup.txt:13 | effect |
| `on_game_start` | ai_personalities_setup.txt:9 | on_actions |
| `on_disloyal_independent_appanage` | appanage_monthly.txt:41 | trigger / effect |
| `on_former_independent_appanage` | appanage_monthly.txt:25 | trigger / effect |
| `on_new_appanage` | appanage_monthly.txt:1 | trigger / effect |
| `on_vassalized_appanage` | appanage_monthly.txt:11 | trigger / effect |
| `on_cabinet_death` | character_death_pulses.txt:335 | trigger / effect |
| `on_character_death` | character_death_pulses.txt:2 | effect / on_actions |
| `on_hafsid_governor_death` | character_death_pulses.txt:422 | random_events |
| `on_horde_pretender_death` | character_death_pulses.txt:347 | trigger / effect |
| `on_prussian_crusader_death` | character_death_pulses.txt:375 | trigger / effect |
| `on_tamil_rebel_death` | character_death_pulses.txt:366 | trigger / effect |
| `on_timurid_governor_death` | character_death_pulses.txt:391 | trigger / events |
| `on_timurid_ruler_death` | character_death_pulses.txt:405 | trigger / effect |
| `on_character_birth` | character.txt:2 | effect |
| `on_character_created` | character.txt:25 |  |
| `colonial_charter_monthly_pulse` | colonial_charter_monthly.txt:3 | random_events / on_actions |
| `biyearly_country_pulse` | country_biyearly.txt:4 | events / random_events / on_actions |
| `four_yearly_country_pulse` | country_four_yearly.txt:1 | effect / events / random_events / on_actions |
| `four_yearly_country_pulse_25_percent` | country_four_yearly.txt:234 | events / random_events |
| `four_yearly_country_pulse_50_percent` | country_four_yearly.txt:214 | effect / random_events |
| `on_albanian_migration` | country_four_yearly.txt:278 | effect |
| `on_four_yearly_check_for_script` | country_four_yearly.txt:251 | effect |
| `cloth_silk_guild_industry_tracker` | country_monthly.txt:359 | trigger / effect |
| `crisis_of_faith_potential_tracker` | country_monthly.txt:330 | trigger / effect |
| `france_naples_administration_broken` | country_monthly.txt:167 | trigger / effect |
| `monthly_country_pulse` | country_monthly.txt:2 | events / random_events / on_actions |
| `oligarchic_capture_potential_tracker` | country_monthly.txt:301 | trigger / effect |
| `on_annexing_subject_monthly_pulse` | country_monthly.txt:253 | random_events / on_actions |
| `on_fraticide_underage_succession_waiting` | country_monthly.txt:268 | trigger / effect |
| `on_papacy_reform_removed` | country_monthly.txt:155 | trigger / effect |
| `on_papal_authority_updated` | country_monthly.txt:111 | trigger / effect |
| `on_papal_opinion_added` | country_monthly.txt:65 | trigger / effect |
| `on_papal_opinion_removed` | country_monthly.txt:89 | trigger / effect |
| `wor_on_wallenstein_prison_occupation` | country_monthly.txt:215 | trigger / effect |
| `wor_on_wallenstein_prison_timeout` | country_monthly.txt:233 | trigger / effect |
| `country_pulse_for_high_infamy` | country_pulse_for_high_infamy.txt:1 | random_events |
| `earthquake_yearly_pulse` | country_yearly.txt:831 | random_events |
| `on_integrated_in_union_removal` | country_yearly.txt:929 | trigger / effect |
| `yearly_country_pulse` | country_yearly.txt:1 | effect / events / random_events / on_actions |
| `yearly_flavor_cop_pulse` | country_yearly.txt:991 | trigger / events |
| `yearly_flavor_hab_tir_pulse` | country_yearly.txt:953 | trigger / events |
| `yearly_flavor_ira_pulse` | country_yearly.txt:971 | trigger / events |
| `yearly_flavor_kni_pulse` | country_yearly.txt:981 | trigger / events |
| `yearly_flavor_kor_pulse` | country_yearly.txt:966 | trigger / events |
| `yearly_flavor_nap_pulse` | country_yearly.txt:948 | trigger / events |
| `yearly_flavor_nav_pulse` | country_yearly.txt:976 | trigger / events |
| `yearly_flavor_pol_union_pulse` | country_yearly.txt:986 | trigger / events |
| `yearly_flavor_sic_pulse` | country_yearly.txt:943 | trigger / events |
| `yearly_flavor_ven_pulse` | country_yearly.txt:959 | trigger / events |
| `yearly_hre_circle_leader_pulse` | country_yearly.txt:1019 | trigger / events |
| `yearly_iberian_union_pulse` | country_yearly.txt:1034 | trigger / events |
| `yearly_infidel_holds_rome_pulse` | country_yearly.txt:999 | trigger / effect |
| `on_culture_group_merged` | culture_unification.txt:1 | effect |
| `on_royal_marriage` | diplomatic_reactions.txt:72 | effect |
| `on_scripted_relation_broken` | diplomatic_reactions.txt:56 |  |
| `on_scripted_relation_created` | diplomatic_reactions.txt:1 | trigger / effect |
| `on_subject_subjugated` | diplomatic_reactions.txt:59 | effect |
| `on_truce_broken` | diplomatic_reactions.txt:95 | effect |
| `on_estate_culture_changed` | estate_changes.txt:2 |  |
| `on_estate_religion_changed` | estate_changes.txt:6 |  |
| `on_expedition_leader_replaced` | expedition_leader_replaced.txt:2 | effect |
| `on_exploration_monthly_pulse` | exploration_mission_monthly.txt:3 | events / random_events / on_actions |
| `on_exploration_success` | exploration_mission_monthly.txt:54 | effect / random_events / on_actions |
| `government_flavor_pulse` | government_flavor_pulse.txt:1 | random_events |
| `government_flavor_pulse_monarchy` | government_flavor_pulse.txt:28 | trigger / random_events |
| `government_flavor_pulse_republic` | government_flavor_pulse.txt:61 | trigger / random_events |
| `government_flavor_pulse_steppe_horde` | government_flavor_pulse.txt:221 | trigger / random_events |
| `government_flavor_pulse_theocracy` | government_flavor_pulse.txt:131 | trigger / random_events |
| `government_flavor_pulse_tribe` | government_flavor_pulse.txt:203 | trigger / random_events |
| `in_regency_yearly_pulse` | in_regency_yearly_pulse.txt:3 | events / random_events / on_actions |
| `on_heir_changed` | inheritance_contract_on_actions.txt:40 |  |
| `on_ruler_death_inheritance_contract` | inheritance_contract_on_actions.txt:2 | effect |
| `earthquake_location_pulse` | location_pulses.txt:23 | random_events / on_actions |
| `volcano_location_pulse` | location_pulses.txt:5 | random_events / on_actions |
| `weather_monthly_pulse` | location_pulses.txt:38 | effect |
| `maghrebi_location_conversion` | migrations.txt:2 | effect |
| `delhi_four_yearly_pulse` | on_country_specific_pulse.txt:127 | trigger / random_events |
| `on_country_specific_pulse` | on_country_specific_pulse.txt:1 | trigger / random_events |
| `on_milanese_library_occupied` | on_country_specific_pulse.txt:102 | trigger / events |
| `on_ruler_death_delhi_tombs_construction` | on_country_specific_pulse.txt:114 | trigger / random_events |
| `on_timurid_occupy_location` | on_country_specific_pulse.txt:25 | trigger / effect |
| `on_timurid_occupy_location_horde` | on_country_specific_pulse.txt:9 | trigger / effect |
| `parliament_monthly_pulse` | parliament_monthly_pulse.txt:1 | random_events |
| `religion_flavor_pulse` | religion_flavor_pulse.txt:1 | random_events |
| `religion_flavor_pulse_buddhist` | religion_flavor_pulse.txt:403 | trigger / random_events |
| `religion_flavor_pulse_catholic` | religion_flavor_pulse.txt:173 | trigger / events / random_events |
| `religion_flavor_pulse_church_aspects` | religion_flavor_pulse.txt:361 | random_events |
| `religion_flavor_pulse_dharmic` | religion_flavor_pulse.txt:440 | trigger / random_events |
| `religion_flavor_pulse_hellenism` | religion_flavor_pulse.txt:525 | trigger / random_events |
| `religion_flavor_pulse_muslim` | religion_flavor_pulse.txt:112 | trigger / random_events |
| `religion_flavor_pulse_nahuatl` | religion_flavor_pulse.txt:494 | trigger / random_events |
| `religion_flavor_pulse_orthodox` | religion_flavor_pulse.txt:319 | trigger / events / random_events / on_actions |
| `religion_flavor_pulse_protestant` | religion_flavor_pulse.txt:237 | trigger / random_events / on_actions |
| `religion_flavor_pulse_shinto` | religion_flavor_pulse.txt:571 | trigger / random_events |
| `on_ending_war` | religious_order_conquest.txt:9 | on_actions |
| `on_took_location_in_peace_treaty` | religious_order_conquest.txt:1 | on_actions |
| `on_winning_war` | religious_order_conquest.txt:5 | on_actions |
| `religious_order_conquest_candidate_pulse` | religious_order_conquest.txt:13 | effect |
| `religious_order_conquest_cleanup_pulse` | religious_order_conquest.txt:92 | effect |
| `religious_order_conquest_offer_pulse` | religious_order_conquest.txt:51 | effect |
| `on_settle_the_frontier_monthly_pulse` | settle_the_frontier_monthly.txt:1 | trigger / random_events |
| `chi_expedition_rotation_pulse` | treasure_voyage.txt:36 | random_events |
| `chi_expedition_war_support_pulse` | treasure_voyage.txt:16 | trigger / events |
| `chinese_expedition_event_pulse` | treasure_voyage.txt:4 | trigger |

> 合并语义：同名钩子在 mod 里**再写一块会追加**（`events`/`random_events`/`on_actions`），`trigger`/`effect` 不能替换（见 `guides\merging.md` §三）。
