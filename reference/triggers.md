# 参考资料：triggers 触发器速查（script_docs 生成）

> 按触发器名排列；"说明"取自引擎文档首段，"支持 scope"决定可用作用域。
> **来源**：游戏内 `script_docs` 生成（`docs\` 日志），**EU5 1.4.5**，2026-10-30。
> **体量**：**检索用清单，不必通读**。
> 首都三触发器（`is_capital` / `is_historical_capital` / `counts_as_capital`）的机制说明见 `vanilla\vanilla-capital-and-historical-capital.md`。

| 名称 | 说明 | 支持 scope | 目标 / 参数 |
|---|---|---|---|
| `active_religious_focus` | Checks if a country is researching a certain religious focus | country |  |
| `add_estate_satisfaction_utility` | Utility of adding however much estate satisfaction to the country - usage: add_estate_satisfaction_utility(<estate>\|<amount>) or add_estate_satisfacti… | country | <, <=, =, !=, >, >= |
| `add_static_modifier_utility` | Checks the AI utility of adding a static modifier to the scoped object add_static_modifier_utility = { modifier = X value >= Y } Where X is the name o… | location, country, character | <, <=, =, !=, >, >= |
| `add_to_temporary_list` | Saves a temporary target for use during the trigger execution This is used to build lists in triggers. If used within an any-trigger, placement within… | none/any |  |
| `adjacent_to_owned_by` | is the area/location adjacent to an area with a country's presence in it? | location, area | country tag/country scope → country |
| `adjacent_to_owned_or_owned_by_subject` | is the area/location adjacent to an area with a country's or one of its subjects' presence in it? | location, area | country tag/country scope → country |
| `adm` | The adm ability of the character | character | <, <=, =, !=, >, >= |
| `advance_no_longer_activated` | Checks if a country has researched a certain advance but it's not useable at the moment because of conditions | country |  |
| `age_in_days` | How old is a character??? | character | <, <=, =, !=, >, >= |
| `age_in_years` | How old is a character??? | character | <, <=, =, !=, >, >= |
| `age_preference` | checks a countries age preference for current age | country |  |
| `agenda_for_estate_type` | Check if the parliament agenda can be available for the specified estate type | parliament_agenda | estate_type scope → estate_type |
| `agenda_for_special_status` | Check if the parliament agenda can be available for the specified special status | parliament_agenda | estate_type scope → estate_type |
| `ai_country_should_colonize` | should this country pursue colonization at all? Wealth or an eager tag, allowed by the colonisation game rule, and not a subject other than a colonial… | country | yes/no |
| `ai_issue_voting_bias` | gets the AI evaluation score for voting bias from the international organization | none/any | <, <=, =, !=, >, >= |
| `ai_parliament_issue_resolution_vote_bias` | gets the AI evaluation score for resolution voting bias from the parliament issue | parliament_issue | <, <=, =, !=, >, >= |
| `ai_policy_reason_to_join` | Gets the AI evaluation score for joining an IO due to its policies ai_policy_reason_to_join = { actor = <country scope> international_organization = <… | policy | <, <=, =, !=, >, >= |
| `ai_policy_resolution_keep_bias` | Gets the AI evaluation score for keeping the policy in a vote ai_policy_resolution_keep_bias = { actor = <country scope> international_organization = … | policy | <, <=, =, !=, >, >= |
| `ai_policy_resolution_propose_bias` | Gets the AI evaluation score for propsoing the policy in a vote ai_policy_resolution_propose_bias = { actor = <country scope> international_organizati… | policy | <, <=, =, !=, >, >= |
| `ai_policy_resolution_vote_bias` | Gets the AI evaluation score for resolution voting bias from the policy ai_policy_resolution_vote_bias = { actor = <country scope> international_organ… | policy | <, <=, =, !=, >, >= |
| `ai_target_antagonism` | Target antagonism set by ambitions. Usage: ai_target_antagonism = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_favours` | Target favors set by ambitions. Usage: ai_target_favours = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_opinion` | Target opinion set by ambitions. Usage: ai_target_opinion = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_spy_network` | Target spy network set by ambitions. Usage: ai_target_spy_network = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_subject_loyalty` | Target subject loyalty set by ambitions. Usage: ai_target_subject_loyalty = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_subvert` | Target subvert set by ambitions. Usage: ai_target_subvert = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_target_trust` | Target trust set by ambitions. Usage: ai_target_trust = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `ai_wants_convert` | religion is one AI wants | religion | yes/no |
| `ai_will_do` | gets the AI evaluation score of the supplied generic action ofr the supplied country | none/any | <, <=, =, !=, >, >= |
| `ai_will_select_calling` | Evaluates the value of the AI selecting this particular calling. Usage: ai_will_select_calling = { calling = <calling scope> (location = <location sco… | country | <, <=, =, !=, >, >= |
| `all_false` | true if all children are false (equivalent to NOR) | none/any |  |
| `all_holy_sites_owned_by_or_below_of` | Check that every holy site of the religion has its location owned by the target country, its subjects, or its subjects' subjects | religion | country tag/country scope → country |
| `allow_female` | checks if a succession law allows female rulers | heir_selection | yes/no |
| `allow_male` | checks if a succession law allows male rulers | heir_selection | yes/no |
| `allow_subject_creation` | Check if a subject type allows subject creation | subject_type | yes/no |
| `allowed_bureaucracies` | Check if the country is allowed bureaucracies | country | yes/no |
| `allows_female_rulers` | country allows female rulers | country | yes/no |
| `allows_male_rulers` | country allows male rulers | country | yes/no |
| `always` | checks if the assigned yes/no value is true always = yes # always succeeds always = no  # always fails always = scope:a_boolean_value # evaluated at r… | none/any | yes/no |
| `always_loyal` | if estate type is crown | estate_type | yes/no |
| `and` | all inside trigger must be true | none/any |  |
| `annexation_cost` | How much does the target country cost for the current country to annex? Usage: annexation_cost = { target = <traget country> value = <script value> } … | country | <, <=, =, !=, >, >= |
| `annexation_progress` | How high is the annexation progress of the target country for the current country scope? Usage: annexation_progress = { target = <traget country> valu… | country | <, <=, =, !=, >, >= |
| `antagonism` | is the country's antagonism towards the target greater or equal than the value? antagonism = { target = X [*value >/</>=/<= Y* or *value = { min max }… | country | <, <=, =, !=, >, >= |
| `any_accepted_culture` | Iterate through all accepted cultures in a country any_accepted_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → culture |
| `any_active_disaster` | Iterate through all active disasters for a country any_active_disaster = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | country | → disaster |
| `any_active_estate` | Iterate through all active estates (non-crown) any_active_estate = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → estate_type |
| `any_active_resolution` | Iterate through all currently active resolutions in an international organization or situation any_active_resolution = { <count=num/all> / <percent=fi… | international_organization, situation | → active_resolution |
| `any_adjacent_ports_to_area` | Iterate through all adjacent ports of an seazone area any_adjacent_ports_to_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | area | → location |
| `any_advance_definition` | Iterate through all advance definitions any_advance_definition = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → advance_type |
| `any_allowed_estate_in_heir_selection` | Iterate through all allowed estates a HeirSelection has any_allowed_estate_in_heir_selection = { <count=num/all> / <percent=fixed_point> <triggers> } … | heir_selection | → estate_type |
| `any_ancestor` | Iterate through all ancestors (parents, grandparents etc) of a character any_ancestor = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | character | → character |
| `any_area` | Iterate through all existing areas any_area = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → area |
| `any_area_in_region` | Iterate through all areas in a region any_area_in_region = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | region | → area |
| `any_area_in_scripted_geography` | Iterate through all areas in a scripted geography any_area_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | scripted_geography | → area |
| `any_area_with_core` | Iterate through all areas with cored locations in a country any_area_with_core = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing … | country | → area |
| `any_area_with_owned_province` | Iterate through all areas with owned provinces in a country any_area_with_owned_province = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | country | → area |
| `any_army` | Iterate through all armies in a country any_army = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → unit |
| `any_artist` | Iterate through all artists in a country any_artist = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → character |
| `any_attacker` | Iterate through all attackers of a war any_attacker = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | war | → country |
| `any_available_dynasty_member` | Iterate through adult dynasty members who are not a ruler or heir (cached) any_available_dynasty_member = { <count=num/all> / <percent=fixed_point> <t… | dynasty | → character |
| `any_avatar_for_god` | Iterate through all Avatars of a God any_avatar_for_god = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | god | → avatar |
| `any_besieging_unit` | Iterate through all units participating in a siege any_besieging_unit = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all s… | siege | → unit |
| `any_betrothal` | Iterate through all royal betrothal countries any_betrothal = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_border_location` | Iterate through all owned location in a country which border locations not owned by the current country scope. any_border_location = { <count=num/all>… | country | → location |
| `any_buildable_building_type` | Iterate through all the building types a country can build any_buildable_building_type = { <count=num/all> / <percent=fixed_point> <triggers> } Reads … | country | → building_type |
| `any_building_owned_by_estate` | Iterate through all buildings that an estate has any_building_owned_by_estate = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | estate | → building |
| `any_building_type` | Iterate through all the building types any_building_type = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → building_type |
| `any_buildings_in_location` | Iterate through all buildings in a location any_buildings_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all s… | location | → building |
| `any_cabinet` | Iterate through all actions in a country's cabinet any_cabinet = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → cabinet |
| `any_cabinet_action` | Iterate through all actions in a country's cabinet actions any_cabinet_action = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | country | → cabinet_action |
| `any_cabinet_character` | Iterate through all characters in a country that is in the cabinet any_cabinet_character = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | country | → character |
| `any_calling` | Iterate through all callings any_calling = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → calling |
| `any_cardinal_in_country` | Iterate through all Cardinals in a country any_cardinal_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | country | → character |
| `any_cardinal_in_religion` | Iterate through all Cardinals in a Religion any_cardinal_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sc… | religion | → character |
| `any_casus_belli_on_us` | Iterate through all countries have a casus belli on us any_casus_belli_on_us = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing fo… | country | → country |
| `any_casus_belli_target` | Iterate through all countries we have a casus belli on any_casus_belli_target = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | country | → country |
| `any_center` | Iterate through all subunits on the center of a combat-side any_center = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | combat_side | → sub_unit |
| `any_character` | Iterate through all characters in a country any_character = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → character |
| `any_character_in_dynasty` | Iterate through all living characters in a Dynasty any_character_in_dynasty = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for… | dynasty | → character |
| `any_character_in_order` | Iterate through all living member characters of a religious order any_character_in_order = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | religious_order | → character |
| `any_character_supporting_rebel` | Iterate through all characters supporting a rebel any_character_supporting_rebel = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | rebels | → character |
| `any_child` | Iterate through all children of a character any_child = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | character | → character |
| `any_circle_member` | Iterate through all member countries of an Imperial Circle any_circle_member = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing fo… | imperial_circle | → country |
| `any_close_relative` | Iterate through all close relatives of a character any_close_relative = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all s… | character | → character |
| `any_coast_border_location` | Iterate through all bordering, or across one seazone of a location any_coast_border_location = { <count=num/all> / <percent=fixed_point> <triggers> } … | location | → location |
| `any_colonial_charter` | Iterate through all colonial charters in a country any_colonial_charter = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → colonial_charter |
| `any_colonial_charter_goal` | Iterate through all colonial charter goal types any_colonial_charter_goal = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → colonial_charter_goal |
| `any_colonial_claim_province_definition` | Iterate through all province definitions with colonial claims from the scope country. Usage with country = <country> any_colonial_claim_province_defin… | country | → province_definition |
| `any_colonial_country` | Iterate through all colonial countries in the world any_colonial_country = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_colonial_overlord` | Iterate through all colonial overlord countries in the world any_colonial_overlord = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_colonial_top_overlord` | Iterate through all countries in the world that have a colonial country among their subjects or their subjects subjects and so on any_colonial_top_ove… | none/any | → country |
| `any_connected_location` | Iterate through all locations in the same country as the scope location that are connected by land or strait any_connected_location = { <count=num/all… | location | → location |
| `any_construction_material_for_building_type` | Iterate through all goods required to construct a building type any_construction_material_for_building_type = { <count=num/all> / <percent=fixed_point… | building_type | → goods |
| `any_continent` | Iterate through all existing continents any_continent = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → continent |
| `any_continent_in_scripted_geography` | Iterate through all continents in a scripted geography any_continent_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <triggers> } Re… | scripted_geography | → continent |
| `any_controlled_location` | Iterate through all controlled location in a country any_controlled_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing fo… | country | → location |
| `any_core_in_location` | Iterate through all cores in a location any_core_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | location | → country |
| `any_core_location` | Iterate through all core locations in a country any_core_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | country | → location |
| `any_country` | Iterate through all existing countries any_country = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_country_annexing_us` | Iterate through all countries which are currently annexing the current country scope. any_country_annexing_us = { <count=num/all> / <percent=fixed_poi… | country | → country |
| `any_country_at_war_with` | Iterate through all countries at war with any_country_at_war_with = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | country | → country |
| `any_country_borrowed_from` | Iterate through all countries a country has borrowed from any_country_borrowed_from = { <count=num/all> / <percent=fixed_point> <triggers> } Reads not… | country | → country |
| `any_country_in_culture` | Iterate through all countries with this primary culture any_country_in_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing … | culture | → country |
| `any_country_in_culture_group` | Iterate through all countries in a culture group. any_country_in_culture_group = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing … | culture_group | → country |
| `any_country_in_diplomatic_range` | Iterate through all countries in diplomatic range any_country_in_diplomatic_range = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothi… | country | → country |
| `any_country_in_dynasty` | Iterate through all countries in a Dynasty any_country_in_dynasty = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | dynasty | → country |
| `any_country_in_hierarchy` | Iterate through every country in the entire overlord/subject hierarchy, from the independent top overlord to the deepest subjects any_country_in_hiera… | country | → country |
| `any_country_in_religion` | Iterate through all countries in a religion any_country_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sco… | religion | → country |
| `any_country_in_religion_group` | Iterate through all countries in a religion group. any_country_in_religion_group = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | group | → country |
| `any_country_in_religious_order` | Iterate through all countries a religious order has a presence in any_country_in_religious_order = { <count=num/all> / <percent=fixed_point> <triggers… | religious_order | → country |
| `any_country_in_religious_school` | Iterate through all countries within a school any_country_in_religious_school = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | religious_school | → country |
| `any_country_including_inactive` | Iterate through all countries, including inactive any_country_including_inactive = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_country_lent_to` | Iterate through all countries a country has lent to any_country_lent_to = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → country |
| `any_country_of_country_type` | Iterate through all countries of the specified type. Usage: country_of_country_type = { type = <country type> } any_country_of_country_type = { <count… | none/any | → country |
| `any_country_sub_unit` | Iterate through all subunits in all units in a country any_country_sub_unit = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for… | country | → sub_unit |
| `any_country_supporting_rebel` | Iterate through all countries supporting a rebel any_country_supporting_rebel = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | rebels | → country |
| `any_country_that_can_be_called_defensively` | Iterate through all countries that may be called into a defensive war. any_country_that_can_be_called_defensively = { <count=num/all> / <percent=fixed… | country | → country |
| `any_country_that_can_be_called_offensively` | Iterate through all countries that may be called into an offensive war. any_country_that_can_be_called_offensively = { <count=num/all> / <percent=fixe… | country | → country |
| `any_country_together_in_war_with` | Iterate through all countries which are an ally in any of the country scope's wars any_country_together_in_war_with = { <count=num/all> / <percent=fix… | country | → country |
| `any_country_wants_casus_belli_with` | all countries a country wants a casus belli with. Only for Ai any_country_wants_casus_belli_with = { <count=num/all> / <percent=fixed_point> <triggers… | country | → country |
| `any_country_wants_military_access_in` | all countries a country wants military access through any_country_wants_military_access_in = { <count=num/all> / <percent=fixed_point> <triggers> } Re… | country | → country |
| `any_country_wants_to_attack` | all countries a country wants to attack any_country_wants_to_attack = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sco… | country | → country |
| `any_country_wants_to_subjugate` | all countries a country wants to subjugate any_country_wants_to_subjugate = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | country | → country |
| `any_country_we_are_annexing` | Iterate through all countries which are currently annexed by the current country scope. any_country_we_are_annexing = { <count=num/all> / <percent=fix… | country | → country |
| `any_country_with_capital_in_geography` | Iterate through all countries which have their capital in the specified geography any_country_with_capital_in_geography = { <count=num/all> / <percent… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `any_country_with_cardinals` | Iterate through all countries with cardinals in a religion any_country_with_cardinals = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | religion | → country |
| `any_country_with_chivalric_order` | Iterate through all countries with an active chivalric order any_country_with_chivalric_order = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_country_with_coalition_grade_antagonism_against_us` | Iterate through all countries who have coalition grade antagonism against us any_country_with_coalition_grade_antagonism_against_us = { <count=num/all… | country | → country |
| `any_country_with_relation_that_can_be_annulled` | Iterate through all countries which have an annullable relation with the scope country. any_country_with_relation_that_can_be_annulled = { <count=num/… | country | → country |
| `any_country_with_special_status_of_type` | Iterate through all countries in the international organization which have the specified special status any_country_with_special_status_of_type = { <c… | international_organization | → country |
| `any_country_with_succession_law` | Iterate through all countries with a cached succession law (set cached = yes in the heir_selection to use this) any_country_with_succession_law = { <c… | none/any | → country |
| `any_culture` | Iterate through all cultures any_culture = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → culture |
| `any_culture_group` | Iterate through all culture groups the culture is in. any_culture_group = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | culture | → culture_group |
| `any_culture_in_culture_group` | Iterate through all cultures in a culture group. any_culture_in_culture_group = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | culture_group | → culture |
| `any_current_avatars` | Iterate through all Avatars a country has any_current_avatars = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → avatar |
| `any_current_bureaucracy` | Iterate through all Bureaucracies a country has any_current_bureaucracy = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → bureaucracy |
| `any_current_bureaucracy_type` | Iterate through all Bureaucracy types a country has any_current_bureaucracy_type = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | country | → bureaucracy_type |
| `any_current_gods` | Iterate through all Gods a country worships any_current_gods = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → god |
| `any_current_law` | Iterate through all laws of a country. any_current_law = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → law |
| `any_current_law_in_international_organization` | Iterate through all laws that are codified in the international organization any_current_law_in_international_organization = { <count=num/all> / <perc… | international_organization | → law |
| `any_current_policy` | Iterate through all policies that are codified in the country any_current_policy = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | country | → policy |
| `any_current_policy_in_international_organization` | Iterate through all policies that are codified in the international organization any_current_policy_in_international_organization = { <count=num/all> … | international_organization | → policy |
| `any_current_reforms` | Iterate through all Government Reforms a country has any_current_reforms = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for al… | country | → government_reform |
| `any_current_war` | Iterate through all wars of a country any_current_war = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → war |
| `any_defender` | Iterate through all defenders of a war any_defender = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | war | → country |
| `any_descendant` | Iterate through all descendants (children, grandchildren etc) of a character any_descendant = { <count=num/all> / <percent=fixed_point> <triggers> } R… | character | → character |
| `any_discriminated_culture` | Iterate through all discriminated cultures in a country any_discriminated_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothi… | country | → culture |
| `any_disloyal_subject` | Iterate through all loyal subject countries any_disloyal_subject = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes… | country | → country |
| `any_dynasty` | Iterate through all dynasties in a country any_dynasty = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → dynasty |
| `any_east_of_province_definition` | Iterate through all province-definitions east of a province-definition any_east_of_province_definition = { <count=num/all> / <percent=fixed_point> <tr… | province_definition | → province_definition |
| `any_election_candidates` | Iterate through all election candidates of a country with elections! any_election_candidates = { <count=num/all> / <percent=fixed_point> <triggers> } … | country | → character |
| `any_enemy` | Iterate through all Enemy countries any_enemy = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_enemy_war_leader` | Iterate through all countries which are leading a war against the scope any_enemy_war_leader = { <count=num/all> / <percent=fixed_point> <triggers> } … | country | → country |
| `any_estate` | Iterate through all estates in a country any_estate = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → estate |
| `any_estate_privilege` | Iterate through all current estate privileges of a Country any_estate_privilege = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing… | country | → estate_privilege |
| `any_estate_type_preferring` | Iterate through all estate types that a prefer a policy any_estate_type_preferring = { <count=num/all> / <percent=fixed_point> <triggers> } Reads noth… | policy | → estate_type |
| `any_estate_type_that_dislikes_bureaucracy` | Iterate through all estate types that do NOT prefer a bureaucracy any_estate_type_that_dislikes_bureaucracy = { <count=num/all> / <percent=fixed_point… | bureaucracy_type | → estate_type |
| `any_estate_type_that_likes_bureaucracy` | Iterate through all estate types that a prefer a bureaucracy any_estate_type_that_likes_bureaucracy = { <count=num/all> / <percent=fixed_point> <trigg… | bureaucracy_type | → estate_type |
| `any_every_rented_out_mercenary` | Iterate through all lent units currently employed by another country, scoped from the lending country any_every_rented_out_mercenary = { <count=num/al… | country | → mercenary |
| `any_expedition` | Iterate through all expeditions for a country any_expedition = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → expedition |
| `any_expedition_type` | Iterate through all expedition types any_expedition_type = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → expedition_type |
| `any_exploration_from_country` | Iterate through all Explorations a country has any_exploration_from_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for… | country | → exploration |
| `any_export` | Iterate through all exports in a market any_export = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | market | → trade |
| `any_export_from_location` | Iterate through all exports from location any_export_from_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | location | → location |
| `any_false` | true if any child is false (equivalent to NAND) | none/any |  |
| `any_food_goods` | Iterate through all food-goods any_food_goods = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → goods |
| `any_foreign_building_countries_in_location` | Iterate through all foreign building countries in a location any_foreign_building_countries_in_location = { <count=num/all> / <percent=fixed_point> <t… | location | → country |
| `any_foreign_buildings_in_location` | Iterate through all foreign buildings in a location any_foreign_buildings_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | location | → building |
| `any_fort_in_country` | Iterate through all Forts in a country any_fort_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → location |
| `any_friendly_coast_border_location` | Iterate through all friendly bordering, or across one seazone of a location any_friendly_coast_border_location = { <count=num/all> / <percent=fixed_po… | location | → location |
| `any_friendly_country` | Iterate through all countries with relations marked as friendly any_friendly_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | country | → country |
| `any_friendly_or_high_opinion_country` | Iterate through all countries with relations marked as friendly or that we have a high opinion of set in defines any_friendly_or_high_opinion_country … | country | → country |
| `any_friendly_to_friendly_country` | Iterate through all friends of our friends any_friendly_to_friendly_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for… | country | → country |
| `any_friendly_to_hostile_country` | Iterate through all friends of our enemies any_friendly_to_hostile_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for … | country | → country |
| `any_god_in_religion` | Iterate through all Gods in a Religion any_god_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | religion | → god |
| `any_good_in_demand` | Iterate through all goods in a goods demand any_good_in_demand = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | demand | → goods |
| `any_goods` | Iterate through all types of goods any_goods = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → goods |
| `any_graphical_culture_in_culture` | Iterate through all graphical culture in a culture any_graphical_culture_in_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads not… | culture | → graphical_culture |
| `any_great_power` | Iterate through all great powers any_great_power = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_heathen_location` | Iterate through all heathen locations in a country any_heathen_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → location |
| `any_heretic_location` | Iterate through all Heretic locations in a country any_heretic_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → location |
| `any_hired_mercenary` | Iterate through mercenaries a country has hired any_hired_mercenary = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sco… | country | → mercenary |
| `any_historical_enemy` | Iterate through all historical Enemy countries any_historical_enemy = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sco… | country | → country |
| `any_historical_rival` | Iterate through all historical rival countries any_historical_rival = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sco… | country | → country |
| `any_holy_site_in_country` | Iterate through all Holy Sites in a country any_holy_site_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sc… | country | → holy_site |
| `any_holy_site_in_religion` | Iterate through all Holy Sites in a Religion any_holy_site_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | religion | → holy_site |
| `any_hostile_country` | Iterate through all countries with relations marked as hostile any_hostile_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads noth… | country | → country |
| `any_hostile_or_low_opinion_country` | Iterate through all countries with relations marked as hostile or that we have a low opinion of set in defines any_hostile_or_low_opinion_country = { … | country | → country |
| `any_hostile_to_friendly_country` | Iterate through all enemies of our friends any_hostile_to_friendly_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for … | country | → country |
| `any_hostile_to_hostile_country` | Iterate through all enemies of our enemies any_hostile_to_hostile_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | country | → country |
| `any_imperial_circle` | Iterate through all Imperial Circles in the scope international organization any_imperial_circle = { <count=num/all> / <percent=fixed_point> <triggers… | international_organization | → imperial_circle |
| `any_imperial_circle_country_is_member_of` | Iterate through all Imperial Circles the scope country is a member of any_imperial_circle_country_is_member_of = { <count=num/all> / <percent=fixed_po… | country | → imperial_circle |
| `any_import` | Iterate through all imports in a market any_import = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | market | → trade |
| `any_import_from_location` | Iterate through all Imports from location any_import_from_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | location | → location |
| `any_in_global_list` | Iterate through all items in global list. list = name or variable = name any_in_global_list = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any |  |
| `any_in_list` | Iterate through all items in list. list = name or variable = name any_in_list = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any |  |
| `any_in_local_list` | Iterate through all items in local list. list = name or variable = name any_in_local_list = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any |  |
| `any_institutions_embraced` | Iterate through all institutions a country has embraced any_institutions_embraced = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothi… | country | → institution |
| `any_international_organization` | Iterate through all international organizations any_international_organization = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → international_organization |
| `any_international_organization_elector` | Iterate through all countries with an elector special status in the international organization any_international_organization_elector = { <count=num/a… | international_organization | → country |
| `any_international_organization_enemy` | Iterate through all countries that are enemies of the international organization any_international_organization_enemy = { <count=num/all> / <percent=f… | international_organization | → country |
| `any_international_organization_member` | Iterate through all countries that are members of the international organization any_international_organization_member = { <count=num/all> / <percent=… | international_organization | → country |
| `any_international_organization_owned_location` | Iterate through all locations that are owned by the international organization any_international_organization_owned_location = { <count=num/all> / <pe… | international_organization | → location |
| `any_international_organization_owner` | Iterate through all international organizations which own the location scope any_international_organization_owner = { <count=num/all> / <percent=fixed… | location | → international_organization |
| `any_international_organization_parliament_opposers` | Iterate through all countries that have voted AGAINST the parliament issue in the in the parliament of the international organization and support the … | international_organization | → country |
| `any_international_organization_parliament_supporter` | Iterate through all countries that have voted FOR the parliament issue in the parliament of the international organization and support the current deb… | international_organization | → country |
| `any_international_organizations_member_of` | Iterate through all international organizations a country is a member of any_international_organizations_member_of = { <count=num/all> / <percent=fixe… | country | → international_organization |
| `any_international_organizations_target_of` | Iterate through all international organizations a country is a target of any_international_organizations_target_of = { <count=num/all> / <percent=fixe… | country | → international_organization |
| `any_invited_religious_figure` | Iterate through all invited religious figures in a Country any_invited_religious_figure = { <count=num/all> / <percent=fixed_point> <triggers> } Reads… | country | → character |
| `any_key_in_global_variable_map` | Iterate through all keys in a global variable map. key_in_global_variable_map = { variable = name <...> } any_key_in_global_variable_map = { <count=nu… | none/any |  |
| `any_key_in_local_variable_map` | Iterate through all keys in a local variable map. key_in_local_variable_map = { variable = name <...> } any_key_in_local_variable_map = { <count=num/a… | none/any |  |
| `any_key_in_variable_map` | Iterate through all keys in a variable map. key_in_variable_map = { variable = name <...> } any_key_in_variable_map = { <count=num/all> / <percent=fix… | none/any |  |
| `any_known_country` | Iterate through all known countries any_known_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_known_institution` | Iterate through all institutions a country knows of any_known_institution = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | country | → institution |
| `any_left_flank` | Iterate through all subunits on the left-flank of a combat-side any_left_flank = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing … | combat_side | → sub_unit |
| `any_lent_loan` | Iterate through all loans that a country lent any_lent_loan = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → loan |
| `any_loan` | Iterate through all loans in a country any_loan = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → loan |
| `any_loan_lent_to_country` | Iterate through all loans a country has lent to the supplied borrower country any_loan_lent_to_country = { <count=num/all> / <percent=fixed_point> <tr… | country | → loan |
| `any_location_in_area` | Iterate through all Locations in a area any_location_in_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | area | → location |
| `any_location_in_continent` | Iterate through all Locations in a continent any_location_in_continent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | continent | → location |
| `any_location_in_market` | Iterate through all locations in a market any_location_in_market = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes… | market | → location |
| `any_location_in_province` | Iterate through all Locations in a province any_location_in_province = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sc… | province | → location |
| `any_location_in_province_definition` | Iterate through all Locations in a province definition any_location_in_province_definition = { <count=num/all> / <percent=fixed_point> <triggers> } Re… | province_definition | → location |
| `any_location_in_region` | Iterate through all Locations in a region any_location_in_region = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes… | region | → location |
| `any_location_in_religious_order` | Iterate through all locations (holdings) owned by a religious order any_location_in_religious_order = { <count=num/all> / <percent=fixed_point> <trigg… | religious_order | → location |
| `any_location_in_scripted_geography` | Iterate through all locations in a scripted geography any_location_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | scripted_geography | → location |
| `any_location_in_sub_continent` | Iterate through all Locations in a sub-continent any_location_in_sub_continent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing … | sub_continent | → location |
| `any_location_in_the_world` | Iterate through all location any_location_in_the_world = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → location |
| `any_location_with_movement` | Iterate through all locations affected by the scope movement any_location_with_movement = { <count=num/all> / <percent=fixed_point> <triggers> } Reads… | movement | → location |
| `any_locations_with_town_rights_in_country` | Iterate through all locations with Town Rights in a country any_locations_with_town_rights_in_country = { <count=num/all> / <percent=fixed_point> <tri… | country | → location |
| `any_loyal_subject` | Iterate through all loyal subject countries any_loyal_subject = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_maritime_area` | Iterate through all maritime areas for a country any_maritime_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | country | → area |
| `any_market_center_in_country` | Iterate through all markets in a country which market centers are owned by the country any_market_center_in_country = { <count=num/all> / <percent=fix… | country | → market |
| `any_market_in_world` | Iterate through all markets in the world any_market_in_world = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → market |
| `any_market_present_in_country` | Iterate through all markets in a country any_market_present_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | country | → market |
| `any_market_with_merchants` | Iterate through all markets a country has active merchants any_market_with_merchants = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | country | → market |
| `any_mercenary` | Iterate through all mercenaries in the world any_mercenary = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → mercenary |
| `any_mercenary_sub_unit` | Iterate through all subunits in a Mercenary any_mercenary_sub_unit = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | mercenary | → sub_unit |
| `any_merchant_in_market` | Iterate through all merchants in a market any_merchant_in_market = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes… | market | → country |
| `any_movement` | Iterate through all movements any_movement = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → movement |
| `any_movement_in_country` | Iterate through all movements in a country any_movement_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | country | → movement |
| `any_movement_in_culture` | Iterate through all movements in a culture any_movement_in_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | culture | → movement |
| `any_movement_in_religion` | Iterate through all movements in a religion any_movement_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all sc… | religion | → movement |
| `any_navy` | Iterate through all navies in a country any_navy = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → unit |
| `any_neighbor_area` | Iterate through all neighboring areas in a area any_neighbor_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | area | → area |
| `any_neighbor_country` | Iterate through all neighbour countries any_neighbor_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_neighbor_location` | Iterate through all neighbors of a location any_neighbor_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | location | → location |
| `any_neighbor_province_definition` | Iterate through all neighboring ProvinceDefinitions in a ProvinceDefinition any_neighbor_province_definition = { <count=num/all> / <percent=fixed_poin… | province_definition | → province_definition |
| `any_new_world_goods` | Iterate through all newworld-goods any_new_world_goods = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → goods |
| `any_nomad_countries_in_location` | Iterate through all nomad pop countries in a location any_nomad_countries_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | location | → country |
| `any_non_state_religion_location` | Iterate through all NonStateReligion locations in a country any_non_state_religion_location = { <count=num/all> / <percent=fixed_point> <triggers> } R… | country | → location |
| `any_old_world_goods` | Iterate through all oldworld-goods any_old_world_goods = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → goods |
| `any_omen_in_country` | Iterate through all Omens active in a country any_omen_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scope… | country | → omen |
| `any_omen_in_god` | Iterate through all Omens associated with a God any_omen_in_god = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | god | → omen |
| `any_omen_in_religion` | Iterate through all Omens in a Religion any_omen_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | religion | → omen |
| `any_order_holding_in_country` | Iterate through all owned locations in a country that are holdings of a religious order any_order_holding_in_country = { <count=num/all> / <percent=fi… | country | → location |
| `any_other_core_country` | Iterate through all other countries which have a core on the current country any_other_core_country = { <count=num/all> / <percent=fixed_point> <trigg… | country | → country |
| `any_other_country` | Iterate through all other countries any_other_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_other_great_power` | Iterate through all other great powers any_other_great_power = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_other_religion_in_same_group` | Iterate through all other religions that has the same group as Religion any_other_religion_in_same_group = { <count=num/all> / <percent=fixed_point> <… | religion | → religion |
| `any_other_revolutionary` | Iterate through all other revolutionary countries any_other_revolutionary = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | country | → country |
| `any_overlord_or_above` | Iterate through your overlord, your overlord's overlord, and so on any_overlord_or_above = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | country | → country |
| `any_ownable_location` | Iterate through all ownable location any_ownable_location = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → location |
| `any_ownable_location_in_area` | Iterate through all ownable Locations in an area any_ownable_location_in_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | area | → location |
| `any_ownable_location_in_continent` | Iterate through all ownable Locations in a continent any_ownable_location_in_continent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads … | continent | → location |
| `any_ownable_location_in_province_definition` | Iterate through all ownable Locations in a province definition any_ownable_location_in_province_definition = { <count=num/all> / <percent=fixed_point>… | province_definition | → location |
| `any_ownable_location_in_region` | Iterate through all ownable Locations in a region any_ownable_location_in_region = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | region | → location |
| `any_ownable_location_in_scripted_geography` | Iterate through all ownable Locations in a scripted geography any_ownable_location_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <… | scripted_geography | → location |
| `any_ownable_location_in_sub_continent` | Iterate through all ownable Locations in a sub continent any_ownable_location_in_sub_continent = { <count=num/all> / <percent=fixed_point> <triggers> … | sub_continent | → location |
| `any_owned_building` | Iterate through all the owned buildings in a country any_owned_building = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → building |
| `any_owned_foreign_building` | Iterate through all the owned foreign buildings in a country any_owned_foreign_building = { <count=num/all> / <percent=fixed_point> <triggers> } Reads… | country | → building |
| `any_owned_foreign_building_location` | Iterate through all the location of owned foreign buildings in a country any_owned_foreign_building_location = { <count=num/all> / <percent=fixed_poin… | country | → location |
| `any_owned_foreign_building_region` | Iterate through all the regions of owned foreign buildings in a country any_owned_foreign_building_region = { <count=num/all> / <percent=fixed_point> … | country | → region |
| `any_owned_location` | Iterate through all owned location in a country any_owned_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scop… | country | → location |
| `any_owned_nomad_pop` | Iterate through all owned nomad pops in a country any_owned_nomad_pop = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all s… | country | → pop |
| `any_owned_non_rural_location` | Iterate through all owned non-rural locations in a country any_owned_non_rural_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads… | country | → location |
| `any_owned_rural_location` | Iterate through all owned rural locations in a country any_owned_rural_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing… | country | → location |
| `any_owner_in_region` | Iterate through all the countries that own locations in a region any_owner_in_region = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | region | → country |
| `any_parent` | Iterate through parents (order: father, mother) of a character. any_parent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for … | character | → character |
| `any_participating_countries` | Iterate through all Countrys participating in 1 side of a combat any_participating_countries = { <count=num/all> / <percent=fixed_point> <triggers> } … | combat_side | → country |
| `any_participating_units` | Iterate through all units participating in 1 side of a combat any_participating_units = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | combat_side | → unit |
| `any_past_court_dialect` | Iterate through all court dialects a country has had before any_past_court_dialect = { <count=num/all> / <percent=fixed_point> <triggers> } Reads noth… | country | → country |
| `any_past_liturgical_dialect` | Iterate through all liturgical dialects a country has had before any_past_liturgical_dialect = { <count=num/all> / <percent=fixed_point> <triggers> } … | country | → country |
| `any_policy_in_law` | Iterate through all policies that are part of the law scope any_policy_in_law = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing f… | law | → policy |
| `any_political_border_location` | Iterate through all owned location in a country which border another country. any_political_border_location = { <count=num/all> / <percent=fixed_point… | country | → location |
| `any_pop` | Iterate through all pops in a location or country any_pop = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | location, country | → pop |
| `any_pops_supporting_rebel` | Iterate through all pops supporting a rebel any_pops_supporting_rebel = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all s… | rebels | → pop |
| `any_port_in_country` | Iterate through all Ports in a country any_port_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → location |
| `any_possible_disaster` | Iterate through all possible disasters for a country any_possible_disaster = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for … | country | → disaster |
| `any_possible_parliament_issue` | Iterate through all possible parliament issues in a country's or an international organization's parliament any_possible_parliament_issue = { <count=n… | country, international_organization | → parliament_issue |
| `any_possible_policy` | Iterate through all possible policies of a Country that is not currently implemeted any_possible_policy = { <count=num/all> / <percent=fixed_point> <t… | country | → policy |
| `any_possible_privilege` | Iterate through all possible & allowed estate privileges of a Country that is not currently implemeted any_possible_privilege = { <count=num/all> / <p… | estate | → estate_privilege |
| `any_possible_recruit_location` | Iterate through all possible recruit locations in a country any_possible_recruit_location = { <count=num/all> / <percent=fixed_point> <triggers> } Rea… | country | → location |
| `any_potential_expansion_target` | Iterate through all countries which are potential expansion targets for an AI country any_potential_expansion_target = { <count=num/all> / <percent=fi… | country | → country |
| `any_present_country` | Iterate through all countries in the specified geography any_present_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing fo… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `any_present_culture_in_country` | Iterate through all cultures present in the country. any_present_culture_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads not… | country | → culture |
| `any_present_culture_in_location` | Iterate through all cultures present in the location. any_present_culture_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | location | → culture |
| `any_present_overlord` | Iterate through all countries which have a subject in the specified geography any_present_overlord = { <count=num/all> / <percent=fixed_point> <trigge… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `any_present_religion_in_country` | Iterate through all religions present in the country. any_present_religion_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | country | → religion |
| `any_present_religion_in_location` | Iterate through all religions present in the location. any_present_religion_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads… | location | → religion |
| `any_primary_or_accepted_culture` | Iterate through primary culture and all accepted cultures in a country. Primary is ordered first. any_primary_or_accepted_culture = { <count=num/all> … | country | → culture |
| `any_primary_or_accepted_or_tolerated_culture` | Iterate through primary culture and all accepted and all tolerated cultures in a country. Primary is ordered first. any_primary_or_accepted_or_tolerat… | country | → culture |
| `any_privateer` | Iterate through all privateers in the world any_privateer = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → privateer |
| `any_privateer_from_country` | Iterate through all privateers a country has any_privateer_from_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → privateer |
| `any_privateer_in_area` | Iterate through all privateers in a area any_privateer_in_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | area | → privateer |
| `any_production_method` | Iterate through all types of production methods. any_production_method = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → production_method |
| `any_production_method_of_building` | Iterate through all available production methods of the building. any_production_method_of_building = { <count=num/all> / <percent=fixed_point> <trigg… | building | → production_method |
| `any_province` | Iterate through all provinces in a country any_province = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → province |
| `any_province_definition` | Iterate through all existing province_definition any_province_definition = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → province_definition |
| `any_province_definition_in_area` | Iterate through all province-definitions in an area any_province_definition_in_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads not… | area | → province_definition |
| `any_province_definition_in_scripted_geography` | Iterate through all province_definitions in a scripted geography any_province_definition_in_scripted_geography = { <count=num/all> / <percent=fixed_po… | scripted_geography | → province_definition |
| `any_province_in_area` | Iterate through all provinces in an area any_province_in_area = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | area | → province |
| `any_province_in_province_definition` | Iterate through all provinces in a province-definition any_province_in_province_definition = { <count=num/all> / <percent=fixed_point> <triggers> } Re… | province_definition | → province |
| `any_rebel` | Iterate through all Rebels in a country any_rebel = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → rebels |
| `any_region` | Iterate through all existing regions any_region = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → region |
| `any_region_in_continent` | Iterate through all regions in a sub-continent any_region_in_continent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | sub_continent | → region |
| `any_region_in_scripted_geography` | Iterate through all regions in a scripted geography any_region_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | scripted_geography | → region |
| `any_related_country` | Iterate through all related countries any_related_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_religion` | Iterate through all religions any_religion = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → religion |
| `any_religion_for_god` | Iterate through all Religions of a God any_religion_for_god = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | god | → religion |
| `any_religion_in_religion_group` | Iterate through all religions in a religion group. any_religion_in_religion_group = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothi… | group | → religion |
| `any_religion_international_organization` | Iterate through all international organisations of a religion any_religion_international_organization = { <count=num/all> / <percent=fixed_point> <tri… | religion | → international_organization |
| `any_religious_aspect` | Iterate through all religious aspects of a Country any_religious_aspect = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → religious_aspect |
| `any_religious_focus` | Iterate through all completed religious focuses of a Country any_religious_focus = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | country | → religious_focus |
| `any_religious_order` | Iterate through all religious orders any_religious_order = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → religious_order |
| `any_religious_order_in_country` | Iterate through all religious orders present in a country any_religious_order_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Read… | country | → religious_order |
| `any_religious_order_in_location` | Iterate through all religious orders present at a location any_religious_order_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Re… | location | → religious_order |
| `any_religious_order_in_religion` | Iterate through all religious orders of a religion any_religious_order_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads noth… | religion | → religious_order |
| `any_religious_order_type` | Iterate through all religious order types any_religious_order_type = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → religious_order_type |
| `any_religious_school_in_religion` | Iterate through all Religious Schools in a Religion any_religious_school_in_religion = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | religion | → religious_school |
| `any_rented_out_mercenary` | Iterate through mercenaries a country has rented out to the market any_rented_out_mercenary = { <count=num/all> / <percent=fixed_point> <triggers> } R… | country | → mercenary |
| `any_required_goods` | Iterate through all goods required by the scope production method. any_required_goods = { <count=num/all> / <percent=fixed_point> <triggers> } Reads n… | production_method | → goods |
| `any_reserves` | Iterate through all subunits on the reserve of a combat-side any_reserves = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | combat_side | → sub_unit |
| `any_retreated` | Iterate through all subunits on the retreated of a combat-side any_retreated = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing fo… | combat_side | → sub_unit |
| `any_revolutionary` | Iterate through all revolutionary states any_revolutionary = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → country |
| `any_right_flank` | Iterate through all subunits on the right-flank of a combat-side any_right_flank = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | combat_side | → sub_unit |
| `any_rival` | Iterate through all rival countries any_rival = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_road_type` | Iterate through all the road types any_road_type = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → road_type |
| `any_royal_marriage` | Iterate through all royal married countries any_royal_marriage = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_ruled_international_organization` | Iterate through IOs a character rules any_ruled_international_organization = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for … | character | → international_organization |
| `any_ruler` | Iterate through all characters that have ever been rulers in a country, including the dead any_ruler = { <count=num/all> / <percent=fixed_point> <trig… | country | → character |
| `any_ruling_countries` | Iterate through countries a character rules any_ruling_countries = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes… | character | → country |
| `any_sound_toll_in_country` | Iterate through all Sound Tolls in a country any_sound_toll_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all … | country | → location |
| `any_spouse` | Iterate through all spouses of a character any_spouse = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | character | → character |
| `any_spy_network_built_in_us` | Iterate through all  countries building spy networks any_spy_network_built_in_us = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | country | → country |
| `any_sub_continent` | Iterate through all existing sub_continents any_sub_continent = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → sub_continent |
| `any_sub_continent_in_continent` | Iterate through all sub-continents in a continent any_sub_continent_in_continent = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | continent | → sub_continent |
| `any_sub_continent_in_scripted_geography` | Iterate through all subcontinents in a scripted geography any_sub_continent_in_scripted_geography = { <count=num/all> / <percent=fixed_point> <trigger… | scripted_geography | → sub_continent |
| `any_sub_unit` | Iterate through all subunits in a unit any_sub_unit = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | unit | → sub_unit |
| `any_subject` | Iterate through all subject countries any_subject = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → country |
| `any_subject_or_below` | Iterate through all subject countries and their subject countries, and so on any_subject_or_below = { <count=num/all> / <percent=fixed_point> <trigger… | country | → country |
| `any_tolerated_culture` | Iterate through all Tolerated cultures in a country any_tolerated_culture = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | country | → culture |
| `any_town_rights_in_country` | Iterate through all Town Rights in a country any_town_rights_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → town_rights |
| `any_town_rights_in_location` | Iterate through all Town Rights in a Location any_town_rights_in_Location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | location | → town_rights |
| `any_trade` | Iterate through all trades in a Country any_trade = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → trade |
| `any_trait` | Iterate through all traits of a character. any_trait = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | character | → trait |
| `any_union_partner` | Iterate through all countries which are in a personal union with the current country scope. any_union_partner = { <count=num/all> / <percent=fixed_poi… | country | → country |
| `any_unit` | Iterate through all units in a country any_unit = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | country | → unit |
| `any_unit_in_location` | Iterate through all units in a location any_unit_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | location | → unit |
| `any_valid_religion_for_aspect` | Iterate through all religion that an aspect can be for any_valid_religion_for_aspect = { <count=num/all> / <percent=fixed_point> <triggers> } Reads no… | religious_aspect | → religion |
| `any_voter` | Iterate through all voters in an active resolution any_voter = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | active_resolution | → country |
| `any_war` | Iterate through all wars going on globally any_war = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → war |
| `any_war_participant` | Iterate through all participants of a war any_war_participant = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all scopes. | war | → country |
| `any_weather_system_in_location` | Iterate through all weather systems in a location any_weather_system_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothin… | location | → weather_system |
| `any_west_of_province_definition` | Iterate through all province-definitions west of a province-definition any_west_of_province_definition = { <count=num/all> / <percent=fixed_point> <tr… | province_definition | → province_definition |
| `any_work_of_art` | Iterate through all WorkOfArts in the world any_work_of_art = { <count=num/all> / <percent=fixed_point> <triggers> } | none/any | → work_of_art |
| `any_work_of_art_by_creator` | Iterate through all work_of_art by a particular artist any_work_of_art_by_creator = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothi… | character | → work_of_art |
| `any_work_of_art_in_country` | Iterate through all work_of_art in a country any_work_of_art_in_country = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for all… | country | → work_of_art |
| `any_work_of_art_in_location` | Iterate through all work_of_art in a location any_work_of_art_in_location = { <count=num/all> / <percent=fixed_point> <triggers> } Reads nothing for a… | location | → work_of_art |
| `area_average_control` | Checks the average_control of an area | area | <, <=, =, !=, >, >= |
| `area_average_integration` | Checks the average_integration of an area | area | <, <=, =, !=, >, >= |
| `area_exploration_progress` | gets the exploration progress (0..1) for a country in the scope area | area | <, <=, =, !=, >, >= |
| `army_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `army_size` | Checks if a country has a certain army size | country | <, <=, =, !=, >, >= |
| `army_size_percentage` | Checks if a country has a certain percentage of regiments compared to expected size | country | <, <=, =, !=, >, >= |
| `army_tradition` | How much army tradition does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `army_tradition_percentage` | How high the percentage of the current army tradition compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `art_is_priceless` | Check if a work of art is priceless (cannot be bought or sold). | work_of_art | yes/no |
| `art_price` | Checks the full sale price of a work of art in gold (worth * quality factor * age factor * sell value modifier). 0 if priceless. | work_of_art | <, <=, =, !=, >, >= |
| `art_progress` | The amount of progress an artist has made on a work of art | character | <, <=, =, !=, >, >= |
| `art_quality` | Checks the quality of the artwork | work_of_art | <, <=, =, !=, >, >= |
| `art_worth` | Checks the effective gold worth of a work of art. -1 if priceless. | work_of_art | <, <=, =, !=, >, >= |
| `artist_skill` | The artist skill of the character | character | <, <=, =, !=, >, >= |
| `artist_type` | Checks if a character is a specific type of artist | character |  |
| `assert_if` | Conditionally cause an assert during run time assert_if = { limit = { X } text = Y }, where X is a trigger and Y is an optional string | none/any |  |
| `assert_read` | Conditionally cause an assert during read time assert_read = X, where X is yes or the string to be printed in the assert | none/any |  |
| `at_war` | country is at war | country | yes/no |
| `attackers_have_access_in` | checks if the attacker side has access in the country in a war | war | country tag/country scope → country |
| `available_army_levy_percentage` | Checks the percentage of available army levies | country | <, <=, =, !=, >, >= |
| `available_merchant_capacity` | gets the market available merchant capacity for a country in the scope market | market | <, <=, =, !=, >, >= |
| `available_navy_levy_percentage` | Checks the percentage of available naval levies | country | <, <=, =, !=, >, >= |
| `average_control` | Checks the average control in the country | country | <, <=, =, !=, >, >= |
| `average_control_in_home_region` | Checks the average control in the home region | country | <, <=, =, !=, >, >= |
| `average_country_literacy` | Checks if a country has a certain average_literacy | country | <, <=, =, !=, >, >= |
| `average_estate_satisfaction` | How high is the average estate satisfaction in the country? The crown estate gets ignored here. | country | <, <=, =, !=, >, >= |
| `average_location_literacy` | Checks if a location has a certain average literacy | location | <, <=, =, !=, >, >= |
| `average_prosperity` | Checks the average prosperity in the country | country | <, <=, =, !=, >, >= |
| `average_satisfaction` | Checks if a location has a certain average satisfaction of its pops | location | <, <=, =, !=, >, >= |
| `average_special_status_power` | Get the average political power of the target special status. Usage: average_special_status_power = { type = <special status> value <operator> <float>… | international_organization | <, <=, =, !=, >, >= |
| `besieger_strength` | Check the total strength of the besiegers for the siege in scope | siege | <, <=, =, !=, >, >= |
| `bias_value` | Get the value of the given bias modifier. Usage: bias_value = { modifier = <bias modifier> value <operator> <script value> } or bias_value(<bias modif… | none/any | <, <=, =, !=, >, >= |
| `birth_age` | What Age was the character born in? E.g. age_1_traditions | character | age scope → age |
| `blocks_full_annexation` | Checks if the peace treaty blocks full annexation. | peace_treaty | yes/no |
| `bond_capacity` | Checks the remaining bond issuance capacity of a country | country | <, <=, =, !=, >, >= |
| `border_distance_to` | gets distance between borders of two nations or a location/province and a nation. Usage - border_distance_to = { country = x value [operator] y } or b… | location, country, province | <, <=, =, !=, >, >= |
| `building_can_be_destroyed_by` | Check if the target country scope is capable of destroying the current building scope | building | country tag/country scope → country |
| `building_can_be_upgraded_by` | Checks if a building can be upgraded by the target country | building | country tag/country scope → country |
| `building_category` | Checks if a building or building type is linked to a certain category | building, building_type | class CBuildingCategoryDataBase key |
| `building_efficiency` | does the location have the specific efficiency of a building | location | <, <=, =, !=, >, >= |
| `building_employed_amount` | What's the current effective amount of employed workers? | building | <, <=, =, !=, >, >= |
| `building_employment_size_amount` | What's the max workers amount? | building | <, <=, =, !=, >, >= |
| `building_goods_input` | Check how much goods the scope building requires. | building | <, <=, =, !=, >, >= |
| `building_index` | Checks building index (order in which it was built) | building | <, <=, =, !=, >, >= |
| `building_level` | Check the level of  this Building? | building | <, <=, =, !=, >, >= |
| `building_levels_under_construction` | Check the level of  this Building? | building | <, <=, =, !=, >, >= |
| `building_manpower_produced` | Checks how much manpower the building type produces | building_type | <, <=, =, !=, >, >= |
| `building_max_level` | Gets the max level for a building | building | <, <=, =, !=, >, >= |
| `building_pop_type` | Checks if a building is linked to a certain pop type | building |  |
| `building_potential_profit` | Checks how much profit the building could make if at full worker capacity and full access to goods | building | <, <=, =, !=, >, >= |
| `building_produced_goods` | Checks if a building produces a certain good | building | goods scope → goods |
| `building_profit` | Checks building profit | building | <, <=, =, !=, >, >= |
| `building_sailors_produced` | Checks how many sailors the building type produces | building_type | <, <=, =, !=, >, >= |
| `building_type_is_obsolete` | Checks if the specified building type is obsolete for the scope country. | country | building_type scope → building_type |
| `building_type_max_level` | Gets the max level for a building type in a location. Usage: [location scope] building_type_max_level = { building_type = <building type scope> [owner… | location | <, <=, =, !=, >, >= |
| `bureaucracy_disliked_by_estate` | is this bureaucracy disliked by this estate? | bureaucracy | estate scope → estate |
| `bureaucracy_disliked_by_estate_type` | is this bureaucracy disliked by this estate type? | bureaucracy | estate_type scope → estate_type |
| `bureaucracy_liked_by_estate` | is this bureaucracy liked by this estate? | bureaucracy | estate scope → estate |
| `bureaucracy_liked_by_estate_type` | is this bureaucracy liked by this estate type? | bureaucracy | estate_type scope → estate_type |
| `bureaucracy_maintenance` | current maintenance of the bureaucracy 0-1 | bureaucracy | <, <=, =, !=, >, >= |
| `bureaucracy_type_disliked_by_estate` | is this bureaucracy type disliked by this estate? | bureaucracy_type | estate scope → estate |
| `bureaucracy_type_disliked_by_estate_type` | is this bureaucracy type disliked by this estate type? | bureaucracy_type | estate_type scope → estate_type |
| `bureaucracy_type_is_enabled` | Can we use a bureaucracy for a country? | country | bureaucracy_type scope → bureaucracy_type |
| `bureaucracy_type_is_visible` | Can we see a bureaucracy for a country? | country | bureaucracy_type scope → bureaucracy_type |
| `bureaucracy_type_liked_by_estate` | is this bureaucracy type liked by this estate? | bureaucracy_type | estate scope → estate |
| `bureaucracy_type_liked_by_estate_type` | is this bureaucracy type liked by this estate type? | bureaucracy_type | estate_type scope → estate_type |
| `cabinet_action_type` | Checks the type of cabinet action (ADM/DIP/MIL) | cabinet_action |  |
| `calc_true_if` | Returns true if the specified number of sub-triggers return true calc_true_if = { amount = 2 <trigger> <trigger> <trigger> } | none/any |  |
| `calling_is_current_of_order` | True if this calling is the supplied religious order's current calling. | calling | religious_order scope → religious_order |
| `calling_valid_for_type` | True if this calling may be adopted by the supplied religious order type (its valid_for_types includes that type). | calling | religious_order_type scope → religious_order_type |
| `can_add_relation` | Can the country have the specified scripted relation with another country. Usage: can_add_relation = { first = <country> second = <country> type = <re… | none/any |  |
| `can_annex_members` | Can a country annex members in the scope international organization? | international_organization | country tag/country scope → country |
| `can_be_bribe` | Check if the parliament agenda can also serve as a bribe | parliament_agenda | yes/no |
| `can_be_force_broken_in_peace_treaty` | Check if a subject type can be force broken in a peace treaty | subject_type | yes/no |
| `can_become_rank` | Check if a location can become the supplied location rank | location | location_rank scope → location_rank |
| `can_betroth_character` | Check if the current character scope can betroth the target character | character | character scope → character |
| `can_build_building` | Checks if the location/country can build the specified building. Location only checks local requirements, country checks the country scope requirement… | location, country | building_type scope → building_type |
| `can_build_unit_type` | Checks if the country can build the specified unit type. | country | unit_type scope → unit_type |
| `can_build_units_of_category` | Checks if the country can build units of the specified category. | country | sub_unit_category scope → sub_unit_category |
| `can_create_casus_belli_of_type_on` | Can the country see and create a cb of the supplied type on the target? Usage - can_create_casus_belli_of_type_on = { type = <cb type key> target = <c… | country |  |
| `can_declare_no_cb_war_on` | Can the country declare a war without any casus belli on the target country? | country | country tag/country scope → country |
| `can_declare_war_on` | Check if the current country could declare war on the target country | country | country tag/country scope → country |
| `can_do_generic_action` | Is the country capable of doing the specified generic action right now? Usage: can_do_generic_action = { generic_action = <generic action> <parameters… | country |  |
| `can_execute_prisoners` | can the prisoners in this unit be executed | unit | yes/no |
| `can_find_trade_route` | can the country find a trade route from market a to market b { from = <market> to = <market> }? | country |  |
| `can_form` | Checks if the country can form the specified formable country. | country | formable_country scope → formable_country |
| `can_hire_prisoners_as_mercenaries` | can we hire the prisoners in this unit as mercenaries | unit | yes/no |
| `can_initiate_policy_votes` | Can a country initiate votes in the scope international organization? | international_organization | country tag/country scope → country |
| `can_join_as_attacker` | Can the target country join the war in scope as attacker ? | war | country tag/country scope → country |
| `can_join_as_defender` | Can the target country join the war in scope as defender ? | war | country tag/country scope → country |
| `can_join_defensive_war_with` | Can the country join in a defensive war with the scope country? | country | country tag/country scope → country |
| `can_join_international_organization` | Can we join the supplied international organization? | country | international_organization scope → international_organization |
| `can_join_offensive_war_with` | Can the country join in an offensive war with the scope country? | country | country tag/country scope → country |
| `can_lead_expedition` | Check if the previous-scope character meets the leader requirements for this expedition type | expedition_type | yes/no |
| `can_lead_international_organization` | Can the country lead the specified international organization? | country | international_organization scope → international_organization |
| `can_leave_international_organization` | Can we leave the supplied international organization? | country | international_organization scope → international_organization |
| `can_make_subject_of` | Can the country in scope become a subject of the target country ? Same checks as the peace treaty become-subject. Usage - can_make_subject_of = { targ… | country |  |
| `can_marry_character` | Check if the current character scope can marry the target character | character | character scope → character |
| `can_pay_price` | Can the country pay the specified price? | country | price scope → price |
| `can_raise_army_levies` | Checks if the country can raise army levies | country | yes/no |
| `can_raise_levies` | Checks if the country can raise any kind of levies | country | yes/no |
| `can_raise_navy_levies` | Checks if the country can raise navy levies | country | yes/no |
| `can_ransom_prisoners` | can the prisoners in this unit be ransomed | unit | yes/no |
| `can_research_advance` | Checks if a country can research but has not yet researched a specific advance. | country |  |
| `can_revoke_privilege` | Can the country declare revoke this estate privilege? can_revoke_privilege = <privilege scope> | country | estate_privilege scope → estate_privilege |
| `can_rival` | Could the current country scope rival the target country ignoring slots and range? | country | country tag/country scope → country |
| `can_see_religious_aspect` | Checks if the input religious aspect is visible for the country in scope. | country | religious_aspect scope → religious_aspect |
| `can_see_situation` | Checks if the 'visible' trigger of the target situation is fulfilled for the country scope. | country | situation scope → situation |
| `can_sell_prisoners_into_slavery` | can the prisoners in this unit be sold as slaves | unit | yes/no |
| `can_serve_in_cabinet_of` | Checks if the character can serve in the cabinet of the target country | character | country tag/country scope → country |
| `can_share_maps_with` | Country can share maps with the supplied country? | country | country tag/country scope → country |
| `can_start_expedition` | Check if the given country can start this expedition type. Scope: expedition_type. Value: country reference (e.g. scope:actor) | expedition_type | country tag/country scope → country |
| `can_start_tutorial_lesson` | Can the specified tutorial lesson be started? can_start_tutorial_lesson = reactive_advice_succession | none/any |  |
| `can_upgrade_subunit` | returns trus if the subunit can be upgraded | sub_unit | yes/no |
| `can_upgrade_unit` | returns trus if the unit can be upgraded | unit | yes/no |
| `can_use_agenda_bribe` | Checks if estate type is allowed in parliament | country |  |
| `can_vote_in_parliament` | Can the countrs scope vote in the target international organization? | country | international_organization scope → international_organization |
| `cancel_exploration_utility` | Utility of an cancelling and exploration to the country - usage: cancel_exploration_utility(<area>) or exploration_utility = { area = <area> value <op… | country | <, <=, =, !=, >, >= |
| `capital_wealth` | Checks the total capital wealth of a country | country | <, <=, =, !=, >, >= |
| `cb_creation_progress_against` | Checks the progress of the casus belli creation against the target country in percentage. Usage: cb_creation_progress_against = { target = <country sc… | country | <, <=, =, !=, >, >= |
| `character_is_eligible_for_active_order` | Checks if the character meets the character_eligible conditions of the active chivalric order of the given country. | character | country tag/country scope → country |
| `character_is_in_order` | Checks if the character is a member of the specified chivalric order. | character | chivalric_order scope → chivalric_order |
| `character_modifier_strength` | Does the scoped character have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 cha… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `character_name` | Checks if the character has the same name key as their first name. | character |  |
| `character_nickname` | Check if the character has the same name key as their nickname | character |  |
| `circles_are_active` | Are Imperial Circles active in the scope international organization? | international_organization | yes/no |
| `climate` | Checks if a location is of a specific climate | location |  |
| `climate_count` | Returns the amount of owned locations with the specified climate. Usage: climate_count = { type = <climate scope> value <operator> <value> } or climta… | country | <, <=, =, !=, >, >= |
| `climate_percent` | Returns the percentage of owned locations with the specified climate. Usage: climate_percent = { type = <climate scope> value <operator> <value> } or … | country | <, <=, =, !=, >, >= |
| `closeness_to_equator` | Returns the value for closeness to equator for the location | location | <, <=, =, !=, >, >= |
| `colonial_charter_distance` | Straight-line distance from country capital to a province's first passable location. Cheap sort proxy for colonial_charter_utility. Usage: colonial_ch… | country | <, <=, =, !=, >, >= |
| `colonial_charter_progress` | Progress of a colonial charter - usage: colonial_charter_progress(<province definition>) or colonial_charter_progress = { province_definition = <provi… | country | <, <=, =, !=, >, >= |
| `colonial_charter_utility` | Utility of a colonial charter - usage: colonial_charter_utility(<province definition>\|<source province>) or colonial_charter_utility = { province_defi… | country | <, <=, =, !=, >, >= |
| `colonial_charter_value` | value of the colonial charter | colonial_charter | <, <=, =, !=, >, >= |
| `colonial_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `colonial_range` | The colonial range of the country | country | <, <=, =, !=, >, >= |
| `colonize_bias` | Any bias to or from this bit of geography set by ambitions. Usage: colonize_bias = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `combat_side_strength` | Checks the strength of the combat side in scope | combat_side | <, <=, =, !=, >, >= |
| `combined_special_status_power` | Get the combined special status power of ALL special statuses in the international organization | international_organization | <, <=, =, !=, >, >= |
| `combined_unique_special_status_power` | Get the combined special status power of all countries with their highest ranking special status in the international organization | international_organization | <, <=, =, !=, >, >= |
| `compare_value` | Compare the current value. | value | <, <=, =, !=, >, >= |
| `conquer_area_preference` | Gets the AI conquer preference for the supplied area - usage: conquer_area_preference(<target>) or conquer_desire = { target = <area> value <operator>… | country | <, <=, =, !=, >, >= |
| `conquer_bias` | Any bias to or from this bit of geography set by ambitions. Usage: conquer_bias = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `conquer_desire` | Gets how much the AI wants to conquer the supplied country - usage: conquer_desire(<target>) or conquer_desire = { target = <country link> value <oper… | country | <, <=, =, !=, >, >= |
| `conquistador_utility` | Utility of a conquistador - usage: conquistador_utility(<area>) or conquistador_utility = { area = <area> value <operator><threshold> } | country | <, <=, =, !=, >, >= |
| `controls` | Does the country control a specific location? | country | province id/location scope → location |
| `country_art_quality` | Checks the total art quality in a Country | country | <, <=, =, !=, >, >= |
| `country_can_join_international_organization` | Can we add a country to the supplied international organization? | country | international_organization scope → international_organization |
| `country_combined_special_status_power` | Get the political power of the country within the target international organization with all of its special statuses combined. Usage: country_combined… | country | <, <=, =, !=, >, >= |
| `country_combined_special_status_power_fraction` | Get the political power fraction of the country within the target international organization with all of its special statuses combined. Usage: country… | country | <, <=, =, !=, >, >= |
| `country_economical_base` | Checks the total economical base of a country | country | <, <=, =, !=, >, >= |
| `country_estate_loan_size` | Checks the size of a loan given by the estates to a country | country | <, <=, =, !=, >, >= |
| `country_exists` | Does the country exist? | none/any | country tag/country scope → country |
| `country_has_been_member_for_years` | Checks if the country has been in the current international organization scope for x years. Usage: country_has_been_member_for_years = { country = <co… | international_organization | <, <=, =, !=, >, >= |
| `country_has_disease` | Checks the presence of a disease in a country. usage <country scope>: country_has_disease = <disease> | country | disease scope → disease |
| `country_has_disease_outbreak` | Checks the presence of a disease outbreak in a country. usage <country scope>: country_has_disease_outbreak = <disease> | country | disease_outbreak scope → disease_outbreak |
| `country_has_estate` | Checks if the country has the specific Estate | country | estate_type scope → estate_type |
| `country_has_order` | Checks if the country's active chivalric order law references the specified order. | country | chivalric_order scope → chivalric_order |
| `country_has_special_status` | Does the country have a special status in this international organization? | international_organization |  |
| `country_highest_rated_special_status_power` | Get the political power of the country within the target international organization of its highest prioritized special status. Usage: highest_rated_sp… | country | <, <=, =, !=, >, >= |
| `country_interaction_acceptance` | How high is the target country's AI value of accepting the country interaction done by the current country scope? Always return 0 if the country inter… | country | <, <=, =, !=, >, >= |
| `country_loan_capacity` | Checks how much more money a country can borrow | country | <, <=, =, !=, >, >= |
| `country_modifier_strength` | Does the scoped country have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 count… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `country_potential_tax_base` | Checks the total potential tax base of a country | country | <, <=, =, !=, >, >= |
| `country_rank_level` | level of the country rank of a country | country | <, <=, =, !=, >, >= |
| `country_rank_level_on_date` | level of the country rank of a country on a particular date | country | <, <=, =, !=, >, >= |
| `country_strength` | Strength of a country, including their troop numbers as well as tax base and manpower | country | <, <=, =, !=, >, >= |
| `country_tax_base` | Checks the total tax base of a country | country | <, <=, =, !=, >, >= |
| `country_total_army_levy_size` | Gets the total number of army levies available to the country | country | <, <=, =, !=, >, >= |
| `country_total_navy_levy_size` | Gets the total number of navy levies available to the country | country | <, <=, =, !=, >, >= |
| `country_type` | Checks what type a country is (location, pop, building, army, navy) | country |  |
| `counts_as_capital` | Check if a location is the capital or a historical capital of its owner | location | yes/no |
| `court_language_utility` | Utility of a court language accorting to Ai | language, dialect | <, <=, =, !=, >, >= |
| `court_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `create_market_utility` | Utility of creating a market - usage: create_market_utility(<location>) or create_market_utility = { location = <location> value <operator><threshold>… | country | <, <=, =, !=, >, >= |
| `creditworthiness` | Checks the effective creditworthiness (0-1 scale; 0.5 is neutral) of a country | country | <, <=, =, !=, >, >= |
| `cultural_influence` | How much influence does the culture have? | culture | <, <=, =, !=, >, >= |
| `cultural_influence_power` | Cultural influence normalized to 0-1 range (influence / INFLUENCE_POWER_SCALE, clamped). | culture | <, <=, =, !=, >, >= |
| `cultural_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `cultural_tradition` | How much tradition does the culture have? | culture | <, <=, =, !=, >, >= |
| `cultural_tradition_power` | Cultural tradition normalized to 0-1 range (tradition / TRADITION_POWER_SCALE, clamped). | culture | <, <=, =, !=, >, >= |
| `cultural_unity` | Checks the fraction of the population sharing the country's primary culture | country | <, <=, =, !=, >, >= |
| `cultural_view` | does the culture have the specified opinion of the target? { target = <target culture> value <>= <value> } | culture |  |
| `culture_group_percentage` | Gets the percentage of the population that follow a particular culture group in the location/province/province_defintion/area/region/subcontinent/cont… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `culture_group_percentage_in_country` | The percentage of a specific culture group in the current country | country | <, <=, =, !=, >, >= |
| `culture_group_population` | Gets the absolute number of the population that follow a particular religion in the location/province/province_defintion/area/region/subcontinent/cont… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `culture_group_population_in_country` | The number of population of a specific culture group in the current country | country | <, <=, =, !=, >, >= |
| `culture_opinion_impact` | Opinion impact of a particular culture on another - usage: culture_opinion_impact(<culture link>) or culture_opinion_impact = { culture = <culture lin… | culture | <, <=, =, !=, >, >= |
| `culture_percentage` | Gets the percentage of the population that follow a particular culture in the location/province/province_defintion/area/region/subcontinent/continent/… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `culture_percentage_in_area` | gets the percentage of the population that follow a particular culture in the area - culture_percentage_in_area = { country = <country> culture = <cul… | area | <, <=, =, !=, >, >= |
| `culture_percentage_in_country` | The percentage of a specific culture in the current country | country | <, <=, =, !=, >, >= |
| `culture_population` | Gets the absolute number of the population that follow a particular culture in the location/province/province_defintion/area/region/subcontinent/conti… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `culture_population_in_country` | The number of population of a specific culture in the current country | country | <, <=, =, !=, >, >= |
| `currency_percentage_towards_limit` | Gets currency progress towards specified limit | country, international_organization | <, <=, =, !=, >, >= |
| `currency_utility` | Utility of an amount of currency to the country - usage: currency_utility(<currency>\|<amount>) or currency_utility = { currency = <currency> amount = … | country | <, <=, =, !=, >, >= |
| `current_age` | Checks if it is a certain age! | none/any |  |
| `current_bonds` | Checks how much a country has in outstanding government bond debt | country | <, <=, =, !=, >, >= |
| `current_date` | Compare the current ingame date. | none/any | <, <=, =, !=, >, >= valid date |
| `current_day` | Compare the current ingame day (1..31) | none/any | <, <=, =, !=, >, >= |
| `current_mission_task` | Checks if the country has the specified mission task in progress. | country | mission_task scope → mission_task |
| `current_month` | Compare the current ingame month (1..12) | none/any | <, <=, =, !=, >, >= |
| `current_ruler_term_years` | Checks the current ruler term length in years. | country | <, <=, =, !=, >, >= |
| `current_tooltip_depth` | What is the number of tooltips open right now? | none/any | <, <=, =, !=, >, >= |
| `current_year` | Compare the current ingame year | none/any | <, <=, =, !=, >, >= |
| `custom_description` | Wraps triggers that get a custom description instead of the auto-generated one custom_description = { text = <trigger_localization_key> subject = <opt… | none/any |  |
| `custom_tooltip` | Replaces the tooltips for the enclosed triggers with a custom text custom_tooltip = { text = <text> subject = <scope> (optional) <trigger> } | none/any |  |
| `days_as_rebel` | Check how many days the character has been a rebel. | character | <, <=, =, !=, >, >= |
| `days_difference` | Returns the difference in days between two dates. Can be used is complex form (scope:first_date\|scope:second_date) or with first = and second = | none/any | <, <=, =, !=, >, >= |
| `days_of_service_as_admiral` | Check how many days the character has served as an admiral. | character | <, <=, =, !=, >, >= |
| `days_of_service_as_general` | Check how many days the character has served as a general. | character | <, <=, =, !=, >, >= |
| `days_of_service_in_cabinet` | Check how many days the character has served in a cabinet. | character | <, <=, =, !=, >, >= |
| `days_since_disaster_end` | Checks if x days have passed since the end of the disaster. Returns -1 if the disaster has never ended. | disaster | <, <=, =, !=, >, >= |
| `days_since_disaster_start` | Checks if x days have passed since the start of the disaster. Returns -1 if the disaster has never started. | disaster | <, <=, =, !=, >, >= |
| `days_since_expedition_start` | Check how many days have passed since the expedition started | expedition | <, <=, =, !=, >, >= |
| `days_since_situation_end` | Checks if x days have passed since the end of the situation. Returns -1 if the situation has never ended. | situation | <, <=, =, !=, >, >= |
| `days_since_situation_start` | Checks if x days have passed since the start of the situation. Returns -1 if the situation has never started. | situation | <, <=, =, !=, >, >= |
| `debt_to_estates` | Checks how much a country has in estate loan debt | country | <, <=, =, !=, >, >= |
| `debug_log` | Log whether the parent trigger succeeded or failed | none/any |  |
| `debug_log_details` | Log whether the parent trigger succeeded or failed. Log which children succeeded or failed | none/any |  |
| `debug_only` | Checks if the game is in debug mode or not. | none/any | yes/no |
| `defender_bonus` | Check the defender bonus of the scope location, topography, vegetation or climate | location, climate, vegetation, topography | <, <=, =, !=, >, >= |
| `defenders_have_access_in` | checks if the defender side has access in the country in a war | war | country tag/country scope → country |
| `defensive_alliance_strength` | Strength of a defensive alliance, including the nation with all countries giving defensive support and those that can be called in for defensive wars | country | <, <=, =, !=, >, >= |
| `definition_is_for_levy` | subunit definition is for levies | sub_unit | yes/no |
| `demands_goods` | Check if the market scope has a demand for the target goods. | market | goods scope → goods |
| `demands_goods_by_pops` | Check if the market has any pop demand for the target goods. | market | goods scope → goods |
| `dependency_length_days` | returns the number of days a country has been in a dependency (overlord/subject) relationship with the target country. Usage: dependency_length_days =… | country | <, <=, =, !=, >, >= |
| `destroy_market_utility` | Utility of destroying a market - usage: destroy_market_utility(<location>) or destroy_market_utility = { location = <location> value <operator><thresh… | country | <, <=, =, !=, >, >= |
| `development` | Checks if a location has a certain Development | location | <, <=, =, !=, >, >= |
| `devotion` | How much devotion does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `devotion_percentage` | How high the percentage of the current devotion compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `dip` | The dip ability of the character | character | <, <=, =, !=, >, >= |
| `diplomatic_capacity_of_new_relation` | Diplomatic capacity that will be used if the country obtains this diplomatic relation | country | <, <=, =, !=, >, >= |
| `diplomatic_capacity_of_new_subject` | Diplomatic capacity that will be used if the country obtains this subject | country | <, <=, =, !=, >, >= |
| `diplomatic_capacity_without_maintenance` | Diplomatic capacity that country would have without paying anything for maintenance | country | <, <=, =, !=, >, >= |
| `diplomatic_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `diplomatic_range` | Is the target country within diplomatic range? | country | <, <=, =, !=, >, >= |
| `disaster_has_ended` | Check if a Disaster has ended | disaster | yes/no |
| `disaster_is_active` | Check if a Disaster is active | disaster | yes/no |
| `discount_needed_for_law_change` | Checks how much more discount % is needed for Ai to change a law | country | <, <=, =, !=, >, >= |
| `disease_affects_pops_here` | Checks if a disease is affecting some migrated pops here. | location | disease scope → disease |
| `disease_country_deaths` | Checks the number of deaths from a disease in a country. usage: disease_country_deaths(<disease>) or disease_country_deaths = { target = <disease> val… | country | <, <=, =, !=, >, >= |
| `disease_has_outbreak_here` | Checks if a disease has an outbreak in a location or subunit. | location, sub_unit | disease scope → disease |
| `disease_has_stagnated` | Checks if a disease has stagnated in a location or subunit. | location, sub_unit | disease scope → disease |
| `disease_is_active` | Checks if a disease is active in the world. | none/any | disease scope → disease |
| `disease_outbreak_country_deaths` | Checks the number of deaths from an outbreak in a country. usage: disease_outbreak_country_deaths(<disease_outbreak>) or disease_outbreak_country_deat… | country | <, <=, =, !=, >, >= |
| `disease_outbreak_is_active` | Checks if a disease outbreak is active in the world. | none/any | disease_outbreak scope → disease_outbreak |
| `disease_outbreak_presence` | Checks the presence of a disease in a location or subunit. usage: disease_outbreak_presence(<disease_outbreak>) or disease_outbreak_presence = { disea… | location, sub_unit | <, <=, =, !=, >, >= |
| `disease_outbreak_total_deaths` | How many people have been killed by this disease outbreak? | disease_outbreak | <, <=, =, !=, >, >= |
| `disease_presence` | Checks the presence of a disease in a location or subunit. usage: disease_presence(<disease>) or disease_presence = { disease = <disease> value <compa… | location, sub_unit | <, <=, =, !=, >, >= |
| `disease_resistance` | Checks the resistance to a disease in a location or subunit. usage: disease_resistance(<disease>) or disease_resistance = { target = <disease> value <… | location, sub_unit | <, <=, =, !=, >, >= |
| `disease_total_deaths` | How many people have been killed by this disease? | disease | <, <=, =, !=, >, >= |
| `distance_to` | gets distance between locations | location | <, <=, =, !=, >, >= |
| `distance_to_area` | gets distance between a location and an area | location | <, <=, =, !=, >, >= |
| `distance_to_squared` | gets distance squared as the crow flies between locations (much quicker than distance_to, useful if you're just comparing) | location | <, <=, =, !=, >, >= |
| `does_estate_want_other_policy` | Checks if a country has at least one law for which the input estate want another policy | country |  |
| `does_owner_want_to_give_away` | Check if a province's owner wants to give it away to a subject | province | yes/no |
| `doom` | How much doom does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `doom_percentage` | How high the percentage of the current doom compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `dynamic_historical_event_fire_year` | Compares the year in which a Dynamic Historical Event fired for the scoped country. Returns 0 if it never fired (or year=1 if migrated from a legacy s… | country | <, <=, =, !=, >, >= |
| `dynamic_historical_event_fired` | Checks whether a Dynamic Historical Event has fired for the scoped country. Usage: dynamic_historical_event_fired = <event_key> | country |  |
| `dynastic_power` | Returns the dynastic power of the scope dynasty or country. For countries, check ruler dynasty or heir dynasty if in regency. Usage: dynastic_power = … | country, dynasty | <, <=, =, !=, >, >= |
| `dynasty_exists` | does a tag exist | none/any |  |
| `dynasty_modifier_strength` | Does the scoped dynasty have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 dynas… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `dynasty_name` | if a dynasty a special name-key | dynasty |  |
| `education` | Checks if a character has a specific education | character |  |
| `effective_skill` | Check the skill level of this cabinet | cabinet | <, <=, =, !=, >, >= |
| `eligible_for_cabinet` | Is this estate type eligible for the cabinet in the target country? Usage: eligible_for_cabinet = { target = <target country> } | estate_type |  |
| `employment_percentage` | Checks if a location has a certain unemployement percentage | location | <, <=, =, !=, >, >= |
| `employment_size` | Returns the employment size of a building type per building level | building_type | <, <=, =, !=, >, >= |
| `employment_system_desire` | returns how much the country wants the target employment system. Usage: employment_system_desire = { target = <employment system> value <comparator> <… | country | <, <=, =, !=, >, >= |
| `entrenchment` | current entrenchment of the bureaucracy 0-100 | bureaucracy | <, <=, =, !=, >, >= |
| `estate_gold` | The gold of an estate | estate | <, <=, =, !=, >, >= |
| `estate_loan_interest` | Checks the interest of a loan | country | <, <=, =, !=, >, >= |
| `estate_max_tax` | the current max-tax of an estate in a country - usage: estate_max_tax(<estate_type link>) or estate_max_tax = { estate_type = <estate_type link> value… | country | <, <=, =, !=, >, >= |
| `estate_opinion` | the current opinion that an estate in a country has of another country - usage: estate_opinion(<estate_type link>\|<country>) or estate_opinion = { est… | country | <, <=, =, !=, >, >= |
| `estate_raised_regiment_count` | Checks the number of raised (in-service) private regiment references for an estate. Country scope requires estate = <type>. | country, estate | <, <=, =, !=, >, >= |
| `estate_satisfaction` | the current satisfaction of an estate in a country - usage: estate_satisfaction(<estate_type link>) or estate_satisfaction = { estate_type = <estate_t… | country | <, <=, =, !=, >, >= |
| `estate_tax` | The current tax the estate has to pay | estate | <, <=, =, !=, >, >= |
| `estate_tax_rate` | The current percentage of tax the estate has to pay. Returns 1 if the estate gets fully taxed even if the max possible tax is below 100% | estate | <, <=, =, !=, >, >= |
| `estate_taxable_income` | The taxable income of an estate | estate | <, <=, =, !=, >, >= |
| `estate_total_regiment_count` | Checks the total (unraised + raised) private regiment count for an estate. Country scope requires estate = <type>. | country, estate | <, <=, =, !=, >, >= |
| `estate_type_allowed_in_cabinet` | Checks if estate type is allowed in cabinet | country |  |
| `estate_type_allowed_in_command` | Checks if estate type is allowed in command of a unit | country |  |
| `estate_type_allowed_in_parliament` | Checks if estate type is allowed in parliament | country |  |
| `estate_unraised_regiment_count` | Checks the number of unraised private regiment definitions for an estate. Country scope requires estate = <type>. | country, estate | <, <=, =, !=, >, >= |
| `estimated_loot_income` | Check how much gold a country would gain from looting the scope location. | location | <, <=, =, !=, >, >= |
| `exists` | Checks whether the specified scope target exists (check for not being the null object) exists = from.owner.var:cool_var.mother | none/any |  |
| `expected_army_size` | Checks if a country has a certain expected army size | country | <, <=, =, !=, >, >= |
| `expected_navy_size` | Checks if a country expects to have a certain amount of ships | country | <, <=, =, !=, >, >= |
| `expedition_is_in_deviation` | Check if an expedition is in deviation | expedition | yes/no |
| `expedition_is_in_port` | Check if an expedition is currently stopped at a waypoint | expedition | yes/no |
| `expedition_is_paused` | Check if an expedition is currently paused (e.g. pause_expedition is still in effect) | expedition | yes/no |
| `expedition_is_returning` | Check if an expedition is on its return leg (returns_home appended the trip back) | expedition | yes/no |
| `expedition_is_stalled` | Check if an expedition is currently stalled (i.e. stall_expedition was called and resume_expedition has not yet been called) | expedition | yes/no |
| `expedition_is_traveling` | Check if an expedition is currently traveling between waypoints | expedition | yes/no |
| `expedition_is_waiting_on_construction` | Check if an expedition is currently waiting on a start_expedition_wait_construction construction to finish | expedition | yes/no |
| `expedition_start_utility` | Utility score for starting an expedition type from a country scope. Usage: expedition_start_utility = { expedition_type = <type> value >= <threshold> … | country | <, <=, =, !=, >, >= |
| `expedition_wait_construction_has_goods_demand` | Check if the expedition's current start_expedition_wait_construction has the given goods_demand. Usage: expedition_wait_construction_has_goods_demand … | expedition | demand scope → demand |
| `experience_percentage` | How many percent experience does this unit have??? | unit | <, <=, =, !=, >, >= |
| `exploration_expected_cost` | gets the exploration expected cost for a country in the scope area | area | <, <=, =, !=, >, >= |
| `exploration_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `exploration_monthly_cost` | what is the monthly cost of an exploration? | exploration | <, <=, =, !=, >, >= |
| `exploration_monthly_progress` | what is the monthly progress of an exploration? | exploration | <, <=, =, !=, >, >= |
| `exploration_needed_time` | gets the exploration needed time (months) for a country in the scope area | area | <, <=, =, !=, >, >= |
| `exploration_progress` | what is the progress of an exploration? | exploration | <, <=, =, !=, >, >= |
| `exploration_time` | what is the total needed progress of an exploration? | exploration | <, <=, =, !=, >, >= |
| `exploration_utility` | Utility of an exploration to the country - usage: exploration_utility(<area>\|<character>) or exploration_utility = { area = <area> character = <charac… | country | <, <=, =, !=, >, >= |
| `explore_bias` | Any bias to or from this bit of geography set by ambitions. Usage: explore_bias = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `favors` | How much favors does the country have in the target? favors = { target = X [*value >/</>=/<= Y* or *value = { min max }*  } | country | <, <=, =, !=, >, >= |
| `favors_needed_to_annul_relations_with` | Gets the number of favours needed to annul relations with the target country diplomatically - usage: favors_needed_to_annul_relations_with(<target>) o… | country | <, <=, =, !=, >, >= |
| `fertility` | The fertility of the character | character | <, <=, =, !=, >, >= |
| `first_name_starts_with_vowel` | Display only, never use in synced script: resolves the character's first name in the display language, so it can differ between machines. Checks if th… | character | yes/no |
| `food_consumption` | Amount of consumed food | location | <, <=, =, !=, >, >= |
| `food_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `food_percentage` | How many percent of food does this unit have??? | unit | <, <=, =, !=, >, >= |
| `food_price` | Checks how much the food in the current market costs | market | <, <=, =, !=, >, >= |
| `food_production` | Amount of food production | location | <, <=, =, !=, >, >= |
| `food_value` | Check the food value of the goods scope. | goods | <, <=, =, !=, >, >= |
| `forbids_sovereign_diplomacy` | Check if a subject type restricts diplomacy | subject_type | yes/no |
| `foreign_debt` | Checks how much a country has in foreign bank loan debt | country | <, <=, =, !=, >, >= |
| `formable_level` | Checks if a country has a certain formable level | country | <, <=, =, !=, >, >= |
| `fort_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `garrison_percentage` | Checks the garrison percentage of the location in scope | location | <, <=, =, !=, >, >= |
| `garrison_strength` | Checks the garrison strength of the location in scope | location | <, <=, =, !=, >, >= |
| `get_antagonism` | how much of an antagonism type does the country have towards another country? | country | <, <=, =, !=, >, >= |
| `get_opinion` | how much of an opinion type does the country have towards another country? | country | <, <=, =, !=, >, >= |
| `get_trust_equilibrium` | how much of a trust equilibrium type does the country have towards another country? | country | <, <=, =, !=, >, >= |
| `gfx_culture_applicable` | Checks if a culture gfx applies to the scope object | location, country, sub_unit, character, dynasty, pop, culture, religion, graphical_culture, religious_order |  |
| `gives_defensive_support_to` | Does the scope country give defensive support to specified country? | country | country tag/country scope → country |
| `gives_fleet_basing_rights` | Check if a subject type gives fleet basing rights | subject_type | yes/no |
| `gives_fleet_basing_rights_to` | Does the scope country give fleet basing rights to the specified country? | country | country tag/country scope → country |
| `gives_food_access` | Check if a subject type gives food access | subject_type | yes/no |
| `gives_food_access_to` | Does the scope country give food access to the specified country? | country | country tag/country scope → country |
| `gives_isolation_exemption_to` | Does the scope country give a trade isolation exemption to specified country? | country | country tag/country scope → country |
| `gives_military_access_to` | Does the scope country give military access to the specified country? | country | country tag/country scope → country |
| `gives_offensive_support_to` | Does the scope country give offensive support to specified country? | country | country tag/country scope → country |
| `giving_scripted_relation` | Checks for giving scripted relation. Usage: giving_scripted_relation = { target = country type = <scripted type> } | country |  |
| `giving_scripted_relation_of_type` | Checks if that scripted relation is given by the country scope to any other country. | country | relation_type scope → relation_type |
| `global_variable_list_size` | Checks the size of a global variable list global_variable_list_size = { name = X target >= Y } Where X is the name of the variable Where Y is a script… | none/any |  |
| `global_variable_map_size` | Checks the size of a variable map variable_map_size = { name = X value >= Y } Where X is the name of the variable map Where Y is a script value or num… | none/any |  |
| `gold` | How much gold does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `gold_percentage` | How high the percentage of the current gold compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `goods` | Checks which good is in this trade | trade | goods scope → goods |
| `goods_category` | tests the goods category - raw_material or produced | goods |  |
| `goods_demand_in_market` | Checks how much demand exists of a good in the market. Usage: goods_demand_in_market = { goods = <goods> value <operator> <value> } or goods_demand_in… | market | <, <=, =, !=, >, >= |
| `goods_method` | tests the goods method - mining/farming/hunting/gathering | goods |  |
| `goods_output` | Check how much goods the scope location produces. | location | <, <=, =, !=, >, >= |
| `goods_supply_in_market` | Checks how much supply exists of a good in the market. Usage: goods_supply_in_market = { goods = <goods> value <operator> <value> } or goods_supply_in… | market | <, <=, =, !=, >, >= |
| `government_power` | How much government power does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `government_power_percentage` | How high the percentage of the current government power compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `great_power_points` | Checks a country's Great Power points accumulated from dominated areas | country | <, <=, =, !=, >, >= |
| `great_power_ranking` | Country's rank among Great Powers (1 = most points) | country | <, <=, =, !=, >, >= |
| `great_power_score` | Checks a country's Great Power points (area-dominance score) | country | <, <=, =, !=, >, >= |
| `had_country_rank_level` | Checks if a country has been or is currently in that level of the country rank | country |  |
| `had_disaster_for_years` | Check if the country scope had the specified disaster type for a specific amount of years. Usage: had_disaster_for_years = { disaster_type = <disaster… | country | <, <=, =, !=, >, >= |
| `harbor_capacity_in_area` | gets the sum of harbor capacity (harbor suitability) at the country's ports in an area | area | <, <=, =, !=, >, >= |
| `harmony` | How much harmony does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `harmony_percentage` | How high the percentage of the current harmony compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `has_accepted_culture` | Check if a country has a culture as an accepted culture | country | culture scope → culture |
| `has_accessible_coastline` | Does the area have a coastline with a port or not? | area | yes/no |
| `has_active_disaster` | Does the country have an active disaster of the given type? Usage: has_active_disaster = disaster_type:<disaster type> | country | disaster_type scope → disaster_type |
| `has_active_resolution` | Does the scope international organization/situation have any active resolutions? | international_organization, situation | yes/no |
| `has_advance` | Checks if a country has a certain advance | country |  |
| `has_advance_available` | Checks if a country has a certain advance available to research. Returns true if the advance has been researched already. | country |  |
| `has_advance_for_employment_system` | Does the country have the necessary advance to be able to adopt the supplied employment system? | country | employment_system scope → employment_system |
| `has_advance_for_succession_law` | Does the country have the necessary advance to be able to adopt the supplied succession law? | country | heir_selection scope → heir_selection |
| `has_ai_disposition_toward` | Does the scope country view the target country with the given AI disposition? Usage: has_ai_disposition_toward = { target = <country link> disposition… | country |  |
| `has_ai_disposition_toward_actor` | Does the country view scope:actor with the given AI disposition? Used by the disposition map mode to color by the selected country's perspective; scop… | country | alarmed, wary, planning_war, covets, domineering, rivals, indifferent, friendly |
| `has_ai_preferred_parliament_law_change` | Checks if the AI has a preferred parliament law change stored | country | yes/no |
| `has_antagonism` | does the country have an antagonism type towards another country? | country |  |
| `has_any_active_disaster` | country has at least an active disaster | country | yes/no |
| `has_any_convertable_pops` | Check if a location has any pops that can be converted to state religion | location | yes/no |
| `has_any_culture_group` | If a culture belongs to any culture group. | culture | yes/no |
| `has_any_disease_present` | Checks if the location or subunit is affected by ANY disease active. | location, sub_unit | yes/no |
| `has_any_disease_trait` | character has a health trait that can kill them (a disease like smallpox, not an injury like one_eyed) | character | yes/no |
| `has_any_mission_active` | Checks if the country has the specified mission as its currently active mission. | country | yes/no |
| `has_any_possible_disaster` | Country has at least one possible disaster about to strike | country | yes/no |
| `has_any_religious_order_presence` | True if at least one religious order of the country's religion holds a location in this country. | country | yes/no |
| `has_any_rgo_of_goods_method` | True if the country owns at least one location whose raw material uses the specified goods method. Usage: has_any_rgo_of_goods_method = hunting | country |  |
| `has_any_town_rights` | Check if a location has town rights | location | yes/no |
| `has_any_visible_situation` | country can see at least one active situation (the situation's visible block passes for it) | country | yes/no |
| `has_area_preference` | Checks if a country has a specific area preference active | country |  |
| `has_art_in_progress` | Checks if an artist is currently working on something | character | yes/no |
| `has_assigned_explorer` | does the country have an assigned explorer in this area? | area | country tag/country scope → country |
| `has_autocephalous_patriarchates` | religion has autocephalous patriarchates | religion | yes/no |
| `has_available_marriage_slot` | Has the character got a slot available for another marriage? (i.e. are they unmarried for non-polygamous people, or have they got less than the max nu… | character | yes/no |
| `has_avatar` | checks if a country has a particular avatar | country | avatar scope → avatar |
| `has_been_formed` | Checks if the formable has been formed already | formable_country | yes/no |
| `has_been_influenced_by_parliament_agenda` | Checks if the country scope has already been influenced by an accepted parliament agenda in the target international organization's parliament. | country | international_organization scope → international_organization |
| `has_blocked_treaties` | Is the country blocked from doing treaties with country? | country | country tag/country scope → country |
| `has_breach` | siege has breach | siege | yes/no |
| `has_building` | Checks if a location has a specific building | location | building_type scope → building_type |
| `has_building_with_at_least_one_level` | Checks if a location has a specific building and it has at least one level | location |  |
| `has_building_with_graphical_tag` | Checks if a location has a building with the specified graphical tags | location |  |
| `has_building_with_graphical_tag_and_at_least_one_level` | Checks if a location has a building with the specified graphical tags and at least one level | location |  |
| `has_bureaucracy_of_type` | checks if the scope country has a bureaucracy of the supplied type | country | bureaucracy_type scope → bureaucracy_type |
| `has_cabinet_action` | is doing something in the cabinet | character, cabinet | yes/no |
| `has_cached_or_cast_vote_for` | Has the supplied country voted or has a cached vote from previous month on the supplied resolution for a specific target in the scope international or… | international_organization, situation |  |
| `has_canal_construction` | Is the location building a canal? | location | yes/no |
| `has_canonization` | religion has canonization | religion | yes/no |
| `has_cardinals` | religion has Cardinals | religion | yes/no |
| `has_casus_belli` | Checks if that war has a CB specified at all | war | yes/no |
| `has_casus_belli_of_type` | Does the country have a cb of the specified type against any country? | country | casus_belli scope → casus_belli |
| `has_casus_belli_of_type_on` | Does the country have a cb of the supplied type on the target? Usage - has_casus_belli_of_type_on = { type = <cb type key> target = <country> } | country |  |
| `has_casus_belli_on` | Does the country have a cb on the target? | country | country tag/country scope → country |
| `has_character_modifier` | Does the scoped character have a given modifier has_character_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_child_education` | Does the scoped child have a given education has_child_education = education | character |  |
| `has_child_education_selected` | Does the scoped child have any given education has_child_education_selected = yes | character | yes/no |
| `has_chivalric_order` | Checks if the country has an active chivalric order (i.e. the ruler has joined one). | country | yes/no |
| `has_circle_leader` | Does this imperial circle have a leader? | imperial_circle | yes/no |
| `has_claim_on_province` | Does the country have a casus belli targetting the specified province? | country | province scope → province |
| `has_colonial_charter` | does the province definition have a colonial charter belonging to the specified country | province_definition | country tag/country scope → country |
| `has_colonial_charter_in` | Does the country have a colonial charter in the target province_definition? | country | province_definition scope → province_definition |
| `has_colonial_charters` | Does the country have colonial charters? | country | yes/no |
| `has_colonial_claim` | country has a claim on a province definition? Usage: has_colonial_claim = <province definition> | country | province_definition scope → province_definition |
| `has_combat` | Check if a location has a combat | location | yes/no |
| `has_commander` | unit has a commander | unit | yes/no |
| `has_competing_foreign_buildings_with` | Does this country own a foreign building in, or adjacent to, a location where the target country also owns a foreign building? | country | country tag/country scope → country |
| `has_completed_religious_focus` | Checks if a country has completed a certain religious focus | country |  |
| `has_consort` | country has a Consort | country | yes/no |
| `has_cooldown` | Does a country, character or international organization have a particular cooldown active | country, character, international_organization |  |
| `has_core` | Does the country has a core of a specific location? | country | province id/location scope → location |
| `has_countries_with_coalition_grade_antagonism` | Country has antagonism towards them from other countries to the point where they could form a coalition against them | country | yes/no |
| `has_countries_with_near_coalition_grade_antagonism` | Country has antagonism towards them from other countries to the point where they are thinking of forming a coalition against them | country | yes/no |
| `has_countries_with_timed_antagonism` | Country has temporary antagonism towards them from other countries | country | yes/no |
| `has_country_modifier` | Does the scoped country have a given modifier has_country_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_country_religious_schools` | countries of this religion belong to a religious school | religion | yes/no |
| `has_culture_group` | If a culture belongs to a specific culture group. | culture | culture_group scope → culture_group |
| `has_culture_with_tag` | Checks if a culture has the specified tags | culture |  |
| `has_customer` | Check if a mercenary has a customer | mercenary | yes/no |
| `has_different_order_of_chivalry` | Checks if this country and the target country both have different orders. | country | country tag/country scope → country |
| `has_different_town_rights_than` | Checks if a location shares no town right types with another location | location | province id/location scope → location |
| `has_diplomacy_with` | Does the country have a certain type of diplomatic relation with another. Usage: has_diplomacy_with = { country = <country> type = <type> } | country |  |
| `has_discovered` | Has the country discovered a specific location? | country | province id/location scope → location |
| `has_discovered_area` | Has the country fully discovered the area? | country | area scope → area |
| `has_discriminated_culture` | Check if a country has a culture as a Discriminated culture | country | culture scope → culture |
| `has_dlc` | Does the host have this DLC enabled? used for synched content | none/any |  |
| `has_doom` | country has doom mechanics | country | yes/no |
| `has_dynasty` | character is in a Dynasty | character | yes/no |
| `has_dynasty_modifier` | Does the scoped dynasty have a given modifier has_dynasty_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_earthquakes` | Check if a location has a Earthquakes | location | yes/no |
| `has_elections` | Checks if an international organization has electors | international_organization | yes/no |
| `has_embraced_institution` | Checks if a country has embraced an institution | country | institution scope → institution |
| `has_employment_system` | Does the country has the supplied employment system? | country | employment_system scope → employment_system |
| `has_enabled_currency` | Checks what currency has been enabled for the international organization (manpower, sailors, gold) | international_organization |  |
| `has_estate` | Checks if a character is of a specific Estate | character | estate_type scope → estate_type |
| `has_estate_privilege` | Checks if a country has a certain estate privilege | country | estate_privilege scope → estate_privilege |
| `has_exploration` | character/country is currently exploring | country, character | yes/no |
| `has_exploration_construction` | character is currently preparing to explore | character | yes/no |
| `has_exports` | Check if a location has a Exports | location | yes/no |
| `has_extended_winter` | does the area have extended winter or not? | area | yes/no |
| `has_fired_unique_event` | Checks if the game has already fired the unique event. For DHEs, evaluated per-country on the current scope's country; for non-DHE fire_only_once even… | none/any |  |
| `has_fixed_liturgical_language` | religion has a set liturgical language | religion | yes/no |
| `has_fort` | Check if a location has any fort | location | yes/no |
| `has_game_rule` | Is the given game rule setting enabled? has_game_rule = faster_conversion | none/any |  |
| `has_game_started` | Has the game started? | none/any | yes/no |
| `has_gfx_tag` | Checks if the unit type has a specific gfx tag | sub_unit, unit_type |  |
| `has_gifted_gold_to` | Has the country an active gold gift cooldown with the target country? | country | country tag/country scope → country |
| `has_global_variable` | Checks whether the specified global variable is set. has_global_variable = name | none/any |  |
| `has_global_variable_list` | Checks whether the specified global variable list is set has_global_variable_list = name | none/any |  |
| `has_global_variable_map` | Checks whether the current scope has the specified variable map has_variable_map = name | none/any |  |
| `has_goods_export_trade_policy` | Checks if the scope country has an active global export trade policy (tariff or subvention) set for a good. Usage: has_goods_export_trade_policy = { g… | country |  |
| `has_goods_import_trade_policy` | Checks if the scope country has an active global import trade policy (tariff or subvention) set for a good. Usage: has_goods_import_trade_policy = { g… | country |  |
| `has_graphical_culture` | Check if a culture has a graphical culture | culture |  |
| `has_graphical_religion` | Check if a religion has a graphical culture | religion |  |
| `has_heir` | country has a Heir | country | yes/no |
| `has_highest_rated_special_status_in_international_organization_of_type` | Does the country have the specified special status as its highest ranking? | country |  |
| `has_historical_rival` | Does the scope country have the specified country as an historical rival? | country | country tag/country scope → country |
| `has_historical_rivals` | Does the scope country have historical rivals? | country | yes/no |
| `has_holy_sites` | religion has holy sites | religion | yes/no |
| `has_honor` | religion has honor | religion | yes/no |
| `has_imports` | Check if a location has a imports | location | yes/no |
| `has_industry_promotion` | Checks if a country has an active industry promotion of a specific industry_type | country | industry_type scope → industry_type |
| `has_institution` | Checks if a country has an institution | location | institution scope → institution |
| `has_insulted` | Has the country an active insult cooldown with the target country? | country | country tag/country scope → country |
| `has_international_organization_modifier` | Does the scoped international organization have a given modifier has_international_organization_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_invited_religious_figure` | country has invited religious figures to work with them | country | yes/no |
| `has_karma` | religion has karma | religion | yes/no |
| `has_land_ownership_rule` | Checks if the international organization has a landownership rule set | international_organization | yes/no |
| `has_latest_road_to` | Check if a location has the latest available road to the target location. Latest is determined by the owner of the location and what buildable road ty… | location | province id/location scope → location |
| `has_law` | Checks if a country has a certain law enabled | country |  |
| `has_leader` | Does the exploration have a leader? | exploration | yes/no |
| `has_leader_requirements` | True if the expedition type defines a leader trigger block | expedition_type | yes/no |
| `has_levies` | unit has levies | unit | yes/no |
| `has_limited_diplomacy` | Check if the country has limited diplomacy | country | yes/no |
| `has_loc_key` | Checks if certain key is assigned to a loan | loan |  |
| `has_local_dlc` | Does the local player have this DLC enabled? used for unsynched content like new graphics | none/any |  |
| `has_local_variable` | Checks whether the specified local variable is set. has_local_variable = name | none/any |  |
| `has_local_variable_list` | Checks whether the specified local variable list is set has_local_variable_list = name | none/any |  |
| `has_local_variable_map` | Checks whether the current scope has the specified variable map has_variable_map = name | none/any |  |
| `has_location` | Is the supplied location owned by the scope international organization? | international_organization | province id/location scope → location |
| `has_location_modifier` | Does the scoped province have a given modifier has_location_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_market_construction` | Is the location building a market? | location | yes/no |
| `has_markets` | country has market centers | country | yes/no |
| `has_max_town_rights` | Check if a location has reached its maximum number of town rights | location | yes/no |
| `has_member` | Checks if an international organization has a certain member | international_organization | country tag/country scope → country |
| `has_mercenaries` | unit has mercenaries | unit | yes/no |
| `has_merchant` | checks if the market has a merchant of the specific country. | market | country tag/country scope → country |
| `has_merchant_power` | does the  market have a merchant power for { country = country key = key }? | market |  |
| `has_migration` | Check if a province definition has a migration going on | province_definition | yes/no |
| `has_mission_task` | Checks if the country has the specified mission task visible in its current mission. | country | mission_task scope → mission_task |
| `has_multiple_players` | Does the game have at least two players currently connected? | none/any | yes/no |
| `has_mutual_scripted_relation` | Checks for a mutual scripted relation. { target = country type = <scripted type> | country |  |
| `has_mutual_scripted_relation_of_type` | Checks if that scripted relation exists between the country scope and any other country. | country | relation_type scope → relation_type |
| `has_new_royal_marriage_with` | Does the country have a royal marriage with the specified country that was formed after the game started, as opposed to one already in place from the … | country | country tag/country scope → country |
| `has_new_world_goods_in_market` | Checks if a market has a supply of any new world goods | market | yes/no |
| `has_newsletter_subscription` | Has the player suscribed to the newsletter? | none/any | yes/no |
| `has_nickname` | Check if the character has any nick name set | character | yes/no |
| `has_non_rural_port` | Checks if a country has at least one non-rural port | country | yes/no |
| `has_old_world_goods_in_market` | Checks if a market has a supply of any old world goods | market | yes/no |
| `has_omens` | religion has omens | religion | yes/no |
| `has_ongoing_parliament_debate` | Country / international organization has an active parliament debate | country, international_organization | yes/no |
| `has_only_prisoners` | returns true if the unit contains ONLY prisoners | unit | yes/no |
| `has_opinion` | does the country have an opinion type towards another country? | country |  |
| `has_or_had_tag` | if a country is a specific historical tag or was it. | country |  |
| `has_origin_in_new_world` | Check if a goods has origin in the new world | goods | yes/no |
| `has_origin_in_old_world` | Check if a goods has origin in the old world | goods | yes/no |
| `has_overlords_ruler` | Check if a subject type has to have its overlord's ruler | subject_type | yes/no |
| `has_owned_buildings` | Has the target country got some buildings here? | location | country tag/country scope → country |
| `has_owner` | Check if a location has a Owner | location | yes/no |
| `has_parliament` | Checks if the country / international organization has a parliament | country, international_organization | yes/no |
| `has_parliament_agendas` | Checks if at least one participant in the parliament is currently pushing an agenda | country, international_organization | yes/no |
| `has_participated_in_parliament` | Checks if the country scope has already participated in the target international organization's parliament. | country | international_organization scope → international_organization |
| `has_passable_land` | Check if a province definition has passable land | province_definition | yes/no |
| `has_patriarchs` | religion has Patriarchs | religion | yes/no |
| `has_periphora` | Check if art is on a periphora or not | work_of_art | yes/no |
| `has_personality` | Does the country have the specified AI personality? | country |  |
| `has_policy` | Checks if a country has a certain policy for a then policy's law | country |  |
| `has_ports` | country has ports? | country | yes/no |
| `has_positive_opinion` | Does the country have a positive opinion? | country | country tag/country scope → country |
| `has_possible_institution_spawn` | Check if a province has an institution that can be promoted to spawn | province | yes/no |
| `has_possible_nomad_targets` | Does the country have any possible nearby places to migrate to? | country | province_definition scope → province_definition |
| `has_potential_royal_marriage` | Could the country do a royal marriage with the specified country? | country | country tag/country scope → country |
| `has_preference_tag` | if a country has specific preference tag | country |  |
| `has_presence_in` | country has a presence in the geography supplied? | country |  |
| `has_pretender` | Checks if a rebel has a pretender character | rebels | yes/no |
| `has_primary_or_accepted_culture` | Check if a country has a culture as a primary or an accepted culture | country | culture scope → culture |
| `has_primary_or_accepted_or_tolerated_culture` | Check if a country has a culture as a primary or an accepted or a tolerated culture | country | culture scope → culture |
| `has_prisoners` | returns true if the unit contains prisoners | unit | yes/no |
| `has_privateers_from` | Does the location has privateers of the target country? | location, province, area | country tag/country scope → country |
| `has_province_modifier` | Does the scoped province have a given modifier has_province_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_proximity_source_building` | Check if a location holds a building that grants local_proximity_source | location | yes/no |
| `has_purity` | religion has purity | religion | yes/no |
| `has_raised_army_levies` | Check if the country has raised army levies | country | yes/no |
| `has_raised_levies` | Check if the country has raised levies | country | yes/no |
| `has_raised_navy_levies` | Check if the country has raised navy levies | country | yes/no |
| `has_rebel` | Check if a pop has allegiance to a rebel | pop | yes/no |
| `has_rebel_modifier` | Does the scoped rebel have a given modifier has_rebel_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_reform` | Checks if a country has a specific reform | country | government_reform scope → government_reform |
| `has_regent` | country has a regent | country | yes/no |
| `has_regular_elections` | does the country have regular elections | country | yes/no |
| `has_regulars` | unit has regulars | unit | yes/no |
| `has_religion_modifier` | Does the scoped religion have a given modifier has_religion_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_religious_aspect` | Checks if a country has a certain religious aspect | country | religious_aspect scope → religious_aspect |
| `has_religious_factions` | religion has religious factions | religion | yes/no |
| `has_religious_focuses` | religion has religious focuses | religion | yes/no |
| `has_religious_head` | religion has ReligiousHead | religion | yes/no |
| `has_religious_influence` | religion has ReligiousInfluence | religion | yes/no |
| `has_religious_orders` | True if this religion has at least one religious order registered to it. | religion | yes/no |
| `has_religious_schools` | religion has religious schools | religion | yes/no |
| `has_river` | Check if a location has a river | location | yes/no |
| `has_road_constructions` | Check whether location has road constructions | location | yes/no |
| `has_road_of_type_to` | Check if a location has a road to the target location of the specified type. Usage: has_road_of_type_to = { target = <target location> type = <road ty… | location |  |
| `has_road_to` | Check if a location has a road to the target location | location | province id/location scope → location |
| `has_road_to_capital` | Check if a location has a road to capital | location | yes/no |
| `has_royal_marriage_with` | Does the country have a royal marriage with specified country? | country | country tag/country scope → country |
| `has_ruler` | country has a ruler | country | yes/no |
| `has_scripted_proximity_to` | Check if a location has scripted proximity to the target location of the specified key. Usage: has_scripted_proximity_to = { target = <target location… | location |  |
| `has_scripted_relation` | Checks if that scripted relation exists between these two countries. { target = country type = <scripted type> | country |  |
| `has_scripted_relation_of_type` | Checks if that scripted relation exists for the country scope with any other country. | country | relation_type scope → relation_type |
| `has_sects` | religion has sects | religion | yes/no |
| `has_sellable_art` | Check if a country has any non-priceless work of art. | country | yes/no |
| `has_shared_culture_group` | If a culture belongs to a specific culture group. | culture | culture scope → culture |
| `has_siege` | Check if a location has a siege | location | yes/no |
| `has_societal_value` | Checks if the country has a specific societal value | country | societal_value_type scope → societal_value_type |
| `has_sound_tolls` | country has sound tolls | country | yes/no |
| `has_spawned` | Has the institution spawned anywhere? | institution | yes/no |
| `has_special_status_available` | Checks if an international organization has a particular special status available | international_organization | special_status scope → special_status |
| `has_special_status_in_international_organization` | Does the country have a special status in the supplied international organization? | country |  |
| `has_support_from` | Checks if a rebel has support froma certain pop type | rebels | pop_type scope → pop_type |
| `has_tag` | Check if that object has the specified tag. | religion, goods, building, law, policy, building_type, casus_belli, peace_treaty, heir_selection, trait |  |
| `has_target_casus_belli_on_us` | Does the target have a cb on the country? | country | country tag/country scope → country |
| `has_temporary_demand` | Checks if a market has a certain temporary demand | market | demand scope → demand |
| `has_temporary_demands` | Checks if a market has a certain temporary demand | market | yes/no |
| `has_tolerated_culture` | Check if a country has a culture as an Tolerated culture | country | culture scope → culture |
| `has_town_rights` | Checks if a location has a specific town_rights | location | town_rights_type scope → town_rights_type |
| `has_trade_treaty_with` | Does the country have a trade agreement with a specified country? | country | country tag/country scope → country |
| `has_trait` | Checks if a character has a specified trait | character |  |
| `has_trait_category` | Checks if any of the character's traits belongs to the specified category. | character |  |
| `has_truce_with` | Is the country at truce with a specified country? | country | country tag/country scope → country |
| `has_trust` | does the country have a trust type towards another country? | country |  |
| `has_unit` | character is assigned to a unit | character | yes/no |
| `has_unit_modifier` | Does the scoped unit have a given modifier has_unit_modifier = name | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization |  |
| `has_unlocked_any_unit_of_category` | Has the country unlocked any unit of the specified category? | country |  |
| `has_variable` | Checks whether the current scope has the specified variable set. has_variable = name | none/any |  |
| `has_variable_list` | Checks whether the current scope has the specified variable list set has_variable_list = name | none/any |  |
| `has_variable_map` | Checks whether the current scope has the specified variable map has_variable_map = name | none/any |  |
| `has_volcano` | Check if a location has a volcano | location | yes/no |
| `has_voted` | Has the supplied country voted on the supplied resolution in the scope international organization/situation? | international_organization, situation |  |
| `has_voted_for` | Has the supplied country voted on the supplied resolution for a specific target in the scope international organization/situation? | international_organization, situation |  |
| `has_voted_for_issue_in_parliament` | Checks if the country scope has voted for the issue in the target international organization's parliament. Returns false if they have voted against it… | country | international_organization scope → international_organization |
| `has_yanantin` | religion has yanantin | religion | yes/no |
| `heathen_population_fraction` | Checks the fraction of the population having a different religious group than the country | country | <, <=, =, !=, >, >= |
| `heir_candidates_count` | Checks amount of heir candidates in an heir selection for a country. Usage - heir_candidates_count = { country = x value [operator] y } or heir_candid… | heir_selection | <, <=, =, !=, >, >= |
| `heir_position` | Character's position in line for its country's throne | character | <, <=, =, !=, >, >= |
| `heir_score` | Get the hypothetical heir score of the character for the target country, even if the character in question could not be an heir. | character | <, <=, =, !=, >, >= |
| `heir_score_country` | Get the hypothetical heir score of the target character for the current country, even if the character in question could not be an heir. | country | <, <=, =, !=, >, >= |
| `heir_score_home` | Get the hypothetical heir score of the character in the country they currently reside in. | character | <, <=, =, !=, >, >= |
| `hemisphere` | Check if a location is either in the northern or southern hemisphere. Usage: hemisphere = northern/southern | location |  |
| `heretic_population_fraction` | Checks the fraction of the population having a different religion in the same group as the country | country | <, <=, =, !=, >, >= |
| `hidden_trigger` | Enclosed triggers are not shown in tooltips hidden_trigger = { <more triggers> } | none/any |  |
| `higher_temporary_taxes_needed` | Checks how much more max tax a country wants | country | <, <=, =, !=, >, >= |
| `hire_price` | how much would it cost to hire this unit as a merc. Usage: hire_price(<cost multiplier>\|<duration in months>) | unit | <, <=, =, !=, >, >= |
| `honor` | How much honor does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `honor_percentage` | How high the percentage of the current honor compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `horde_unity` | How much horde_unity does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `horde_unity_percentage` | How high the percentage of the current horde_unity compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `hre_election_vote_score` | How much the voter wants to elect the scope country as leader of the supplied international organization. Usage: hre_election_vote_score = { internati… | country | <, <=, =, !=, >, >= |
| `hre_io_is_in_formation_period` | Is the scope IO currently in the circle formation period? | international_organization | yes/no |
| `imperial_circle_member_count` | Number of members in this imperial circle. | imperial_circle | <, <=, =, !=, >, >= |
| `imperial_circle_satisfaction` | Satisfaction level (0–100) of this Imperial Circle. | imperial_circle | <, <=, =, !=, >, >= |
| `implementation_progress_percentage` | Checks if the current government reform/avatar/estate privilege/god/policy/law/cabinet action scope has been implemented in percentage. | law, policy, estate_privilege, cabinet_action, government_reform, god, omen, avatar | <, <=, =, !=, >, >= |
| `importance` | Checks if a Holy Site has this Importance | holy_site | <, <=, =, !=, >, >= |
| `in_assault` | siege is in assault | siege | yes/no |
| `in_cabinet` | character is in cabinet | character | yes/no |
| `in_civil_war` | country is in civil war | country | yes/no |
| `in_combat` | unit is in combat | unit | yes/no |
| `in_marriage_union_with` | Is the country in a marriage union with specified country? | country | country tag/country scope → country |
| `in_retreat` | unit is in Retreat | unit | yes/no |
| `in_same_imperial_circle` | Are the scope country and target country members of the same Imperial Circle? | country | country tag/country scope → country |
| `in_siege` | unit is in Siege | unit | yes/no |
| `in_trade_range_of` | Is the market within trading range of a merchant the target country? | market | country tag/country scope → country |
| `in_union_with` | Is the country in a union with specified country? | country | country tag/country scope → country |
| `in_war_of_casus_belli` | Is the country in any war with the specified casus belli? | country | casus_belli scope → casus_belli |
| `in_zone_of_control` | Check if a location is in the zone of control of a friendly fort | location | yes/no |
| `industry_best_area_income` | gets the monthly income a country has in its highest-income area for this industry_type - industry_best_area_income = { country = <country> value ><= … | industry_type | <, <=, =, !=, >, >= |
| `industry_best_area_levels` | gets the building levels a country has in its highest-income area for this industry_type - industry_best_area_levels = { country = <country> value ><=… | industry_type | <, <=, =, !=, >, >= |
| `industry_best_area_tax_base` | gets the tax base a country has in its highest-income area for this industry_type - industry_best_area_tax_base = { country = <country> value ><= <val… | industry_type | <, <=, =, !=, >, >= |
| `industry_building_levels_in_area` | gets the total building levels a country has in an area producing the goods of an industry_type - industry_building_levels_in_area = { country = <coun… | area | <, <=, =, !=, >, >= |
| `industry_income_in_area` | gets the total monthly income a country has in an area from buildings producing the goods of an industry_type - industry_income_in_area = { country = … | area | <, <=, =, !=, >, >= |
| `industry_tax_base_in_area` | gets the total tax base a country has in an area from buildings producing the goods of an industry_type - industry_tax_base_in_area = { country = <cou… | area | <, <=, =, !=, >, >= |
| `inflation` | How much inflation does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `inflation_percentage` | How high the percentage of the current inflation compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `integration_level` | Checks the integration level of a location | location |  |
| `integration_progress` | Checks the integration progress of a location | location | <, <=, =, !=, >, >= |
| `international_organization_can_add_land` | Can we add a location to the scope international organization? | none/any | international_organization scope → international_organization |
| `international_organization_can_own_land` | Can the international organization own land? | international_organization | yes/no |
| `international_organization_can_remove_land` | Can we remove a location from the scope international organization? | none/any | international_organization scope → international_organization |
| `international_organization_economical_base` | Checks if an international organization has a certain economical base based on their members | international_organization | <, <=, =, !=, >, >= |
| `international_organization_has_internal_peace` | Checks if no member country is in a direct war with another member country | international_organization | yes/no |
| `international_organization_has_law` | Has the scope international organization enacted a policy for the supplied law? | international_organization | law scope → law |
| `international_organization_has_laws` | Has the scope international organization enacted a policy for the supplied law? | international_organization | yes/no |
| `international_organization_has_leader` | Does the international organization have a leader country? | international_organization | yes/no |
| `international_organization_has_policy` | Has the scope international organization enacted the supplied policy? | international_organization | policy scope → policy |
| `international_organization_land_can_be_removed_by_peace_treaty` | Return true if locations of this international organization can be removed by the 'remove location from IO' peace treaty | international_organization | yes/no |
| `international_organization_leader_count` | Checks how many leaders (defined as 'leaders' in the IO type) are currently present in the current international organization | international_organization | <, <=, =, !=, >, >= |
| `international_organization_leader_reign` | Checks if the ruler (or regency in case of a power vacuum) of an international organization has ruled for x years | international_organization | <, <=, =, !=, >, >= |
| `international_organization_leader_reign_in_days` | Checks if the ruler (or regency in case of a power vacuum) of an international organization has ruled for x days | international_organization | <, <=, =, !=, >, >= |
| `international_organization_lifetime` | Checks if the international organization has existed for x years | international_organization | <, <=, =, !=, >, >= |
| `international_organization_lifetime_in_days` | Checks if the international organization has existed for x days | international_organization | <, <=, =, !=, >, >= |
| `international_organization_locations_owned_percentage` | The percentage of the locations of an international organization owned by a country | international_organization | <, <=, =, !=, >, >= |
| `international_organization_modifier_strength` | Does the scoped international organization have a given modifier with the compared strength. Default modifiers without any scale changes have a streng… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `international_organization_num_locations` | Checks if an international organization has a certain amount of owned locations | international_organization | <, <=, =, !=, >, >= |
| `international_organization_population` | Checks if an international organization has a certain population based on the locations it owns | international_organization | <, <=, =, !=, >, >= |
| `intrinsic_disease_resistance` | Checks the intrinsic disease resistance in a location (e.g. from buildings) | location | <, <=, =, !=, >, >= |
| `io_exists` | Does the international organization exist? | none/any | international_organization scope → international_organization |
| `io_total_great_power_score` | Returns the cached sum of great power scores of all members of an international organization (updated monthly) | international_organization | <, <=, =, !=, >, >= |
| `io_total_military_strength` | Returns the cached sum of army strengths of all members of an international organization (updated monthly) | international_organization | <, <=, =, !=, >, >= |
| `io_total_tax_base` | Returns the cached sum of total tax of all members of an international organization (updated monthly) | international_organization | <, <=, =, !=, >, >= |
| `io_within_diplomatic_range` | Is the target international organization within diplomatic range? | international_organization | country tag/country scope → country |
| `ironman` | Checks if the game is running in ironman. | none/any | yes/no |
| `is_a_defender` | Is the target country a defender in the war? | war | country tag/country scope → country |
| `is_a_threat_for_us` | Is the country views the target country as a threat? | country | country tag/country scope → country |
| `is_accepted_in` | If a culture is accepted in the target country? | culture | country tag/country scope → country |
| `is_active` | If a culture is active. | culture | yes/no |
| `is_active_parliament` | country has an active parliament called | country, international_organization | yes/no |
| `is_adjacent_to_lake` | Check if a location is a adjacent to a lake | location | yes/no |
| `is_admiral` | character is an admiral | character | yes/no |
| `is_admiral_of` | Character is admiral of the target country. | character | country tag/country scope → country |
| `is_adolescent` | character is Adolescent | character | yes/no |
| `is_adult` | character is Adult | character | yes/no |
| `is_ai` | country is run by AI | country | yes/no |
| `is_ai_preferred_parliament_change_law` | Checks if this law is the AI's preferred parliament law change target (pass actor country as argument) | law | country tag/country scope → country |
| `is_ai_preferred_parliament_change_policy` | Checks if this policy is the AI's preferred parliament policy change target (pass actor country as argument) | policy | country tag/country scope → country |
| `is_alert_shown` | Is the alert with the specified name shown? | none/any |  |
| `is_alert_triggered` | Is the alert with the specified name triggered? | none/any |  |
| `is_alive` | character is alive | character | yes/no |
| `is_allowed_for` | Returns true if the current database object is allowed (but not necessarily visible) for the target country. | law, policy, building_type, religious_aspect, estate_privilege, cabinet_action, government_reform, production_method, parliament_issue, parliament_type, god, omen, avatar, unit_type, levy_setup, parliament_agenda, artist_type, mission, mission_task, regency_type, unit_ability, road_type, heir_selection, formable_country | country tag/country scope → country |
| `is_allowed_for_international_organization` | Returns true if the current database object is available to the target international organization. | law, policy, parliament_issue, parliament_type, parliament_agenda | international_organization scope → international_organization |
| `is_already_merged` | If a culture_group is already merged. | culture_group | yes/no |
| `is_an_attacker` | Is the target country an attacker in the war? | war | country tag/country scope → country |
| `is_annexing` | Is the country annexing the specified country? | country | country tag/country scope → country |
| `is_annexing_any_country` | Is the country annexing any other country? | country | yes/no |
| `is_area_coastal_sea` | is the area coastal sea or not? | area | yes/no |
| `is_area_fully_discovered` | is the area fully discovered or not? | area | country tag/country scope → country |
| `is_area_passable` | is the area passable or not? | area | yes/no |
| `is_area_sea` | is the area sea or not? | area | yes/no |
| `is_army` | unit is Army | unit | yes/no |
| `is_art_destroyed` | Check if art is destroyed or not | work_of_art | yes/no |
| `is_artist` | character is Artist | character | yes/no |
| `is_artist_of` | Character is artist of the target country. | character | country tag/country scope → country |
| `is_at_max_level` | Checks if a building is working at full capacity | building | yes/no |
| `is_at_war_with` | Is the country at war with a specified country? | country | country tag/country scope → country |
| `is_auto_raise_taxrate_for_all_estates` | Check if all estates have auto raise taxrates? | country | yes/no |
| `is_available_for` | Returns true if the current database object is available to the target country. | law, policy, building_type, religious_aspect, estate_privilege, cabinet_action, government_reform, production_method, parliament_issue, parliament_type, god, omen, avatar, unit_type, levy_setup, parliament_agenda, artist_type, mission, mission_task, regency_type, unit_ability, road_type, heir_selection, formable_country | country tag/country scope → country |
| `is_available_for_international_organization` | Returns true if the current database object is available to the target international organization. | law, policy, parliament_issue, parliament_type, parliament_agenda | international_organization scope → international_organization |
| `is_being_annexed` | Is the country being annexed by any other country? | country | yes/no |
| `is_being_annexed_by` | Is the country getting annexed by the specified country? | country | country tag/country scope → country |
| `is_being_explored` | is the area being explored by a country? | area | country tag/country scope → country |
| `is_betrothed` | character is betrothed to someone | character | yes/no |
| `is_bombard_phase` | Check if a combat is currently in the bombard phase | combat | yes/no |
| `is_bond` | Checks if the loan is a government bond | loan | yes/no |
| `is_border` | Check if a location borders another country | location | yes/no |
| `is_building_owned_by` | Checks if a building is owned by a country | building | country tag/country scope → country |
| `is_burgher_positive_deficit` | Checks if a building location does not have negative burgher deficit | location | yes/no |
| `is_camera_in_zoom_level` | Is camera in a specified zoom level? SMALL / MEDIUM / LARGE | none/any |  |
| `is_canal_open` | Returns true if the canal at this location is built and open. | location | yes/no |
| `is_canal_restricted` | Returns true if the canal owner has restricted passage to allies and subjects only. | location | yes/no |
| `is_capital` | Check if a location is a capital | location | yes/no |
| `is_carrying_or_loaded_unit` | Mercenary's underlying unit is embarked on another fleet or has armies embarked on board. Gate destruction/delist actions on this — otherwise the emba… | mercenary | yes/no |
| `is_carrying_troops` | unit is carrying troops | unit | yes/no |
| `is_child` | character is Child | character | yes/no |
| `is_child_of` | Is the character a child of the target character? | character | character scope → character |
| `is_city` | Check if a location is a city | location | yes/no |
| `is_civil_war_for` | Is the current war a civil war for the target country? | war | country tag/country scope → country |
| `is_close_relative` | Is the character a close relative (Child, Parent, Sibling/Half-sibling, Nephew/Niece, Aunt/Uncle, Grandparent or Grandchild) of the target character? | character | character scope → character |
| `is_coastal` | Check if a location is coastal | location | yes/no |
| `is_colonial_overlord` | Country is an overlord of a colonial subject | country | yes/no |
| `is_colonial_subject` | Country is a type of colonial subject | country | yes/no |
| `is_colonial_top_overlord` | Country is the top overlord of a colonial subject | country | yes/no |
| `is_condottieri` | subunit is a lent unit (condottieri) | sub_unit | yes/no |
| `is_connected_to` | Check if a location is connected by land/strait to another location in the same country | location | province id/location scope → location |
| `is_connected_to_through_realm` | Check if a location is connected by land/strait to another location in the same realm (top overlord and all subjects) | location | province id/location scope → location |
| `is_consort` | character is Consort | character | yes/no |
| `is_consort_of` | Character is consort of the target country. | character | country tag/country scope → country |
| `is_core_of` | Is the location a core of the target country? | location | country tag/country scope → country |
| `is_country_leader_of_circle` | Is the specified country the leader of this Imperial Circle? | imperial_circle | country tag/country scope → country |
| `is_courtier` | Character is a courtier and has no roles assigned | character | yes/no |
| `is_creating_cb_against` | Checks if the current country scope is creating a casus belli against the target country. | country | country tag/country scope → country |
| `is_creating_cb_of_type` | Checks if the current country scope is creating a casus belli of the specified type against the target country. Usage: is_creating_cb_of_type = { targ… | country |  |
| `is_crossing` | Check if a combat has any crossing (river, strait, sea landing | combat | yes/no |
| `is_currently_being_integrated` | Check if a location is currently being integrated | location | yes/no |
| `is_cut_down_in_size_cb` | is it a cut down in size CB | casus_belli | yes/no |
| `is_defined_from_culture` | If a culture was originally defined as (or merged from) the target culture. | culture | culture scope → culture |
| `is_demanded_in_market` | Check if the goods scope is demanded in the target market. | goods | market scope → market |
| `is_demanded_in_market_by_buildings` | Check if the goods scope is demanded in the target market by buildings. | goods | market scope → market |
| `is_demanded_in_market_by_burgher_trades` | Check if the goods scope is demanded in the target market by burgher trades. | goods | market scope → market |
| `is_demanded_in_market_by_constructions` | Check if the goods scope is demanded in the target market by constructions. | goods | market scope → market |
| `is_demanded_in_market_by_pops` | Check if the goods scope is demanded in the target market by pops. | goods | market scope → market |
| `is_demanded_in_market_by_roads` | Check if the goods scope is demanded in the target market by roads. | goods | market scope → market |
| `is_demanded_in_market_by_trades` | Check if the goods scope is demanded in the target market by trades. | goods | market scope → market |
| `is_demanded_in_market_by_units` | Check if the goods scope is demanded in the target market by units. | goods | market scope → market |
| `is_discovered_by` | Is the scope location/country discovered by the target country? | location, country | country tag/country scope → country |
| `is_discriminated_in` | If a culture is discriminated in the target country? | culture | country tag/country scope → country |
| `is_disloyal_subject` | Is the country a disloyal subject? | country | yes/no |
| `is_dominant_country_of` | Check if a country is the dominant country of a culture | country | culture scope → culture |
| `is_dormant_imperial_circle` | Is this imperial circle dormant? | imperial_circle | yes/no |
| `is_during_bankruptcy` | country is having a bankruptcy | country | yes/no |
| `is_dynastic_descendant_of` | Is the character a dynastic descendant of the target dynasty? | character | dynasty scope → dynasty |
| `is_dynasty_head` | character is DynastyHead | character | yes/no |
| `is_east_of` | Check if a location is east of another location | location | province id/location scope → location |
| `is_elector_in_international_organization` | Checks if the country is an elector in the target international organization. | country | international_organization scope → international_organization |
| `is_eligible_for_betrothal` | Check if the current character scope can betroth someone | character | yes/no |
| `is_eligible_for_marriage` | Check if the current character scope can marry | character | yes/no |
| `is_eligible_for_royal_betrothal` | if character is eligible for betrothal. Is more restrictive and focused on rulers / heirs than the can_marry trigger | character | yes/no |
| `is_eligible_for_royal_marriage` | if character is eligible for royal marriage. Is more restrictive and focused on rulers / heirs than the can_marry trigger | character | yes/no |
| `is_eligible_heir` | Checks if the character can be an eligible heir for the specified country | character | country tag/country scope → country |
| `is_eligible_heir_baseline` | Checks if the character can be an eligible heir for the specified country without checking the heir selection law | character | country tag/country scope → country |
| `is_eligible_military_leader` | Checks if the character can be an eligible military leader for the specified country | character | country tag/country scope → country |
| `is_embargoed_by` | Is the country embargoed by the specified country? | country | country tag/country scope → country |
| `is_embargoing` | Is the country embargoing the specified country? | country | country tag/country scope → country |
| `is_embraced_for` | Is the institution embraced by the target country? | institution | country tag/country scope → country |
| `is_enemy_of` | Is the country a enemy of a specified country? | country | country tag/country scope → country |
| `is_enemy_of_international_organization` | Is the country an enemy of the supplied international organization? | country | international_organization scope → international_organization |
| `is_estate_loan` | Checks if the loan is an estate loan (not from a banking country) | loan | yes/no |
| `is_estate_private_regiment` | subunit was raised from an estate private army | sub_unit | yes/no |
| `is_exiled` | unit is Exiled | unit | yes/no |
| `is_expedition_leader` | Check if a character is currently leading an expedition | character | yes/no |
| `is_explorer` | character is an explorer | character | yes/no |
| `is_explorer_of` | Character is explorer of the target country. | character | country tag/country scope → country |
| `is_export` | Check if a trade is an export | trade | yes/no |
| `is_export_banned` | Checks if export of specific goods is banned in this market | market | goods scope → goods |
| `is_female` | character is Female | character | yes/no |
| `is_fighting_war_together_with` | Is the country fighting a war together with a specified country? | country | country tag/country scope → country |
| `is_food` | Check if a goods is food | goods | yes/no |
| `is_foreign` | Checks if a building can be built in foreign locations (not owned) | building_type | yes/no |
| `is_formable_visible` | Checks if the country can even see the formable of the formable country in the first place. | country | formable_country scope → formable_country |
| `is_friendly_with` | Is the country friendly with specified country? | country | country tag/country scope → country |
| `is_frozen` | Check if a location is currently frozen over | location | yes/no |
| `is_full_capacity` | Checks if a building is working at full capacity | building | yes/no |
| `is_full_expanded_rgo` | Check if a location has its RGO fully expanded | location | yes/no |
| `is_fully_implemented_in` | Checks if the current government reform/avatar/estate privilege/god/policy/law/cabinet action scope has been fully implemented in the specified countr… | law, policy, estate_privilege, cabinet_action, government_reform, god, omen, avatar | country tag/country scope → country |
| `is_gamestate_tutorial_active` | Is the gamestate tutorial active? See save_progress_in_gamestate in tutorial_lesson_chains documentation. | none/any | yes/no |
| `is_general` | character is a general | character | yes/no |
| `is_general_of` | Character is general of the target country. | character | country tag/country scope → country |
| `is_god_female` | is the god female? | god | yes/no |
| `is_goods_valid_for_location` | Checks if a specific goods can be the raw material in the scope location | location | goods scope → goods |
| `is_great_power` | country is a great power | country | yes/no |
| `is_head_of_religious_order_type` | True if this character is the head (Grand Master) of any religious order whose type matches the supplied religious_order_type — used to exempt militar… | character | religious_order_type scope → religious_order_type |
| `is_hegemon` | country is a Hegemon | country | yes/no |
| `is_hegemon_type` | Is the country a Hegemon of the specified type? | country | hegemony scope → hegemony |
| `is_heir` | character is Heir | character | yes/no |
| `is_heir_of` | Character is heir of the target country. | character | country tag/country scope → country |
| `is_heir_of_court_country` | character is Heir of their court country | character | yes/no |
| `is_historical_capital` | Check if a location is a historical capital of its owner | location | yes/no |
| `is_historical_rival_of` | Is the country an historical rival of a specified country? | country | country tag/country scope → country |
| `is_holding_of_any_religious_order` | True if this location has at least one religious order with presence (i.e. is a holding). | location | yes/no |
| `is_holy_site_for` | Is the holy site relevant to the target religion? | holy_site | religion scope → religion |
| `is_host` | Is this country player the host of a session multiplayer? | country | yes/no |
| `is_hostile_with` | Is the country hostile of specified country? | country | country tag/country scope → country |
| `is_human` | country is controlled by a human | country | yes/no |
| `is_immortal` | character is immortal | character | yes/no |
| `is_imperial_circle_leader` | Is this country the leader of any Imperial Circle? | country | yes/no |
| `is_implementable_in` | Checks if the current government reform/avatar/estate privilege/god/policy/law/cabinet action scope can be implemented in the specified country. Does … | law, policy, estate_privilege, cabinet_action, government_reform, god, omen, avatar | country tag/country scope → country |
| `is_import_banned` | Checks if import of specific goods is banned in this market | market | goods scope → goods |
| `is_in_any_same_international_organization` | Is the country in any same international organization as the target country? | country | country tag/country scope → country |
| `is_in_chivalric_order` | Checks if the character is a member of any chivalric order. | character | yes/no |
| `is_in_defensive_war` | Country is a defender in any of its current wars. | country | yes/no |
| `is_in_list` | Checks if a target in in a list | none/any |  |
| `is_in_losing_war` | Country is currently in a war with less than 0 war score. | country | yes/no |
| `is_in_promoted_industry_area` | Check if the location's area has an industry promotion owned by the location's owner | location | yes/no |
| `is_in_same_international_organization` | Is the country in the same international organization as the target country? Usage: is_in_same_international_organization = { international_organizati… | country |  |
| `is_in_scripted_geography` | Checks if the scope is part of the scripted geography on RHS scope e.g is_in_scripted_geography = <scripted geography scope> | location, province_definition, area, region, sub_continent, continent | scripted_geography scope → scripted_geography |
| `is_in_surplus_in_market` | Gets the possible trade surplus of the scope goods in the target market. | goods | <, <=, =, !=, >, >= |
| `is_in_war` | Is the target country in the war? | war | country tag/country scope → country |
| `is_infant` | character is Infant | character | yes/no |
| `is_integrating` | Is the country integrating any of its owned locations in province? | country | province_definition scope → province_definition |
| `is_international_organization_annullable` | Is the international organization able to be annulled by treaty? | international_organization | yes/no |
| `is_international_organization_unique` | Is the international organization unique? | international_organization | yes/no |
| `is_invoking_omen` | country is invoking omen | country | yes/no |
| `is_key_in_global_variable_map` | Checks if a target is a key in a global variable map is_key_in_global_variable_map = { name = X target = Y } | none/any |  |
| `is_key_in_local_variable_map` | Checks if a target is a key in a local variable map is_key_in_local_variable_map = { name = X target = Y } | none/any |  |
| `is_key_in_variable_map` | Checks if a target is a key in a variable map is_key_in_variable_map = { name = X target = Y } | none/any |  |
| `is_known_by_country` | Checks if the country is known by the specified country | country | country tag/country scope → country |
| `is_labourer_positive_deficit` | Checks if a building location does not have negative labourer deficit | location | yes/no |
| `is_lacking_goods` | Checks if a building is lacking goods | building | yes/no |
| `is_land` | Check if a location is land | location | yes/no |
| `is_latest_road_type_for` | Check if a road type is the latest one for a country | road_type | country tag/country scope → country |
| `is_leader_of_any_imperial_circle_in_io` | Is this country the leader of any Imperial Circle in the specified IO? | country | international_organization scope → international_organization |
| `is_leader_of_imperial_circle` | Is this country the leader of the specified Imperial Circle? | country | imperial_circle scope → imperial_circle |
| `is_leader_of_international_organization` | Is the country the Leader of the specified international organization? | country | international_organization scope → international_organization |
| `is_leading_largest_army_of` | The character is leading the largest land unit of the target country | character | country tag/country scope → country |
| `is_leading_largest_navy_of` | The character is leading the largest navy unit of the target country | character | country tag/country scope → country |
| `is_lent_unit` | Check if a mercenary is a unit lent from an ally | mercenary | yes/no |
| `is_levy` | subunit is levy | sub_unit | yes/no |
| `is_linked_to_foreign_building` | Check if a pop is linked to a foreign building | pop | yes/no |
| `is_location_holy_site_for` | Is the location a holy site for the target religion? | location | religion scope → religion |
| `is_location_holy_site_type` | Is the location a holy site of the specified type? | location | holy_site_type scope → holy_site_type |
| `is_location_valid_for_goods` | Checks if the scope goods can be the raw material in the supplied location | goods | province id/location scope → location |
| `is_locked` | Check if a trade is locked | trade | yes/no |
| `is_locked_for` | Returns true if the current database object is locked for the target country. | law, policy, government_reform, parliament_type, heir_selection | country tag/country scope → country |
| `is_locked_for_international_organization` | Returns true if the current database object is locked for the target international organization. | law, policy, parliament_type | international_organization scope → international_organization |
| `is_looted` | Check if a location is looted | location | yes/no |
| `is_loyal` | character is loyal to their ruler | character | yes/no |
| `is_major_reform` | Checks if the government reform is major | government_reform | yes/no |
| `is_map_mode_active` | Is map mode active? | none/any |  |
| `is_market_center` | Check if a location is a market center | location | yes/no |
| `is_married` | character is Married | character | yes/no |
| `is_matrilineal_descendant_of` | Is the character a dynastic descendant of the target dynasty via a matrilineal line?? | character | dynasty scope → dynasty |
| `is_max_level` | Checks if a building is at maximum level | building | yes/no |
| `is_member_of_circle` | Is this country a member of the specified Imperial Circle? | country | imperial_circle scope → imperial_circle |
| `is_member_of_international_organization` | Is the country in the supplied international organization? | country | international_organization scope → international_organization |
| `is_member_of_international_organization_of_type` | Is the country in an international organization of the specified type { type = x [target = <country>] }? | country |  |
| `is_mercenary` | subunit is Mercenary | sub_unit | yes/no |
| `is_mercenary_hired_by` | Check if a mercenary is hired by a specific country | mercenary | country tag/country scope → country |
| `is_mercenary_leader` | character is a mercenary leader | character | yes/no |
| `is_mercenary_of` | Character is mercenary of the target country. | character | country tag/country scope → country |
| `is_mercenary_owned_by` | Check if a mercenary is owned by a specific country | mercenary | country tag/country scope → country |
| `is_merged_culture_group` | If a culture has been merged from a culture group. | culture | yes/no |
| `is_merged_culture_group_of` | If a culture has been merged from this specific culture group. | culture | culture_group scope → culture_group |
| `is_mining_rgo` | Check if a location has a mining_rgo | location | yes/no |
| `is_movement_locked` | unit is movement locked | unit | yes/no |
| `is_moving` | unit is moving | unit | yes/no |
| `is_multiplayer_session` | Is the current game session multiplayer? | none/any | yes/no |
| `is_naval_combat` | Check if a combat is between navies on the sea | combat | yes/no |
| `is_navy` | unit is Navy | unit | yes/no |
| `is_neighbor_of` | Is the country or location a Neighbor to the specified country? | location, country | country tag/country scope → country |
| `is_neighbor_of_country_or_across_one_seazone` | Is the country or location a Neighbor to the specified country, or only a single seazone apart? | location, country | country tag/country scope → country |
| `is_neighbor_of_international_organization` | Is the country or location a neighbor to the specified international organization? | location, country | international_organization scope → international_organization |
| `is_neighbor_of_location` | Check if a location is neighbour to another | location | province id/location scope → location |
| `is_neighbor_of_location_or_across_one_seazone` | Check if a location is neighbour to another or just across a single seazone. | location | province id/location scope → location |
| `is_neighbor_of_province_definition` | Check if a province definition is neighbour to another | province_definition | province_definition scope → province_definition |
| `is_no_cb` | is it no CB | casus_belli | yes/no |
| `is_no_cb_war` | Checks if that war was started without any casus belli ('no cb') | war | yes/no |
| `is_not_profitable` | Checks if a building is not profitable or not has prfot at all | building | yes/no |
| `is_on_opposite_sides` | Check if the two countries are in opposing sides. Usage: is_on_opposite_sides = { country = <country> target = <target> } | war |  |
| `is_on_same_side` | Check if the two countries are on the same side. Usage: is_on_same_side = { country = <country> target = <target> } | war |  |
| `is_opened` | Checks if a building is opened | building | yes/no |
| `is_overlord` | country is an overlord | country | yes/no |
| `is_overseas_for_owner` | Check if a location or province is overseas for owber | location, province | yes/no |
| `is_ownable` | Check if a location is ownable, i.e. not sea, lake or an impassable | location | yes/no |
| `is_owned_by_any_international_organization` | Check if a location is owned by any international organization | location | yes/no |
| `is_owned_by_country` | Returns true if the rebel is owned by the target country | rebels | country tag/country scope → country |
| `is_owned_by_international_organization` | Check if a location is owned by an international organization | location | international_organization scope → international_organization |
| `is_owned_or_owned_by_subjects_of` | Check if the location is owned by the target country or its subjects | location | country tag/country scope → country |
| `is_owned_or_owned_by_subjects_or_below_of` | Check if the location is owned by the target country or its subjects or the subjects' subject | location | country tag/country scope → country |
| `is_parent_of` | Is the character a parent of the target character? | character | character scope → character |
| `is_passable` | Check if a location is passable | location | yes/no |
| `is_patrilineal_descendant_of` | Is the character a dynastic descendant of the target dynasty via a patrilineal line?? | character | dynasty scope → dynasty |
| `is_player_playstyle` | Player has only one playstyle and is equal to MILITARY, ADMINISTRATIVE or DIPLOMATIC | country |  |
| `is_port` | Check if a location has a port | location | yes/no |
| `is_pregnant` | character is Pregnant | character | yes/no |
| `is_primary_in` | If a culture is a primary culture in the target country? | culture | country tag/country scope → country |
| `is_primary_or_accepted_in` | If a culture is a primary culture or accepted in the target country? | culture | country tag/country scope → country |
| `is_produced_by_production_method` | Returns true if the trade good is produced by the specified production method. | goods | production_method scope → production_method |
| `is_produced_in_location_market` | Checks if a specific goods in produced in the location market | location | goods scope → goods |
| `is_produced_in_market` | Checks if a specific goods in produced in this market | market | goods scope → goods |
| `is_production_method_locked` | Checks if a building has its production method locked | building | yes/no |
| `is_profitable` | Checks if a building is profitable | building | yes/no |
| `is_projected_to_run_out_of_food_stockpile` | Checks if a market is projected to run out of food | market | yes/no |
| `is_province_capital` | Check if a location is the province capital | location | yes/no |
| `is_proximity_source` | Check if a location is a source of proximity | location | yes/no |
| `is_real_country` | Checks if a country is a real country as opposed to mercenaries or pirates | country | yes/no |
| `is_rebel_country` | Checks if a country is a rebel country created from a civil war | country | yes/no |
| `is_regency_extended` | country has an extended regency? | country | yes/no |
| `is_regent` | character is Regent | character | yes/no |
| `is_regent_of` | Character is regent of the target country. | character | country tag/country scope → country |
| `is_regiment` | unit is regiment | sub_unit | yes/no |
| `is_regional_power` | country is a regional power | country | yes/no |
| `is_relevant` | Checks if an international organization is relevant to the supplied country | international_organization | country tag/country scope → country |
| `is_religion_enabled` | is the religion enabled or not | religion | yes/no |
| `is_religious_aspect_enabled` | "Checks if the input religious aspect is enabled for the country in scope. Meaning if the allow trigger in religious aspect DB object returns true. ex… | country | religious_aspect scope → religious_aspect |
| `is_religious_figure` | character is religious figure | character | yes/no |
| `is_religious_order_member` | True if this character is (or was in life) a member of any religious order. | character | yes/no |
| `is_required_for_formable` | Check if the location scope is required by the formable | location | formable_country scope → formable_country |
| `is_revolution_target` | Check if the country is the target of the revolution | country | yes/no |
| `is_revolutionary` | Check if the country is revolutionary | country | yes/no |
| `is_rival_of` | Is the country a rival of a specified country? | country | country tag/country scope → country |
| `is_river_crossing` | Check if a combat has a river crossing | combat | yes/no |
| `is_ruler` | character is Ruler | character | yes/no |
| `is_ruler_of` | Character is ruler of the target country. | character | country tag/country scope → country |
| `is_saint` | Checks if a character is a saint in any religion | character | yes/no |
| `is_saint_of` | Checks if a character is a saint in the specific religion | character | religion scope → religion |
| `is_same_gender` | Is the character same gender as target character? | character | character scope → character |
| `is_sea_landing` | Check if a combat has an amphibious landing | combat | yes/no |
| `is_selectable_issue_for` | Check if the parliament issue scope is selectable by country in the target international organization. Usage: is_selectable_issue_for = { actor = <cou… | parliament_issue |  |
| `is_selected_omen_god` | If a god is the one asked for guidance for selecting an omen in the target country | god | country tag/country scope → country |
| `is_set` | Checks whether the specified scope target has been set (includes being the null object) is_set = from.owner.var:cool_var.mother | none/any |  |
| `is_ship` | subunit is Ship | sub_unit | yes/no |
| `is_sibling_of` | Is the character a sibling of the target character? | character | character scope → character |
| `is_situation_active` | Checks if the target situation is currently active | none/any | situation scope → situation |
| `is_south_of` | Check if a location is south of another location | location | province id/location scope → location |
| `is_special_building` | Checks if a building is special | building_type | yes/no |
| `is_spouse_of` | Is the character a spouse of the target character? | character | character scope → character |
| `is_starving` | Check if a province is starving | province | yes/no |
| `is_strait_crossing` | Check if a combat has a strait crossing | combat | yes/no |
| `is_subject` | country is a subject | country | yes/no |
| `is_subject_of` | Is the country a subject to the specified country? | country | country tag/country scope → country |
| `is_subject_or_below_of` | Is the country a subject of (or subject of a subject of) the specified country? | country | country tag/country scope → country |
| `is_subject_type` | Is the country a subject of the specified type? | country |  |
| `is_subject_type_annullable` | Check if a subject type can be annulled by a peace treaty | subject_type | yes/no |
| `is_subsidized` | Checks if a building is subsidized | building | yes/no |
| `is_supported_by_character` | Returns true if the rebel is supported by the target character | rebels | character scope → character |
| `is_supported_by_country` | Returns true if the rebel is supported by the target country | rebels | country tag/country scope → country |
| `is_system_automated` | Checks if an automated system is active for the country. Usage: is_system_automated = credit | country |  |
| `is_target_in_global_variable_list` | Checks if a target is in a global variable list is_target_in_global_variable_list = { name = X target = Y } Where X is the name of the variable Where … | none/any |  |
| `is_target_in_local_variable_list` | Checks if a target is in a local variable list is_target_in_local_variable_list = { name = X target = Y } Where X is the name of the variable Where Y … | none/any |  |
| `is_target_in_variable_list` | Checks if a target is in a variable list on the current scope is_target_in_variable_list = { name = X target = Y } Where X is the name of the variable… | none/any |  |
| `is_target_of_international_organization_of_type` | Is the country a target of an international organization of the specified type? | country |  |
| `is_threat_to` | Current country scope is a threat and have a casus belli to the target country | country | country tag/country scope → country |
| `is_tolerated_in` | If a culture is tolerated in the target country? | culture | country tag/country scope → country |
| `is_tooltip_with_name_open` | Is the tooltip with the specified name open? | none/any |  |
| `is_trade_cb` | is it a trade CB | casus_belli | yes/no |
| `is_traded_in_market` | Checks if a specific goods in traded in this market | market | goods scope → goods |
| `is_tutorial_active` | Is the tutorial active? | none/any | yes/no |
| `is_tutorial_lesson_active` | Is this the current tutorial lesson? is_tutorial_lesson_active = reactive_advice_succession | none/any |  |
| `is_tutorial_lesson_chain_completed` | Has the tutorial lesson chain with the specified key been finished? | none/any |  |
| `is_tutorial_lesson_completed` | has the tutorial lesson with the specified name been finished? | none/any |  |
| `is_tutorial_lesson_step_completed` | Has the tutorial lesson step been finished? is_tutorial_lesson_step_completed = lesson_key:step_key | none/any |  |
| `is_unified_culture` | Check if a location has culture unified with the owner | location | yes/no |
| `is_unique_reform` | Checks if the government reform is unique | government_reform | yes/no |
| `is_unit_locked` | Check if a Unit is locked | unit | yes/no |
| `is_unit_type` | Checks if the unit type scope is a specific unit type | unit_type | class CSubUnitDefinitionDataBase key → unit_type |
| `is_upgradeable` | Checks if a building is upgradeable | building_type | yes/no |
| `is_upgraded_level` | Checks if a building could have been upgraded | building_type | yes/no |
| `is_upper_class` | Check if a pop is upper class or not | pop | yes/no |
| `is_used_by_production_method` | Returns true if the trade good is used by the specified production method. | goods | production_method scope → production_method |
| `is_valid_colonial_charter` | is this colonial charter valid, or blocked by a recognised claim? | country | province_definition scope → province_definition |
| `is_valid_for_exploration` | character is valid for an exploration | character | yes/no |
| `is_valid_goal_for_charter` | Whether this colonial charter goal is available for the given colonial charter. The null goal (automatic) is always valid | colonial_charter_goal |  |
| `is_valid_subject_for_goal` | Whether the country is a valid subject for the given colonial charter goal | country |  |
| `is_value_in_global_variable_map` | Checks if a target is a value in a global variable map is_value_in_global_variable_map = { name = X target = Y } | none/any |  |
| `is_value_in_local_variable_map` | Checks if a target is a value in a local variable map is_value_in_local_variable_map = { name = X target = Y } | none/any |  |
| `is_value_in_variable_map` | Checks if a target is a value in a variable map is_value_in_variable_map = { name = X target = Y } | none/any |  |
| `is_visible_for` | Returns true if the current database object is visible (but not necessarily allowed) to the target country. | law, policy, building_type, religious_aspect, estate_privilege, cabinet_action, government_reform, production_method, parliament_issue, parliament_type, god, omen, avatar, unit_type, levy_setup, parliament_agenda, artist_type, mission, mission_task, regency_type, unit_ability, road_type, heir_selection, formable_country | country tag/country scope → country |
| `is_visible_for_international_organization` | Returns true if the current database object is available to the target international organization. | law, policy, parliament_issue, parliament_type, parliament_agenda | international_organization scope → international_organization |
| `is_war_leader_of` | Checks if the current country scope is a war leader of the target war | country | war scope → war |
| `is_waypoint_in_expedition` | Check if a location is a waypoint of the target expedition. Scope: location. Value: expedition reference (e.g. scope:target_expedition) | location | expedition scope → expedition |
| `is_widgetid_open` | Is the widget with the specified `widgetid` open (visible and not animating)? The fastest and safest way to check. (replaces old `is_widget_open` func… | none/any |  |
| `is_withering` | Checks if a market is withering (sustained at-or-below MARKET_WITHERING_LOCATION_THRESHOLD locations with a fallback market available for most of them… | market | yes/no |
| `join_organization_ai_desire` | Returns the AI desire to join the specified target international organization. Usage: join_organization_ai_desire = { international_organization = <or… | country | <, <=, =, !=, >, >= |
| `join_war_reason` | Checks the reason for a country joining a war. join_war_reason = { country = <> reason = <Instigator,Target,Alliance,Subject,Overlord,SupportRevolt,Su… | war |  |
| `karma` | How much karma does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `karma_percentage` | How high the percentage of the current karma compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `knows_about_institution` | Checks if a country has knows about an institution | country | institution scope → institution |
| `knows_country` | Checks if the country knows of the specified country | country | country tag/country scope → country |
| `land_war_attrition_casualties` | Amount of land casualties taken by the country in this war due to attrition "scope:war.land_war_attrition_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `land_war_battle_casualties` | Amount of land casualties taken by the country in this war due to battle "scope:war.land_war_battle_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `land_war_capture_casualties` | Amount of land casualties taken by the country in this war due to capture "scope:war.land_war_capture_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `land_war_casualties` | Amount of land casualties taken by the country in this war "scope:war.land_war_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `land_war_disease_casualties` | Amount of land casualties taken by the country in this war due to disease "scope:war.land_war_disease_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `language_percentage_in_country` | The percentage of speakers of a specific language in the current country | country | <, <=, =, !=, >, >= |
| `language_population_in_country` | The number of speakers of a specific language in the current country | country | <, <=, =, !=, >, >= |
| `language_power` | How much power does the language has (percent of best)? | language, dialect | <, <=, =, !=, >, >= |
| `last_months_balance` | Checks the last realized monthly balance of a country. Zero until a first month has been processed | country | <, <=, =, !=, >, >= |
| `law_enabled_to_international_organization` | Can we select a policy for a law in the scope international organization? | international_organization | law scope → law |
| `law_is_locked_in_international_organization` | Is a law locked in the scope international organization? | international_organization | law scope → law |
| `law_visible_to_international_organization` | Can we see a policy for a law in the scope international organization? | international_organization | law scope → law |
| `leader_change_method` | Check if the international organization has the specified leader changed method (rotation/vote/lottery/none) | international_organization |  |
| `leader_change_trigger_type` | Check if the international organization has the specified leader changed trigger type (rulerchang/timed/none) | international_organization |  |
| `leader_special_status_power` | Get the special status power of all special statuses with the 'leader' trait | international_organization | <, <=, =, !=, >, >= |
| `leader_special_status_power_fraction` | Get the fraction of the special status power of all special statuses with the 'leader' trait | international_organization | <, <=, =, !=, >, >= |
| `leader_type` | Check if the international organization has the specified leader type (character/country/none) | international_organization |  |
| `legitimacy` | How much legitimacy does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `legitimacy_percentage` | How high the percentage of the current legitimacy compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `liberty_desire` | Checks the amount of liberty desire a country has | country | <, <=, =, !=, >, >= |
| `list_size` | Checks the size of a list list_size = { name = X value >= Y } Where X is the name of the list Where Y is a script value | none/any | <, <=, =, !=, >, >= |
| `liturgical_language_utility` | Utility of a liturgical language accorting to Ai | language, dialect | <, <=, =, !=, >, >= |
| `loan_amount` | Checks the amount of a loan | loan | <, <=, =, !=, >, >= |
| `loan_interest` | Checks the interest of a loan | loan | <, <=, =, !=, >, >= |
| `local_control` | Checks if a location has a certain control | location | <, <=, =, !=, >, >= |
| `local_cultural_unity` | Checks the percentage the dominant-culture has in a location | location | <, <=, =, !=, >, >= |
| `local_estate_power` | Checks the raw local estate power in location | location | <, <=, =, !=, >, >= |
| `local_political_power_fraction` | Checks the fraction this location has of the total political power of a country | location | <, <=, =, !=, >, >= |
| `local_relative_estate_power` | Checks the relative local estate power in location | location | <, <=, =, !=, >, >= |
| `local_religious_unity` | Checks the percentage the dominant-religion has in a location | location | <, <=, =, !=, >, >= |
| `local_variable_list_size` | Checks the size of a local variable list local_variable_list_size = { name = X target >= Y } Where X is the name of the variable Where Y is a script v… | none/any |  |
| `local_variable_map_size` | Checks the size of a variable map variable_map_size = { name = X value >= Y } Where X is the name of the variable map Where Y is a script value or num… | none/any |  |
| `location_art_quality` | Checks the total art quality in a location | location | <, <=, =, !=, >, >= |
| `location_building_level` | Checks if a location has a building type at a certain level (with optional owner) | location | <, <=, =, !=, >, >= |
| `location_can_be_added_to_international_organization` | Can we add a location to the scope international organization? | international_organization | province id/location scope → location |
| `location_can_be_removed_from_international_organization` | Can we remove a location from the scope international organization? | international_organization | province id/location scope → location |
| `location_counter` | Checks if the province/province_defintion/area/region/subcontinent/continent/scripted_geography has this amount of location | province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `location_highest_work_of_art_quality` | Checks the quality of the highest-quality work of art in a location | location | <, <=, =, !=, >, >= |
| `location_holy_site_importance` | Importance of the most important holy site in the location | location | <, <=, =, !=, >, >= |
| `location_key` | Checks if a location is the specific one (from named_location) | location |  |
| `location_maritime_merchant_power` | gets the maritime merchant power for a country in the scope location | location | <, <=, =, !=, >, >= |
| `location_maritime_presence_power` | gets the maritime presence power for a country in the scope location. Usage: location_maritime_presence_power = { country = <country scope> value <ope… | location | <, <=, =, !=, >, >= |
| `location_max_population` | Checks if a location has a certain pixel count | location | <, <=, =, !=, >, >= |
| `location_max_winter_level` | Checks the maximum winter level of a location | location |  |
| `location_modifier_strength` | Does the scoped province have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 loca… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `location_net_building_profit` | Checks the net profit from buildings in a location | location | <, <=, =, !=, >, >= |
| `location_num_holy_sites` | Number of holy sites in the location | location | <, <=, =, !=, >, >= |
| `location_num_works_of_art` | Checks if a location has a certain number of works of art | location | <, <=, =, !=, >, >= |
| `location_peace_cost` | "gets the peace cost for the location according to giver and taker countries usage in trigger: location_peace_cost = { giver = <country> taker = <coun… | location | <, <=, =, !=, >, >= |
| `location_population_percentage` | Checks if a location has a certain percentage of population capacity | location | <, <=, =, !=, >, >= |
| `location_potential_tax_base` | Checks the potential tax-base of a location | location | <, <=, =, !=, >, >= |
| `location_privateer_power` | gets the maritime privateeer power for a country in the scope location | location | <, <=, =, !=, >, >= |
| `location_progress_for_formable` | Checks the progress of the country scope to form the specified formable in percentage. Usage: location_progress_for_formable = { formable_country = <f… | country | <, <=, =, !=, >, >= |
| `location_size` | Checks if a location has a certain pixel count | location | <, <=, =, !=, >, >= |
| `location_tax_base` | Checks the tax-base of a location | location | <, <=, =, !=, >, >= |
| `location_unemployed_population_for_building_type` | Checks if a location has a certain unemployed population for the supplied building type (with optional owner) | location | <, <=, =, !=, >, >= |
| `location_valid_for_calling` | True if this location satisfies the supplied calling's location_trigger requirements and the founding-location requirements of the calling's order typ… | location | calling scope → calling |
| `location_valid_for_religious_order_type` | True if this location satisfies the supplied religious order type's default founding-location requirements (owner / region / frontier). | location | religious_order_type scope → religious_order_type |
| `location_within_range` | Checks if a location has a certain population within range | location | country tag/country scope → country |
| `location_works_of_art_star_rating` | Checks if a country has a certain amount of work of arts | location | <, <=, =, !=, >, >= |
| `long_term_eco_growth` | Checks the long-term (50-year) economic base growth rate of a country | country | <, <=, =, !=, >, >= |
| `long_term_trigger_currency_utility` | Checks the AI utility of adding an amount of a certain trigger every month to the scoped object long_term_trigger_currency_utility = { trigger = X siz… | country | <, <=, =, !=, >, >= |
| `lowest_prosperity` | Find the location in a province with the lowest prosperity | province | <, <=, =, !=, >, >= |
| `lowest_war_score` | Checks the lowest war score of ongoing wars | country | <, <=, =, !=, >, >= |
| `lowest_war_strength_balance` | Checks the lowest war strength balance of ongoing wars | country | <, <=, =, !=, >, >= |
| `loyalty_from_subject_bloc_strength` | Checks the loyalty change all subjects as a bloc would cause for a subject of the given type. Usage: loyalty_from_subject_bloc_strength = { type = <su… | country | <, <=, =, !=, >, >= |
| `manpower` | How much Manpower does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `manpower_percentage` | Checks the percentage of manpower a country has compared to its maximum | country | <, <=, =, !=, >, >= |
| `maritime_presence_bias` | Any bias to or from this bit of geography set by ambitions. Usage: maritime_presence_bias = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `market_access` | Checks if a location has certain market access | location | <, <=, =, !=, >, >= |
| `market_food` | Checks how much food is in the market stockpile | market | <, <=, =, !=, >, >= |
| `market_food_deficit` | Checks how much food is missing in the market | market | <, <=, =, !=, >, >= |
| `market_food_percentage` | Checks how much food is in the market stockpile percentage wise | market | <, <=, =, !=, >, >= |
| `market_food_traded` | Checks how much food is traded in the market | market | <, <=, =, !=, >, >= |
| `market_location_count` | Number of locations currently assigned to this market | market | <, <=, =, !=, >, >= |
| `market_max_food` | Checks how much food can be stockpiled in the market | market | <, <=, =, !=, >, >= |
| `market_monthly_food_balance` | Checks what the food balance is in the market | market | <, <=, =, !=, >, >= |
| `market_population` | Checks how many pops are in the market | market | <, <=, =, !=, >, >= |
| `market_possible_goods_trade_surplus` | gets the possible trade surplus for the goods in the scope market | market | <, <=, =, !=, >, >= |
| `max_age` | Checks if the unit type's age is the given age or earlier | sub_unit, unit_type | class CAgeDatabase key → age |
| `max_bureaucracy_slots` | Checks if a country has a certain amount of maximum bureaucracy slots | country | <, <=, =, !=, >, >= |
| `max_control` | Checks the max control in a location | location | <, <=, =, !=, >, >= |
| `max_countries_with_special_status` | gets the max number of countries with a specific special status in an international organization | international_organization | <, <=, =, !=, >, >= |
| `max_garrison_strength` | Checks the max garrison strength of the location in scope | location | <, <=, =, !=, >, >= |
| `max_manpower` | Checks if a country has a certain Max manpower | country | <, <=, =, !=, >, >= |
| `max_possible_candidates` | "Maximum number of candidates for the heir selection. i.e. the number of choices the player will have when an election occurs ex: max_possible_candida… | heir_selection | <, <=, =, !=, >, >= |
| `max_religious_aspects` | Checks the amount of church aspects the religion has | religion | <, <=, =, !=, >, >= |
| `max_rgo_workers` | Checks if a location has a certain max number of RGO workers | location | <, <=, =, !=, >, >= |
| `max_sailors` | Checks if a country has a certain Max Sailors | country | <, <=, =, !=, >, >= |
| `max_sects` | number of sects available per country from the scope religion | religion | <, <=, =, !=, >, >= |
| `mercenary_has_owner` | Check if a mercenary has an owner | mercenary | yes/no |
| `mercenary_hire_cost` | Cost to hire this sub unit as a mercenary | sub_unit | <, <=, =, !=, >, >= |
| `mercenary_maintenance_cost` | Cost to maintain this sub unit as a mercenary | sub_unit | <, <=, =, !=, >, >= |
| `merchant_capacity` | gets the market merchant capacity for a country in the scope market | market | <, <=, =, !=, >, >= |
| `merchant_power_in_market` | gets the market merchant power for a country in the scope market | market | <, <=, =, !=, >, >= |
| `merged_culture_group_contains_culture` | True if this merged culture was formed from a culture group that contained the target culture. | culture | culture scope → culture |
| `migration_attraction` | Checks if a location has a certain migration_attraction | location | <, <=, =, !=, >, >= |
| `mil` | The mil ability of the character | character | <, <=, =, !=, >, >= |
| `military_strength` | Checks the total military strength (max manpower, army size, levy power) of a country | country | <, <=, =, !=, >, >= |
| `min_age` | Checks if the unit type's age is the given age or later | sub_unit, unit_type | class CAgeDatabase key → age |
| `mission_completed` | Checks if the country has completed the mission. | country | mission scope → mission |
| `mission_task_bypassed` | Checks if the country has bypassed the mission task. | country | mission_task scope → mission_task |
| `mission_task_completed` | Checks if the country has completed the mission task. | country | mission_task scope → mission_task |
| `modifier_utility` | Checks the AI utility of a modifier | location, country, unit, character, religion, province, religious_school, international_organization, policy, religious_aspect, government_reform, god, omen, avatar | <, <=, =, !=, >, >= |
| `modifier_utility_include_locations` | Checks the AI utility of a modifier with location checks | location, country, unit, character, religion, province, religious_school, international_organization, policy, religious_aspect, government_reform, god, omen, avatar | <, <=, =, !=, >, >= |
| `monarch_power_percentage_in_country` | The percentage of a specific monarch power in the current country | country | <, <=, =, !=, >, >= |
| `monthly_balance` | Checks the last realized monthly balance of a country. Zero until a first month has been processed. Alias of last_months_balance | country | <, <=, =, !=, >, >= |
| `monthly_conversion` | Checks if a location has an potential conversion of X per month | location | <, <=, =, !=, >, >= |
| `monthly_cost` | Checks the monthly cost of a mercenary | mercenary | <, <=, =, !=, >, >= |
| `monthly_income_total` | Checks if a country has a certain income | country | <, <=, =, !=, >, >= |
| `monthly_income_trade_and_tax` | Checks if a country has a certain trade and tax income | country | <, <=, =, !=, >, >= |
| `monthly_manpower` | Checks if a country has a certain monthly manpower | country | <, <=, =, !=, >, >= |
| `monthly_minting_inflation` | Actual monthly inflation currently generated by minting | country | <, <=, =, !=, >, >= |
| `monthly_political_influence` | Return the amount of political influence the country gains per month | country | <, <=, =, !=, >, >= |
| `monthly_population_change` | Check how much the population changes on a monthly base based on the previous month's data | country | <, <=, =, !=, >, >= |
| `monthly_sailors` | Checks if a country has a certain monthly Sailors | country | <, <=, =, !=, >, >= |
| `monthly_satisfaction_change` | The monthly satisfaction change of an estate | estate | <, <=, =, !=, >, >= |
| `monthly_trade_income` | Checks if a country has a certain income from trade | country | <, <=, =, !=, >, >= |
| `months_at_or_below_withering_threshold` | How many consecutive months this market has had <= MARKET_WITHERING_LOCATION_THRESHOLD locations | market | <, <=, =, !=, >, >= |
| `months_between_leader_changes` | Checks if a country has a specific reform | international_organization | <, <=, =, !=, >, >= |
| `months_difference` | Returns the difference in months between two dates. Can be used is complex form (scope:first_date\|scope:second_date) or with first = and second = | none/any | <, <=, =, !=, >, >= |
| `months_left` | Checks the months left of loan | loan | <, <=, =, !=, >, >= |
| `months_since_last_parliament_called` | Checks how many months its been since the country / international organization last called a parliament | country, international_organization | <, <=, =, !=, >, >= |
| `months_since_peace` | Checks how many months its been since a country was at peace | country | <, <=, =, !=, >, >= |
| `months_since_war` | Checks how many months its been since a country was at War | country | <, <=, =, !=, >, >= |
| `morale_percentage` | How many percent morale does this unit have??? | unit | <, <=, =, !=, >, >= |
| `movement_presence` | Checks the number of pops affected by a movement in a location or subunit. usage: movement_presence(<movement>) or movement_presence = { movement = <m… | location, sub_unit | <, <=, =, !=, >, >= |
| `nand` | a negated AND trigger | none/any |  |
| `naval_range` | The naval range of the country | country | <, <=, =, !=, >, >= |
| `navy_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `navy_size` | Checks if a country has a certain amount of ships | country | <, <=, =, !=, >, >= |
| `navy_size_percentage` | Checks if a country has a certain percentage of ships compared to expected size | country | <, <=, =, !=, >, >= |
| `navy_tradition` | How much navy tradition does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `navy_tradition_percentage` | How high the percentage of the current navy tradition compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `navy_war_attrition_casualties` | Amount of naval casualties taken by the country in this war due to attrition "scope:war.navy_war_attrition_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `navy_war_battle_casualties` | Amount of naval casualties taken by the country in this war due to battle "scope:war.navy_war_battle_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `navy_war_capture_casualties` | Amount of naval casualties taken by the country in this war due to capture "scope:war.navy_war_capture_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `navy_war_casualties` | Amount of naval casualties taken by the country in this war "scope:war.navy_war_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `navy_war_disease_casualties` | Amount of naval casualties taken by the country in this war due to disease "scope:war.navy_war_disease_casualties(c:FRA)" | war | <, <=, =, !=, >, >= |
| `need_reforms` | religion needs reforms | religion | yes/no |
| `needs_opinion_with` | Determines if a country needs X more relations with another nation. Usage: needs_opinion_with = { target = <country> value <comparator> <value> } | country | <, <=, =, !=, >, >= |
| `needs_subject` | Whether this charter goal requires a subject country to be selected | colonial_charter_goal | yes/no |
| `nor` | a negated OR trigger | none/any |  |
| `not` | negates content of trigger | none/any |  |
| `num_adult_available_dynasty_members` | Return the number of living adult dynasty members who are neither rulers nor heirs | dynasty | <, <=, =, !=, >, >= |
| `num_adult_capable_characters` | Checks if a country has a certain amount of adult characters who can do cabinet or military stuff | country | <, <=, =, !=, >, >= |
| `num_affected_locations` | How many locations are affected? | disease_outbreak | <, <=, =, !=, >, >= |
| `num_army_constructions` | Check how many army_constructions a location has | location | <, <=, =, !=, >, >= |
| `num_artists` | Checks if a country has a certain amount of artists | country | <, <=, =, !=, >, >= |
| `num_avatars` | Checks if a country has a certain amount of avatars | country | <, <=, =, !=, >, >= |
| `num_bonds` | Checks if a country has a certain amount of government bonds outstanding | country | <, <=, =, !=, >, >= |
| `num_buildings` | Checks if a location has a certain amount of  buildings | location | <, <=, =, !=, >, >= |
| `num_buildings_owned_by_estate` | Checks how many buildings an estate has | estate | <, <=, =, !=, >, >= |
| `num_bureaucracies` | Checks if a country has a certain amount of bureaucracies active | country | <, <=, =, !=, >, >= |
| `num_cabinet_capable_characters` | Checks if a country has a certain amount of characters who can do cabinet | country | <, <=, =, !=, >, >= |
| `num_cardinals` | Checks if a country has a certain amount of Cardinals | country | <, <=, =, !=, >, >= |
| `num_cbs` | Checks if a country has a certain amount of CBs | country | <, <=, =, !=, >, >= |
| `num_characters` | Checks if a country has a certain amount of living characters | country | <, <=, =, !=, >, >= |
| `num_civil_constructions` | Check how many civil_constructions a location has | location | <, <=, =, !=, >, >= |
| `num_colonial_charters` | Checks if a country has a certain amount of colonial charters | country | <, <=, =, !=, >, >= |
| `num_countries_in_religion` | number of countries in the religion | religion | <, <=, =, !=, >, >= |
| `num_countries_with_special_status` | gets the number of countries with a particular special status in an international organization | international_organization | <, <=, =, !=, >, >= |
| `num_country_that_can_be_called_offensively` | Checks the number of countries that can be called into an offensive war by this country | country | <, <=, =, !=, >, >= |
| `num_embraced_institutions` | Checks if a country has a certain number of institutions embraced | country | <, <=, =, !=, >, >= |
| `num_explorations` | Checks if a country has a certain amount of Explorations | country | <, <=, =, !=, >, >= |
| `num_explorations_including_in_construction` | Checks if a country has a certain amount of Explorations including those under construction | country | <, <=, =, !=, >, >= |
| `num_foreign_buildings` | Checks if a location has a certain amount of foreign buildings | location | <, <=, =, !=, >, >= |
| `num_forts` | Checks if a country has a certain amount of forts | country | <, <=, =, !=, >, >= |
| `num_known_institutions` | Checks if a country knows a certain number of institutions | country | <, <=, =, !=, >, >= |
| `num_loans` | Checks if a country has a certain amount of loans | country | <, <=, =, !=, >, >= |
| `num_locations` | Checks if a country has a certain amount of owned locations | country | <, <=, =, !=, >, >= |
| `num_locations_affected` | calculates the number of locations affected by the movement. num_locations_affected = { *value >/</>=/<= Y* or *value = { min max }*  } | movement | <, <=, =, !=, >, >= |
| `num_locations_in_religious_order` | The number of holdings this religious order holds worldwide. | religious_order | <, <=, =, !=, >, >= |
| `num_locations_owned_or_owned_by_subjects` | Checks if a country or its direct subjects has a certain amount of owned locations | country | <, <=, =, !=, >, >= |
| `num_locations_owned_or_owned_by_subjects_or_below` | Checks if a country, its subjects or its subjects' subjects has a certain amount of owned locations | country | <, <=, =, !=, >, >= |
| `num_navy_constructions` | Check how many navy_constructions a location has | location | <, <=, =, !=, >, >= |
| `num_of_active_parliament_agendas` | Check how many parliament agendas are currently available to the country or international organization. | country, international_organization | <, <=, =, !=, >, >= |
| `num_of_advances_researched` | Checks how many advances a country currently has researched. | country | <, <=, =, !=, >, >= |
| `num_of_children` | The number of children of the character | character | <, <=, =, !=, >, >= |
| `num_of_common_rivals` | calculates the amount of common rivals between two countries. num_of_common_rivals = { target = X [*value >/</>=/<= Y* or *value = { min max }*  } | country | <, <=, =, !=, >, >= |
| `num_of_common_rivals_and_enemies` | calculates the amount of common rivals and enemies between two countries. num_of_common_rivals_and_enemies = { target = X [*value >/</>=/<= Y* or *val… | country | <, <=, =, !=, >, >= |
| `num_of_diplomats` | Checks if a country has an amount of diplomats | country | <, <=, =, !=, >, >= |
| `num_of_electors` | Checks how many electors the international organization has | international_organization | <, <=, =, !=, >, >= |
| `num_of_locations_owned_by_io` | Checks if a country has an amount of locations owned by certain IO | country | <, <=, =, !=, >, >= |
| `num_of_locations_with_high_conquer_desire` | Gets how many locations the AI wants to conquer from the supplied country - usage: num_of_locations_with_high_conquer_desire(<target>) or num_of_locat… | country | <, <=, =, !=, >, >= |
| `num_of_markets_with_merchants` | Checks if a country has merchants in the specified amount of markets. | country | <, <=, =, !=, >, >= |
| `num_of_non_rural` | Checks if a country has an amount of towns and cities | country | <, <=, =, !=, >, >= |
| `num_of_non_rural_ports` | Checks if a country has an amount of non-rural ports | country | <, <=, =, !=, >, >= |
| `num_of_ports` | Checks if a country has an amount of ports | country | <, <=, =, !=, >, >= |
| `num_of_rebel_characters` | Get the amount of characters which support the rebel | rebels | <, <=, =, !=, >, >= |
| `num_of_rebel_supporters` | Get the amount of countries which support the rebel | rebels | <, <=, =, !=, >, >= |
| `num_of_religious_aspects` | Gets the total amount of church aspects in the country | country | <, <=, =, !=, >, >= |
| `num_of_spouses` | The number of spouses of the character | character | <, <=, =, !=, >, >= |
| `num_of_trades` | Checks if a country has an amount of trades active | country | <, <=, =, !=, >, >= |
| `num_of_traits` | The number of traits the character has | character | <, <=, =, !=, >, >= |
| `num_of_traits_of_category` | The number of traits of a specified category the character has. usage: num_of_trait_by_category(<trait_category>) or num_of_trait_by_category = { type… | character | <, <=, =, !=, >, >= |
| `num_omens` | Checks if a country has a certain amount of omens | country | <, <=, =, !=, >, >= |
| `num_open_bureaucracy_slots` | Checks if a country has a certain amount of open bureaucracy slots | country | <, <=, =, !=, >, >= |
| `num_open_cabinet_positions` | Checks if a country has a certain amount of open cabinet positions | country | <, <=, =, !=, >, >= |
| `num_open_reform_slots` | Checks if a country has a certain amount of open government reform slots | country | <, <=, =, !=, >, >= |
| `num_order_holdings` | The number of this country's owned locations that are religious order holdings. | country | <, <=, =, !=, >, >= |
| `num_order_holdings_of_order` | The number of this country's owned locations that are holdings to the target religious order. Usage: num_order_holdings_of_order = { religious_order =… | country | <, <=, =, !=, >, >= |
| `num_owned_foreign_buildings_in_location` | The number of foreign buildings in a location owned by a count | location | <, <=, =, !=, >, >= |
| `num_possible_privileges` | Checks if the scope country or estate has a certain amount of privileges | country, estate | <, <=, =, !=, >, >= |
| `num_possible_rivals` | Checks if a country has a certain amount of possible rivals | country | <, <=, =, !=, >, >= |
| `num_privileges` | Checks if the scope country or estate has a certain amount of privileges | country, estate | <, <=, =, !=, >, >= |
| `num_province_definitions_in_area` | Checks if an area has a certain amount of province definitions | area | <, <=, =, !=, >, >= |
| `num_provinces` | Checks if a country has a certain amount of provinces | country | <, <=, =, !=, >, >= |
| `num_rebels` | Checks if a country has a certain amount of Rebels | country | <, <=, =, !=, >, >= |
| `num_reforms` | Checks if a country has a certain amount of government reforms | country | <, <=, =, !=, >, >= |
| `num_regiments` | Checks if a country has a certain amount of regiments | country | <, <=, =, !=, >, >= |
| `num_relations_above_limit` | Amount above relations limit | country | <, <=, =, !=, >, >= |
| `num_rivals` | Checks if a country has a certain amount of rivals | country | <, <=, =, !=, >, >= |
| `num_roads` | Check how many roads a location has | location | <, <=, =, !=, >, >= |
| `num_subjects` | Checks the total number of subjects of a country | country | <, <=, =, !=, >, >= |
| `num_subunits` | How many sub units does this unit have? | unit | <, <=, =, !=, >, >= |
| `num_union_countries` | Return the number of countries under any union ruled by the scoped dynasty | dynasty | <, <=, =, !=, >, >= |
| `num_unions` | Return the number of unions ruled by the scoped dynasty | dynasty | <, <=, =, !=, >, >= |
| `num_works_of_art` | Checks if a country has a certain number of works of art | country | <, <=, =, !=, >, >= |
| `object_bias` | Any bias to or from this object set by ambitions. Usage: object_bias = { target = <object> value >= <threshold> } | country | <, <=, =, !=, >, >= |
| `offensive_alliance_strength` | Strength of an offensive alliance, including the nation with all countries giving offensive support and those that can be called in for offensive wars | country | <, <=, =, !=, >, >= |
| `offer_relation_acceptance` | How high is the target country's AI value of accepting the scripted relation offered by the current country scope? Usage: offer_relation_acceptance = … | country | <, <=, =, !=, >, >= |
| `only_allowed_overlord_court_language` | Check if a subject type only allows use of the overlord's court language | subject_type | yes/no |
| `only_allowed_overlord_primary_culture` | Check if a subject type only allows use of the overlord's primary culture | subject_type | yes/no |
| `only_allowed_overlord_primary_or_kindred_culture` | Check if a subject type only allows use of the overlord's primary or kindred culture | subject_type | yes/no |
| `opinion` | is the country's opinion of the target greater or equal than the value? opinion = { target = X [*value >/</>=/<= Y* or *value = { min max }*  } | country | <, <=, =, !=, >, >= |
| `opinion_difference_between` | Get the opinion of the current country scope against the first target country and subtract it with the opinion the current scope has of the second cou… | country | <, <=, =, !=, >, >= |
| `or` | at least one entry inside trigger must be true | none/any |  |
| `organization_strength_relative_to_country` | Gets the relative strength of the scope organization to the supplied country - usage: organization_strength_relative_to_country(<target>\|<bool exclude… | international_organization | <, <=, =, !=, >, >= |
| `original_location_rank` | Check if a location's rank at campaign start matches the supplied location rank | location | location_rank scope → location_rank |
| `original_tag` | if a country is a specific historical tag or was it. | country |  |
| `overlord_can_build_markets` | Check if a subject type allows the overlord to build markets | subject_type | yes/no |
| `overlord_can_destroy_markets` | Check if a subject type allows the overlord to destroy markets | subject_type | yes/no |
| `overlord_can_enforce_peace_on_subject` | Check if a subject type allows an overlord to enforce peace on a subject | subject_type | yes/no |
| `own_entire_area` | Does the country own all locations in area? | country | area scope → area |
| `own_entire_province` | Does the country own all locations in province? | country | province_definition scope → province_definition |
| `owned_by_or_its_subjects` | Checks if the geographic scope is completely owned by the target country or its subjects. | province_definition, area, region, sub_continent, continent | country tag/country scope → country |
| `owns` | Does the country own a specific location? | country | province id/location scope → location |
| `owns_any_foreign_buildings_in` | Does the country own any foreign buildings in the target country? | country | country tag/country scope → country |
| `owns_most_foreign_buildings_in_location` | Does the country own the majority of the foreign buildings in the target location? | country | province id/location scope → location |
| `owns_or_has_subject_in` | country directly owns or has a subject in the geography supplied? | country |  |
| `owns_or_non_sovereign_subject_owns` | Does the country or any of its direct non-sovereign subjects own a specific location? | country | province id/location scope → location |
| `owns_or_non_sovereign_subject_owns_entire_province` | Does the country or non-sovereign subjects own all locations in province? | country | province_definition scope → province_definition |
| `parliament_issue_chance` | The chance an issue will be selected | country, international_organization | <, <=, =, !=, >, >= |
| `parliament_issue_support` | The current support in parliament for an issue | country, international_organization | <, <=, =, !=, >, >= |
| `parliament_issue_will_pass` | Check if the parliament issue of the country / international in debate will pass | country, international_organization | yes/no |
| `parliament_type_enabled_in_international_organization` | Is a parliament type enabled in the scope international organization? | international_organization | parliament_type scope → parliament_type |
| `parliament_type_is_enabled_in` | Is a parliament type enabled in the scope country? | country | parliament_type scope → parliament_type |
| `parliament_type_is_locked_in` | Is a parliament type locked in the scope country? | country | parliament_type scope → parliament_type |
| `parliament_type_is_locked_in_international_organization` | Is a parliament type locked in the scope international organization? | international_organization | parliament_type scope → parliament_type |
| `parliament_type_utility` | Utility of a parliament type that can subtract the utility of current parliament modifiers - usage: parliament_type_utility(<type>\|<bool>) or parliame… | country | <, <=, =, !=, >, >= |
| `parliament_type_visible_in` | Can we see a parliament type in the scope country? | country | parliament_type scope → parliament_type |
| `parliament_type_visible_in_international_organization` | Can we see a parliament type in the scope international organization? | international_organization | parliament_type scope → parliament_type |
| `payment_contribution` | Gets how much the country has to pay for the specified IO and payment type. Usage: payment_contribution = { international_organization = <> payment = … | country | <, <=, =, !=, >, >= |
| `payment_maintenance` | gets the payment maintenance level for a country in an international organization. Usage: payment_maintenance = { international_organization = <> paym… | country | <, <=, =, !=, >, >= |
| `peace_treaty_antagonism` | Get how much antagonism the specified peace treaty type would cause for the current country scope against the target country. Usage: peace_treaty_anta… | country | <, <=, =, !=, >, >= |
| `peace_treaty_war_score_cost` | Get how much war score the specified peace treaty type would cost for the current country scope against the target country. Usage: war_score_cost = { … | country | <, <=, =, !=, >, >= |
| `peasant_enfranchisment` | Checks the level of peasant enfranchisement in a location | location | <, <=, =, !=, >, >= |
| `player_proficiency` | Is player proficiency equal to NOVICE, EXPERIENCED, ADVANCED or EXPERT? | country |  |
| `player_proficiency_greater` | Is player proficiency greater than NOVICE, EXPERIENCED, ADVANCED or EXPERT? | country |  |
| `player_proficiency_greater_eq` | Is player proficiency greater or equal to NOVICE, EXPERIENCED, ADVANCED or EXPERT? | country |  |
| `player_proficiency_less` | Is player proficiency less than NOVICE, EXPERIENCED, ADVANCED or EXPERT? | country |  |
| `player_proficiency_less_eq` | Is player proficiency less or equal to NOVICE, EXPERIENCED, ADVANCED or EXPERT? | country |  |
| `policy_enabled_to_international_organization` | Can we enact a policy in the scope international organization? | international_organization | policy scope → policy |
| `policy_has_ai_join_reason` | Check if the policy has an ai join reason to begin with | policy | yes/no |
| `policy_has_ai_keep_value` | Check if the policy has an ai keep value to begin with | policy | yes/no |
| `policy_has_ai_propose_value` | Check if the policy has an ai proposal value to begin with | policy | yes/no |
| `policy_has_ai_vote_value` | Check if the policy has an ai vote value to begin with | policy | yes/no |
| `policy_is_locked_in_international_organization` | Is a policy locked in the scope international organization? | international_organization | policy scope → policy |
| `policy_level` | Check the defined level of the policy | policy | <, <=, =, !=, >, >= |
| `policy_possible_for_country` | Check the is a policy is possible for target counntry | policy | country tag/country scope → country |
| `policy_utility` | Utility of a policy that can subtract the utility of current policy - usage: policy_utility(<law>\|<policy>\|<bool>) or policy_utility = { law = <law> p… | country | <, <=, =, !=, >, >= |
| `policy_visible_to_international_organization` | Can we see a policy in the scope international organization? | international_organization | policy scope → policy |
| `political_influence` | How much political influence does the country have? | country, international_organization | <, <=, =, !=, >, >= |
| `political_influence_percentage` | How high the percentage of the current political influence compared to the maximum does the country have? | country, international_organization | <, <=, =, !=, >, >= |
| `pop_character_chance` | How likely are characters to spawn from this pop? | pop | <, <=, =, !=, >, >= |
| `pop_knows_about_goods` | Checks if a pop knows about a goods enough to demand it | pop | goods scope → goods |
| `pop_literacy` | How literate is this pop? | pop | <, <=, =, !=, >, >= |
| `pop_satisfaction` | How satisfied is this pop? | pop | <, <=, =, !=, >, >= |
| `pop_size` | How big is this pop? | pop | <, <=, =, !=, >, >= |
| `pop_type_percentage_in_country` | The percentage of population with the specific pop type in the current country | country | <, <=, =, !=, >, >= |
| `pop_type_population_in_country` | The number of population with the specific pop type in the current country | country | <, <=, =, !=, >, >= |
| `population` | Checks if the location/province/province_defintion/area/region/subcontinent/continent has a certain population | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `population_in_area` | gets the amount of population in an area | area | <, <=, =, !=, >, >= |
| `population_with_traits` | Checks if the location has x amount of population with specific pop traits. Usage: population_with_traits = { limit = { <pop triggers> } OR scripted_t… | location | <, <=, =, !=, >, >= → pop |
| `possible_military_leaders` | Checks if a country has a certain amount of possible military leaders | country | <, <=, =, !=, >, >= |
| `potential_army_size` | Gets the total number of armies the country has and could raise | country | <, <=, =, !=, >, >= |
| `potential_navy_size` | Gets the total number of navies the country has and could raise | country | <, <=, =, !=, >, >= |
| `power` | The power of an estate | estate | <, <=, =, !=, >, >= |
| `power_projection` | Checks if a country has a power projection | country | <, <=, =, !=, >, >= |
| `powerful_ally_weight` | Sum of alliance_weight modifiers across all allies (Great Powers = 1.0, Regional Powers = 0.5 by default) | country | <, <=, =, !=, >, >= |
| `prestige` | How much prestige does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `prestige_percentage` | How high the percentage of the current prestige compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `prev_antagonism_towards_this` | Gets the previous scope country's antagonism towards the current scope country | country | <, <=, =, !=, >, >= |
| `prev_favors_with_this` | Gets the previous scope country's favors with the current scope country | country | <, <=, =, !=, >, >= |
| `prev_opinion_of_this` | Gets the previous scope country's opinion of the current scope country | country | <, <=, =, !=, >, >= |
| `prev_spy_network_in_this` | Gets the previous scope country's spy network in the current scope country | country | <, <=, =, !=, >, >= |
| `prev_trust_equilibrium_of_this` | Gets the previous scope country's trust equilibrium of the current scope country | country | <, <=, =, !=, >, >= |
| `prev_trust_of_this` | Gets the previous scope country's trust of the current scope country | country | <, <=, =, !=, >, >= |
| `price_in_market` | Gets the price of the scoped goods in the supplied market price_in_market = { market = X value >= Y } Where X is the name of the market Where Y is a s… | goods | <, <=, =, !=, >, >= |
| `prisoner_strength` | gets the total strength of the prisoners in the unit | unit | <, <=, =, !=, >, >= |
| `privateer_power` | How much power does a privateer has? | privateer | <, <=, =, !=, >, >= |
| `privateer_utility` | How useful is a privateer here? | area | <, <=, =, !=, >, >= |
| `production_method_profit` | Checks production method profit | production_method | <, <=, =, !=, >, >= |
| `proper_culture_nobles` | Checks the proportion of your population that is primary or accepted culture nobles | country | <, <=, =, !=, >, >= |
| `prosperity` | Checks if a location has a certain prosperity | location | <, <=, =, !=, >, >= |
| `province_army_levy_percentage` | Percentage of army levies that can be raised from a province | province | <, <=, =, !=, >, >= |
| `province_army_levy_size` | Total army levies that can be had from a province | province | <, <=, =, !=, >, >= |
| `province_average_control` | Checks the average_control of a province | province | <, <=, =, !=, >, >= |
| `province_average_development` | Checks the average_development of a province | province | <, <=, =, !=, >, >= |
| `province_average_integration` | Checks the average_integration of a province | province | <, <=, =, !=, >, >= |
| `province_average_max_control` | Checks the average maximum control of a province | province | <, <=, =, !=, >, >= |
| `province_average_population` | Checks the average population per location of a province | province | <, <=, =, !=, >, >= |
| `province_cabinet_action` | Checks if a cabinet action role affects a province | cabinet_action | yes/no |
| `province_cultural_unity` | Checks the cultural_unity of a province | province | <, <=, =, !=, >, >= |
| `province_food` | Checks the food of a province | province | <, <=, =, !=, >, >= |
| `province_food_percentage` | Checks the food percentage of capacity in a province | province | <, <=, =, !=, >, >= |
| `province_location_wealth` | Checks if a Province has a certain total location wealth (possible tax base) | province | <, <=, =, !=, >, >= |
| `province_max_food` | Checks the maximum amount of food the province can have | province | <, <=, =, !=, >, >= |
| `province_modifier_strength` | Does the scoped province have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 prov… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `province_monthly_food_production` | Checks how much food the province produces per month | province | <, <=, =, !=, >, >= |
| `province_navy_levy_percentage` | Percentage of naval levies that can be raised from a province | province | <, <=, =, !=, >, >= |
| `province_navy_levy_size` | Total navy levies that can be had from a province | province | <, <=, =, !=, >, >= |
| `province_pop_type_population` | Checks how much of a pop type lives in the scope location. | province | <, <=, =, !=, >, >= |
| `province_population` | Checks if a Province has a certain population | province | <, <=, =, !=, >, >= |
| `province_possible_institutions` | Checks the number of institutions that can be promoted in a province | province | <, <=, =, !=, >, >= |
| `province_prosperity` | Checks if a Province has a certain level of average prosperity | province | <, <=, =, !=, >, >= |
| `province_rebel_progress` | Checks if a Province has a certain rebel progress | province | <, <=, =, !=, >, >= |
| `province_religious_unity` | Checks the religious_unity of a province | province | <, <=, =, !=, >, >= |
| `province_satisfaction` | Checks if a Province has a certain level of average satisfaction | province | <, <=, =, !=, >, >= |
| `province_tax_base` | Checks if a Province has a certain total tax base | province | <, <=, =, !=, >, >= |
| `proximity` | Checks the proximity to owner capital in a location | location | <, <=, =, !=, >, >= |
| `purity` | How much purity does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `purity_percentage` | How high the percentage of the current purity compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `random_integer` | Uniformly random integer between 0 and 2^31-1. It will be the same if evaluated on the same scope and day. | none/any | <, <=, =, !=, >, >= |
| `rank_index` | Checks if a location has a Location Rank of a certain index | location | <, <=, =, !=, >, >= |
| `raw_material_amount` | Check how many locations in the province_defintion/area/region/subcontinent/continent produce the specified raw material. Usage: raw_material_amount =… | market, province_definition, area, region, sub_continent, continent | <, <=, =, !=, >, >= |
| `raw_material_occurrence` | Check how many locations world wide produce this raw material | goods | <, <=, =, !=, >, >= |
| `raw_material_output` | Check how much raw material the scope location produces. | location | <, <=, =, !=, >, >= |
| `rebel_category` | Checks if a rebel is of a certain category | country, rebels |  |
| `rebel_estate_type` | Checks if the rebel is from the specified estate type | rebels | estate_type scope → estate_type |
| `rebel_last_months_progress` | Check last month's progress of a rebel | rebels | <, <=, =, !=, >, >= |
| `rebel_locations` | Get the total amount of locations supporting the rebel | rebels | <, <=, =, !=, >, >= |
| `rebel_modifier_strength` | Does the scoped rebel have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 rebel_m… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `rebel_name_key` | if a rebel has a specific name key | rebels |  |
| `rebel_progress` | Check the progress of a rebel | rebels | <, <=, =, !=, >, >= |
| `rebel_size` | Get the total amount of population supporting the rebel | rebels | <, <=, =, !=, >, >= |
| `receives_defensive_support_from` | Does the scope country receive defensive support from the specified country? | country | country tag/country scope → country |
| `receives_fleet_basing_rights_from` | Does the scope country receive fleet basing rights from the specified country? | country | country tag/country scope → country |
| `receives_food_access_from` | Does the scope country receive food access from the specified country? | country | country tag/country scope → country |
| `receives_isolation_exemption_from` | Does the scope country receive a trade isolation exemption from the specified country? | country | country tag/country scope → country |
| `receives_military_access_from` | Does the scope country receive military access from the specified country? | country | country tag/country scope → country |
| `receives_offensive_support_from` | Does the scope country receive offensive support from the specified country? | country | country tag/country scope → country |
| `receiving_scripted_relation` | Checks for receiving scripted relation. Usage: receiving_scripted_relation = { target = country type = <scripted type> } | country |  |
| `receiving_scripted_relation_of_type` | Checks if that scripted relation is received by the country scope from any other country. | country | relation_type scope → relation_type |
| `reform_desire` | Checks the reform desire of the religion | religion | <, <=, =, !=, >, >= |
| `regular_army_size` | Checks if a country has a certain army size of regulars (maximum strength) | country | <, <=, =, !=, >, >= |
| `regular_navy_size` | Checks if a country has a certain amount of regular ships | country | <, <=, =, !=, >, >= |
| `relative_defensive_alliance_strength` | Gets the relative strength of the scope country including defensive alliances to the supplied one - usage: relative_defensive_alliance_strength(<targe… | country | <, <=, =, !=, >, >= |
| `relative_military_strength` | calculates the relative military strength of the scope country to the target. relative_military_strength = { target = X [*value >/</>=/<= Y* or *value… | country | <, <=, =, !=, >, >= |
| `relative_raw_material_price` | Checks the price of a location's raw material in its market as a percentage of the base price of that material | location | <, <=, =, !=, >, >= |
| `relative_strength` | Gets the relative strength of the scope country to the supplied one - usage: relative_strength(<target>) or relative_strength = { target = <country li… | country | <, <=, =, !=, >, >= |
| `release_only` | Checks if the game is in release mode or not. | none/any | yes/no |
| `relevant_countries` | Do we have any diplomatic action with the target country? | country | country tag/country scope → country |
| `religion_group_percentage` | Gets the percentage of the population that follow a particular religion group in the location/province/province_defintion/area/region/subcontinent/con… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `religion_group_percentage_in_country` | The percentage of a specific religion group in the current country | country | <, <=, =, !=, >, >= |
| `religion_group_population` | Gets the absolute number of the population that follow a particular religion group in the location/province/province_defintion/area/region/subcontinen… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `religion_group_population_in_country` | The number of population a specific religion group in the current country | country | <, <=, =, !=, >, >= |
| `religion_modifier_strength` | Does the scoped religion have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 reli… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `religion_percentage` | Gets the percentage of the population that follow a particular religion in the location/province/province_defintion/area/region/subcontinent/continent… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `religion_percentage_in_country` | The percentage of a specific religion in the current country | country | <, <=, =, !=, >, >= |
| `religion_population` | Gets the absolute number of the population that follow a particular religion in the location/province/province_defintion/area/region/subcontinent/cont… | location, province, province_definition, area, region, sub_continent, continent, scripted_geography | <, <=, =, !=, >, >= |
| `religion_population_in_country` | The number of population with a specific religion in the current country | country | <, <=, =, !=, >, >= |
| `religious_figure_type` | Checks if a character is a specific type of religious figure | character |  |
| `religious_influence` | How much religious influence does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `religious_influence_percentage` | How high the percentage of the current religious influence compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `religious_order_average_holding_prosperity` | The mean prosperity across this religious order's holdings, in the -1 to 1 prosperity range. | religious_order | <, <=, =, !=, >, >= |
| `religious_order_type_foundable_by` | True if the supplied country satisfies this religious order type's default founding-country requirements (e.g. religion). | religious_order_type | country tag/country scope → country |
| `religious_order_zeal` | The religious order's global Zeal value 0-100 (the survival meter). At 0 the order is auto-dissolved. | religious_order | <, <=, =, !=, >, >= |
| `religious_unity` | Checks the fraction of the population sharing the country's religion | country | <, <=, =, !=, >, >= |
| `religious_view` | does the Religion have the specified opinion of the target? | religion |  |
| `religious_view_impact` | Opinion impact of a particular religion on another - usage: religious_view_impact(<religion link>) or religious_view_impact = { culture = <religion li… | religion | <, <=, =, !=, >, >= |
| `relocate_market_utility` | Utility of relocating a market - usage: relocate_market_utility(<location>,<location>) or relocate_market_utility = { location = <location> new_locati… | country | <, <=, =, !=, >, >= |
| `remaining_debt` | Checks the remaining debt of a loan | loan | <, <=, =, !=, >, >= |
| `remaining_parliament_days` | Checks how many days are left in the parliament of the country / international organization before it concludes. Returns -1 when there is no parliamen… | country, international_organization | <, <=, =, !=, >, >= |
| `remove_static_modifier_utility` | Checks the AI utility of removing a static modifier from the scoped object remove_static_modifier_utility = { modifier = X value >= Y } Where X is the… | location, country, character | <, <=, =, !=, >, >= |
| `republican_tradition` | How much republican_tradition does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `republican_tradition_percentage` | How high the percentage of the current republican_tradition compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `request_relation_acceptance` | How high is the target country's AI value of accepting the scripted relation requested by the current country scope? Usage: request_relation_acceptanc… | country | <, <=, =, !=, >, >= |
| `requires_goods` | Returns true if the production method requires the specified trade good. | production_method | goods scope → goods |
| `requires_vote` | Does the law require a vote? | law | yes/no |
| `research_progress` | Checks the progress of the current research in the country | country | <, <=, =, !=, >, >= |
| `resolution_is_active` | Is the resolution currently being debated in the scope international organization/situation? | international_organization, situation | resolution scope → resolution |
| `resolution_opinion` | Gets the current scope country's opinion of a resolution. Usage: resolution_opinion(<international organization>\|<resolution>\|<vote>) or resolution_op… | country | <, <=, =, !=, >, >= |
| `reverse_country_interaction_acceptance` | How high is the current country's AI value of accepting the country interaction done by the specified country scope? Always return 0 if the country in… | country | <, <=, =, !=, >, >= |
| `reverse_cultural_view` | does the target have the specified opinion of the culture? { target = <target culture> value <>= <value> } | culture |  |
| `reverse_offer_relation_acceptance` | How high is the current country's AI value of accepting the scripted relation offered by the specified country scope? Usage: reverse_offer_relation_ac… | country | <, <=, =, !=, >, >= |
| `reverse_religious_view` | does the target have the specified opinion of the Religion? | religion |  |
| `reverse_religious_view_impact` | Reverse opinion impact of a particular religion on another - usage: reverse_religious_view_impact(<religion link>) or reverse_religious_view_impact = … | religion | <, <=, =, !=, >, >= |
| `reverse_request_relation_acceptance` | How high is the current country's AI value of accepting the scripted relation requested by the specified country scope? Usage: reverse_request_relatio… | country | <, <=, =, !=, >, >= |
| `reverse_school_opinion` | does the target have the specified opinion of the school? | religious_school |  |
| `rgo_level` | Checks if a location has a certain rgo level | location | <, <=, =, !=, >, >= |
| `rgo_profit` | Checks a location's raw goods profit per RGO level | location | <, <=, =, !=, >, >= |
| `rgo_workers` | Checks if a location has a certain number of RGO workers | location | <, <=, =, !=, >, >= |
| `righteousness` | How much righteousness does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `righteousness_percentage` | How high the percentage of the current righteousness compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `rite_power` | How much rite power does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `rite_power_percentage` | How high the percentage of the current rite power compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `ruled_country_on_or_after` | Check if the character has ruled the target country on or after the specified date. Usage: ruled_country_on_or_after = { country = <country scope> dat… | character |  |
| `ruler_reign` | Checks if the ruler of a country has ruled for x years | country | <, <=, =, !=, >, >= |
| `ruler_reign_in_days` | Checks if the ruler or regent of a country has ruled for x days | country | <, <=, =, !=, >, >= |
| `ruler_term_start_date` | Gets the start date of the current ruler term | country | <, <=, =, !=, >, >= valid date |
| `sailors` | How much Sailors does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `sailors_percentage` | Checks the percentage of Sailors a country has compared to its maximum | country | <, <=, =, !=, >, >= |
| `satisfaction` | The satisfaction of an estate | estate | <, <=, =, !=, >, >= |
| `save_temporary_scope_as` | Saves a temporary target for use during the trigger execution | none/any |  |
| `save_temporary_scope_value_as` | Saves a numerical or bool value as an arbitrarily-named temporary target to be referenced later in the same effect save_temporary_scope_value_as = { n… | none/any |  |
| `school_opinion` | does the school have the specified opinion of the target? | religious_school |  |
| `scope_type` | Checks the type of the scope object | none/any |  |
| `self_control` | How much self control does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `self_control_percentage` | How high the percentage of the current self control compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `short_term_eco_growth` | Checks the short-term (5-year) economic base growth rate of a country | country | <, <=, =, !=, >, >= |
| `short_term_trigger_currency_utility` | Checks the AI utility of adding an amount of a certain trigger to the scoped object short_term_trigger_currency_utility = { trigger = X size = <size> … | country | <, <=, =, !=, >, >= |
| `should_consider_court_language_change` | AI-only cheap precheck: could the country plausibly benefit from changing its court language? | country | yes/no |
| `shows_stats` | character's adm/dip/mil abilities are shown in the UI | character | yes/no |
| `situation_has_ended` | Check if a situation has ended | situation | yes/no |
| `situation_is_active` | Check if a situation is active | situation | yes/no |
| `slider_minting_value` | How much minting is going on (0..1) | country | <, <=, =, !=, >, >= |
| `societal_value_cabinet_action` | Checks if a cabinet action role has a societal value | cabinet_action | yes/no |
| `societal_value_progress` | Gets progress towards societal value | country | <, <=, =, !=, >, >= |
| `special_status_can_be_bestowed` | Can the supplied special status be bestowed on the supplied country in the scope international organization? | international_organization |  |
| `special_status_power` | Get the political power of the specified country in an organization with that specified special status. Usage: special_status_power = { country = <cou… | international_organization | <, <=, =, !=, >, >= |
| `special_status_power_fraction` | Get the political power fraction of the specified country in an organization with that specified special status. Usage: special_status_power_fraction … | international_organization | <, <=, =, !=, >, >= |
| `spy_network` | How much spy-network does the country have in the target? spy_network = { target = X [*value >/</>=/<= Y* or *value = { min max }*  } | country | <, <=, =, !=, >, >= |
| `stability` | How much Stability does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `stability_percentage` | How high the percentage of the current Stability compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `state_religion_clergy` | Checks the proportion of your population that is true faith clergy | country | <, <=, =, !=, >, >= |
| `strength_percentage` | How many percent strength does this unit have??? | unit | <, <=, =, !=, >, >= |
| `sub_unit_type` | Checks if a sub_unit is of a specific type | sub_unit | unit_type scope → unit_type |
| `subject_can_be_annexed` | Check if a subject type allows an overlord to annex the subject | subject_type | yes/no |
| `subject_can_be_created_by` | Check if a subject type can be created by the supplied country | subject_type |  |
| `subject_level` | Get the level of the subject type. | subject_type | <, <=, =, !=, >, >= |
| `subject_loyalty` | Checks a country's subject loyalty | country | <, <=, =, !=, >, >= |
| `subject_type_annullment_favours_required` | returns the favours needed to annul this relation diplomatically | subject_type | <, <=, =, !=, >, >= |
| `subjects_relative_power` | Compares to relative power of all subjects combined | country | <, <=, =, !=, >, >= |
| `subunit_morale` | How many morale does this subunit have??? | sub_unit | <, <=, =, !=, >, >= |
| `subunit_morale_percentage` | How many percent morale does this subunit have??? | sub_unit | <, <=, =, !=, >, >= |
| `subunit_number` | What is the regimental number for this subnunit | sub_unit | <, <=, =, !=, >, >= |
| `subunit_strength` | How many  strength does this subunit have??? | sub_unit | <, <=, =, !=, >, >= |
| `subunit_strength_percentage` | How many percent strength does this subunit have??? | sub_unit | <, <=, =, !=, >, >= |
| `supports_rebel` | Checks if a country supports the target rebel | country | rebels scope → rebels |
| `switch` | Switch on a trigger for the evaluation of another trigger with an optional fallback trigger. switch = { trigger = simple_assign_trigger case_1 = { <tr… | none/any |  |
| `tag` | if a country is a specific historical tag | country |  |
| `tag_exists` | does a tag exist | none/any |  |
| `target_satisfaction` | The target satisfaction of an estate | estate | <, <=, =, !=, >, >= |
| `this_antagonism_towards_prev` | Gets the current scope country's antagonism towards the previous scope country | country | <, <=, =, !=, >, >= |
| `this_favors_with_prev` | Gets the current scope country's favors with the previous scope country | country | <, <=, =, !=, >, >= |
| `this_opinion_of_prev` | Gets the current scope country's opinion of the previous scope country | country | <, <=, =, !=, >, >= |
| `this_spy_network_in_prev` | Gets the current scope country's spy network in the previous scope country | country | <, <=, =, !=, >, >= |
| `this_trust_equilibrium_of_prev` | Gets the current scope country's trust equilibrium of the previous scope country | country | <, <=, =, !=, >, >= |
| `this_trust_of_prev` | Gets the current scope country's trust of the previous scope country | country | <, <=, =, !=, >, >= |
| `threat_level_to` | Return the threat level the scope country has towards the target country scope. Usage: c = { country = <country scope> value <operator> <value> } or t… | country | <, <=, =, !=, >, >= |
| `time_of_year` | Check if the current date is within the bounds time_of_year = { min = 11.1 # default: beginning of year max = 2.29 # default: end of year } Dates are … | none/any |  |
| `tithe` | Checks the tithe percentage of the religion | religion | <, <=, =, !=, >, >= |
| `topography` | Checks if a location is of a specific Topography type | location |  |
| `topography_count` | Returns the amount of owned locations with the specified topography. Usage: topography_count = { type = <topography scope> value <operator> <value> } … | country | <, <=, =, !=, >, >= |
| `topography_percent` | Returns the percentage of owned locations with the specified topography. Usage: topography_percent = { type = <topography scope> value <operator> <val… | country | <, <=, =, !=, >, >= |
| `total_abilities` | The total ability of the character | character | <, <=, =, !=, >, >= |
| `total_accepted_culture_population` | Checks if a country has an acceputed or primary culture population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_building_levels` | Checks if a location has a certain total amount of building levels | location | <, <=, =, !=, >, >= |
| `total_cardinals` | Checks the total amount of cardinals of the religion | religion | <, <=, =, !=, >, >= |
| `total_control_scaled_population` | Checks if a country has value that is population * local_control its in | country | <, <=, =, !=, >, >= |
| `total_debt` | Checks how much a country has in total debt | country | <, <=, =, !=, >, >= |
| `total_development` | Gets the total amount of development in the country | country | <, <=, =, !=, >, >= |
| `total_discriminated_culture_population` | Checks if a country has a discriminated culture population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_dynastic_power` | Check the total amount of dynastic power the scoped dynasty or country has. In case of country, the dynasty of the ruler or of the heir in case of reg… | country, dynasty | <, <=, =, !=, >, >= |
| `total_effective_goods_production_buildings` | Returns the number of effective building levels which produce the specified good. total_effective_goods_production_buildings = { goods = <goods> value… | country | <, <=, =, !=, >, >= |
| `total_enemies` | counts the number of enemies of an international organization | international_organization | <, <=, =, !=, >, >= |
| `total_foreign_buildings_levels` | Checks the total number of foreign buildings of a country | country | <, <=, =, !=, >, >= |
| `total_goods_traded` | Check the total amount of goods that went through this market last month | market | <, <=, =, !=, >, >= |
| `total_goods_value_traded` | Check the total value of goods that went through this market last month | market | <, <=, =, !=, >, >= |
| `total_heathen_population` | Checks if a country has a heathen population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_heretic_population` | Checks if a country has a heretic population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_locations_owned` | counts the number of locations owned by an international organization | international_organization | <, <=, =, !=, >, >= |
| `total_members` | counts the number of members in an international organization | international_organization | <, <=, =, !=, >, >= |
| `total_merchant_capacity` | Checks if a country has a certain total merchant capacity | country | <, <=, =, !=, >, >= |
| `total_merchant_power` | Check the level of  this Building? | market | <, <=, =, !=, >, >= |
| `total_not_tolerated_culture_population` | Checks if a country has an intolerated culture population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_payment_contribution` | Gets the sum all member countries have to pay for the specified IO and payment type. Usage: total_payment_contribution = { payment = <> } | international_organization | <, <=, =, !=, >, >= |
| `total_population` | Checks if a country has a certain population | country | <, <=, =, !=, >, >= |
| `total_population_in_international_organization` | Checks if the country has the defined amount of pops in the target IO. Usage: total_population_in_international_organization = { international_organiz… | country | <, <=, =, !=, >, >= |
| `total_population_in_international_organization_percentage` | Checks if the country has the defined amount of pops in the target IO. Usage: total_population_in_international_organization_percentage = { internatio… | country | <, <=, =, !=, >, >= |
| `total_primary_culture_population` | Checks if a country has a primary culture population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_special_status_power` | Get the political power of all countries in an organization with that specified special status. Usage: total_special_status_power = { type = <special … | international_organization | <, <=, =, !=, >, >= |
| `total_special_status_power_fraction` | Get the percentage political power of the target special status compared to the total amount of political power of all special statuses combined. Usag… | international_organization | <, <=, =, !=, >, >= |
| `total_tolerated_culture_population` | Checks if a country has a tolerated culture population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_true_faith_population` | Checks if a country has a true faith population size of the specified value | country | <, <=, =, !=, >, >= |
| `total_unique_special_status_power` | Get the political power of all countries in an organization with that specified special status. Usage: total_special_status_power = { type = <special … | international_organization | <, <=, =, !=, >, >= |
| `trade_buy` | What is the current price for the buy of a trade? | trade | <, <=, =, !=, >, >= |
| `trade_capacity` | How much trade capacity is used by this trade? | trade | <, <=, =, !=, >, >= |
| `trade_capacity_usage_percent` | How much of the assigned capacity is being used? (%) | trade | <, <=, =, !=, >, >= |
| `trade_profit` | What is the current profit of a trade? | trade | <, <=, =, !=, >, >= |
| `trade_sell` | What is the current price for the sell of a trade? | trade | <, <=, =, !=, >, >= |
| `trade_volume` | How big volume was traded by this trade? | trade | <, <=, =, !=, >, >= |
| `transport_cost` | Returns the transport cost of the goods scope. | goods | <, <=, =, !=, >, >= |
| `tribal_cohesion` | How much tribal_cohesion does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `tribal_cohesion_percentage` | How high the percentage of the current tribal_cohesion compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `trigger_else` | Evaluates the triggers if the display_triggers of preceding 'trigger_if' or 'trigger_else_if' is not mettrigger_if = { limit = { <display_triggers> } … | none/any |  |
| `trigger_else_if` | Evaluates the enclosed triggers if the display_triggers of the preceding `trigger_if` or `trigger_else_if` is not met and its own display_trigger of t… | none/any |  |
| `trigger_if` | Evaluates the triggers if the display_triggers of the limit are met trigger_if = { limit = { <display_triggers> } <triggers> } | none/any |  |
| `trust` | How much trust does the country have in the target? trust = { target = X [*value >/</>=/<= Y* or *value = { min max }*  } | country | <, <=, =, !=, >, >= |
| `trust_equilibrium` | is the country's trust equilibrium towards the target greater or equal than the value? trust_equilibrium = { target = X [*value >/</>=/<= Y* or *value… | country | <, <=, =, !=, >, >= |
| `tutorial_balance` | Calculates the current monthly balance live. Expensive: only for tutorial lessons, never in AI weights or mass-evaluated triggers | country | <, <=, =, !=, >, >= |
| `unemployed_pops_of_pop_type_in_location` | gets the number of unemployed pops of the pop type in the location | location | <, <=, =, !=, >, >= |
| `unemployed_pops_of_pop_type_in_province` | gets the number of unemployed pops of the pop type in the province | province | <, <=, =, !=, >, >= |
| `unfilled_jobs_in_province` | gets the number of unfilled jobs in the province | province | <, <=, =, !=, >, >= |
| `unfilled_jobs_in_province_percentage` | gets the percentage of unfilled jobs in the province | province | <, <=, =, !=, >, >= |
| `union_length_days` | returns the number of days a country has been in a union with the target country. Usage: union_length_days = { target = <country> value <comparator> <… | country | <, <=, =, !=, >, >= |
| `union_partner_weight` | Sum of union_weight modifiers across all co-members (Great Powers = 1.0, Regional Powers = 0.5 by default) | country | <, <=, =, !=, >, >= |
| `unique_international_organization_type_exists` | Does an international organization of this type exist? | none/any | international_organization_type scope → international_organization_type |
| `unit_category` | Checks if the unit type belongs to a specific unit category | sub_unit, unit_type | class CSubUnitCategoryDataBase key → sub_unit_category |
| `unit_modifier_strength` | Does the scoped unit have a given modifier with the compared strength. Default modifiers without any scale changes have a strength value of 1 unit_mod… | location, country, unit, character, dynasty, religion, province, rebels, mercenary, international_organization | <, <=, =, !=, >, >= |
| `unit_strength` | Check the strength of the unit in scope | unit | <, <=, =, !=, >, >= |
| `upkeep_maintenance` | What is the xx position (0-1) the country has? | country | <, <=, =, !=, >, >= |
| `used_cultures_capacity` | Checks if a country has a certain cost of cultures accepted & tolerated | country | <, <=, =, !=, >, >= |
| `used_diplomatic_capacity` | Diplomatic capacity used by the country | country | <, <=, =, !=, >, >= |
| `used_fort_limit` | How much Fort Limit is currently being used? | country | <, <=, =, !=, >, >= |
| `used_fort_limit_percentage` | What percentage of our Fort Limit is currently being used? | country | <, <=, =, !=, >, >= |
| `used_merchant_capacity` | gets the market used merchant capacity for a country in the scope market | market | <, <=, =, !=, >, >= |
| `uses_elections` | Does this succession law use elections? | heir_selection | yes/no |
| `uses_exterior_background` | character uses the exterior illustration background variant | character | yes/no |
| `uses_government_power` | Checks if a country has a certain government_power (e.g. 'legitimacy') | country |  |
| `valid_estate_for_heir_selection` | Checks if the character's estate is allowed for the target heir selection | character | heir_selection scope → heir_selection |
| `valid_gender_for_heir_selection` | Checks if the character's gender is allowed for the target heir selection | character | heir_selection scope → heir_selection |
| `variable_list_size` | Checks the size of a variable list on the current scope variable_list_size = { name = X target >= Y } Where X is the name of the variable Where Y is a… | none/any |  |
| `variable_map_size` | Checks the size of a variable map variable_map_size = { name = X value >= Y } Where X is the name of the variable map Where Y is a script value or num… | none/any |  |
| `vegetation` | Checks if a location is of a specific Vegetation type | location |  |
| `vegetation_count` | Returns the amount of owned locations with the specified vegetation. Usage: vegetation_count = { type = <vegetation scope> value <operator> <value> } … | country | <, <=, =, !=, >, >= |
| `vegetation_percent` | Returns the percentage of owned locations with the specified vegetation. Usage: vegetation_percent = { type = <vegetation scope> value <operator> <val… | country | <, <=, =, !=, >, >= |
| `vote_impact_in_resolution` | Check how much vote impact the current country scope would make when voting in the target resolution of the target IO. Usage: vote_impact_in_resolutio… | country | <, <=, =, !=, >, >= |
| `vote_is_locked` | Is a country's vote locked in the scope international organization/situation? | international_organization, situation |  |
| `vote_percentage_impact_in_resolution` | Check how much vote percentage impact the current country scope would make when voting in the target resolution of the target IO. Usage: vote_percenta… | country | <, <=, =, !=, >, >= |
| `vote_type` | Checks the type of the vote in a resolution | resolution |  |
| `votes_for_resolution` | Checks the number of votes for a particular outcome of a resolution. usage: votes_for_resolution(<resolution_key>\|<thing>) or votes_for_resolution = {… | international_organization, situation | <, <=, =, !=, >, >= |
| `wants_casus_belli_with` | Does country want a casus belli with another nation? Only for Ai | country | country tag/country scope → country |
| `wants_military_access_in` | country wants military access in this other country? | country | country tag/country scope → country |
| `wants_opinion_with` | Does country want more opinion with another nation? Only for Ai | country | country tag/country scope → country |
| `wants_to_attack` | country wants to attack another country? | country | country tag/country scope → country |
| `wants_to_give_away_any_province` | Country wants to give any province to a subject? | country | yes/no |
| `wants_to_subjugate` | country wants to subjugate another country? | country | country tag/country scope → country |
| `war_casualties` | Amount of casualties taken by a side in this war. war_casualties = { unit_type = army/navy (default: army) side = attacker/defender (default: attacker… | war | <, <=, =, !=, >, >= |
| `war_enthusiasm` | The war enthusiasm of the current country scope in the target war. Usage: war_enthusiasm = { war = <war scope> value = <script value> } or war_enthusi… | country | <, <=, =, !=, >, >= |
| `war_exhaustion` | How much WarExhaustion does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `war_exhaustion_percentage` | How high the percentage of the current WarExhaustion compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `war_goal_type` | Check if the war goal type of the war is the specified type. | war |  |
| `war_length` | Checks how many months the current war has been going. | war | <, <=, =, !=, >, >= |
| `war_length_in_years` | Checks how many years the current war has been going. | war | <, <=, =, !=, >, >= |
| `war_participation` | The real participation score (a percent, 0..1) of the current country scope in the target war. Usage: war_participation = { war = <war scope> value = … | country | <, <=, =, !=, >, >= |
| `war_score_in_war` | Check how much war score the current country has in the target war. Usage: war_score_in_war = { war = <war> value <operatore> <real> } or war_score_in… | country | <, <=, =, !=, >, >= |
| `war_score_in_war_whole_side` | Check how much war score the war side of the current country has in the target war. Usage: war_score_in_war_whole_side = { war = <war> value <operator… | country | <, <=, =, !=, >, >= |
| `war_score_of_country` | Check how much war score the target country has in the current war. Usage: war_score_of_country = { country = <country> value <operatore> <real> } or … | war | <, <=, =, !=, >, >= |
| `war_score_of_country_side` | Check how much war score the war side of the target country has in the current war. Usage: war_score_of_country_side = { country = <country> value <op… | war | <, <=, =, !=, >, >= |
| `war_score_versus` | Gets the war score of the scope country against the supplied one - usage: war_score_versus(<target>) or war_score_versus = { target = <country link> v… | country | <, <=, =, !=, >, >= |
| `war_stalling_length` | Checks how many months with no action have passed in the current war. | war | <, <=, =, !=, >, >= |
| `war_stalling_length_in_years` | Checks how many years with no action have passed in the current war. | war | <, <=, =, !=, >, >= |
| `weighted_calc_true_if` | Returns true if the sum of weights of fulfilled sub-triggers amount to the specified sum weighted_calc_true_if = { amount = 10 5 = { <trigger> } 15 = … | none/any |  |
| `winter_level` | winter level check | location |  |
| `winter_power` |  | location | <, <=, =, !=, >, >= |
| `within_colonial_range_of` | Is the location within Colonial range of the target country? | location, province, area | country tag/country scope → country |
| `within_diplomatic_range` | Is the target country within diplomatic range? | country | country tag/country scope → country |
| `within_naval_range_of` | Is the location within naval range of the target country? | location, province, area | country tag/country scope → country |
| `world_art_quality` | Checks the total art quality in the world | none/any | <, <=, =, !=, >, >= |
| `world_culture_group_percentage` | Gets the percentage of the population that follow a particular culture group in the world - world_culture_group_percentage = { culture_group = <cultur… | none/any | <, <=, =, !=, >, >= |
| `world_culture_group_population` | Gets the absolute number of the population that follow a particular culture group in the world - world_culture_group_population = { culture_group = <c… | none/any | <, <=, =, !=, >, >= |
| `world_culture_percentage` | Gets the percentage of the population that follow a particular culture in the world - world_culture_percentage = { culture = <culture> value ><= <valu… | none/any | <, <=, =, !=, >, >= |
| `world_culture_population` | Gets the absolute number of the population that follow a particular culture in the world - world_culture_population = { culture = <culture> value ><= … | none/any | <, <=, =, !=, >, >= |
| `world_religion_group_percentage` | Gets the percentage of the population that follow a particular religion group in the world - world_religion_group_percentage = { religion_group = <rel… | none/any | <, <=, =, !=, >, >= |
| `world_religion_group_population` | Gets the absolute number of the population that follow a particular religion group in the world - world_religion_group_population = { religion_group =… | none/any | <, <=, =, !=, >, >= |
| `world_religion_percentage` | Gets the percentage of the population that follow a particular religion in the world - world_religion_percentage = { religion = <religion> value ><= <… | none/any | <, <=, =, !=, >, >= |
| `world_religion_population` | Gets the absolute number of the population that follow a particular religion in the world - world_religion_population = { religion = <religion> value … | none/any | <, <=, =, !=, >, >= |
| `would_be_overseas_for` | Check if a location would be overseas for the given country | location | country tag/country scope → country |
| `yanantin` | How much yanantin does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `yanantin_percentage` | How high the percentage of the current yanantin compared to the maximum does the country/IO have? | country, international_organization | <, <=, =, !=, >, >= |
| `yearly_gold` | How much gold does the country get per year? | country | <, <=, =, !=, >, >= |
| `yearly_manpower` | How many Manpower does the country get per year? | country | <, <=, =, !=, >, >= |
| `yearly_sailors` | How many Sailors does the country get per year? | country | <, <=, =, !=, >, >= |
| `yearly_salary` | The yearly salary of the character | character | <, <=, =, !=, >, >= |
| `years_active` | Checks how long a religion has been enabled | religion | <, <=, =, !=, >, >= |
| `years_as_rebel` | Check how many years the character has been a rebel. | character | <, <=, =, !=, >, >= |
| `years_difference` | Returns the difference in years between two dates. Can be used is complex form (scope:first_date\|scope:second_date) or with first = and second = | none/any | <, <=, =, !=, >, >= |
| `years_in_international_organization` | Checks if the country scope has been in the target international organization scope for x years. Usage: years_in_international_organization = { intern… | country | <, <=, =, !=, >, >= |
| `years_of_service_as_admiral` | Check how many years the character has served as an admiral. | character | <, <=, =, !=, >, >= |
| `years_of_service_as_general` | Check how many years the character has served as a general. | character | <, <=, =, !=, >, >= |
| `years_of_service_in_cabinet` | Check how many years the character has served in a cabinet. | character | <, <=, =, !=, >, >= |
| `years_since_disaster_end` | Checks if x years have passed since the end of the disaster. Returns -1 if the disaster has never ended. | disaster | <, <=, =, !=, >, >= |
| `years_since_disaster_start` | Checks if x years have passed since the start of the disaster. Returns -1 if the disaster has never started. | disaster | <, <=, =, !=, >, >= |
| `years_since_game_start` | Compare the number of years elapsed since the game started | none/any | <, <=, =, !=, >, >= |
| `years_since_situation_end` | Checks if x years have passed since the end of the situation. Returns -1 if the situation has never ended. | situation | <, <=, =, !=, >, >= |
| `years_since_situation_start` | Checks if x years have passed since the start of the situation. Returns -1 if the situation has never started. | situation | <, <=, =, !=, >, >= |
