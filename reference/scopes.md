# 参考资料：作用域（scopes）与作用域链（scope links）

> **用途**：查"某个 scope 能否评估触发器/执行效果/切换"，以及"从 A 能否链到 B"。
> **来源**：游戏内 `script_docs`（`event_scopes.log` + `event_targets.log`），**EU5 1.4.5**，2026-10-30。
> **体量**：**检索用清单**。
> ⚠️ **与 wiki 的 scope 名单有差异**：wiki 只列 120 种且命名旧（`bool`/`flag`/`rebel`/`religion_group`/`sub_unit`…），本表由 `script_docs` 生成（143 种），**以本表为准**；差异明细与"wiki 旧名→引擎实际"对照见 **`reference\scope-aliases.md`**。

## 一、scope 类型（143 种）

| scope | 评估触发器 | 执行效果 | 切换 scope | 存变量 |
|---|---|---|---|---|
| `active_resolution` | yes | yes | yes | no |
| `advance_type` | yes | yes | yes | no |
| `age` | yes | yes | yes | no |
| `ai_personality` | yes | yes | yes | no |
| `ambition` | yes | yes | yes | yes |
| `ambition_definition` | yes | yes | yes | no |
| `area` | yes | yes | yes | no |
| `artist_type` | yes | yes | yes | no |
| `audio_culture` | yes | yes | yes | no |
| `avatar` | yes | yes | yes | no |
| `boolean` | yes | yes | yes | no |
| `building` | yes | yes | yes | no |
| `building_type` | yes | yes | yes | no |
| `bureaucracy` | yes | yes | yes | no |
| `bureaucracy_type` | yes | yes | yes | no |
| `cabinet` | yes | yes | yes | yes |
| `cabinet_action` | yes | yes | yes | no |
| `calling` | yes | yes | yes | no |
| `casus_belli` | yes | yes | yes | no |
| `character` | yes | yes | yes | yes |
| `character_interaction` | yes | yes | yes | no |
| `child_education` | yes | yes | yes | no |
| `chivalric_order` | yes | yes | yes | no |
| `climate` | yes | yes | yes | no |
| `colonial_charter` | yes | yes | yes | yes |
| `colonial_charter_goal` | yes | yes | yes | no |
| `color` | yes | yes | yes | no |
| `combat` | yes | yes | yes | no |
| `combat_side` | yes | yes | yes | no |
| `continent` | yes | yes | yes | no |
| `country` | yes | yes | yes | yes |
| `country_interaction` | yes | yes | yes | no |
| `country_rank` | yes | yes | yes | no |
| `culture` | yes | yes | yes | yes |
| `culture_group` | yes | yes | yes | no |
| `date` | yes | no | no | no |
| `demand` | yes | yes | yes | no |
| `dialect` | yes | yes | yes | no |
| `disaster` | yes | yes | yes | yes |
| `disaster_type` | yes | yes | yes | no |
| `disease` | yes | yes | yes | no |
| `disease_outbreak` | yes | yes | yes | no |
| `dynasty` | yes | yes | yes | no |
| `employment_system` | yes | yes | yes | no |
| `estate` | yes | yes | yes | no |
| `estate_privilege` | yes | yes | yes | no |
| `estate_type` | yes | yes | yes | no |
| `ethnicity` | yes | yes | yes | no |
| `expedition` | yes | yes | yes | yes |
| `expedition_type` | yes | yes | yes | no |
| `exploration` | yes | yes | yes | no |
| `flag` | no | no | no | no |
| `formable_country` | yes | yes | yes | no |
| `generic_action` | yes | yes | yes | no |
| `god` | yes | yes | yes | no |
| `goods` | yes | yes | yes | no |
| `government` | yes | yes | yes | no |
| `government_reform` | yes | yes | yes | no |
| `graphical_culture` | yes | yes | yes | no |
| `group` | yes | yes | yes | no |
| `hegemony` | yes | yes | yes | no |
| `heir_selection` | yes | yes | yes | no |
| `holy_site` | yes | yes | yes | no |
| `holy_site_definition` | yes | yes | yes | no |
| `holy_site_type` | yes | yes | yes | no |
| `imperial_circle` | yes | yes | yes | no |
| `industry_promotion` | yes | yes | yes | no |
| `industry_type` | yes | yes | yes | no |
| `institution` | yes | yes | yes | no |
| `international_organization` | yes | yes | yes | yes |
| `international_organization_type` | yes | yes | yes | no |
| `invalid` | no | no | no | no |
| `land_ownership_rule` | yes | yes | yes | no |
| `language` | yes | yes | yes | no |
| `language_family` | yes | yes | yes | no |
| `law` | yes | yes | yes | no |
| `levy_setup` | yes | yes | yes | no |
| `loan` | yes | yes | yes | no |
| `location` | yes | yes | yes | yes |
| `location_rank` | yes | yes | yes | no |
| `market` | yes | yes | yes | no |
| `mercenary` | yes | yes | yes | no |
| `military_stance` | yes | yes | yes | no |
| `mission` | yes | yes | yes | no |
| `mission_task` | yes | yes | yes | no |
| `movement` | yes | yes | yes | no |
| `movement_definition` | yes | yes | yes | no |
| `none/any` | yes | yes | yes | no |
| `omen` | yes | yes | yes | no |
| `parliament_agenda` | yes | yes | yes | no |
| `parliament_issue` | yes | yes | yes | no |
| `parliament_type` | yes | yes | yes | no |
| `payment` | yes | yes | yes | no |
| `peace_treaty` | yes | yes | yes | no |
| `policy` | yes | yes | yes | no |
| `pop` | yes | yes | yes | no |
| `pop_type` | yes | yes | yes | no |
| `price` | yes | yes | yes | no |
| `privateer` | yes | yes | yes | no |
| `production_method` | yes | yes | yes | no |
| `province` | yes | yes | yes | yes |
| `province_definition` | yes | yes | yes | no |
| `rebels` | yes | yes | yes | yes |
| `recruitment_method` | yes | yes | yes | no |
| `regency_type` | yes | yes | yes | no |
| `region` | yes | yes | yes | no |
| `relation_type` | yes | yes | yes | no |
| `religion` | yes | yes | yes | yes |
| `religious_aspect` | yes | yes | yes | no |
| `religious_faction` | yes | yes | yes | no |
| `religious_figure` | yes | yes | yes | no |
| `religious_focus` | yes | yes | yes | no |
| `religious_order` | yes | yes | yes | no |
| `religious_order_type` | yes | yes | yes | no |
| `religious_school` | yes | yes | yes | no |
| `resolution` | yes | yes | yes | no |
| `road_type` | yes | yes | yes | no |
| `scriptable_hint_definition` | yes | yes | yes | no |
| `scripted_geography` | yes | yes | yes | no |
| `siege` | yes | yes | yes | no |
| `situation` | yes | yes | yes | yes |
| `societal_value_type` | yes | yes | yes | no |
| `special_status` | yes | yes | yes | no |
| `sub_continent` | yes | yes | yes | no |
| `sub_unit` | yes | yes | yes | no |
| `sub_unit_category` | yes | yes | yes | no |
| `subject_type` | yes | yes | yes | no |
| `topography` | yes | yes | yes | no |
| `town_rights` | yes | yes | yes | no |
| `town_rights_type` | yes | yes | yes | no |
| `trade` | yes | yes | yes | no |
| `trait` | yes | yes | yes | no |
| `unit` | yes | yes | yes | yes |
| `unit_ability` | yes | yes | yes | no |
| `unit_formation_preference` | yes | yes | yes | no |
| `unit_template` | yes | yes | yes | no |
| `unit_type` | yes | yes | yes | no |
| `value` | yes | yes | yes | no |
| `vegetation` | yes | yes | yes | no |
| `war` | yes | yes | yes | yes |
| `weather_system` | yes | yes | yes | no |
| `work_of_art` | yes | yes | yes | no |
| `work_of_art_type` | yes | yes | yes | no |

## 二、作用域链 / event targets（357 条）

| 链名 | 输入 scope | 输出 scope | 需数据 |
|---|---|---|---|
| `active_chivalric_order` | country | chivalric_order |  |
| `active_mission` | country | mission |  |
| `active_outbreak` | location, sub_unit | disease_outbreak | yes |
| `active_resolution` | international_organization, situation | active_resolution | yes |
| `advance_age` | advance_type | age |  |
| `advance_type` |  | advance_type | yes |
| `age` |  | age | yes |
| `ai_best_proximity_candidate` | country | location |  |
| `ai_personality` | country | ai_personality |  |
| `ai_personality` |  | ai_personality | yes |
| `ambition_definition` |  | ambition_definition | yes |
| `ambition_definition` | ambition | ambition_definition |  |
| `area` |  | area | yes |
| `area` | location, province, province_definition, privateer, exploration | area |  |
| `area_exploration` | area | exploration | yes |
| `array_define` |  | value | yes |
| `artist_type` |  | artist_type | yes |
| `assimilating_culture` | location | culture |  |
| `assimilating_from_culture` | location, province | culture |  |
| `attacker_leader` | war | country |  |
| `autocephalous_patriarchate` | country | international_organization |  |
| `avatar` | holy_site | avatar |  |
| `avatar` |  | avatar | yes |
| `bias_value` |  | value | yes |
| `birth_location` | character | location |  |
| `borrower` | loan | country |  |
| `building` | location | building | yes |
| `building_base_cost_in_gold` | building_type | value |  |
| `building_type` | building | building_type |  |
| `building_type` |  | building_type | yes |
| `bureaucracy_type` |  | bureaucracy_type | yes |
| `bureaucracy_type` | bureaucracy | bureaucracy_type |  |
| `c` |  | country | yes |
| `cabinet_action` |  | cabinet_action | yes |
| `cabinet_action` | character, cabinet | cabinet_action |  |
| `cabinet_member` | cabinet | character |  |
| `calling` |  | calling | yes |
| `capacity_market` | trade | market |  |
| `capital` | country, dynasty, province, area | location |  |
| `cardinal` | location | character |  |
| `cast_vote_in_active_resolution` | active_resolution |  | yes |
| `casus_belli` | war | casus_belli |  |
| `casus_belli` |  | casus_belli | yes |
| `character` |  | character | yes |
| `character_interaction` |  | character_interaction | yes |
| `child_education` |  | child_education | yes |
| `chivalric_order` |  | chivalric_order | yes |
| `circle_leader` | imperial_circle | country |  |
| `civil_war` | country | war |  |
| `civil_war_opponent` | country | country |  |
| `climate` |  | climate | yes |
| `combat` | location, unit, combat_side | combat |  |
| `combat_attacker` | combat | combat_side |  |
| `combat_defender` | combat | combat_side |  |
| `commander` | combat_side | country |  |
| `commanding_country` | combat_side | country |  |
| `compare_complex_value` |  | value | yes |
| `compare_date` |  | date |  |
| `compare_value` |  | value |  |
| `concession_policy` | rebels | policy |  |
| `concession_privilege` | rebels | estate_privilege |  |
| `consort` | country | character |  |
| `continent` | location, province, province_definition, area, region, sub_continent | continent |  |
| `continent` |  | continent | yes |
| `controller` | location, sub_unit | country |  |
| `country_color` | country | color |  |
| `country_government_reform_fully_implemented_date` | country | date | yes |
| `country_government_reform_implementation_date` | country | date | yes |
| `country_interaction` |  | country_interaction | yes |
| `country_rank` | country | country_rank |  |
| `country_rank` |  | country_rank | yes |
| `country_rank_on_date` | country | country_rank | yes |
| `country_stance` | country | military_stance |  |
| `court_dialect` | country | dialect |  |
| `court_language` | country | language |  |
| `creator` | work_of_art | character |  |
| `culture` |  | culture | yes |
| `culture` | country, sub_unit, character, dynasty, pop, rebels, mercenary, movement | culture |  |
| `culture_group` |  | culture_group | yes |
| `current_mission_task` | country | mission_task |  |
| `customer` | mercenary | country |  |
| `default_price` |  | value | yes |
| `defender_leader` | war | country |  |
| `define` |  | value, color, date | yes |
| `demand` |  | demand | yes |
| `dialect` | character, dynasty, pop, culture, religion, market, estate | dialect |  |
| `dialect` |  | dialect | yes |
| `disaster_type` | disaster | disaster_type |  |
| `disaster_type` |  | disaster_type | yes |
| `disease` | disease_outbreak | disease |  |
| `disease` |  | disease | yes |
| `dominant_country` | culture | country |  |
| `dominant_culture` | location, country, province, estate | culture |  |
| `dominant_dialect` | location, country | dialect |  |
| `dominant_language` | location, country | language |  |
| `dominant_religion` | location, country, province, estate | religion |  |
| `dominant_upper_class_culture` | country | culture |  |
| `dynasty` | character | dynasty |  |
| `dynasty` |  | dynasty | yes |
| `dynasty_head` | dynasty | character |  |
| `dynasty_home` | dynasty | location |  |
| `employer` | character | country |  |
| `employment_system` |  | employment_system | yes |
| `enemy_side` | combat_side | combat_side |  |
| `estate` | country | estate | yes |
| `estate_power` | country | value | yes |
| `estate_privilege` |  | estate_privilege | yes |
| `estate_satisfaction` | country | value | yes |
| `estate_target_satisfaction` | country | value | yes |
| `estate_tax_base` | estate | value |  |
| `estate_tax_base` | country | value | yes |
| `estate_tax_percentage` | country | value | yes |
| `estate_type` |  | estate_type | yes |
| `estate_type` | character, pop, rebels, building, estate_privilege, parliament_issue, estate | estate_type |  |
| `ethnicity` |  | ethnicity | yes |
| `ethnicity` | character | ethnicity |  |
| `expedition_current_location` | expedition | location |  |
| `expedition_leader` | expedition | character |  |
| `expedition_owner` | expedition | country |  |
| `expedition_type` |  | expedition_type | yes |
| `expedition_type` | expedition | expedition_type |  |
| `exploration` | character | exploration |  |
| `father` | character | character |  |
| `first_spouse` | character | character |  |
| `flag` |  | flag | yes |
| `formable_country` |  | formable_country | yes |
| `from_market` | trade | market |  |
| `generic_action` |  | generic_action | yes |
| `gfx_culture` |  | graphical_culture | yes |
| `gfx_culture` | culture | graphical_culture |  |
| `global_var` |  |  | yes |
| `global_variable_map` |  |  | yes |
| `god` |  | god | yes |
| `god` | holy_site, omen, avatar | god |  |
| `goods` |  | goods | yes |
| `government_reform` |  | government_reform | yes |
| `government_type` | country | government |  |
| `government_type` |  | government | yes |
| `group` | religion | group |  |
| `hegemony` |  | hegemony | yes |
| `heir` | country | character |  |
| `heir_selection` |  | heir_selection | yes |
| `holy_site` | god, avatar | holy_site |  |
| `holy_site_definition` |  | holy_site_definition | yes |
| `holy_site_type` |  | holy_site_type | yes |
| `implementation_price` | bureaucracy_type, bureaucracy | price | yes |
| `industry_type` | industry_promotion | industry_type |  |
| `industry_type` |  | industry_type | yes |
| `institution` |  | institution | yes |
| `institution_progress` | location | value | yes |
| `interaction_target` | cabinet |  | yes |
| `international_organization` | imperial_circle | international_organization |  |
| `international_organization` |  | international_organization | yes |
| `international_organization_target` | international_organization | country |  |
| `international_organization_type` |  | international_organization_type | yes |
| `international_organization_type` | international_organization | international_organization_type |  |
| `joined_chivalric_order` | character | chivalric_order |  |
| `known_in_country` | country | value | yes |
| `land_ownership_rule` | international_organization | land_ownership_rule |  |
| `land_ownership_rule` |  | land_ownership_rule | yes |
| `language` | country, sub_unit, character, dynasty, culture, religion, market, dialect | language |  |
| `language` |  | language | yes |
| `language_family` |  | language_family | yes |
| `language_family` | language | language_family |  |
| `largest_army` | country | unit |  |
| `largest_navy` | country | unit |  |
| `last_dynasty_in_location` | location | dynasty |  |
| `last_leader_country` | international_organization | country |  |
| `last_valid_ruler` | country | character |  |
| `law` | policy | law |  |
| `law` |  | law | yes |
| `law_policy` | country, international_organization | policy | yes |
| `leader` | unit, exploration, religious_order | character |  |
| `leader_at_index` | international_organization | character | yes |
| `leader_country` | international_organization | country |  |
| `leadership_election_resolution` | international_organization | resolution |  |
| `leading_unit` | combat_side | unit |  |
| `levy_setup` |  | levy_setup | yes |
| `linked_pop` | building | pop |  |
| `liturgical_dialect` | country | dialect |  |
| `liturgical_language` | country | language |  |
| `local_var` |  |  | yes |
| `local_variable_map` |  |  | yes |
| `location` | character, pop, combat, siege, market, exploration, work_of_art, holy_site, building, town_rights | location |  |
| `location` |  | location | yes |
| `location_rank` |  | location_rank | yes |
| `location_rank` | location | location_rank |  |
| `low_control_best_tax_base` | country | province |  |
| `market` | location | market |  |
| `market_price` | market | value | yes |
| `marriage_union` | country | international_organization |  |
| `max_great_powers` |  | value |  |
| `mercenary` | sub_unit | mercenary |  |
| `mercenary_home` | mercenary | location |  |
| `mission` |  | mission | yes |
| `mission_task` |  | mission_task | yes |
| `modifier` | location, country, unit, character, dynasty, religion, province, international_organization | value, boolean | yes |
| `most_powerful_merchant` | market | country |  |
| `mother` | character | character |  |
| `movement_definition` |  | movement_definition | yes |
| `movement_type` | movement | movement_definition |  |
| `name_culture` | sub_unit | culture |  |
| `named_script_value` |  | value, color |  |
| `naturally_assimilating_culture` | location | culture |  |
| `no` |  | boolean |  |
| `num_estate_privileges` | country | value | yes |
| `num_location_rank` | country | value | yes |
| `num_pop_type` | location | value | yes |
| `num_pop_type_in_country` | country | value | yes |
| `num_pop_type_in_province` | province | value | yes |
| `num_possible_estate_privileges` | country | value | yes |
| `omen` |  | omen | yes |
| `order_calling` | religious_order | calling |  |
| `order_type` | religious_order | religious_order_type |  |
| `origin` | work_of_art, institution, disease_outbreak, disease | location |  |
| `original_attacker_leader` | war | country |  |
| `original_capital` | country | location | 语义未确认；见 `vanilla\vanilla-capital-and-historical-capital.md` |
| `original_defender_leader` | war | country |  |
| `original_outbreak` | disease | disease_outbreak |  |
| `overlord` | country | country |  |
| `owner` | location, unit, sub_unit, character, pop, colonial_charter, market, province, rebels, trade, privateer, exploration, mercenary, work_of_art, loan, building, disaster, cabinet, estate, bureaucracy, industry_promotion | country |  |
| `owning_unit` | sub_unit | unit |  |
| `parliament_agenda` |  | parliament_agenda | yes |
| `parliament_issue` |  | parliament_issue | yes |
| `parliament_issue` | country, international_organization | parliament_issue |  |
| `parliament_seat` | country, international_organization | location |  |
| `parliament_type` | country, international_organization | parliament_type |  |
| `parliament_type` |  | parliament_type | yes |
| `payment` |  | payment | yes |
| `peace_treaty` |  | peace_treaty | yes |
| `percentage_pop_type_in_country` | country | value | yes |
| `percentage_pop_type_in_location` | location | value | yes |
| `policy` |  | policy | yes |
| `pop_type` | pop | pop_type |  |
| `pop_type` |  | pop_type | yes |
| `pretender` | rebels | character |  |
| `prev` |  |  |  |
| `previous_owner` | location | country |  |
| `previous_ruler` | country | character |  |
| `price` | policy | price |  |
| `price` |  | price | yes |
| `produced_goods` | production_method | goods |  |
| `produced_in_country` | country | value | yes |
| `produced_in_market` | market | value | yes |
| `produced_in_world` |  | value | yes |
| `production_method` |  | production_method | yes |
| `province` | location | province |  |
| `province` | country | province | yes |
| `province_capital` | province | location |  |
| `province_definition` | location, colonial_charter, province | province_definition |  |
| `province_definition` |  | province_definition | yes |
| `raw_material` | location | goods |  |
| `raw_material_location` | location | goods |  |
| `rebel` | character, pop | rebels |  |
| `recruitment_method` |  | recruitment_method | yes |
| `regency_type` |  | regency_type | yes |
| `regency_type` | country | regency_type |  |
| `regent` | country | character |  |
| `region` | location, province, province_definition, area | region |  |
| `region` |  | region | yes |
| `relation_type` |  | relation_type | yes |
| `religion` | country, sub_unit, character, dynasty, pop, rebels, mercenary, movement, religious_order | religion |  |
| `religion` |  | religion | yes |
| `religion_group` |  | group | yes |
| `religious_aspect` |  | religious_aspect | yes |
| `religious_faction` |  | religious_faction | yes |
| `religious_figure` |  | religious_figure | yes |
| `religious_figure` | character | religious_figure |  |
| `religious_focus` |  | religious_focus | yes |
| `religious_head` | religion | country |  |
| `religious_order` | location, character | religious_order |  |
| `religious_order_type` |  | religious_order_type | yes |
| `religious_school` | country, character | religious_school |  |
| `religious_school` |  | religious_school | yes |
| `removal_price` | bureaucracy_type, bureaucracy | price | yes |
| `removal_price_modifier` | bureaucracy | value |  |
| `resolution` | active_resolution | resolution |  |
| `resolution` |  | resolution | yes |
| `resolution_proposer` | active_resolution | country |  |
| `resolution_target` | active_resolution |  | yes |
| `resolution_vote` |  |  | yes |
| `revolutionary_target` |  | country |  |
| `road_type` |  | road_type | yes |
| `root` |  |  |  |
| `rule_end_date` | character | date | yes |
| `ruler` | country | character |  |
| `ruler_or_heir_if_regent` | country | character |  |
| `ruler_or_regent` | country | character |  |
| `scope` |  |  | yes |
| `scriptable_hint_definition` |  | scriptable_hint_definition | yes |
| `scripted_geography` |  | scripted_geography | yes |
| `sea_zone` | location | location |  |
| `second_best_market` | location | market |  |
| `secondary_culture` | location | culture |  |
| `secondary_otherwise_primary_culture` | location | culture |  |
| `siege` | location, unit | siege |  |
| `siege_defender` | siege | country |  |
| `siege_main_attacker` | siege | country |  |
| `situation` |  | situation | yes |
| `societal_value` | country | value | yes |
| `societal_value_type` |  | societal_value_type | yes |
| `special_status` |  | special_status | yes |
| `special_status` | parliament_issue | special_status |  |
| `stockpile_in_market` | market | value | yes |
| `sub_continent` |  | sub_continent | yes |
| `sub_continent` | location, province, province_definition, area, region | sub_continent |  |
| `sub_unit_category` | sub_unit | sub_unit_category |  |
| `sub_unit_category` |  | sub_unit_category | yes |
| `sub_unit_count` | unit | value | yes |
| `sub_unit_fraction` | unit | value | yes |
| `sub_unit_strength` | unit | value | yes |
| `subject_military_stance` |  | military_stance | yes |
| `subject_type` | country | subject_type |  |
| `subject_type` |  | subject_type | yes |
| `subunit_home` | sub_unit | location |  |
| `succession_law` | country | heir_selection |  |
| `target_price` | market | value | yes |
| `this` |  |  |  |
| `to_market` | trade | market |  |
| `top_overlord` | country | country |  |
| `top_overlord_in_war` | war | country | yes |
| `top_overlord_or_this` | country | country |  |
| `top_owner` | location | country |  |
| `topography` |  | topography | yes |
| `total_building_levels_including_construction` | country | value | yes |
| `total_effective_building_levels` | country | value | yes |
| `total_sub_unit_category_in_unit` | unit | value | yes |
| `total_sub_unit_count` | country | value | yes |
| `total_sub_unit_strength` | country | value | yes |
| `total_sub_unit_type_count` | country | value | yes |
| `total_sub_unit_type_strength` | country | value | yes |
| `town_rights_type` |  | town_rights_type | yes |
| `town_rights_type` | town_rights | town_rights_type |  |
| `traded_goods` | trade | goods |  |
| `traded_in_market` | market | value | yes |
| `trait` |  | trait | yes |
| `type` | sub_unit | unit_type |  |
| `union` | country | international_organization |  |
| `unit` | character | unit |  |
| `unit_ability` |  | unit_ability | yes |
| `unit_destination` | unit | location |  |
| `unit_formation_preference` |  | unit_formation_preference | yes |
| `unit_location` | unit | location |  |
| `unit_next_location` | unit | location |  |
| `unit_type` |  | unit_type | yes |
| `upgrade_demand` | production_method | demand |  |
| `value` |  | value |  |
| `var` |  |  | yes |
| `variable_map` |  |  | yes |
| `vegetation` |  | vegetation | yes |
| `vote_in_active_resolution` | active_resolution |  | yes |
| `war_goal_province` | war | province |  |
| `war_with_country` | country | war | yes |
| `work_of_art` |  | work_of_art | yes |
| `work_of_art_type` | work_of_art | work_of_art_type |  |
| `work_of_art_type` |  | work_of_art_type | yes |
| `yes` |  | boolean |  |
