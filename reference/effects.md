# 参考资料：effects 效果速查（script_docs 生成）

> 按效果名排列；"支持 scope"决定该效果能写在哪个作用域里。
> **来源**：游戏内 `script_docs` 生成（`docs\` 日志），**EU5 1.4.5**，2026-10-30。
> **体量**：**检索用清单，不必通读**。

| 名称 | 说明 | 支持 scope | 目标 / 参数 |
|---|---|---|---|
| `abandon_colonial_charter` | Abandons a colonial charter | none/any | → colonial_charter |
| `abandon_location` | Abandons the target location! | country | → location |
| `activate_situation` | activates a situation | none/any | → situation |
| `add_accepted_culture` | Adds an accepted culture to a country | country | → culture |
| `add_access_for_attackers` | adds access in the target country for all attackers in a war | war | → country |
| `add_access_for_defenders` | adds access in the target country for all defenders in a war | war | → country |
| `add_additional_migration` | adds a new migration source to a colonial charter. usage: add_additional_migration = <province> | colonial_charter |  |
| `add_adm` | Adds adm ability | character |  |
| `add_antagonism` | Adds an antagonism modifier, add_antagonism = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `add_area_preference` | Adds an area preference to the AI of a country | country |  |
| `add_army_tradition` | Adds army tradition | country, international_organization |  |
| `add_artist_skill` | gives (or takes) artist_skill to a character | character |  |
| `add_avatar` | adds an avatar to a country | country | → avatar |
| `add_bonus_warscore` | adds some bonus war score to the supplied country. Usage: add_bonus_warscore = { country = country scope amount = script value } | war |  |
| `add_breach` | adds a breach to the current siege | siege |  |
| `add_bureaucracy` | Adds a new bureaucracy of the supplied type to the country's government. | country | → bureaucracy_type |
| `add_casus_belli` | Adds a casus belli for the current country scope against the target country. Usage: casus_belli = { target = <country scope> type = <casus belli type>… | country |  |
| `add_character_modifier` | add a modifier to a character add_character_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = s… | character |  |
| `add_circle_satisfaction` | Add or subtract satisfaction from an imperial circle (clamped 0–100). | imperial_circle |  |
| `add_colonial_claim` | Add a colonial claim to the target province_definition. Usage from country scope: add_colonial_claim = { province_definition = <province definition> r… | country |  |
| `add_cooldown` | adds a cooldown for a country, character or international organization. Usage: add_cooldown = { type = <token> days/weeks/months/years = <integer> } | country, character, international_organization |  |
| `add_core` | makes the location a core of the target country | location | → country |
| `add_country_modifier` | add a modifier to a country add_country_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = strin… | country |  |
| `add_country_to_circle` | Add a country to an imperial circle. | imperial_circle | → country |
| `add_country_to_international_organization` | add a country to an international organization | international_organization | → country |
| `add_country_to_international_organization_no_update` | add a country to an international organization without updating diplo statuses | international_organization | → country |
| `add_cultural_influence` | gives (or takes) cultural_influence to a culture | culture |  |
| `add_cultural_tradition` | gives (or takes) cultural_tradition to a culture | culture |  |
| `add_devotion` | Adds devotion | country, international_organization |  |
| `add_dip` | Adds dip ability | character |  |
| `add_diplomats` | gives (or takes) diplomats for a country | country |  |
| `add_discriminated_culture` | Adds a discriminated culture to a country | country | → culture |
| `add_doom` | Adds doom | country, international_organization |  |
| `add_dynasty_modifier` | add a modifier to a dynasty add_dynasty_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = strin… | dynasty |  |
| `add_enemy_to_international_organization` | add a country to an international organization | international_organization | → country |
| `add_estate_private_regiment` | Adds one unraised private regiment definition to an estate. estate = <type> unit_type = <subunit_def> | country |  |
| `add_estate_satisfaction` | Changes the satisfaction of an estate | country |  |
| `add_extended_winter` | Adds extended winter to target area | none/any | → area |
| `add_favors` | Adds favors (target = x value = y} | country |  |
| `add_fertility` | gives (or takes) fertility to a character | character |  |
| `add_food` | gives (or takes) Food to a unit | unit |  |
| `add_food_percentage` | gives (or takes) Food to a unit as a percentage of their max storage | unit |  |
| `add_god` | adds a god to a country | country | → god |
| `add_gold` | Adds gold | country, international_organization |  |
| `add_gold_to_estate` | Changes the gold of an estate | country |  |
| `add_goods_supply` | Adds goods to a market's stockpile. Usage: add_goods_supply = { goods = <goods> amount = <amount> } | market |  |
| `add_government_power` | Adds government power | country, international_organization |  |
| `add_harmony` | Adds Harmony | country, international_organization |  |
| `add_historical_rival` | adds target country as an historical rival | country | → country |
| `add_honor` | Adds honor | country, international_organization |  |
| `add_horde_unity` | Adds horde_unity | country, international_organization |  |
| `add_inflation` | Adds inflation | country, international_organization |  |
| `add_internal_flag` | adds effect to be read internally (no effect in the gamestate) | none/any |  |
| `add_international_organization` | add an international organization to a religion | religion | → international_organization |
| `add_international_organization_modifier` | add a modifier to an international organization add_international_organization_modifier = { name = name days/months/years=x mode = add/extend/replace/… | international_organization |  |
| `add_karma` | Adds karma | country, international_organization |  |
| `add_legitimacy` | Adds legitimacy | country, international_organization |  |
| `add_liberty_desire` | gives (or takes) liberty desire for a (subject) country | country |  |
| `add_location_as_core` | Adds the target location as core | country | → location |
| `add_location_modifier` | add a modifier to a location add_location_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend  <size = x> <desc = st… | location |  |
| `add_location_to_international_organization` | add a location to an international organization | international_organization | → location |
| `add_manpower` | Adds Manpower | country, international_organization |  |
| `add_merchant_power` | Adds temporary power to a merchant (country = <country> power = value key = localisation months = y } | market |  |
| `add_migration` | sets up a migration { <from = provdef> <from_location = location> <to = provdef> <to_location = location> <culture = x> <religion = y> <type = z>  mon… | none/any |  |
| `add_mil` | Adds mil ability | character |  |
| `add_morale` | gives (or takes) morale to a unit | unit |  |
| `add_movement_modifier` | add a modifier to a movement add_movement_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = str… | movement |  |
| `add_navy_tradition` | Adds navy tradition | country, international_organization |  |
| `add_new_waypoint` | appends a new waypoint location to an active expedition's path. Also accepts a block form, add_new_waypoint = { location = x hidden = yes }, to add a … | expedition | → location |
| `add_omen` | adds an omen to a country | country | → omen |
| `add_omen_god` | adds god to a country to ask for guidance on selecting an omen | country | → god |
| `add_opinion` | Adds an opinion modifier, add_opinion = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `add_policy` | Add a policy to a country. | country | → policy |
| `add_policy_to_international_organization` | add a policy to an international organization | international_organization | → policy |
| `add_policy_wanted_by_estate` | For the country in scope, set the policy wanted by the input estate in the input law | country |  |
| `add_political_influence` | Adds political influence | country, international_organization |  |
| `add_pop` | sets up a pop culture/religion/type/literacy/size possible to set. | location |  |
| `add_pop_satisfaction` | change the Satisfaction of a pop | pop |  |
| `add_pop_size` | change the size of a pop | pop |  |
| `add_prestige` | Adds prestige | country, international_organization |  |
| `add_province_modifier` | add a modifier to a unit add_province_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = string>… | province |  |
| `add_purity` | Adds purity | country, international_organization |  |
| `add_random_trait_from_category` | adds a random trait to a character from a category (admiral, artist, explorer, general, child, ruler) | character |  |
| `add_rebel_modifier` | add a modifier to a rebel add_rebel_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = string> <… | rebels |  |
| `add_rebel_progress` | change the progress of a rebel faction | rebels |  |
| `add_recovered_army_levy_percentage` | adds recovered land levies into the province based on percentage | province |  |
| `add_recovered_navy_levy_percentage` | adds recovered naval levies into the province based on percentage | province |  |
| `add_reform` | adds a government reform. | country | → government_reform |
| `add_reform_desire` | gives (or takes) reform desire for a religion | religion |  |
| `add_religion_modifier` | add a modifier to a unit add_religion_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = string>… | religion |  |
| `add_religious_aspect` | adds a religious_aspect to a country | country | → religious_aspect |
| `add_religious_focus` | Add a completed religious focus to the country scope | country | → religious_focus |
| `add_religious_focus_progress` | Change the progress for the current religious focus to the country scope | country |  |
| `add_religious_influence` | Adds religious influence | country, international_organization |  |
| `add_republican_tradition` | Adds republican_tradition | country, international_organization |  |
| `add_research_progress` | gives (or takes) research progress for a country | country |  |
| `add_righteousness` | Adds Righteousness | country, international_organization |  |
| `add_rite_power` | Adds rite power | country, international_organization |  |
| `add_rival` | adds target country as a rival | country | → country |
| `add_road_to` | adds a road from the scope location to the target location. usage: add_road_to = { target = <target location> type = <road type> } | location |  |
| `add_sailors` | Adds sailors | country, international_organization |  |
| `add_scripted_proximity_modifier_to` | Adds a scripted proximity efficiency modifier from the scope location to the target location. target = <location>, key = <localizable string>, value =… | location |  |
| `add_scripted_proximity_to` | Adds proximity from the scope location to the target location. target = <location>, key = <localizable string>, value = <script value> | location |  |
| `add_self_control` | Adds Self Control | country, international_organization |  |
| `add_spreader` | Adds a character who is spreading the movement. usage: add_spreader = { character = <character> location = <location> } | movement |  |
| `add_spy_network` | Adds spy_network (target = x value = y} | country |  |
| `add_stability` | Adds stability | country, international_organization |  |
| `add_subunit` | Adds a subunit to a Unit | unit | → unit_type |
| `add_subunit_experience` | gives (or takes) experience to a subunit | sub_unit |  |
| `add_subunit_morale` | gives (or takes) morale to a subunit | sub_unit |  |
| `add_subunit_strength` | gives (or takes) Strength to a subunit | sub_unit |  |
| `add_subunit_strength_percentage` | gives (or takes) Strength in % of max to a subunit | sub_unit |  |
| `add_temporary_demand` | Adds a  temporary demand to a market ( type = demand, scale = x, months = months } | market |  |
| `add_timeline_note` | Records a world-global timeline note (key + current date) for later display on the timeline view. add_timeline_note = { key = <string> [location = <lo… | none/any |  |
| `add_to_cabinet` | Add the specified character to the cabinet | cabinet | → character |
| `add_to_global_variable_list` | Adds the event target to a global variable list for the given duration add_to_global_variable_list = { name = X target = Y days/weeks/months/years = Z… | none/any |  |
| `add_to_global_variable_map` | Adds the event target to a variable map for the given duration add_to_variable_map = { name = X key = Y value = Z days/weeks/months/years = T } Where … | none/any |  |
| `add_to_list` | Adds the current scope to an arbitrarily-named list (or creates the list if not already present) to be referenced later in the (unbroken) event chain … | none/any |  |
| `add_to_local_variable_list` | Adds the event target to a local variable list for the given duration add_to_local_variable_list = { name = X target = Y days/weeks/months/years = Z }… | none/any |  |
| `add_to_local_variable_map` | Adds the event target to a variable map for the given duration add_to_variable_map = { name = X key = Y value = Z days/weeks/months/years = T } Where … | none/any |  |
| `add_to_temporary_list` | Adds the current scope to an arbitrarily-named list (or creates the list if not already present) to be referenced later in the same effect add_to_temp… | none/any |  |
| `add_to_variable_list` | Adds the event target to a variable list on the current scope for the given duration add_to_variable_list = { name = X target = Y days/weeks/months/ye… | none/any |  |
| `add_to_variable_map` | Adds the event target to a variable map for the given duration add_to_variable_map = { name = X key = Y value = Z days/weeks/months/years = T } Where … | none/any |  |
| `add_tolerated_culture` | Adds an Tolerated culture to a country | country | → culture |
| `add_trait` | adds a trait to a character | character | → trait |
| `add_tribal_cohesion` | Adds tribal_cohesion | country, international_organization |  |
| `add_truce_with` | Adds a truce with the target country. Usage: add_truce_with = { target = <target country> [days/months/years = x] [mutual = yes] } | country |  |
| `add_trust_equilibrium` | Adds a trust equilibrium modifier, add_trust_equilibrium = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `add_unit_modifier` | add a modifier to a unit add_unit_modifier = { name = name days/months/years=x mode = add/extend/replace/add_and_extend <size = x> <desc = string> <sh… | unit |  |
| `add_vfx` | Adds VFX effects on the map | location |  |
| `add_war_exhaustion` | Adds WarExhaustion | country, international_organization |  |
| `add_yanantin` | Adds yanantin | country, international_organization |  |
| `add_yearly_gold` | Adds a proportion of your yearly gold | country |  |
| `add_yearly_manpower` | Adds a proportion of your yearly Manpower | country |  |
| `add_yearly_sailors` | Adds a proportion of your yearly sailors | country |  |
| `adopt_character` | makes a character adopt another | character | → character |
| `align_societal_values_to` | Aligns the societal values of the current country scope to the ones of the target country. | country | → country |
| `annex_country` | annexes the target country. Usage: annex_country = { country = <country> [reason = <Diplomatic\|CivilWar\|MilitaryConquest>] [transfer = yes] } | country |  |
| `annul_all_treaties_with` | The current country scope annuls all treaties with the target country. | country | → country |
| `apply_rebel_demands` | Executes the concession effect of the target rebel's demand | country | → rebels |
| `assert_if` | Conditionally cause an assert during run time assert_if = { limit = { X } text = Y }, where X is a trigger and Y is an optional string | none/any |  |
| `assert_read` | Conditionally cause an assert during read time assert_read = X, where X is yes or the string to be printed in the assert | none/any |  |
| `attach_to_religious_order` | Attaches the current location to the supplied religious order — the location becomes a holding, joining the order's location list and mirror-registeri… | location | → religious_order |
| `become_hired_by` | mercenaries are hired by the supplied country | mercenary | → country |
| `block_treaties` | Block all possible treaties with the target country | country | → country |
| `break_betrothal` | Breaks the betrothal between character and target character | character | → character |
| `bribe_estate` | The current country scope bribes the estate of the target country using the specified parliament agenda. Usage: bribe_estate = { type = <parliament ag… | country |  |
| `buy_goods_from_market` | A country buys goods from a target market, paying the market buy price in gold and removing the goods from the market stockpile. Usage: buy_goods_from… | country |  |
| `bypass_mission_task` | The country scope bypasses the specified mission task. Does not do anything if the mission task is not part of the currently active mission and if it … | country | → mission_task |
| `cancel_area_exploration` | country stops exploring the target_area | country | → area |
| `cancel_exploration` | cancels the target exploration | none/any | → exploration |
| `cancel_loan` | cancels a loan | none/any | → loan |
| `cancel_subject` | cancel the subject status of the target | country | → country |
| `cancel_trade` | cancels the scope trade | trade |  |
| `change_annexation_progress` | Changes the progress of annexation for the current country scope against the target country. No effect if there is no active annexation going on alrea… | country |  |
| `change_art_quality` | change the quality of a work of art | work_of_art |  |
| `change_art_worth` | Adds delta to the worth override of a work of art. Cannot make a work priceless. Copies type worth first if override is 0. | work_of_art |  |
| `change_assigned_merchant_capacity` | change desired merchant capacity to use on this trade by the supplied amount | trade |  |
| `change_building_level` | change the level of a building | building |  |
| `change_building_level_in_location` | changes building level in location { building = <type> value = <change>  {owner = country} } | location |  |
| `change_building_owner` | assigns a new owner to a building | building | → country |
| `change_casus_belli_creation_progress` | Changes the progress when creating a casus belli against a target country. Usage: change_casus_belli_creation_progress = { target = <country> value <o… | country |  |
| `change_character_allegiance` | change rebel allegiance of Character to target Rebel | character | → rebels |
| `change_character_culture` | changes character's culture | character | → culture |
| `change_character_estate` | changes character's estate | character | → estate_type |
| `change_character_modifier_size` | Change the strength of a modifier applied to the scope character change_character_modifier_size = { modifier = <modifier> value = <script math> [recal… | character |  |
| `change_character_religion` | Changes religion for a character | character | → religion |
| `change_colonial_charter_owner` | change owner of a colonial charter to target country | colonial_charter | → country |
| `change_control` | changes control | location |  |
| `change_country_adjective` | changes the adjective of a country | country |  |
| `change_country_color` | changes the color of a country | country |  |
| `change_country_dynastic_name` | Changes the name, adjective, and flag of a country to the dynasty of the ruler | country |  |
| `change_country_flag` | changes the flag of a country | country |  |
| `change_country_modifier_size` | Change the strength of a modifier applied to the scope country change_country_modifier_size = { modifier = <modifier> value = <script math> [recalcula… | country |  |
| `change_country_name` | changes the name of a country | country |  |
| `change_country_type` | Changes the country type! | country |  |
| `change_creditworthiness` | Changes the creditworthiness of a country by the given amount (positive or negative) | country |  |
| `change_cultural_view` | Changes the cultural view of target.. { target = x change = relation_change } | culture |  |
| `change_culture` | Changes culture for a country | country | → culture |
| `change_development` | changes development | location |  |
| `change_disease_presence` | Adds a value to the presence of a disease in a location or subunit. Disease must already be present there. Usage: change_disease_presence = { disease … | location, sub_unit |  |
| `change_dynasty` | changes dynasty to target dynasty | character | → dynasty |
| `change_dynasty_modifier_size` | Change the strength of a modifier applied to the scope dynasty change_dynasty_modifier_size = { modifier = <modifier> value = <script math> [recalcula… | dynasty |  |
| `change_entrenchment` | Modifies the entrenchment of a bureaucracy by however much | bureaucracy |  |
| `change_exploration_progress` | change the progress of an exploration | exploration |  |
| `change_explorer` | sets a new explorer for an exploration. Usage = change_explorer { area = x character = y } in the current country | country |  |
| `change_father` | changes father to target character | character | → character |
| `change_garrison_size` | Changes the garrison size of the target location by the specified value. | location |  |
| `change_global_variable` | Changes the value of a numeric global variable change_global_variable = { name = X operation = Y } Where X is the name of the numeric variable to modi… | none/any |  |
| `change_government_type` | Changes government-type for a country | country | → government |
| `change_heir_selection` | Changes heir_selection for a country | country | → heir_selection |
| `change_institution_progress` | changes Institution progress in location { institution = <type> value = <change>} | location |  |
| `change_integration_level` | changes the name of a location | location |  |
| `change_integration_progress` | changes the current integration progress | location |  |
| `change_international_organization_modifier_size` | Change the strength of a modifier applied to the scope international organization change_international_organization_modifier_size = { modifier = <modi… | international_organization |  |
| `change_language` | changes the language of a culture | culture | → dialect |
| `change_loan_amount` | changes the amount a loan has | loan |  |
| `change_loan_borrower` | changes the borrower of a loan | loan | → country |
| `change_loan_interest` | changes the interest of a loan | loan |  |
| `change_loan_owner` | changes the owner of a loan | loan | → country |
| `change_local_variable` | Changes the value of a numeric local variable change_local_variable = { name = X operation = Y } Where X is the name of the numeric variable to modify… | none/any |  |
| `change_location_controller` | change controller of Location to target country | location | → country |
| `change_location_modifier_size` | Change the strength of a modifier applied to the scope location change_location_modifier_size = { modifier = <modifier> value = <script math> [recalcu… | location |  |
| `change_location_owner` | change owner of Location to target country | location | → country |
| `change_location_owner_forcefully` | change owner of Location to target country but forcefully | location | → country |
| `change_location_rank` | changes the location_rank of the location (ie, makes it a city, town, rural_settlement etc | location | → location_rank |
| `change_maritime_presence_power` | Add or remove maritime presence power for the specified country in the current location scope. Usage: change_maritime_presence_power = { country = <co… | location |  |
| `change_max_raw_material_workers` | increases rgo size | location |  |
| `change_mother` | changes mother to target character | character | → character |
| `change_parliament_issue_support` | gives (or takes) support for the issue in parliament | country, international_organization |  |
| `change_player` | Changes the player in the current country scope to play another country | country | → country |
| `change_pop_allegiance` | change rebel allegiance of pop to target Rebel | pop | → rebels |
| `change_pop_culture` | Changes culture for a pop | pop | → culture |
| `change_pop_owner` | change owner of pop to target country | pop | → country |
| `change_pop_religion` | Changes religion for a pop | pop | → religion |
| `change_pop_type` | changes pop type of the pop | pop | → pop_type |
| `change_privateer_owner` | changes the owner of a privateer to the target country | privateer | → country |
| `change_privateer_power` | change the power of a privateer | privateer |  |
| `change_prosperity` | changes prosperity | location |  |
| `change_province_food` | changes the province food stockpile | province |  |
| `change_province_food_percentage` | changes the province food stockpile by a percentage of the max | province |  |
| `change_province_integration` | Changes integration_level of a province | province |  |
| `change_province_modifier_size` | Change the strength of a modifier applied to the scope province change_province_modifier_size = { modifier = <modifier> value = <script math> [recalcu… | province |  |
| `change_province_owner` | change owner of Province to target country | province | → country |
| `change_raw_material` | changes the raw-material produced in a location | location | → goods |
| `change_rebel_modifier_size` | Change the strength of a modifier applied to the scope rebel change_rebel_modifier_size = { modifier = <modifier> value = <script math> [recalculate_i… | rebels |  |
| `change_religion` | Changes religion for a country | country | → religion |
| `change_religion_modifier_size` | Change the strength of a modifier applied to the scope religion change_religion_modifier_size = { modifier = <modifier> value = <script math> [recalcu… | religion |  |
| `change_religion_view` | Changes the religion view of target.. { target = x change = relation_change } | religion |  |
| `change_religious_order_calling` | Changes the religious order's calling to the supplied calling (must be valid for the order's type). Per-holding modifiers, projected country modifiers… | religious_order | → calling |
| `change_religious_order_zeal` | Modifies the religious order's global Zeal by the supplied amount (clamped 0..100). At Zeal 0 the order auto-dissolves. | religious_order |  |
| `change_score` | gives (or takes) score a country | country |  |
| `change_siege_progress` | Advances the siege by the given script value | location |  |
| `change_societal_value` | Changes societal value | country |  |
| `change_subject_type` | Change the subject type for the current country scope to the specified subject type | country | → subject_type |
| `change_subunit_type` | changes the sub unit type to a new type. Usage: change_subunit_type = <sub unit definition tag> | sub_unit | → unit_type |
| `change_unit_modifier_size` | Change the strength of a modifier applied to the scope unit change_unit_modifier_size = { modifier = <modifier> value = <script math> [recalculate_imm… | unit |  |
| `change_unit_owner` | change owner of unit to target country | unit | → country |
| `change_variable` | Changes the value of a numeric variable on the current scope change_variable = { name = X operation = Y } Where X is the name of the numeric variable … | none/any |  |
| `clamp_global_variable` | Clamps a global variable the specified max and min clamp_global_variable = { name = X max = Y min = Z } Where X is the name of the variable Where Y an… | none/any |  |
| `clamp_local_variable` | Clamps a local variable the specified max and min clamp_local_variable = { name = X max = Y min = Z } Where X is the name of the variable Where Y and … | none/any |  |
| `clamp_variable` | Clamps a variable the specified max and min on the current scope clamp_variable = { name = X max = Y min = Z } Where X is the name of the variable Whe… | none/any |  |
| `clear_circle_leader` | Clear the leader of an imperial circle. The circle remains but becomes leaderless. | imperial_circle |  |
| `clear_estate_private_regiments` | Clears all unraised private regiment definitions from an estate. estate = <type> | country |  |
| `clear_expedition_waypoints` | Clears all remaining waypoints from an expedition's path. Use add_new_waypoint afterwards to set a new destination. | expedition |  |
| `clear_global_variable_list` | Empties the specified global list clear_global_variable_list = variable_name | none/any |  |
| `clear_global_variable_map` | Empties the map clear_variable_map = variable_name | none/any |  |
| `clear_local_variable_list` | Empties the specified local list clear_local_variable_list = variable_name | none/any |  |
| `clear_local_variable_map` | Empties the map clear_variable_map = variable_name | none/any |  |
| `clear_saved_scope` | Clears a saved scope from the top scope save_scope_as = cool_scope -> clear_saved_scope = cool_scope | none/any |  |
| `clear_variable_list` | Empties the specified list on the current scope clear_variable_list = variable_name | none/any |  |
| `clear_variable_map` | Empties the map clear_variable_map = variable_name | none/any |  |
| `close_all_views` | Closes all views. close_all_views = yes | none/any |  |
| `complete_mission_task` | The country scope completes the specified mission task. Does not do anything if the mission task is not part of the currently active mission and if it… | country | → mission_task |
| `conditional_effect` | An effect which works similar to a simple if effect, but it will always display the effect and conditions for the effect to happen. Also has an else s… | none/any |  |
| `construct_building` | "starts the construction of the specified building in the location. construct_building = { building_type = scoped building type cost_multiplier = valu… | location |  |
| `construct_estate_building` | starts the construction of the specified building by an estate in the location Usage: location scope, construct_estate_building = { building_type = x … | location |  |
| `construct_location_rank` | starts a construction to change the location_rank of the location (ie, makes it a city, town, rural_settlement etc | location | → location_rank |
| `construct_rgo_upgrade` | starts the construction of an RGO upgrade in the location. | location |  |
| `construct_road` | "starts the construction of a road from A to B owned by the scope country. construct_road = { road_type = scoped road type from = location to = locati… | country |  |
| `copy_country_color` | Copy the color of the target country and apply it to the current country scope | none/any | → country |
| `copy_country_flag` | Copy the flag of the target country and apply it to the current country scope | none/any | → country |
| `copy_country_name_and_adjective` | Copy the name and adjective of the target country and apply it to the current country scope | none/any | → country |
| `create_ambition` | Creates an ambition for a country. usage: create_ambition = { ambition_definition = <ambition_definition> [any other optional parameters] } | none/any | → ambition |
| `create_army_country_from_province` | Creates a new army country with the current Province scope as capital and then scopes to the new country | province | → country |
| `create_army_country_from_province_definition` | Creates a new army country with the current Province definition scope as capital and then scopes to the new country | province_definition | → country |
| `create_army_country_in_location` | Creates a new army country in the relevant country and then scopes to the new country | location | → country |
| `create_art` | creates a piece of art in the current location { type = <type> artist = <char> location = <location> quality = <calc> key = name_key (if not, it uses … | location | → work_of_art |
| `create_betrothal` | Betrothes character to target character | character | → character |
| `create_building_country_in_location` | Creates a new building country in the relevant country and then scopes to the new country | location | → country |
| `create_character` | Creates a character for a country. The effect has the following parameters: religion = <religion scope> mother = <character scope> father = <character… | country | → character |
| `create_colonial_charter` | Creates a colonial charter in the target province_definition | country |  |
| `create_country_from_cores_in_our_locations` | creates the country from its cores | country | → country |
| `create_country_from_location` | Creates a new country with the current location scope as capital and then scopes to the new country | location | → country |
| `create_dynasty_from_location` | Creates a new dynasty with the desired name | location |  |
| `create_estate_loan` | scope country takes a specialized estate loan with { amount = gold interest = percent months = months } | country | → loan |
| `create_holy_site` | Creates a holy site | none/any | → holy_site |
| `create_international_organization` | Creates an empty international organization, changing the scope to the created IO. Make sure to use add_country_to_international_organization / set_le… | none/any | → international_organization |
| `create_location_country_from_province` | Creates a new country with the current Province scope as capital and then scopes to the new country | province | → country |
| `create_location_country_from_province_definition` | Creates a new country with the current Province definition scope as capital and then scopes to the new country | province_definition | → country |
| `create_market` | creates a market. Usage: create_market { builder = <country> location = <location> price = <link to price> instant = <yes/no> price_modifier = <script… | none/any |  |
| `create_mercenary` | Creates a mercenary company. Usage:name = <name of the company> borrower = <country that is the customer> cost_multiplier = <cost> home = <home locati… | none/any | → mercenary |
| `create_named_dynasty` | creates a dynasty with the specificed name | country |  |
| `create_navy_country_from_province` | Creates a new navy country with the current Province scope as capital and then scopes to the new country | province | → country |
| `create_navy_country_from_province_definition` | Creates a new navy country with the current Province definition scope as capital and then scopes to the new country | province_definition | → country |
| `create_navy_country_in_location` | Creates a new navy country in the relevant country and then scopes to the new country | location | → country |
| `create_num_sub_unit` | creates # of the specified subunit | location |  |
| `create_num_sub_unit_of_category` | creates # of the best subunit of the specified category | location |  |
| `create_rebel` | Creates a new rebel with name = x and data = { category = x <government = t> <culture = y> <religion = x> <estate = z> } in the current country | country | → rebels |
| `create_relation` | Creates a relation with the target country. create_relation = { type = <scripted relation type key> first = <country> second = <country> [years/months… | none/any |  |
| `create_route` | Finds route between a select start and end location. { start = <location> end = <location> limit = <location trigger> with scope:from provided for pre… | none/any |  |
| `create_sub_unit` | creates the specified subunit | location | → unit_type |
| `create_sub_unit_of_category` | creates the best subunit of the specified category | location | → sub_unit_category |
| `create_sub_unit_with_owner` | spawns a subunit in location { type = <type>  owner = country  origin = location (for generating culture) } | location |  |
| `create_trade` | creates a trade. Usage: in a country scope: create_trade { from = <market> to = <market> merchant = <market where the merchant is> goods = <goods> loc… | country |  |
| `create_union` | Current country scope creates a union with the target scope. Unions get merged if either country has a union already. | country | → country |
| `custom_description` | Wraps effects that get a custom description instead of the auto-generated one custom_description = { text = <effect_localization_key> subject = <optio… | none/any |  |
| `custom_description_no_bullet` | Wraps effects that get a custom description instead of the auto-generated one. Also ensures no bullet point appears custom_description_no_bullet = { t… | none/any |  |
| `custom_label` | just a tooltip, the scope as object (for grouping, localization). custom_label = key; alternatively custom_label = { text = key subject = scope (optio… | none/any |  |
| `custom_tooltip` | just a tooltip, the scope as subject (for grouping, localization). custom_tooltip = key; alternatively custom_tooltip = { text = key subject = scope (… | none/any |  |
| `damage_unit_morale_percent` | Applies the given morale damage to each Cohort in the unit as a percentage of their current morale | unit |  |
| `damage_unit_percent` | Applies the given damage to each Cohort in the unit as a percentage of their current strength | unit |  |
| `debug_log` | Log a string to the debug log when this effect executes, debug_log = message, the message can be a localization string with ROOT, SCOPE and PREV avail… | none/any |  |
| `debug_log_date` | Logs the current date to the debug.log | none/any |  |
| `debug_log_scopes` | Log the current scope to the debug log when this effect executes debug_log_scopes = yes # log full scope info debug_log_scopes = no  # log only curren… | none/any |  |
| `declare_war` | declares a war with the target country. The country scope will leave all wars they fight together with the target country. | country | → country |
| `declare_war_with_cb` | declares a war with a specific cb  { target = <country> type = <cb_type> [target_province = ; target_country =, character =, apply_declaration_costs =… | country | → war |
| `define_unique_country_tag` | defines a unique country tag for a dynamic country | country |  |
| `demote_accepted_culture` | Removes an accepted culture and adds it as tolerated, with a single pop status update | country | → culture |
| `destroy_all_buildings_of_type` | destroys the specified building in the location | location | → building_type |
| `destroy_art` | destroys the target art | none/any | → work_of_art |
| `destroy_building` | Destroys the specified building in the location. Does nothing and is not shown in the tooltip if the target building is indestructible. | location | → building |
| `destroy_building_country` | destroys building based country removing all their buildings and releasing subjects. Usage: kill_building_country = <country> | none/any | → country |
| `destroy_building_forcefully` | destroys the specified building in the location | location | → building |
| `destroy_colonial_charter` | destroys the target colonial_charter | none/any | → colonial_charter |
| `destroy_holy_site` | destroys the target holy site | none/any | → holy_site |
| `destroy_international_organization` | destroys the target international organization. Usage: destroy_international_organization = { target = <io scope> reason = <loc key for reason> } | country |  |
| `destroy_international_organization_no_instigator` | destroys the target international organization. Usage: destroy_international_organization_no_instigator = { target = <io scope> reason = <loc key for … | none/any |  |
| `destroy_market` | destroys the market | market |  |
| `destroy_mercenary` | Mercenary gets routed and scatters | none/any | → mercenary |
| `destroy_movement` | Destroys the unit from the current scope | movement |  |
| `destroy_pop` | destroys a pop | none/any | → pop |
| `destroy_rebel` | Destroys the target rebel | none/any | → rebels |
| `destroy_subunit` | Destroys the unit from the current scope | sub_unit |  |
| `destroy_unit` | Destroys the unit from the current scope | unit |  |
| `discover_area` | country discovers the target_area | country | → area |
| `discover_location` | target country discoves this location | location | → country |
| `dismiss_mercenary` | dismisses a mercenary for the target country | country | → mercenary |
| `dismiss_privateer` | dismisses a privateer for the target country | area | → country |
| `distribute_gold_to_banking_estates` | Distributes gold proportionally to all banking estates in a country | country |  |
| `divorce_character` | Marries character to target character | character | → character |
| `drop_antagonism_bomb` | Drops an antagonism bomb at a location, causing antagonism with surrounding countries depending on how they feel about you and the target. Usage: drop… | country |  |
| `else` | Executes enclosed effects if limit criteria of preceding 'if' or 'else_if' is not met if = { limit = { <triggers> } <effects> } else = { <effects> } | none/any |  |
| `else_if` | Executes enclosed effects if limit criteria of preceding 'if' or 'else_if' is not met, and its own limit is met if = { limit = { <triggers> } <effects… | none/any |  |
| `enable_religion` | enable/disable a religion | religion |  |
| `end_expedition` | Ends an active expedition immediately, firing its on_end effects. Usage: end_expedition = scope:expedition | none/any | → expedition |
| `end_mission` | The country scope ends its currently active mission. If set yes, it will complete it, otherwise it will abort the mission. | country |  |
| `end_parliament` | End the current parliament session. If set 'yes', pass the issue, if set 'no' then fail the issue instead. | country, international_organization |  |
| `end_situation` | End a situation. Usage: end_situation = <situation> or end_situation = { situation = <situation> [outcome = "<outcome name>"] }. If an outcome name is… | none/any | → situation |
| `end_vote` | Sets the final winning vote for a resolution. end_vote = { resolution = <resolution> vote = <vote> } | international_organization, situation |  |
| `error_log` | Log a string to the error log when this effect executes, error_log = message, the message can be a localization string with ROOT, SCOPE and PREV avail… | none/any |  |
| `estate_add_gold` | Changes the gold of an estate | estate |  |
| `every_accepted_culture` | Iterate through all accepted cultures in a country every_accepted_culture = { limit = { <triggers> } <effects> } | country | → culture |
| `every_active_disaster` | Iterate through all active disasters for a country every_active_disaster = { limit = { <triggers> } <effects> } | country | → disaster |
| `every_active_estate` | Iterate through all active estates (non-crown) every_active_estate = { limit = { <triggers> } <effects> } | none/any | → estate_type |
| `every_active_resolution` | Iterate through all currently active resolutions in an international organization or situation every_active_resolution = { limit = { <triggers> } <eff… | international_organization, situation | → active_resolution |
| `every_adjacent_ports_to_area` | Iterate through all adjacent ports of an seazone area every_adjacent_ports_to_area = { limit = { <triggers> } <effects> } | area | → location |
| `every_advance_definition` | Iterate through all advance definitions every_advance_definition = { limit = { <triggers> } <effects> } | none/any | → advance_type |
| `every_allowed_estate_in_heir_selection` | Iterate through all allowed estates a HeirSelection has every_allowed_estate_in_heir_selection = { limit = { <triggers> } <effects> } | heir_selection | → estate_type |
| `every_ancestor` | Iterate through all ancestors (parents, grandparents etc) of a character every_ancestor = { limit = { <triggers> } <effects> } | character | → character |
| `every_area` | Iterate through all existing areas every_area = { limit = { <triggers> } <effects> } | none/any | → area |
| `every_area_in_region` | Iterate through all areas in a region every_area_in_region = { limit = { <triggers> } <effects> } | region | → area |
| `every_area_in_scripted_geography` | Iterate through all areas in a scripted geography every_area_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → area |
| `every_area_with_core` | Iterate through all areas with cored locations in a country every_area_with_core = { limit = { <triggers> } <effects> } | country | → area |
| `every_area_with_owned_province` | Iterate through all areas with owned provinces in a country every_area_with_owned_province = { limit = { <triggers> } <effects> } | country | → area |
| `every_army` | Iterate through all armies in a country every_army = { limit = { <triggers> } <effects> } | country | → unit |
| `every_artist` | Iterate through all artists in a country every_artist = { limit = { <triggers> } <effects> } | country | → character |
| `every_attacker` | Iterate through all attackers of a war every_attacker = { limit = { <triggers> } <effects> } | war | → country |
| `every_available_dynasty_member` | Iterate through adult dynasty members who are not a ruler or heir (cached) every_available_dynasty_member = { limit = { <triggers> } <effects> } | dynasty | → character |
| `every_avatar_for_god` | Iterate through all Avatars of a God every_avatar_for_god = { limit = { <triggers> } <effects> } | god | → avatar |
| `every_besieging_unit` | Iterate through all units participating in a siege every_besieging_unit = { limit = { <triggers> } <effects> } | siege | → unit |
| `every_betrothal` | Iterate through all royal betrothal countries every_betrothal = { limit = { <triggers> } <effects> } | country | → country |
| `every_border_location` | Iterate through all owned location in a country which border locations not owned by the current country scope. every_border_location = { limit = { <tr… | country | → location |
| `every_buildable_building_type` | Iterate through all the building types a country can build every_buildable_building_type = { limit = { <triggers> } <effects> } | country | → building_type |
| `every_building_owned_by_estate` | Iterate through all buildings that an estate has every_building_owned_by_estate = { limit = { <triggers> } <effects> } | estate | → building |
| `every_building_type` | Iterate through all the building types every_building_type = { limit = { <triggers> } <effects> } | none/any | → building_type |
| `every_buildings_in_location` | Iterate through all buildings in a location every_buildings_in_location = { limit = { <triggers> } <effects> } | location | → building |
| `every_cabinet` | Iterate through all actions in a country's cabinet every_cabinet = { limit = { <triggers> } <effects> } | country | → cabinet |
| `every_cabinet_action` | Iterate through all actions in a country's cabinet actions every_cabinet_action = { limit = { <triggers> } <effects> } | country | → cabinet_action |
| `every_cabinet_character` | Iterate through all characters in a country that is in the cabinet every_cabinet_character = { limit = { <triggers> } <effects> } | country | → character |
| `every_calling` | Iterate through all callings every_calling = { limit = { <triggers> } <effects> } | none/any | → calling |
| `every_cardinal_in_country` | Iterate through all Cardinals in a country every_cardinal_in_country = { limit = { <triggers> } <effects> } | country | → character |
| `every_cardinal_in_religion` | Iterate through all Cardinals in a Religion every_cardinal_in_religion = { limit = { <triggers> } <effects> } | religion | → character |
| `every_casus_belli_on_us` | Iterate through all countries have a casus belli on us every_casus_belli_on_us = { limit = { <triggers> } <effects> } | country | → country |
| `every_casus_belli_target` | Iterate through all countries we have a casus belli on every_casus_belli_target = { limit = { <triggers> } <effects> } | country | → country |
| `every_center` | Iterate through all subunits on the center of a combat-side every_center = { limit = { <triggers> } <effects> } | combat_side | → sub_unit |
| `every_character` | Iterate through all characters in a country every_character = { limit = { <triggers> } <effects> } | country | → character |
| `every_character_in_dynasty` | Iterate through all living characters in a Dynasty every_character_in_dynasty = { limit = { <triggers> } <effects> } | dynasty | → character |
| `every_character_in_order` | Iterate through all living member characters of a religious order every_character_in_order = { limit = { <triggers> } <effects> } | religious_order | → character |
| `every_character_supporting_rebel` | Iterate through all characters supporting a rebel every_character_supporting_rebel = { limit = { <triggers> } <effects> } | rebels | → character |
| `every_child` | Iterate through all children of a character every_child = { limit = { <triggers> } <effects> } | character | → character |
| `every_circle_member` | Iterate through all member countries of an Imperial Circle every_circle_member = { limit = { <triggers> } <effects> } | imperial_circle | → country |
| `every_close_relative` | Iterate through all close relatives of a character every_close_relative = { limit = { <triggers> } <effects> } | character | → character |
| `every_coast_border_location` | Iterate through all bordering, or across one seazone of a location every_coast_border_location = { limit = { <triggers> } <effects> } | location | → location |
| `every_colonial_charter` | Iterate through all colonial charters in a country every_colonial_charter = { limit = { <triggers> } <effects> } | country | → colonial_charter |
| `every_colonial_charter_goal` | Iterate through all colonial charter goal types every_colonial_charter_goal = { limit = { <triggers> } <effects> } | none/any | → colonial_charter_goal |
| `every_colonial_claim_province_definition` | Iterate through all province definitions with colonial claims from the scope country. Usage with country = <country> every_colonial_claim_province_def… | country | → province_definition |
| `every_colonial_country` | Iterate through all colonial countries in the world every_colonial_country = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_colonial_overlord` | Iterate through all colonial overlord countries in the world every_colonial_overlord = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_colonial_top_overlord` | Iterate through all countries in the world that have a colonial country among their subjects or their subjects subjects and so on every_colonial_top_o… | none/any | → country |
| `every_connected_location` | Iterate through all locations in the same country as the scope location that are connected by land or strait every_connected_location = { limit = { <t… | location | → location |
| `every_construction_material_for_building_type` | Iterate through all goods required to construct a building type every_construction_material_for_building_type = { limit = { <triggers> } <effects> } | building_type | → goods |
| `every_continent` | Iterate through all existing continents every_continent = { limit = { <triggers> } <effects> } | none/any | → continent |
| `every_continent_in_scripted_geography` | Iterate through all continents in a scripted geography every_continent_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → continent |
| `every_controlled_location` | Iterate through all controlled location in a country every_controlled_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_core_in_location` | Iterate through all cores in a location every_core_in_location = { limit = { <triggers> } <effects> } | location | → country |
| `every_core_location` | Iterate through all core locations in a country every_core_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_country` | Iterate through all existing countries every_country = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_country_annexing_us` | Iterate through all countries which are currently annexing the current country scope. every_country_annexing_us = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_at_war_with` | Iterate through all countries at war with every_country_at_war_with = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_borrowed_from` | Iterate through all countries a country has borrowed from every_country_borrowed_from = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_in_culture` | Iterate through all countries with this primary culture every_country_in_culture = { limit = { <triggers> } <effects> } | culture | → country |
| `every_country_in_culture_group` | Iterate through all countries in a culture group. every_country_in_culture_group = { limit = { <triggers> } <effects> } | culture_group | → country |
| `every_country_in_diplomatic_range` | Iterate through all countries in diplomatic range every_country_in_diplomatic_range = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_in_dynasty` | Iterate through all countries in a Dynasty every_country_in_dynasty = { limit = { <triggers> } <effects> } | dynasty | → country |
| `every_country_in_hierarchy` | Iterate through every country in the entire overlord/subject hierarchy, from the independent top overlord to the deepest subjects every_country_in_hie… | country | → country |
| `every_country_in_religion` | Iterate through all countries in a religion every_country_in_religion = { limit = { <triggers> } <effects> } | religion | → country |
| `every_country_in_religion_group` | Iterate through all countries in a religion group. every_country_in_religion_group = { limit = { <triggers> } <effects> } | group | → country |
| `every_country_in_religious_order` | Iterate through all countries a religious order has a presence in every_country_in_religious_order = { limit = { <triggers> } <effects> } | religious_order | → country |
| `every_country_in_religious_school` | Iterate through all countries within a school every_country_in_religious_school = { limit = { <triggers> } <effects> } | religious_school | → country |
| `every_country_including_inactive` | Iterate through all countries, including inactive every_country_including_inactive = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_country_lent_to` | Iterate through all countries a country has lent to every_country_lent_to = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_of_country_type` | Iterate through all countries of the specified type. Usage: country_of_country_type = { type = <country type> } every_country_of_country_type = { limi… | none/any | → country |
| `every_country_sub_unit` | Iterate through all subunits in all units in a country every_country_sub_unit = { limit = { <triggers> } <effects> } | country | → sub_unit |
| `every_country_supporting_rebel` | Iterate through all countries supporting a rebel every_country_supporting_rebel = { limit = { <triggers> } <effects> } | rebels | → country |
| `every_country_that_can_be_called_defensively` | Iterate through all countries that may be called into a defensive war. every_country_that_can_be_called_defensively = { limit = { <triggers> } <effect… | country | → country |
| `every_country_that_can_be_called_offensively` | Iterate through all countries that may be called into an offensive war. every_country_that_can_be_called_offensively = { limit = { <triggers> } <effec… | country | → country |
| `every_country_together_in_war_with` | Iterate through all countries which are an ally in any of the country scope's wars every_country_together_in_war_with = { limit = { <triggers> } <effe… | country | → country |
| `every_country_wants_casus_belli_with` | all countries a country wants a casus belli with. Only for Ai every_country_wants_casus_belli_with = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_wants_military_access_in` | all countries a country wants military access through every_country_wants_military_access_in = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_wants_to_attack` | all countries a country wants to attack every_country_wants_to_attack = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_wants_to_subjugate` | all countries a country wants to subjugate every_country_wants_to_subjugate = { limit = { <triggers> } <effects> } | country | → country |
| `every_country_we_are_annexing` | Iterate through all countries which are currently annexed by the current country scope. every_country_we_are_annexing = { limit = { <triggers> } <effe… | country | → country |
| `every_country_with_capital_in_geography` | Iterate through all countries which have their capital in the specified geography every_country_with_capital_in_geography = { limit = { <triggers> } <… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `every_country_with_cardinals` | Iterate through all countries with cardinals in a religion every_country_with_cardinals = { limit = { <triggers> } <effects> } | religion | → country |
| `every_country_with_chivalric_order` | Iterate through all countries with an active chivalric order every_country_with_chivalric_order = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_country_with_coalition_grade_antagonism_against_us` | Iterate through all countries who have coalition grade antagonism against us every_country_with_coalition_grade_antagonism_against_us = { limit = { <t… | country | → country |
| `every_country_with_relation_that_can_be_annulled` | Iterate through all countries which have an annullable relation with the scope country. every_country_with_relation_that_can_be_annulled = { limit = {… | country | → country |
| `every_country_with_special_status_of_type` | Iterate through all countries in the international organization which have the specified special status every_country_with_special_status_of_type = { … | international_organization | → country |
| `every_country_with_succession_law` | Iterate through all countries with a cached succession law (set cached = yes in the heir_selection to use this) every_country_with_succession_law = { … | none/any | → country |
| `every_culture` | Iterate through all cultures every_culture = { limit = { <triggers> } <effects> } | none/any | → culture |
| `every_culture_group` | Iterate through all culture groups the culture is in. every_culture_group = { limit = { <triggers> } <effects> } | culture | → culture_group |
| `every_culture_in_culture_group` | Iterate through all cultures in a culture group. every_culture_in_culture_group = { limit = { <triggers> } <effects> } | culture_group | → culture |
| `every_current_avatars` | Iterate through all Avatars a country has every_current_avatars = { limit = { <triggers> } <effects> } | country | → avatar |
| `every_current_bureaucracy` | Iterate through all Bureaucracies a country has every_current_bureaucracy = { limit = { <triggers> } <effects> } | country | → bureaucracy |
| `every_current_bureaucracy_type` | Iterate through all Bureaucracy types a country has every_current_bureaucracy_type = { limit = { <triggers> } <effects> } | country | → bureaucracy_type |
| `every_current_gods` | Iterate through all Gods a country worships every_current_gods = { limit = { <triggers> } <effects> } | country | → god |
| `every_current_law` | Iterate through all laws of a country. every_current_law = { limit = { <triggers> } <effects> } | country | → law |
| `every_current_law_in_international_organization` | Iterate through all laws that are codified in the international organization every_current_law_in_international_organization = { limit = { <triggers> … | international_organization | → law |
| `every_current_policy` | Iterate through all policies that are codified in the country every_current_policy = { limit = { <triggers> } <effects> } | country | → policy |
| `every_current_policy_in_international_organization` | Iterate through all policies that are codified in the international organization every_current_policy_in_international_organization = { limit = { <tri… | international_organization | → policy |
| `every_current_reforms` | Iterate through all Government Reforms a country has every_current_reforms = { limit = { <triggers> } <effects> } | country | → government_reform |
| `every_current_war` | Iterate through all wars of a country every_current_war = { limit = { <triggers> } <effects> } | country | → war |
| `every_defender` | Iterate through all defenders of a war every_defender = { limit = { <triggers> } <effects> } | war | → country |
| `every_descendant` | Iterate through all descendants (children, grandchildren etc) of a character every_descendant = { limit = { <triggers> } <effects> } | character | → character |
| `every_discriminated_culture` | Iterate through all discriminated cultures in a country every_discriminated_culture = { limit = { <triggers> } <effects> } | country | → culture |
| `every_disloyal_subject` | Iterate through all loyal subject countries every_disloyal_subject = { limit = { <triggers> } <effects> } | country | → country |
| `every_dynasty` | Iterate through all dynasties in a country every_dynasty = { limit = { <triggers> } <effects> } | country | → dynasty |
| `every_east_of_province_definition` | Iterate through all province-definitions east of a province-definition every_east_of_province_definition = { limit = { <triggers> } <effects> } | province_definition | → province_definition |
| `every_election_candidates` | Iterate through all election candidates of a country with elections! every_election_candidates = { limit = { <triggers> } <effects> } | country | → character |
| `every_enemy` | Iterate through all Enemy countries every_enemy = { limit = { <triggers> } <effects> } | country | → country |
| `every_enemy_war_leader` | Iterate through all countries which are leading a war against the scope every_enemy_war_leader = { limit = { <triggers> } <effects> } | country | → country |
| `every_estate` | Iterate through all estates in a country every_estate = { limit = { <triggers> } <effects> } | country | → estate |
| `every_estate_privilege` | Iterate through all current estate privileges of a Country every_estate_privilege = { limit = { <triggers> } <effects> } | country | → estate_privilege |
| `every_estate_type_preferring` | Iterate through all estate types that a prefer a policy every_estate_type_preferring = { limit = { <triggers> } <effects> } | policy | → estate_type |
| `every_estate_type_that_dislikes_bureaucracy` | Iterate through all estate types that do NOT prefer a bureaucracy every_estate_type_that_dislikes_bureaucracy = { limit = { <triggers> } <effects> } | bureaucracy_type | → estate_type |
| `every_estate_type_that_likes_bureaucracy` | Iterate through all estate types that a prefer a bureaucracy every_estate_type_that_likes_bureaucracy = { limit = { <triggers> } <effects> } | bureaucracy_type | → estate_type |
| `every_every_rented_out_mercenary` | Iterate through all lent units currently employed by another country, scoped from the lending country every_every_rented_out_mercenary = { limit = { <… | country | → mercenary |
| `every_expedition` | Iterate through all expeditions for a country every_expedition = { limit = { <triggers> } <effects> } | country | → expedition |
| `every_expedition_type` | Iterate through all expedition types every_expedition_type = { limit = { <triggers> } <effects> } | country | → expedition_type |
| `every_exploration_from_country` | Iterate through all Explorations a country has every_exploration_from_country = { limit = { <triggers> } <effects> } | country | → exploration |
| `every_export` | Iterate through all exports in a market every_export = { limit = { <triggers> } <effects> } | market | → trade |
| `every_export_from_location` | Iterate through all exports from location every_export_from_location = { limit = { <triggers> } <effects> } | location | → location |
| `every_food_goods` | Iterate through all food-goods every_food_goods = { limit = { <triggers> } <effects> } | none/any | → goods |
| `every_foreign_building_countries_in_location` | Iterate through all foreign building countries in a location every_foreign_building_countries_in_location = { limit = { <triggers> } <effects> } | location | → country |
| `every_foreign_buildings_in_location` | Iterate through all foreign buildings in a location every_foreign_buildings_in_location = { limit = { <triggers> } <effects> } | location | → building |
| `every_fort_in_country` | Iterate through all Forts in a country every_fort_in_country = { limit = { <triggers> } <effects> } | country | → location |
| `every_friendly_coast_border_location` | Iterate through all friendly bordering, or across one seazone of a location every_friendly_coast_border_location = { limit = { <triggers> } <effects> … | location | → location |
| `every_friendly_country` | Iterate through all countries with relations marked as friendly every_friendly_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_friendly_or_high_opinion_country` | Iterate through all countries with relations marked as friendly or that we have a high opinion of set in defines every_friendly_or_high_opinion_countr… | country | → country |
| `every_friendly_to_friendly_country` | Iterate through all friends of our friends every_friendly_to_friendly_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_friendly_to_hostile_country` | Iterate through all friends of our enemies every_friendly_to_hostile_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_god_in_religion` | Iterate through all Gods in a Religion every_god_in_religion = { limit = { <triggers> } <effects> } | religion | → god |
| `every_good_in_demand` | Iterate through all goods in a goods demand every_good_in_demand = { limit = { <triggers> } <effects> } | demand | → goods |
| `every_goods` | Iterate through all types of goods every_goods = { limit = { <triggers> } <effects> } | none/any | → goods |
| `every_graphical_culture_in_culture` | Iterate through all graphical culture in a culture every_graphical_culture_in_culture = { limit = { <triggers> } <effects> } | culture | → graphical_culture |
| `every_great_power` | Iterate through all great powers every_great_power = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_heathen_location` | Iterate through all heathen locations in a country every_heathen_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_heretic_location` | Iterate through all Heretic locations in a country every_heretic_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_hired_mercenary` | Iterate through mercenaries a country has hired every_hired_mercenary = { limit = { <triggers> } <effects> } | country | → mercenary |
| `every_historical_enemy` | Iterate through all historical Enemy countries every_historical_enemy = { limit = { <triggers> } <effects> } | country | → country |
| `every_historical_rival` | Iterate through all historical rival countries every_historical_rival = { limit = { <triggers> } <effects> } | country | → country |
| `every_holy_site_in_country` | Iterate through all Holy Sites in a country every_holy_site_in_country = { limit = { <triggers> } <effects> } | country | → holy_site |
| `every_holy_site_in_religion` | Iterate through all Holy Sites in a Religion every_holy_site_in_religion = { limit = { <triggers> } <effects> } | religion | → holy_site |
| `every_hostile_country` | Iterate through all countries with relations marked as hostile every_hostile_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_hostile_or_low_opinion_country` | Iterate through all countries with relations marked as hostile or that we have a low opinion of set in defines every_hostile_or_low_opinion_country = … | country | → country |
| `every_hostile_to_friendly_country` | Iterate through all enemies of our friends every_hostile_to_friendly_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_hostile_to_hostile_country` | Iterate through all enemies of our enemies every_hostile_to_hostile_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_imperial_circle` | Iterate through all Imperial Circles in the scope international organization every_imperial_circle = { limit = { <triggers> } <effects> } | international_organization | → imperial_circle |
| `every_imperial_circle_country_is_member_of` | Iterate through all Imperial Circles the scope country is a member of every_imperial_circle_country_is_member_of = { limit = { <triggers> } <effects> … | country | → imperial_circle |
| `every_import` | Iterate through all imports in a market every_import = { limit = { <triggers> } <effects> } | market | → trade |
| `every_import_from_location` | Iterate through all Imports from location every_import_from_location = { limit = { <triggers> } <effects> } | location | → location |
| `every_in_global_list` | Iterate through all items in global list. list = name or variable = name every_in_global_list = { limit = { <triggers> } <effects> } | none/any |  |
| `every_in_list` | Iterate through all items in list. list = name or variable = name every_in_list = { limit = { <triggers> } <effects> } | none/any |  |
| `every_in_local_list` | Iterate through all items in local list. list = name or variable = name every_in_local_list = { limit = { <triggers> } <effects> } | none/any |  |
| `every_institutions_embraced` | Iterate through all institutions a country has embraced every_institutions_embraced = { limit = { <triggers> } <effects> } | country | → institution |
| `every_international_organization` | Iterate through all international organizations every_international_organization = { limit = { <triggers> } <effects> } | none/any | → international_organization |
| `every_international_organization_elector` | Iterate through all countries with an elector special status in the international organization every_international_organization_elector = { limit = { … | international_organization | → country |
| `every_international_organization_enemy` | Iterate through all countries that are enemies of the international organization every_international_organization_enemy = { limit = { <triggers> } <ef… | international_organization | → country |
| `every_international_organization_member` | Iterate through all countries that are members of the international organization every_international_organization_member = { limit = { <triggers> } <e… | international_organization | → country |
| `every_international_organization_owned_location` | Iterate through all locations that are owned by the international organization every_international_organization_owned_location = { limit = { <triggers… | international_organization | → location |
| `every_international_organization_owner` | Iterate through all international organizations which own the location scope every_international_organization_owner = { limit = { <triggers> } <effect… | location | → international_organization |
| `every_international_organization_parliament_opposers` | Iterate through all countries that have voted AGAINST the parliament issue in the in the parliament of the international organization and support the … | international_organization | → country |
| `every_international_organization_parliament_supporter` | Iterate through all countries that have voted FOR the parliament issue in the parliament of the international organization and support the current deb… | international_organization | → country |
| `every_international_organizations_member_of` | Iterate through all international organizations a country is a member of every_international_organizations_member_of = { limit = { <triggers> } <effec… | country | → international_organization |
| `every_international_organizations_target_of` | Iterate through all international organizations a country is a target of every_international_organizations_target_of = { limit = { <triggers> } <effec… | country | → international_organization |
| `every_invited_religious_figure` | Iterate through all invited religious figures in a Country every_invited_religious_figure = { limit = { <triggers> } <effects> } | country | → character |
| `every_key_in_global_variable_map` | Iterate through all keys in a global variable map. key_in_global_variable_map = { variable = name <...> } every_key_in_global_variable_map = { limit =… | none/any |  |
| `every_key_in_local_variable_map` | Iterate through all keys in a local variable map. key_in_local_variable_map = { variable = name <...> } every_key_in_local_variable_map = { limit = { … | none/any |  |
| `every_key_in_variable_map` | Iterate through all keys in a variable map. key_in_variable_map = { variable = name <...> } every_key_in_variable_map = { limit = { <triggers> } <effe… | none/any |  |
| `every_known_country` | Iterate through all known countries every_known_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_known_institution` | Iterate through all institutions a country knows of every_known_institution = { limit = { <triggers> } <effects> } | country | → institution |
| `every_left_flank` | Iterate through all subunits on the left-flank of a combat-side every_left_flank = { limit = { <triggers> } <effects> } | combat_side | → sub_unit |
| `every_lent_loan` | Iterate through all loans that a country lent every_lent_loan = { limit = { <triggers> } <effects> } | country | → loan |
| `every_loan` | Iterate through all loans in a country every_loan = { limit = { <triggers> } <effects> } | country | → loan |
| `every_loan_lent_to_country` | Iterate through all loans a country has lent to the supplied borrower country every_loan_lent_to_country = { limit = { <triggers> } <effects> } | country | → loan |
| `every_location_in_area` | Iterate through all Locations in a area every_location_in_area = { limit = { <triggers> } <effects> } | area | → location |
| `every_location_in_continent` | Iterate through all Locations in a continent every_location_in_continent = { limit = { <triggers> } <effects> } | continent | → location |
| `every_location_in_market` | Iterate through all locations in a market every_location_in_market = { limit = { <triggers> } <effects> } | market | → location |
| `every_location_in_province` | Iterate through all Locations in a province every_location_in_province = { limit = { <triggers> } <effects> } | province | → location |
| `every_location_in_province_definition` | Iterate through all Locations in a province definition every_location_in_province_definition = { limit = { <triggers> } <effects> } | province_definition | → location |
| `every_location_in_region` | Iterate through all Locations in a region every_location_in_region = { limit = { <triggers> } <effects> } | region | → location |
| `every_location_in_religious_order` | Iterate through all locations (holdings) owned by a religious order every_location_in_religious_order = { limit = { <triggers> } <effects> } | religious_order | → location |
| `every_location_in_scripted_geography` | Iterate through all locations in a scripted geography every_location_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → location |
| `every_location_in_sub_continent` | Iterate through all Locations in a sub-continent every_location_in_sub_continent = { limit = { <triggers> } <effects> } | sub_continent | → location |
| `every_location_in_the_world` | Iterate through all location every_location_in_the_world = { limit = { <triggers> } <effects> } | none/any | → location |
| `every_location_with_movement` | Iterate through all locations affected by the scope movement every_location_with_movement = { limit = { <triggers> } <effects> } | movement | → location |
| `every_locations_with_town_rights_in_country` | Iterate through all locations with Town Rights in a country every_locations_with_town_rights_in_country = { limit = { <triggers> } <effects> } | country | → location |
| `every_loyal_subject` | Iterate through all loyal subject countries every_loyal_subject = { limit = { <triggers> } <effects> } | country | → country |
| `every_maritime_area` | Iterate through all maritime areas for a country every_maritime_area = { limit = { <triggers> } <effects> } | country | → area |
| `every_market_center_in_country` | Iterate through all markets in a country which market centers are owned by the country every_market_center_in_country = { limit = { <triggers> } <effe… | country | → market |
| `every_market_in_world` | Iterate through all markets in the world every_market_in_world = { limit = { <triggers> } <effects> } | none/any | → market |
| `every_market_present_in_country` | Iterate through all markets in a country every_market_present_in_country = { limit = { <triggers> } <effects> } | country | → market |
| `every_market_with_merchants` | Iterate through all markets a country has active merchants every_market_with_merchants = { limit = { <triggers> } <effects> } | country | → market |
| `every_mercenary` | Iterate through all mercenaries in the world every_mercenary = { limit = { <triggers> } <effects> } | none/any | → mercenary |
| `every_mercenary_sub_unit` | Iterate through all subunits in a Mercenary every_mercenary_sub_unit = { limit = { <triggers> } <effects> } | mercenary | → sub_unit |
| `every_merchant_in_market` | Iterate through all merchants in a market every_merchant_in_market = { limit = { <triggers> } <effects> } | market | → country |
| `every_movement` | Iterate through all movements every_movement = { limit = { <triggers> } <effects> } | none/any | → movement |
| `every_movement_in_country` | Iterate through all movements in a country every_movement_in_country = { limit = { <triggers> } <effects> } | country | → movement |
| `every_movement_in_culture` | Iterate through all movements in a culture every_movement_in_culture = { limit = { <triggers> } <effects> } | culture | → movement |
| `every_movement_in_religion` | Iterate through all movements in a religion every_movement_in_religion = { limit = { <triggers> } <effects> } | religion | → movement |
| `every_navy` | Iterate through all navies in a country every_navy = { limit = { <triggers> } <effects> } | country | → unit |
| `every_neighbor_area` | Iterate through all neighboring areas in a area every_neighbor_area = { limit = { <triggers> } <effects> } | area | → area |
| `every_neighbor_country` | Iterate through all neighbour countries every_neighbor_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_neighbor_location` | Iterate through all neighbors of a location every_neighbor_location = { limit = { <triggers> } <effects> } | location | → location |
| `every_neighbor_province_definition` | Iterate through all neighboring ProvinceDefinitions in a ProvinceDefinition every_neighbor_province_definition = { limit = { <triggers> } <effects> } | province_definition | → province_definition |
| `every_new_world_goods` | Iterate through all newworld-goods every_new_world_goods = { limit = { <triggers> } <effects> } | none/any | → goods |
| `every_nomad_countries_in_location` | Iterate through all nomad pop countries in a location every_nomad_countries_in_location = { limit = { <triggers> } <effects> } | location | → country |
| `every_non_state_religion_location` | Iterate through all NonStateReligion locations in a country every_non_state_religion_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_old_world_goods` | Iterate through all oldworld-goods every_old_world_goods = { limit = { <triggers> } <effects> } | none/any | → goods |
| `every_omen_in_country` | Iterate through all Omens active in a country every_omen_in_country = { limit = { <triggers> } <effects> } | country | → omen |
| `every_omen_in_god` | Iterate through all Omens associated with a God every_omen_in_god = { limit = { <triggers> } <effects> } | god | → omen |
| `every_omen_in_religion` | Iterate through all Omens in a Religion every_omen_in_religion = { limit = { <triggers> } <effects> } | religion | → omen |
| `every_order_holding_in_country` | Iterate through all owned locations in a country that are holdings of a religious order every_order_holding_in_country = { limit = { <triggers> } <eff… | country | → location |
| `every_other_core_country` | Iterate through all other countries which have a core on the current country every_other_core_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_other_country` | Iterate through all other countries every_other_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_other_great_power` | Iterate through all other great powers every_other_great_power = { limit = { <triggers> } <effects> } | country | → country |
| `every_other_religion_in_same_group` | Iterate through all other religions that has the same group as Religion every_other_religion_in_same_group = { limit = { <triggers> } <effects> } | religion | → religion |
| `every_other_revolutionary` | Iterate through all other revolutionary countries every_other_revolutionary = { limit = { <triggers> } <effects> } | country | → country |
| `every_overlord_or_above` | Iterate through your overlord, your overlord's overlord, and so on every_overlord_or_above = { limit = { <triggers> } <effects> } | country | → country |
| `every_ownable_location` | Iterate through all ownable location every_ownable_location = { limit = { <triggers> } <effects> } | none/any | → location |
| `every_ownable_location_in_area` | Iterate through all ownable Locations in an area every_ownable_location_in_area = { limit = { <triggers> } <effects> } | area | → location |
| `every_ownable_location_in_continent` | Iterate through all ownable Locations in a continent every_ownable_location_in_continent = { limit = { <triggers> } <effects> } | continent | → location |
| `every_ownable_location_in_province_definition` | Iterate through all ownable Locations in a province definition every_ownable_location_in_province_definition = { limit = { <triggers> } <effects> } | province_definition | → location |
| `every_ownable_location_in_region` | Iterate through all ownable Locations in a region every_ownable_location_in_region = { limit = { <triggers> } <effects> } | region | → location |
| `every_ownable_location_in_scripted_geography` | Iterate through all ownable Locations in a scripted geography every_ownable_location_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → location |
| `every_ownable_location_in_sub_continent` | Iterate through all ownable Locations in a sub continent every_ownable_location_in_sub_continent = { limit = { <triggers> } <effects> } | sub_continent | → location |
| `every_owned_building` | Iterate through all the owned buildings in a country every_owned_building = { limit = { <triggers> } <effects> } | country | → building |
| `every_owned_foreign_building` | Iterate through all the owned foreign buildings in a country every_owned_foreign_building = { limit = { <triggers> } <effects> } | country | → building |
| `every_owned_foreign_building_location` | Iterate through all the location of owned foreign buildings in a country every_owned_foreign_building_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_owned_foreign_building_region` | Iterate through all the regions of owned foreign buildings in a country every_owned_foreign_building_region = { limit = { <triggers> } <effects> } | country | → region |
| `every_owned_location` | Iterate through all owned location in a country every_owned_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_owned_nomad_pop` | Iterate through all owned nomad pops in a country every_owned_nomad_pop = { limit = { <triggers> } <effects> } | country | → pop |
| `every_owned_non_rural_location` | Iterate through all owned non-rural locations in a country every_owned_non_rural_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_owned_rural_location` | Iterate through all owned rural locations in a country every_owned_rural_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_owner_in_region` | Iterate through all the countries that own locations in a region every_owner_in_region = { limit = { <triggers> } <effects> } | region | → country |
| `every_parent` | Iterate through parents (order: father, mother) of a character. every_parent = { limit = { <triggers> } <effects> } | character | → character |
| `every_participating_countries` | Iterate through all Countrys participating in 1 side of a combat every_participating_countries = { limit = { <triggers> } <effects> } | combat_side | → country |
| `every_participating_units` | Iterate through all units participating in 1 side of a combat every_participating_units = { limit = { <triggers> } <effects> } | combat_side | → unit |
| `every_past_court_dialect` | Iterate through all court dialects a country has had before every_past_court_dialect = { limit = { <triggers> } <effects> } | country | → country |
| `every_past_liturgical_dialect` | Iterate through all liturgical dialects a country has had before every_past_liturgical_dialect = { limit = { <triggers> } <effects> } | country | → country |
| `every_policy_in_law` | Iterate through all policies that are part of the law scope every_policy_in_law = { limit = { <triggers> } <effects> } | law | → policy |
| `every_political_border_location` | Iterate through all owned location in a country which border another country. every_political_border_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_pop` | Iterate through all pops in a location or country every_pop = { limit = { <triggers> } <effects> } | location, country | → pop |
| `every_pops_supporting_rebel` | Iterate through all pops supporting a rebel every_pops_supporting_rebel = { limit = { <triggers> } <effects> } | rebels | → pop |
| `every_port_in_country` | Iterate through all Ports in a country every_port_in_country = { limit = { <triggers> } <effects> } | country | → location |
| `every_possible_disaster` | Iterate through all possible disasters for a country every_possible_disaster = { limit = { <triggers> } <effects> } | country | → disaster |
| `every_possible_parliament_issue` | Iterate through all possible parliament issues in a country's or an international organization's parliament every_possible_parliament_issue = { limit … | country, international_organization | → parliament_issue |
| `every_possible_policy` | Iterate through all possible policies of a Country that is not currently implemeted every_possible_policy = { limit = { <triggers> } <effects> } | country | → policy |
| `every_possible_privilege` | Iterate through all possible & allowed estate privileges of a Country that is not currently implemeted every_possible_privilege = { limit = { <trigger… | estate | → estate_privilege |
| `every_possible_recruit_location` | Iterate through all possible recruit locations in a country every_possible_recruit_location = { limit = { <triggers> } <effects> } | country | → location |
| `every_potential_expansion_target` | Iterate through all countries which are potential expansion targets for an AI country every_potential_expansion_target = { limit = { <triggers> } <eff… | country | → country |
| `every_present_country` | Iterate through all countries in the specified geography every_present_country = { limit = { <triggers> } <effects> } | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `every_present_culture_in_country` | Iterate through all cultures present in the country. every_present_culture_in_country = { limit = { <triggers> } <effects> } | country | → culture |
| `every_present_culture_in_location` | Iterate through all cultures present in the location. every_present_culture_in_location = { limit = { <triggers> } <effects> } | location | → culture |
| `every_present_overlord` | Iterate through all countries which have a subject in the specified geography every_present_overlord = { limit = { <triggers> } <effects> } | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `every_present_religion_in_country` | Iterate through all religions present in the country. every_present_religion_in_country = { limit = { <triggers> } <effects> } | country | → religion |
| `every_present_religion_in_location` | Iterate through all religions present in the location. every_present_religion_in_location = { limit = { <triggers> } <effects> } | location | → religion |
| `every_primary_or_accepted_culture` | Iterate through primary culture and all accepted cultures in a country. Primary is ordered first. every_primary_or_accepted_culture = { limit = { <tri… | country | → culture |
| `every_primary_or_accepted_or_tolerated_culture` | Iterate through primary culture and all accepted and all tolerated cultures in a country. Primary is ordered first. every_primary_or_accepted_or_toler… | country | → culture |
| `every_privateer` | Iterate through all privateers in the world every_privateer = { limit = { <triggers> } <effects> } | none/any | → privateer |
| `every_privateer_from_country` | Iterate through all privateers a country has every_privateer_from_country = { limit = { <triggers> } <effects> } | country | → privateer |
| `every_privateer_in_area` | Iterate through all privateers in a area every_privateer_in_area = { limit = { <triggers> } <effects> } | area | → privateer |
| `every_production_method` | Iterate through all types of production methods. every_production_method = { limit = { <triggers> } <effects> } | none/any | → production_method |
| `every_production_method_of_building` | Iterate through all available production methods of the building. every_production_method_of_building = { limit = { <triggers> } <effects> } | building | → production_method |
| `every_province` | Iterate through all provinces in a country every_province = { limit = { <triggers> } <effects> } | country | → province |
| `every_province_definition` | Iterate through all existing province_definition every_province_definition = { limit = { <triggers> } <effects> } | none/any | → province_definition |
| `every_province_definition_in_area` | Iterate through all province-definitions in an area every_province_definition_in_area = { limit = { <triggers> } <effects> } | area | → province_definition |
| `every_province_definition_in_scripted_geography` | Iterate through all province_definitions in a scripted geography every_province_definition_in_scripted_geography = { limit = { <triggers> } <effects> … | scripted_geography | → province_definition |
| `every_province_in_area` | Iterate through all provinces in an area every_province_in_area = { limit = { <triggers> } <effects> } | area | → province |
| `every_province_in_province_definition` | Iterate through all provinces in a province-definition every_province_in_province_definition = { limit = { <triggers> } <effects> } | province_definition | → province |
| `every_rebel` | Iterate through all Rebels in a country every_rebel = { limit = { <triggers> } <effects> } | country | → rebels |
| `every_region` | Iterate through all existing regions every_region = { limit = { <triggers> } <effects> } | none/any | → region |
| `every_region_in_continent` | Iterate through all regions in a sub-continent every_region_in_continent = { limit = { <triggers> } <effects> } | sub_continent | → region |
| `every_region_in_scripted_geography` | Iterate through all regions in a scripted geography every_region_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → region |
| `every_related_country` | Iterate through all related countries every_related_country = { limit = { <triggers> } <effects> } | country | → country |
| `every_religion` | Iterate through all religions every_religion = { limit = { <triggers> } <effects> } | none/any | → religion |
| `every_religion_for_god` | Iterate through all Religions of a God every_religion_for_god = { limit = { <triggers> } <effects> } | god | → religion |
| `every_religion_in_religion_group` | Iterate through all religions in a religion group. every_religion_in_religion_group = { limit = { <triggers> } <effects> } | group | → religion |
| `every_religion_international_organization` | Iterate through all international organisations of a religion every_religion_international_organization = { limit = { <triggers> } <effects> } | religion | → international_organization |
| `every_religious_aspect` | Iterate through all religious aspects of a Country every_religious_aspect = { limit = { <triggers> } <effects> } | country | → religious_aspect |
| `every_religious_focus` | Iterate through all completed religious focuses of a Country every_religious_focus = { limit = { <triggers> } <effects> } | country | → religious_focus |
| `every_religious_order` | Iterate through all religious orders every_religious_order = { limit = { <triggers> } <effects> } | none/any | → religious_order |
| `every_religious_order_in_country` | Iterate through all religious orders present in a country every_religious_order_in_country = { limit = { <triggers> } <effects> } | country | → religious_order |
| `every_religious_order_in_location` | Iterate through all religious orders present at a location every_religious_order_in_location = { limit = { <triggers> } <effects> } | location | → religious_order |
| `every_religious_order_in_religion` | Iterate through all religious orders of a religion every_religious_order_in_religion = { limit = { <triggers> } <effects> } | religion | → religious_order |
| `every_religious_order_type` | Iterate through all religious order types every_religious_order_type = { limit = { <triggers> } <effects> } | none/any | → religious_order_type |
| `every_religious_school_in_religion` | Iterate through all Religious Schools in a Religion every_religious_school_in_religion = { limit = { <triggers> } <effects> } | religion | → religious_school |
| `every_rented_out_mercenary` | Iterate through mercenaries a country has rented out to the market every_rented_out_mercenary = { limit = { <triggers> } <effects> } | country | → mercenary |
| `every_required_goods` | Iterate through all goods required by the scope production method. every_required_goods = { limit = { <triggers> } <effects> } | production_method | → goods |
| `every_reserves` | Iterate through all subunits on the reserve of a combat-side every_reserves = { limit = { <triggers> } <effects> } | combat_side | → sub_unit |
| `every_retreated` | Iterate through all subunits on the retreated of a combat-side every_retreated = { limit = { <triggers> } <effects> } | combat_side | → sub_unit |
| `every_revolutionary` | Iterate through all revolutionary states every_revolutionary = { limit = { <triggers> } <effects> } | none/any | → country |
| `every_right_flank` | Iterate through all subunits on the right-flank of a combat-side every_right_flank = { limit = { <triggers> } <effects> } | combat_side | → sub_unit |
| `every_rival` | Iterate through all rival countries every_rival = { limit = { <triggers> } <effects> } | country | → country |
| `every_road_type` | Iterate through all the road types every_road_type = { limit = { <triggers> } <effects> } | none/any | → road_type |
| `every_royal_marriage` | Iterate through all royal married countries every_royal_marriage = { limit = { <triggers> } <effects> } | country | → country |
| `every_ruled_international_organization` | Iterate through IOs a character rules every_ruled_international_organization = { limit = { <triggers> } <effects> } | character | → international_organization |
| `every_ruler` | Iterate through all characters that have ever been rulers in a country, including the dead every_ruler = { limit = { <triggers> } <effects> } | country | → character |
| `every_ruling_countries` | Iterate through countries a character rules every_ruling_countries = { limit = { <triggers> } <effects> } | character | → country |
| `every_sound_toll_in_country` | Iterate through all Sound Tolls in a country every_sound_toll_in_country = { limit = { <triggers> } <effects> } | country | → location |
| `every_spouse` | Iterate through all spouses of a character every_spouse = { limit = { <triggers> } <effects> } | character | → character |
| `every_spy_network_built_in_us` | Iterate through all  countries building spy networks every_spy_network_built_in_us = { limit = { <triggers> } <effects> } | country | → country |
| `every_sub_continent` | Iterate through all existing sub_continents every_sub_continent = { limit = { <triggers> } <effects> } | none/any | → sub_continent |
| `every_sub_continent_in_continent` | Iterate through all sub-continents in a continent every_sub_continent_in_continent = { limit = { <triggers> } <effects> } | continent | → sub_continent |
| `every_sub_continent_in_scripted_geography` | Iterate through all subcontinents in a scripted geography every_sub_continent_in_scripted_geography = { limit = { <triggers> } <effects> } | scripted_geography | → sub_continent |
| `every_sub_unit` | Iterate through all subunits in a unit every_sub_unit = { limit = { <triggers> } <effects> } | unit | → sub_unit |
| `every_subject` | Iterate through all subject countries every_subject = { limit = { <triggers> } <effects> } | country | → country |
| `every_subject_or_below` | Iterate through all subject countries and their subject countries, and so on every_subject_or_below = { limit = { <triggers> } <effects> } | country | → country |
| `every_tolerated_culture` | Iterate through all Tolerated cultures in a country every_tolerated_culture = { limit = { <triggers> } <effects> } | country | → culture |
| `every_town_rights_in_country` | Iterate through all Town Rights in a country every_town_rights_in_country = { limit = { <triggers> } <effects> } | country | → town_rights |
| `every_town_rights_in_location` | Iterate through all Town Rights in a Location every_town_rights_in_Location = { limit = { <triggers> } <effects> } | location | → town_rights |
| `every_trade` | Iterate through all trades in a Country every_trade = { limit = { <triggers> } <effects> } | country | → trade |
| `every_trait` | Iterate through all traits of a character. every_trait = { limit = { <triggers> } <effects> } | character | → trait |
| `every_union_partner` | Iterate through all countries which are in a personal union with the current country scope. every_union_partner = { limit = { <triggers> } <effects> } | country | → country |
| `every_unit` | Iterate through all units in a country every_unit = { limit = { <triggers> } <effects> } | country | → unit |
| `every_unit_in_location` | Iterate through all units in a location every_unit_in_location = { limit = { <triggers> } <effects> } | location | → unit |
| `every_valid_religion_for_aspect` | Iterate through all religion that an aspect can be for every_valid_religion_for_aspect = { limit = { <triggers> } <effects> } | religious_aspect | → religion |
| `every_voter` | Iterate through all voters in an active resolution every_voter = { limit = { <triggers> } <effects> } | active_resolution | → country |
| `every_war` | Iterate through all wars going on globally every_war = { limit = { <triggers> } <effects> } | none/any | → war |
| `every_war_participant` | Iterate through all participants of a war every_war_participant = { limit = { <triggers> } <effects> } | war | → country |
| `every_weather_system_in_location` | Iterate through all weather systems in a location every_weather_system_in_location = { limit = { <triggers> } <effects> } | location | → weather_system |
| `every_west_of_province_definition` | Iterate through all province-definitions west of a province-definition every_west_of_province_definition = { limit = { <triggers> } <effects> } | province_definition | → province_definition |
| `every_work_of_art` | Iterate through all WorkOfArts in the world every_work_of_art = { limit = { <triggers> } <effects> } | none/any | → work_of_art |
| `every_work_of_art_by_creator` | Iterate through all work_of_art by a particular artist every_work_of_art_by_creator = { limit = { <triggers> } <effects> } | character | → work_of_art |
| `every_work_of_art_in_country` | Iterate through all work_of_art in a country every_work_of_art_in_country = { limit = { <triggers> } <effects> } | country | → work_of_art |
| `every_work_of_art_in_location` | Iterate through all work_of_art in a location every_work_of_art_in_location = { limit = { <triggers> } <effects> } | location | → work_of_art |
| `execute_prisoners` | Prisoners get executed | unit |  |
| `execute_propose_effect` | Execute the popose_effect of any implementable object (policies, avatar, cabinet_action, estate_privilege, god, government_reform). Usage: execute_pro… | none/any |  |
| `expedition_return_home` | For a returns_home expedition type: starts the trip back to its origin instead of ending it outright. Usage: expedition_return_home = scope:expedition | none/any | → expedition |
| `extend_regency` | extends the regency by X years | country |  |
| `fail_expedition` | Fails an active expedition immediately, firing its on_fail effects. Non-repeatable expeditions remain available to retry. Usage: fail_expedition = sco… | none/any | → expedition |
| `finalize_resolution` | End the target resolution for the current situation or international organization. | international_organization, situation | → resolution |
| `find_route` | Finds route between start location and closest eligible end location { start = <location> end = <location trigger> limit = <location trigger> with sco… | none/any |  |
| `fire_generic_action` | fires a generic action with the country as the actor and the supplied thing as the recipient | none/any |  |
| `floodfill_locations` | Generates a list of floodfilled locations from adjacencies. Needs a limit specified to avoid floodfilling everything. | location |  |
| `force_city_gfx_rebuild` | Rebuilds the city gfx | none/any | → location |
| `force_recalc_country_active_status` | Recalculates active country status | none/any | → country |
| `force_refresh_culture_and_religion` | Recalculates dominant relgion and culture in a location | none/any | → location |
| `force_union` | Current country scope forces a union with the target scope. In case of a union merge, the country which forces the union keeps the ruler. | country | → country |
| `form_country` | Forms a new country (using formable system) | country | → formable_country |
| `form_new_culture` | Creates a new culture in this country from the primary culture | country |  |
| `found_dynasty` | Makes the character found a new dynasty. If no name is specified, it will be named after the character's birth Location, but the effect still needs to… | character |  |
| `found_religious_order` | Founds a new religious order of the supplied type and calling, belonging to the supplied religion, with the current location as its first holding. Opt… | location |  |
| `garrison_sortie` | "The defender of the siege in scope executes a garrison sortie. This creates a unit with fort garrison and attacks the sieging armies. If the siege de… | siege |  |
| `give_loan` | gives a loan of { target = country amount = gold interest = percent months = months } | country | → loan |
| `grant_culture_bonus_advances` | Grants all bonus advances from a culture's definition to the country grant_culture_bonus_advances = scope:culture | country | → culture |
| `grant_estate_privilege` | grants a special estate priviledge. | country | → estate_privilege |
| `grant_parliament_agenda` | Accept the specified target parliament agenda. | country, international_organization | → parliament_agenda |
| `grant_parliament_agenda_for_estate` | Accept the parliament agenda of the target estate type. | country | → estate_type |
| `grant_parliament_agenda_for_special_status` | Accept the parliament agenda of the target special status type. | international_organization | → special_status |
| `grant_town_rights` | grant town-rights to a location | location | → town_rights_type |
| `hidden_effect` | Enclosed effects are not shown in tooltips hidden_effect = { <more effects> } | none/any |  |
| `hire_mercenary` | hires a mercenary for the target country. Usage: hire_mercenary = <mercenary> or hire_mercenary = { target = <mercenary> location = <location> } to pl… | country | → mercenary |
| `hire_prisoners_as_mercenaries` | Prisoners become mercenaries for the supplied country | unit |  |
| `hire_privateer` | hires a privateer for the target country | area | → country |
| `hre_start_circle_formation_effect` | Start the Imperial Circle formation process for the scope IO. No-op if already active or circles are already locked. | international_organization |  |
| `if` | Executes enclosed effects if limit criteria are met if = { limit = { <triggers> } <effects> } | none/any |  |
| `impregnate` | Impregnates a target character | character | → character |
| `international_organization_add_special_status` | international organization bestows special status on a country | international_organization |  |
| `international_organization_chooses_new_leader` | international organization chooses a new leader | none/any | → international_organization |
| `international_organization_remove_special_status` | international organization removes special status from a country | international_organization |  |
| `io_recalculate_leader` | force an international organization to recalculate its character leader from the current ruler of the leader country | none/any | → international_organization |
| `join_active_chivalric_order_of` | Joins the active chivalric order of the specified country. Does nothing if the country has no active order. | character | → country |
| `join_chivalric_order` | Joins the specified chivalric order. Removes any existing order membership first. | character | → chivalric_order |
| `join_religious_order` | The current character becomes a member of the supplied religious order — added to the order's member roster and keeping the membership on the characte… | character | → religious_order |
| `join_war_against` | joins the target war as an enemy of the target country. Usage: join_war_against = { war = war scope target = country scope call_in_subjects = yes/no i… | country |  |
| `join_war_as_attacker` | joins the target war as an attacker. Usage: join_war_as_attacker = { war = war scope call_in_subjects = yes/no ignore_rules = yes/no } | country |  |
| `join_war_as_defender` | joins the target war as a defender. Usage: join_war_as_defender = { war = war scope call_in_subjects = yes/no ignore_rules = yes/no } | country |  |
| `join_war_with` | joins the target war as an ally of the target country. Usage: join_war_with = { war = war scope target = country scope call_in_subjects = yes/no ignor… | country |  |
| `kill_character` | Kills the target character. Usage: kill_character = <character> or kill_character = { target = <character> [location = <location>] [disease = <disease… | none/any | → character |
| `kill_character_silently` | Kills the target character without message popups. Usage: kill_character = <character> or kill_character = { target = <character> [location = <locatio… | none/any | → character |
| `leave_all_wars_with` | The current country scope will leave every war with or against the target country. | country | → country |
| `leave_chivalric_order` | Removes the character from any chivalric order they are currently a member of. | character |  |
| `leave_war` | leaves a war, forced by another country. Usage: leave_war = { war = <war> actor = <country forcing them to do it> } | country |  |
| `lend_unit_to_country` | Lends the entire unit to a war ally as a condottieri mercenary. Usage: lend_unit_to_country = <country scope> | unit | → country |
| `lift_fog_of_war` | lifts fog of war for a year on the target country | country | → country |
| `lock_maintenance` | Sets the maintenance of a bureaucracy to be locked or not | bureaucracy |  |
| `lock_unit` | Locks a Unit for the specified number of days | unit |  |
| `loot_location` | The unit or country scope will loot the target location. | country, unit | → location |
| `make_saint` | Makes a character a saint | character | → country |
| `make_saint_in_character_religion` | Makes a character a saint in their religion, regardless of the country's religion | character | → country |
| `make_subject_of` | Becomes a subject of the target country (target = x type = y } | country |  |
| `make_unit_available_for_hire` | make the usit available for hire as a mercenary. Usage: make_unit_available_for_hire = { cost_multiplier = <> months = <> } | unit | → mercenary |
| `marry_character` | Marries character to target character | character | → character |
| `marry_character_ignore_blocks` | Marries character to target character, ignoring social restrictions such as estate, modifiers and same-sex ban | character | → character |
| `merge_culture_group` | Merges the primary culture's culture group in this country | country | → culture_group |
| `move_art` | Moves art to a target location | work_of_art | → location |
| `move_country` | Moves character to target country | character | → country |
| `move_prisoners_to_safety` | Prisoners are moved to a safe location | unit |  |
| `move_to_assist_on_adjacent_combat` | Sets unit flag to join adjacent combat automatically. yes/no | unit |  |
| `ordered_accepted_culture` | Iterate through all accepted cultures in a country ordered_accepted_culture = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → culture |
| `ordered_active_disaster` | Iterate through all active disasters for a country ordered_active_disaster = { limit = { <triggers> } order_by = script_value position = int min = int… | country | → disaster |
| `ordered_active_estate` | Iterate through all active estates (non-crown) ordered_active_estate = { limit = { <triggers> } order_by = script_value position = int min = int max =… | none/any | → estate_type |
| `ordered_active_resolution` | Iterate through all currently active resolutions in an international organization or situation ordered_active_resolution = { limit = { <triggers> } or… | international_organization, situation | → active_resolution |
| `ordered_adjacent_ports_to_area` | Iterate through all adjacent ports of an seazone area ordered_adjacent_ports_to_area = { limit = { <triggers> } order_by = script_value position = int… | area | → location |
| `ordered_advance_definition` | Iterate through all advance definitions ordered_advance_definition = { limit = { <triggers> } order_by = script_value position = int min = int max = s… | none/any | → advance_type |
| `ordered_allowed_estate_in_heir_selection` | Iterate through all allowed estates a HeirSelection has ordered_allowed_estate_in_heir_selection = { limit = { <triggers> } order_by = script_value po… | heir_selection | → estate_type |
| `ordered_ancestor` | Iterate through all ancestors (parents, grandparents etc) of a character ordered_ancestor = { limit = { <triggers> } order_by = script_value position … | character | → character |
| `ordered_area` | Iterate through all existing areas ordered_area = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_r… | none/any | → area |
| `ordered_area_in_region` | Iterate through all areas in a region ordered_area_in_region = { limit = { <triggers> } order_by = script_value position = int min = int max = script_… | region | → area |
| `ordered_area_in_scripted_geography` | Iterate through all areas in a scripted geography ordered_area_in_scripted_geography = { limit = { <triggers> } order_by = script_value position = int… | scripted_geography | → area |
| `ordered_area_with_core` | Iterate through all areas with cored locations in a country ordered_area_with_core = { limit = { <triggers> } order_by = script_value position = int m… | country | → area |
| `ordered_area_with_owned_province` | Iterate through all areas with owned provinces in a country ordered_area_with_owned_province = { limit = { <triggers> } order_by = script_value positi… | country | → area |
| `ordered_army` | Iterate through all armies in a country ordered_army = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value ch… | country | → unit |
| `ordered_artist` | Iterate through all artists in a country ordered_artist = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value… | country | → character |
| `ordered_attacker` | Iterate through all attackers of a war ordered_attacker = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value… | war | → country |
| `ordered_available_dynasty_member` | Iterate through adult dynasty members who are not a ruler or heir (cached) ordered_available_dynasty_member = { limit = { <triggers> } order_by = scri… | dynasty | → character |
| `ordered_avatar_for_god` | Iterate through all Avatars of a God ordered_avatar_for_god = { limit = { <triggers> } order_by = script_value position = int min = int max = script_v… | god | → avatar |
| `ordered_besieging_unit` | Iterate through all units participating in a siege ordered_besieging_unit = { limit = { <triggers> } order_by = script_value position = int min = int … | siege | → unit |
| `ordered_betrothal` | Iterate through all royal betrothal countries ordered_betrothal = { limit = { <triggers> } order_by = script_value position = int min = int max = scri… | country | → country |
| `ordered_border_location` | Iterate through all owned location in a country which border locations not owned by the current country scope. ordered_border_location = { limit = { <… | country | → location |
| `ordered_buildable_building_type` | Iterate through all the building types a country can build ordered_buildable_building_type = { limit = { <triggers> } order_by = script_value position… | country | → building_type |
| `ordered_building_owned_by_estate` | Iterate through all buildings that an estate has ordered_building_owned_by_estate = { limit = { <triggers> } order_by = script_value position = int mi… | estate | → building |
| `ordered_building_type` | Iterate through all the building types ordered_building_type = { limit = { <triggers> } order_by = script_value position = int min = int max = script_… | none/any | → building_type |
| `ordered_buildings_in_location` | Iterate through all buildings in a location ordered_buildings_in_location = { limit = { <triggers> } order_by = script_value position = int min = int … | location | → building |
| `ordered_cabinet` | Iterate through all actions in a country's cabinet ordered_cabinet = { limit = { <triggers> } order_by = script_value position = int min = int max = s… | country | → cabinet |
| `ordered_cabinet_action` | Iterate through all actions in a country's cabinet actions ordered_cabinet_action = { limit = { <triggers> } order_by = script_value position = int mi… | country | → cabinet_action |
| `ordered_cabinet_character` | Iterate through all characters in a country that is in the cabinet ordered_cabinet_character = { limit = { <triggers> } order_by = script_value positi… | country | → character |
| `ordered_calling` | Iterate through all callings ordered_calling = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_rang… | none/any | → calling |
| `ordered_cardinal_in_country` | Iterate through all Cardinals in a country ordered_cardinal_in_country = { limit = { <triggers> } order_by = script_value position = int min = int max… | country | → character |
| `ordered_cardinal_in_religion` | Iterate through all Cardinals in a Religion ordered_cardinal_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int m… | religion | → character |
| `ordered_casus_belli_on_us` | Iterate through all countries have a casus belli on us ordered_casus_belli_on_us = { limit = { <triggers> } order_by = script_value position = int min… | country | → country |
| `ordered_casus_belli_target` | Iterate through all countries we have a casus belli on ordered_casus_belli_target = { limit = { <triggers> } order_by = script_value position = int mi… | country | → country |
| `ordered_center` | Iterate through all subunits on the center of a combat-side ordered_center = { limit = { <triggers> } order_by = script_value position = int min = int… | combat_side | → sub_unit |
| `ordered_character` | Iterate through all characters in a country ordered_character = { limit = { <triggers> } order_by = script_value position = int min = int max = script… | country | → character |
| `ordered_character_in_dynasty` | Iterate through all living characters in a Dynasty ordered_character_in_dynasty = { limit = { <triggers> } order_by = script_value position = int min … | dynasty | → character |
| `ordered_character_in_order` | Iterate through all living member characters of a religious order ordered_character_in_order = { limit = { <triggers> } order_by = script_value positi… | religious_order | → character |
| `ordered_character_supporting_rebel` | Iterate through all characters supporting a rebel ordered_character_supporting_rebel = { limit = { <triggers> } order_by = script_value position = int… | rebels | → character |
| `ordered_child` | Iterate through all children of a character ordered_child = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | character | → character |
| `ordered_circle_member` | Iterate through all member countries of an Imperial Circle ordered_circle_member = { limit = { <triggers> } order_by = script_value position = int min… | imperial_circle | → country |
| `ordered_close_relative` | Iterate through all close relatives of a character ordered_close_relative = { limit = { <triggers> } order_by = script_value position = int min = int … | character | → character |
| `ordered_coast_border_location` | Iterate through all bordering, or across one seazone of a location ordered_coast_border_location = { limit = { <triggers> } order_by = script_value po… | location | → location |
| `ordered_colonial_charter` | Iterate through all colonial charters in a country ordered_colonial_charter = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → colonial_charter |
| `ordered_colonial_charter_goal` | Iterate through all colonial charter goal types ordered_colonial_charter_goal = { limit = { <triggers> } order_by = script_value position = int min = … | none/any | → colonial_charter_goal |
| `ordered_colonial_claim_province_definition` | Iterate through all province definitions with colonial claims from the scope country. Usage with country = <country> ordered_colonial_claim_province_d… | country | → province_definition |
| `ordered_colonial_country` | Iterate through all colonial countries in the world ordered_colonial_country = { limit = { <triggers> } order_by = script_value position = int min = i… | none/any | → country |
| `ordered_colonial_overlord` | Iterate through all colonial overlord countries in the world ordered_colonial_overlord = { limit = { <triggers> } order_by = script_value position = i… | none/any | → country |
| `ordered_colonial_top_overlord` | Iterate through all countries in the world that have a colonial country among their subjects or their subjects subjects and so on ordered_colonial_top… | none/any | → country |
| `ordered_connected_location` | Iterate through all locations in the same country as the scope location that are connected by land or strait ordered_connected_location = { limit = { … | location | → location |
| `ordered_construction_material_for_building_type` | Iterate through all goods required to construct a building type ordered_construction_material_for_building_type = { limit = { <triggers> } order_by = … | building_type | → goods |
| `ordered_continent` | Iterate through all existing continents ordered_continent = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | none/any | → continent |
| `ordered_continent_in_scripted_geography` | Iterate through all continents in a scripted geography ordered_continent_in_scripted_geography = { limit = { <triggers> } order_by = script_value posi… | scripted_geography | → continent |
| `ordered_controlled_location` | Iterate through all controlled location in a country ordered_controlled_location = { limit = { <triggers> } order_by = script_value position = int min… | country | → location |
| `ordered_core_in_location` | Iterate through all cores in a location ordered_core_in_location = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | location | → country |
| `ordered_core_location` | Iterate through all core locations in a country ordered_core_location = { limit = { <triggers> } order_by = script_value position = int min = int max … | country | → location |
| `ordered_country` | Iterate through all existing countries ordered_country = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value … | none/any | → country |
| `ordered_country_annexing_us` | Iterate through all countries which are currently annexing the current country scope. ordered_country_annexing_us = { limit = { <triggers> } order_by … | country | → country |
| `ordered_country_at_war_with` | Iterate through all countries at war with ordered_country_at_war_with = { limit = { <triggers> } order_by = script_value position = int min = int max … | country | → country |
| `ordered_country_borrowed_from` | Iterate through all countries a country has borrowed from ordered_country_borrowed_from = { limit = { <triggers> } order_by = script_value position = … | country | → country |
| `ordered_country_in_culture` | Iterate through all countries with this primary culture ordered_country_in_culture = { limit = { <triggers> } order_by = script_value position = int m… | culture | → country |
| `ordered_country_in_culture_group` | Iterate through all countries in a culture group. ordered_country_in_culture_group = { limit = { <triggers> } order_by = script_value position = int m… | culture_group | → country |
| `ordered_country_in_diplomatic_range` | Iterate through all countries in diplomatic range ordered_country_in_diplomatic_range = { limit = { <triggers> } order_by = script_value position = in… | country | → country |
| `ordered_country_in_dynasty` | Iterate through all countries in a Dynasty ordered_country_in_dynasty = { limit = { <triggers> } order_by = script_value position = int min = int max … | dynasty | → country |
| `ordered_country_in_hierarchy` | Iterate through every country in the entire overlord/subject hierarchy, from the independent top overlord to the deepest subjects ordered_country_in_h… | country | → country |
| `ordered_country_in_religion` | Iterate through all countries in a religion ordered_country_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int ma… | religion | → country |
| `ordered_country_in_religion_group` | Iterate through all countries in a religion group. ordered_country_in_religion_group = { limit = { <triggers> } order_by = script_value position = int… | group | → country |
| `ordered_country_in_religious_order` | Iterate through all countries a religious order has a presence in ordered_country_in_religious_order = { limit = { <triggers> } order_by = script_valu… | religious_order | → country |
| `ordered_country_in_religious_school` | Iterate through all countries within a school ordered_country_in_religious_school = { limit = { <triggers> } order_by = script_value position = int mi… | religious_school | → country |
| `ordered_country_including_inactive` | Iterate through all countries, including inactive ordered_country_including_inactive = { limit = { <triggers> } order_by = script_value position = int… | none/any | → country |
| `ordered_country_lent_to` | Iterate through all countries a country has lent to ordered_country_lent_to = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → country |
| `ordered_country_of_country_type` | Iterate through all countries of the specified type. Usage: country_of_country_type = { type = <country type> } ordered_country_of_country_type = { li… | none/any | → country |
| `ordered_country_sub_unit` | Iterate through all subunits in all units in a country ordered_country_sub_unit = { limit = { <triggers> } order_by = script_value position = int min … | country | → sub_unit |
| `ordered_country_supporting_rebel` | Iterate through all countries supporting a rebel ordered_country_supporting_rebel = { limit = { <triggers> } order_by = script_value position = int mi… | rebels | → country |
| `ordered_country_that_can_be_called_defensively` | Iterate through all countries that may be called into a defensive war. ordered_country_that_can_be_called_defensively = { limit = { <triggers> } order… | country | → country |
| `ordered_country_that_can_be_called_offensively` | Iterate through all countries that may be called into an offensive war. ordered_country_that_can_be_called_offensively = { limit = { <triggers> } orde… | country | → country |
| `ordered_country_together_in_war_with` | Iterate through all countries which are an ally in any of the country scope's wars ordered_country_together_in_war_with = { limit = { <triggers> } ord… | country | → country |
| `ordered_country_wants_casus_belli_with` | all countries a country wants a casus belli with. Only for Ai ordered_country_wants_casus_belli_with = { limit = { <triggers> } order_by = script_valu… | country | → country |
| `ordered_country_wants_military_access_in` | all countries a country wants military access through ordered_country_wants_military_access_in = { limit = { <triggers> } order_by = script_value posi… | country | → country |
| `ordered_country_wants_to_attack` | all countries a country wants to attack ordered_country_wants_to_attack = { limit = { <triggers> } order_by = script_value position = int min = int ma… | country | → country |
| `ordered_country_wants_to_subjugate` | all countries a country wants to subjugate ordered_country_wants_to_subjugate = { limit = { <triggers> } order_by = script_value position = int min = … | country | → country |
| `ordered_country_we_are_annexing` | Iterate through all countries which are currently annexed by the current country scope. ordered_country_we_are_annexing = { limit = { <triggers> } ord… | country | → country |
| `ordered_country_with_capital_in_geography` | Iterate through all countries which have their capital in the specified geography ordered_country_with_capital_in_geography = { limit = { <triggers> }… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `ordered_country_with_cardinals` | Iterate through all countries with cardinals in a religion ordered_country_with_cardinals = { limit = { <triggers> } order_by = script_value position … | religion | → country |
| `ordered_country_with_chivalric_order` | Iterate through all countries with an active chivalric order ordered_country_with_chivalric_order = { limit = { <triggers> } order_by = script_value p… | none/any | → country |
| `ordered_country_with_coalition_grade_antagonism_against_us` | Iterate through all countries who have coalition grade antagonism against us ordered_country_with_coalition_grade_antagonism_against_us = { limit = { … | country | → country |
| `ordered_country_with_relation_that_can_be_annulled` | Iterate through all countries which have an annullable relation with the scope country. ordered_country_with_relation_that_can_be_annulled = { limit =… | country | → country |
| `ordered_country_with_special_status_of_type` | Iterate through all countries in the international organization which have the specified special status ordered_country_with_special_status_of_type = … | international_organization | → country |
| `ordered_country_with_succession_law` | Iterate through all countries with a cached succession law (set cached = yes in the heir_selection to use this) ordered_country_with_succession_law = … | none/any | → country |
| `ordered_culture` | Iterate through all cultures ordered_culture = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_rang… | none/any | → culture |
| `ordered_culture_group` | Iterate through all culture groups the culture is in. ordered_culture_group = { limit = { <triggers> } order_by = script_value position = int min = in… | culture | → culture_group |
| `ordered_culture_in_culture_group` | Iterate through all cultures in a culture group. ordered_culture_in_culture_group = { limit = { <triggers> } order_by = script_value position = int mi… | culture_group | → culture |
| `ordered_current_avatars` | Iterate through all Avatars a country has ordered_current_avatars = { limit = { <triggers> } order_by = script_value position = int min = int max = sc… | country | → avatar |
| `ordered_current_bureaucracy` | Iterate through all Bureaucracies a country has ordered_current_bureaucracy = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → bureaucracy |
| `ordered_current_bureaucracy_type` | Iterate through all Bureaucracy types a country has ordered_current_bureaucracy_type = { limit = { <triggers> } order_by = script_value position = int… | country | → bureaucracy_type |
| `ordered_current_gods` | Iterate through all Gods a country worships ordered_current_gods = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | country | → god |
| `ordered_current_law` | Iterate through all laws of a country. ordered_current_law = { limit = { <triggers> } order_by = script_value position = int min = int max = script_va… | country | → law |
| `ordered_current_law_in_international_organization` | Iterate through all laws that are codified in the international organization ordered_current_law_in_international_organization = { limit = { <triggers… | international_organization | → law |
| `ordered_current_policy` | Iterate through all policies that are codified in the country ordered_current_policy = { limit = { <triggers> } order_by = script_value position = int… | country | → policy |
| `ordered_current_policy_in_international_organization` | Iterate through all policies that are codified in the international organization ordered_current_policy_in_international_organization = { limit = { <t… | international_organization | → policy |
| `ordered_current_reforms` | Iterate through all Government Reforms a country has ordered_current_reforms = { limit = { <triggers> } order_by = script_value position = int min = i… | country | → government_reform |
| `ordered_current_war` | Iterate through all wars of a country ordered_current_war = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | country | → war |
| `ordered_defender` | Iterate through all defenders of a war ordered_defender = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value… | war | → country |
| `ordered_descendant` | Iterate through all descendants (children, grandchildren etc) of a character ordered_descendant = { limit = { <triggers> } order_by = script_value pos… | character | → character |
| `ordered_discriminated_culture` | Iterate through all discriminated cultures in a country ordered_discriminated_culture = { limit = { <triggers> } order_by = script_value position = in… | country | → culture |
| `ordered_disloyal_subject` | Iterate through all loyal subject countries ordered_disloyal_subject = { limit = { <triggers> } order_by = script_value position = int min = int max =… | country | → country |
| `ordered_dynasty` | Iterate through all dynasties in a country ordered_dynasty = { limit = { <triggers> } order_by = script_value position = int min = int max = script_va… | country | → dynasty |
| `ordered_east_of_province_definition` | Iterate through all province-definitions east of a province-definition ordered_east_of_province_definition = { limit = { <triggers> } order_by = scrip… | province_definition | → province_definition |
| `ordered_election_candidates` | Iterate through all election candidates of a country with elections! ordered_election_candidates = { limit = { <triggers> } order_by = script_value po… | country | → character |
| `ordered_enemy` | Iterate through all Enemy countries ordered_enemy = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check… | country | → country |
| `ordered_enemy_war_leader` | Iterate through all countries which are leading a war against the scope ordered_enemy_war_leader = { limit = { <triggers> } order_by = script_value po… | country | → country |
| `ordered_estate` | Iterate through all estates in a country ordered_estate = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value… | country | → estate |
| `ordered_estate_privilege` | Iterate through all current estate privileges of a Country ordered_estate_privilege = { limit = { <triggers> } order_by = script_value position = int … | country | → estate_privilege |
| `ordered_estate_type_preferring` | Iterate through all estate types that a prefer a policy ordered_estate_type_preferring = { limit = { <triggers> } order_by = script_value position = i… | policy | → estate_type |
| `ordered_estate_type_that_dislikes_bureaucracy` | Iterate through all estate types that do NOT prefer a bureaucracy ordered_estate_type_that_dislikes_bureaucracy = { limit = { <triggers> } order_by = … | bureaucracy_type | → estate_type |
| `ordered_estate_type_that_likes_bureaucracy` | Iterate through all estate types that a prefer a bureaucracy ordered_estate_type_that_likes_bureaucracy = { limit = { <triggers> } order_by = script_v… | bureaucracy_type | → estate_type |
| `ordered_every_rented_out_mercenary` | Iterate through all lent units currently employed by another country, scoped from the lending country ordered_every_rented_out_mercenary = { limit = {… | country | → mercenary |
| `ordered_expedition` | Iterate through all expeditions for a country ordered_expedition = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | country | → expedition |
| `ordered_expedition_type` | Iterate through all expedition types ordered_expedition_type = { limit = { <triggers> } order_by = script_value position = int min = int max = script_… | country | → expedition_type |
| `ordered_exploration_from_country` | Iterate through all Explorations a country has ordered_exploration_from_country = { limit = { <triggers> } order_by = script_value position = int min … | country | → exploration |
| `ordered_export` | Iterate through all exports in a market ordered_export = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value … | market | → trade |
| `ordered_export_from_location` | Iterate through all exports from location ordered_export_from_location = { limit = { <triggers> } order_by = script_value position = int min = int max… | location | → location |
| `ordered_food_goods` | Iterate through all food-goods ordered_food_goods = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check… | none/any | → goods |
| `ordered_foreign_building_countries_in_location` | Iterate through all foreign building countries in a location ordered_foreign_building_countries_in_location = { limit = { <triggers> } order_by = scri… | location | → country |
| `ordered_foreign_buildings_in_location` | Iterate through all foreign buildings in a location ordered_foreign_buildings_in_location = { limit = { <triggers> } order_by = script_value position … | location | → building |
| `ordered_fort_in_country` | Iterate through all Forts in a country ordered_fort_in_country = { limit = { <triggers> } order_by = script_value position = int min = int max = scrip… | country | → location |
| `ordered_friendly_coast_border_location` | Iterate through all friendly bordering, or across one seazone of a location ordered_friendly_coast_border_location = { limit = { <triggers> } order_by… | location | → location |
| `ordered_friendly_country` | Iterate through all countries with relations marked as friendly ordered_friendly_country = { limit = { <triggers> } order_by = script_value position =… | country | → country |
| `ordered_friendly_or_high_opinion_country` | Iterate through all countries with relations marked as friendly or that we have a high opinion of set in defines ordered_friendly_or_high_opinion_coun… | country | → country |
| `ordered_friendly_to_friendly_country` | Iterate through all friends of our friends ordered_friendly_to_friendly_country = { limit = { <triggers> } order_by = script_value position = int min … | country | → country |
| `ordered_friendly_to_hostile_country` | Iterate through all friends of our enemies ordered_friendly_to_hostile_country = { limit = { <triggers> } order_by = script_value position = int min =… | country | → country |
| `ordered_god_in_religion` | Iterate through all Gods in a Religion ordered_god_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int max = scrip… | religion | → god |
| `ordered_good_in_demand` | Iterate through all goods in a goods demand ordered_good_in_demand = { limit = { <triggers> } order_by = script_value position = int min = int max = s… | demand | → goods |
| `ordered_goods` | Iterate through all types of goods ordered_goods = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_… | none/any | → goods |
| `ordered_graphical_culture_in_culture` | Iterate through all graphical culture in a culture ordered_graphical_culture_in_culture = { limit = { <triggers> } order_by = script_value position = … | culture | → graphical_culture |
| `ordered_great_power` | Iterate through all great powers ordered_great_power = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value ch… | none/any | → country |
| `ordered_heathen_location` | Iterate through all heathen locations in a country ordered_heathen_location = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → location |
| `ordered_heretic_location` | Iterate through all Heretic locations in a country ordered_heretic_location = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → location |
| `ordered_hired_mercenary` | Iterate through mercenaries a country has hired ordered_hired_mercenary = { limit = { <triggers> } order_by = script_value position = int min = int ma… | country | → mercenary |
| `ordered_historical_enemy` | Iterate through all historical Enemy countries ordered_historical_enemy = { limit = { <triggers> } order_by = script_value position = int min = int ma… | country | → country |
| `ordered_historical_rival` | Iterate through all historical rival countries ordered_historical_rival = { limit = { <triggers> } order_by = script_value position = int min = int ma… | country | → country |
| `ordered_holy_site_in_country` | Iterate through all Holy Sites in a country ordered_holy_site_in_country = { limit = { <triggers> } order_by = script_value position = int min = int m… | country | → holy_site |
| `ordered_holy_site_in_religion` | Iterate through all Holy Sites in a Religion ordered_holy_site_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int… | religion | → holy_site |
| `ordered_hostile_country` | Iterate through all countries with relations marked as hostile ordered_hostile_country = { limit = { <triggers> } order_by = script_value position = i… | country | → country |
| `ordered_hostile_or_low_opinion_country` | Iterate through all countries with relations marked as hostile or that we have a low opinion of set in defines ordered_hostile_or_low_opinion_country … | country | → country |
| `ordered_hostile_to_friendly_country` | Iterate through all enemies of our friends ordered_hostile_to_friendly_country = { limit = { <triggers> } order_by = script_value position = int min =… | country | → country |
| `ordered_hostile_to_hostile_country` | Iterate through all enemies of our enemies ordered_hostile_to_hostile_country = { limit = { <triggers> } order_by = script_value position = int min = … | country | → country |
| `ordered_imperial_circle` | Iterate through all Imperial Circles in the scope international organization ordered_imperial_circle = { limit = { <triggers> } order_by = script_valu… | international_organization | → imperial_circle |
| `ordered_imperial_circle_country_is_member_of` | Iterate through all Imperial Circles the scope country is a member of ordered_imperial_circle_country_is_member_of = { limit = { <triggers> } order_by… | country | → imperial_circle |
| `ordered_import` | Iterate through all imports in a market ordered_import = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value … | market | → trade |
| `ordered_import_from_location` | Iterate through all Imports from location ordered_import_from_location = { limit = { <triggers> } order_by = script_value position = int min = int max… | location | → location |
| `ordered_in_global_list` | Iterate through all items in global list. list = name or variable = name ordered_in_global_list = { limit = { <triggers> } order_by = script_value pos… | none/any |  |
| `ordered_in_list` | Iterate through all items in list. list = name or variable = name ordered_in_list = { limit = { <triggers> } order_by = script_value position = int mi… | none/any |  |
| `ordered_in_local_list` | Iterate through all items in local list. list = name or variable = name ordered_in_local_list = { limit = { <triggers> } order_by = script_value posit… | none/any |  |
| `ordered_institutions_embraced` | Iterate through all institutions a country has embraced ordered_institutions_embraced = { limit = { <triggers> } order_by = script_value position = in… | country | → institution |
| `ordered_international_organization` | Iterate through all international organizations ordered_international_organization = { limit = { <triggers> } order_by = script_value position = int m… | none/any | → international_organization |
| `ordered_international_organization_elector` | Iterate through all countries with an elector special status in the international organization ordered_international_organization_elector = { limit = … | international_organization | → country |
| `ordered_international_organization_enemy` | Iterate through all countries that are enemies of the international organization ordered_international_organization_enemy = { limit = { <triggers> } o… | international_organization | → country |
| `ordered_international_organization_member` | Iterate through all countries that are members of the international organization ordered_international_organization_member = { limit = { <triggers> } … | international_organization | → country |
| `ordered_international_organization_owned_location` | Iterate through all locations that are owned by the international organization ordered_international_organization_owned_location = { limit = { <trigge… | international_organization | → location |
| `ordered_international_organization_owner` | Iterate through all international organizations which own the location scope ordered_international_organization_owner = { limit = { <triggers> } order… | location | → international_organization |
| `ordered_international_organization_parliament_opposers` | Iterate through all countries that have voted AGAINST the parliament issue in the in the parliament of the international organization and support the … | international_organization | → country |
| `ordered_international_organization_parliament_supporter` | Iterate through all countries that have voted FOR the parliament issue in the parliament of the international organization and support the current deb… | international_organization | → country |
| `ordered_international_organizations_member_of` | Iterate through all international organizations a country is a member of ordered_international_organizations_member_of = { limit = { <triggers> } orde… | country | → international_organization |
| `ordered_international_organizations_target_of` | Iterate through all international organizations a country is a target of ordered_international_organizations_target_of = { limit = { <triggers> } orde… | country | → international_organization |
| `ordered_invited_religious_figure` | Iterate through all invited religious figures in a Country ordered_invited_religious_figure = { limit = { <triggers> } order_by = script_value positio… | country | → character |
| `ordered_key_in_global_variable_map` | Iterate through all keys in a global variable map. key_in_global_variable_map = { variable = name <...> } ordered_key_in_global_variable_map = { limit… | none/any |  |
| `ordered_key_in_local_variable_map` | Iterate through all keys in a local variable map. key_in_local_variable_map = { variable = name <...> } ordered_key_in_local_variable_map = { limit = … | none/any |  |
| `ordered_key_in_variable_map` | Iterate through all keys in a variable map. key_in_variable_map = { variable = name <...> } ordered_key_in_variable_map = { limit = { <triggers> } ord… | none/any |  |
| `ordered_known_country` | Iterate through all known countries ordered_known_country = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | country | → country |
| `ordered_known_institution` | Iterate through all institutions a country knows of ordered_known_institution = { limit = { <triggers> } order_by = script_value position = int min = … | country | → institution |
| `ordered_left_flank` | Iterate through all subunits on the left-flank of a combat-side ordered_left_flank = { limit = { <triggers> } order_by = script_value position = int m… | combat_side | → sub_unit |
| `ordered_lent_loan` | Iterate through all loans that a country lent ordered_lent_loan = { limit = { <triggers> } order_by = script_value position = int min = int max = scri… | country | → loan |
| `ordered_loan` | Iterate through all loans in a country ordered_loan = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value che… | country | → loan |
| `ordered_loan_lent_to_country` | Iterate through all loans a country has lent to the supplied borrower country ordered_loan_lent_to_country = { limit = { <triggers> } order_by = scrip… | country | → loan |
| `ordered_location_in_area` | Iterate through all Locations in a area ordered_location_in_area = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | area | → location |
| `ordered_location_in_continent` | Iterate through all Locations in a continent ordered_location_in_continent = { limit = { <triggers> } order_by = script_value position = int min = int… | continent | → location |
| `ordered_location_in_market` | Iterate through all locations in a market ordered_location_in_market = { limit = { <triggers> } order_by = script_value position = int min = int max =… | market | → location |
| `ordered_location_in_province` | Iterate through all Locations in a province ordered_location_in_province = { limit = { <triggers> } order_by = script_value position = int min = int m… | province | → location |
| `ordered_location_in_province_definition` | Iterate through all Locations in a province definition ordered_location_in_province_definition = { limit = { <triggers> } order_by = script_value posi… | province_definition | → location |
| `ordered_location_in_region` | Iterate through all Locations in a region ordered_location_in_region = { limit = { <triggers> } order_by = script_value position = int min = int max =… | region | → location |
| `ordered_location_in_religious_order` | Iterate through all locations (holdings) owned by a religious order ordered_location_in_religious_order = { limit = { <triggers> } order_by = script_v… | religious_order | → location |
| `ordered_location_in_scripted_geography` | Iterate through all locations in a scripted geography ordered_location_in_scripted_geography = { limit = { <triggers> } order_by = script_value positi… | scripted_geography | → location |
| `ordered_location_in_sub_continent` | Iterate through all Locations in a sub-continent ordered_location_in_sub_continent = { limit = { <triggers> } order_by = script_value position = int m… | sub_continent | → location |
| `ordered_location_in_the_world` | Iterate through all location ordered_location_in_the_world = { limit = { <triggers> } order_by = script_value position = int min = int max = script_va… | none/any | → location |
| `ordered_location_with_movement` | Iterate through all locations affected by the scope movement ordered_location_with_movement = { limit = { <triggers> } order_by = script_value positio… | movement | → location |
| `ordered_locations_with_town_rights_in_country` | Iterate through all locations with Town Rights in a country ordered_locations_with_town_rights_in_country = { limit = { <triggers> } order_by = script… | country | → location |
| `ordered_loyal_subject` | Iterate through all loyal subject countries ordered_loyal_subject = { limit = { <triggers> } order_by = script_value position = int min = int max = sc… | country | → country |
| `ordered_maritime_area` | Iterate through all maritime areas for a country ordered_maritime_area = { limit = { <triggers> } order_by = script_value position = int min = int max… | country | → area |
| `ordered_market_center_in_country` | Iterate through all markets in a country which market centers are owned by the country ordered_market_center_in_country = { limit = { <triggers> } ord… | country | → market |
| `ordered_market_in_world` | Iterate through all markets in the world ordered_market_in_world = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | none/any | → market |
| `ordered_market_present_in_country` | Iterate through all markets in a country ordered_market_present_in_country = { limit = { <triggers> } order_by = script_value position = int min = int… | country | → market |
| `ordered_market_with_merchants` | Iterate through all markets a country has active merchants ordered_market_with_merchants = { limit = { <triggers> } order_by = script_value position =… | country | → market |
| `ordered_mercenary` | Iterate through all mercenaries in the world ordered_mercenary = { limit = { <triggers> } order_by = script_value position = int min = int max = scrip… | none/any | → mercenary |
| `ordered_mercenary_sub_unit` | Iterate through all subunits in a Mercenary ordered_mercenary_sub_unit = { limit = { <triggers> } order_by = script_value position = int min = int max… | mercenary | → sub_unit |
| `ordered_merchant_in_market` | Iterate through all merchants in a market ordered_merchant_in_market = { limit = { <triggers> } order_by = script_value position = int min = int max =… | market | → country |
| `ordered_movement` | Iterate through all movements ordered_movement = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_ra… | none/any | → movement |
| `ordered_movement_in_country` | Iterate through all movements in a country ordered_movement_in_country = { limit = { <triggers> } order_by = script_value position = int min = int max… | country | → movement |
| `ordered_movement_in_culture` | Iterate through all movements in a culture ordered_movement_in_culture = { limit = { <triggers> } order_by = script_value position = int min = int max… | culture | → movement |
| `ordered_movement_in_religion` | Iterate through all movements in a religion ordered_movement_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int m… | religion | → movement |
| `ordered_navy` | Iterate through all navies in a country ordered_navy = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value ch… | country | → unit |
| `ordered_neighbor_area` | Iterate through all neighboring areas in a area ordered_neighbor_area = { limit = { <triggers> } order_by = script_value position = int min = int max … | area | → area |
| `ordered_neighbor_country` | Iterate through all neighbour countries ordered_neighbor_country = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | country | → country |
| `ordered_neighbor_location` | Iterate through all neighbors of a location ordered_neighbor_location = { limit = { <triggers> } order_by = script_value position = int min = int max … | location | → location |
| `ordered_neighbor_province_definition` | Iterate through all neighboring ProvinceDefinitions in a ProvinceDefinition ordered_neighbor_province_definition = { limit = { <triggers> } order_by =… | province_definition | → province_definition |
| `ordered_new_world_goods` | Iterate through all newworld-goods ordered_new_world_goods = { limit = { <triggers> } order_by = script_value position = int min = int max = script_va… | none/any | → goods |
| `ordered_nomad_countries_in_location` | Iterate through all nomad pop countries in a location ordered_nomad_countries_in_location = { limit = { <triggers> } order_by = script_value position … | location | → country |
| `ordered_non_state_religion_location` | Iterate through all NonStateReligion locations in a country ordered_non_state_religion_location = { limit = { <triggers> } order_by = script_value pos… | country | → location |
| `ordered_old_world_goods` | Iterate through all oldworld-goods ordered_old_world_goods = { limit = { <triggers> } order_by = script_value position = int min = int max = script_va… | none/any | → goods |
| `ordered_omen_in_country` | Iterate through all Omens active in a country ordered_omen_in_country = { limit = { <triggers> } order_by = script_value position = int min = int max … | country | → omen |
| `ordered_omen_in_god` | Iterate through all Omens associated with a God ordered_omen_in_god = { limit = { <triggers> } order_by = script_value position = int min = int max = … | god | → omen |
| `ordered_omen_in_religion` | Iterate through all Omens in a Religion ordered_omen_in_religion = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | religion | → omen |
| `ordered_order_holding_in_country` | Iterate through all owned locations in a country that are holdings of a religious order ordered_order_holding_in_country = { limit = { <triggers> } or… | country | → location |
| `ordered_other_core_country` | Iterate through all other countries which have a core on the current country ordered_other_core_country = { limit = { <triggers> } order_by = script_v… | country | → country |
| `ordered_other_country` | Iterate through all other countries ordered_other_country = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | country | → country |
| `ordered_other_great_power` | Iterate through all other great powers ordered_other_great_power = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | country | → country |
| `ordered_other_religion_in_same_group` | Iterate through all other religions that has the same group as Religion ordered_other_religion_in_same_group = { limit = { <triggers> } order_by = scr… | religion | → religion |
| `ordered_other_revolutionary` | Iterate through all other revolutionary countries ordered_other_revolutionary = { limit = { <triggers> } order_by = script_value position = int min = … | country | → country |
| `ordered_overlord_or_above` | Iterate through your overlord, your overlord's overlord, and so on ordered_overlord_or_above = { limit = { <triggers> } order_by = script_value positi… | country | → country |
| `ordered_ownable_location` | Iterate through all ownable location ordered_ownable_location = { limit = { <triggers> } order_by = script_value position = int min = int max = script… | none/any | → location |
| `ordered_ownable_location_in_area` | Iterate through all ownable Locations in an area ordered_ownable_location_in_area = { limit = { <triggers> } order_by = script_value position = int mi… | area | → location |
| `ordered_ownable_location_in_continent` | Iterate through all ownable Locations in a continent ordered_ownable_location_in_continent = { limit = { <triggers> } order_by = script_value position… | continent | → location |
| `ordered_ownable_location_in_province_definition` | Iterate through all ownable Locations in a province definition ordered_ownable_location_in_province_definition = { limit = { <triggers> } order_by = s… | province_definition | → location |
| `ordered_ownable_location_in_region` | Iterate through all ownable Locations in a region ordered_ownable_location_in_region = { limit = { <triggers> } order_by = script_value position = int… | region | → location |
| `ordered_ownable_location_in_scripted_geography` | Iterate through all ownable Locations in a scripted geography ordered_ownable_location_in_scripted_geography = { limit = { <triggers> } order_by = scr… | scripted_geography | → location |
| `ordered_ownable_location_in_sub_continent` | Iterate through all ownable Locations in a sub continent ordered_ownable_location_in_sub_continent = { limit = { <triggers> } order_by = script_value … | sub_continent | → location |
| `ordered_owned_building` | Iterate through all the owned buildings in a country ordered_owned_building = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → building |
| `ordered_owned_foreign_building` | Iterate through all the owned foreign buildings in a country ordered_owned_foreign_building = { limit = { <triggers> } order_by = script_value positio… | country | → building |
| `ordered_owned_foreign_building_location` | Iterate through all the location of owned foreign buildings in a country ordered_owned_foreign_building_location = { limit = { <triggers> } order_by =… | country | → location |
| `ordered_owned_foreign_building_region` | Iterate through all the regions of owned foreign buildings in a country ordered_owned_foreign_building_region = { limit = { <triggers> } order_by = sc… | country | → region |
| `ordered_owned_location` | Iterate through all owned location in a country ordered_owned_location = { limit = { <triggers> } order_by = script_value position = int min = int max… | country | → location |
| `ordered_owned_nomad_pop` | Iterate through all owned nomad pops in a country ordered_owned_nomad_pop = { limit = { <triggers> } order_by = script_value position = int min = int … | country | → pop |
| `ordered_owned_non_rural_location` | Iterate through all owned non-rural locations in a country ordered_owned_non_rural_location = { limit = { <triggers> } order_by = script_value positio… | country | → location |
| `ordered_owned_rural_location` | Iterate through all owned rural locations in a country ordered_owned_rural_location = { limit = { <triggers> } order_by = script_value position = int … | country | → location |
| `ordered_owner_in_region` | Iterate through all the countries that own locations in a region ordered_owner_in_region = { limit = { <triggers> } order_by = script_value position =… | region | → country |
| `ordered_parent` | Iterate through parents (order: father, mother) of a character. ordered_parent = { limit = { <triggers> } order_by = script_value position = int min =… | character | → character |
| `ordered_participating_countries` | Iterate through all Countrys participating in 1 side of a combat ordered_participating_countries = { limit = { <triggers> } order_by = script_value po… | combat_side | → country |
| `ordered_participating_units` | Iterate through all units participating in 1 side of a combat ordered_participating_units = { limit = { <triggers> } order_by = script_value position … | combat_side | → unit |
| `ordered_past_court_dialect` | Iterate through all court dialects a country has had before ordered_past_court_dialect = { limit = { <triggers> } order_by = script_value position = i… | country | → country |
| `ordered_past_liturgical_dialect` | Iterate through all liturgical dialects a country has had before ordered_past_liturgical_dialect = { limit = { <triggers> } order_by = script_value po… | country | → country |
| `ordered_policy_in_law` | Iterate through all policies that are part of the law scope ordered_policy_in_law = { limit = { <triggers> } order_by = script_value position = int mi… | law | → policy |
| `ordered_political_border_location` | Iterate through all owned location in a country which border another country. ordered_political_border_location = { limit = { <triggers> } order_by = … | country | → location |
| `ordered_pop` | Iterate through all pops in a location or country ordered_pop = { limit = { <triggers> } order_by = script_value position = int min = int max = script… | location, country | → pop |
| `ordered_pops_supporting_rebel` | Iterate through all pops supporting a rebel ordered_pops_supporting_rebel = { limit = { <triggers> } order_by = script_value position = int min = int … | rebels | → pop |
| `ordered_port_in_country` | Iterate through all Ports in a country ordered_port_in_country = { limit = { <triggers> } order_by = script_value position = int min = int max = scrip… | country | → location |
| `ordered_possible_disaster` | Iterate through all possible disasters for a country ordered_possible_disaster = { limit = { <triggers> } order_by = script_value position = int min =… | country | → disaster |
| `ordered_possible_parliament_issue` | Iterate through all possible parliament issues in a country's or an international organization's parliament ordered_possible_parliament_issue = { limi… | country, international_organization | → parliament_issue |
| `ordered_possible_policy` | Iterate through all possible policies of a Country that is not currently implemeted ordered_possible_policy = { limit = { <triggers> } order_by = scri… | country | → policy |
| `ordered_possible_privilege` | Iterate through all possible & allowed estate privileges of a Country that is not currently implemeted ordered_possible_privilege = { limit = { <trigg… | estate | → estate_privilege |
| `ordered_possible_recruit_location` | Iterate through all possible recruit locations in a country ordered_possible_recruit_location = { limit = { <triggers> } order_by = script_value posit… | country | → location |
| `ordered_potential_expansion_target` | Iterate through all countries which are potential expansion targets for an AI country ordered_potential_expansion_target = { limit = { <triggers> } or… | country | → country |
| `ordered_present_country` | Iterate through all countries in the specified geography ordered_present_country = { limit = { <triggers> } order_by = script_value position = int min… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `ordered_present_culture_in_country` | Iterate through all cultures present in the country. ordered_present_culture_in_country = { limit = { <triggers> } order_by = script_value position = … | country | → culture |
| `ordered_present_culture_in_location` | Iterate through all cultures present in the location. ordered_present_culture_in_location = { limit = { <triggers> } order_by = script_value position … | location | → culture |
| `ordered_present_overlord` | Iterate through all countries which have a subject in the specified geography ordered_present_overlord = { limit = { <triggers> } order_by = script_va… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `ordered_present_religion_in_country` | Iterate through all religions present in the country. ordered_present_religion_in_country = { limit = { <triggers> } order_by = script_value position … | country | → religion |
| `ordered_present_religion_in_location` | Iterate through all religions present in the location. ordered_present_religion_in_location = { limit = { <triggers> } order_by = script_value positio… | location | → religion |
| `ordered_primary_or_accepted_culture` | Iterate through primary culture and all accepted cultures in a country. Primary is ordered first. ordered_primary_or_accepted_culture = { limit = { <t… | country | → culture |
| `ordered_primary_or_accepted_or_tolerated_culture` | Iterate through primary culture and all accepted and all tolerated cultures in a country. Primary is ordered first. ordered_primary_or_accepted_or_tol… | country | → culture |
| `ordered_privateer` | Iterate through all privateers in the world ordered_privateer = { limit = { <triggers> } order_by = script_value position = int min = int max = script… | none/any | → privateer |
| `ordered_privateer_from_country` | Iterate through all privateers a country has ordered_privateer_from_country = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → privateer |
| `ordered_privateer_in_area` | Iterate through all privateers in a area ordered_privateer_in_area = { limit = { <triggers> } order_by = script_value position = int min = int max = s… | area | → privateer |
| `ordered_production_method` | Iterate through all types of production methods. ordered_production_method = { limit = { <triggers> } order_by = script_value position = int min = int… | none/any | → production_method |
| `ordered_production_method_of_building` | Iterate through all available production methods of the building. ordered_production_method_of_building = { limit = { <triggers> } order_by = script_v… | building | → production_method |
| `ordered_province` | Iterate through all provinces in a country ordered_province = { limit = { <triggers> } order_by = script_value position = int min = int max = script_v… | country | → province |
| `ordered_province_definition` | Iterate through all existing province_definition ordered_province_definition = { limit = { <triggers> } order_by = script_value position = int min = i… | none/any | → province_definition |
| `ordered_province_definition_in_area` | Iterate through all province-definitions in an area ordered_province_definition_in_area = { limit = { <triggers> } order_by = script_value position = … | area | → province_definition |
| `ordered_province_definition_in_scripted_geography` | Iterate through all province_definitions in a scripted geography ordered_province_definition_in_scripted_geography = { limit = { <triggers> } order_by… | scripted_geography | → province_definition |
| `ordered_province_in_area` | Iterate through all provinces in an area ordered_province_in_area = { limit = { <triggers> } order_by = script_value position = int min = int max = sc… | area | → province |
| `ordered_province_in_province_definition` | Iterate through all provinces in a province-definition ordered_province_in_province_definition = { limit = { <triggers> } order_by = script_value posi… | province_definition | → province |
| `ordered_rebel` | Iterate through all Rebels in a country ordered_rebel = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value c… | country | → rebels |
| `ordered_region` | Iterate through all existing regions ordered_region = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value che… | none/any | → region |
| `ordered_region_in_continent` | Iterate through all regions in a sub-continent ordered_region_in_continent = { limit = { <triggers> } order_by = script_value position = int min = int… | sub_continent | → region |
| `ordered_region_in_scripted_geography` | Iterate through all regions in a scripted geography ordered_region_in_scripted_geography = { limit = { <triggers> } order_by = script_value position =… | scripted_geography | → region |
| `ordered_related_country` | Iterate through all related countries ordered_related_country = { limit = { <triggers> } order_by = script_value position = int min = int max = script… | country | → country |
| `ordered_religion` | Iterate through all religions ordered_religion = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check_ra… | none/any | → religion |
| `ordered_religion_for_god` | Iterate through all Religions of a God ordered_religion_for_god = { limit = { <triggers> } order_by = script_value position = int min = int max = scri… | god | → religion |
| `ordered_religion_in_religion_group` | Iterate through all religions in a religion group. ordered_religion_in_religion_group = { limit = { <triggers> } order_by = script_value position = in… | group | → religion |
| `ordered_religion_international_organization` | Iterate through all international organisations of a religion ordered_religion_international_organization = { limit = { <triggers> } order_by = script… | religion | → international_organization |
| `ordered_religious_aspect` | Iterate through all religious aspects of a Country ordered_religious_aspect = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → religious_aspect |
| `ordered_religious_focus` | Iterate through all completed religious focuses of a Country ordered_religious_focus = { limit = { <triggers> } order_by = script_value position = int… | country | → religious_focus |
| `ordered_religious_order` | Iterate through all religious orders ordered_religious_order = { limit = { <triggers> } order_by = script_value position = int min = int max = script_… | none/any | → religious_order |
| `ordered_religious_order_in_country` | Iterate through all religious orders present in a country ordered_religious_order_in_country = { limit = { <triggers> } order_by = script_value positi… | country | → religious_order |
| `ordered_religious_order_in_location` | Iterate through all religious orders present at a location ordered_religious_order_in_location = { limit = { <triggers> } order_by = script_value posi… | location | → religious_order |
| `ordered_religious_order_in_religion` | Iterate through all religious orders of a religion ordered_religious_order_in_religion = { limit = { <triggers> } order_by = script_value position = i… | religion | → religious_order |
| `ordered_religious_order_type` | Iterate through all religious order types ordered_religious_order_type = { limit = { <triggers> } order_by = script_value position = int min = int max… | none/any | → religious_order_type |
| `ordered_religious_school_in_religion` | Iterate through all Religious Schools in a Religion ordered_religious_school_in_religion = { limit = { <triggers> } order_by = script_value position =… | religion | → religious_school |
| `ordered_rented_out_mercenary` | Iterate through mercenaries a country has rented out to the market ordered_rented_out_mercenary = { limit = { <triggers> } order_by = script_value pos… | country | → mercenary |
| `ordered_required_goods` | Iterate through all goods required by the scope production method. ordered_required_goods = { limit = { <triggers> } order_by = script_value position … | production_method | → goods |
| `ordered_reserves` | Iterate through all subunits on the reserve of a combat-side ordered_reserves = { limit = { <triggers> } order_by = script_value position = int min = … | combat_side | → sub_unit |
| `ordered_retreated` | Iterate through all subunits on the retreated of a combat-side ordered_retreated = { limit = { <triggers> } order_by = script_value position = int min… | combat_side | → sub_unit |
| `ordered_revolutionary` | Iterate through all revolutionary states ordered_revolutionary = { limit = { <triggers> } order_by = script_value position = int min = int max = scrip… | none/any | → country |
| `ordered_right_flank` | Iterate through all subunits on the right-flank of a combat-side ordered_right_flank = { limit = { <triggers> } order_by = script_value position = int… | combat_side | → sub_unit |
| `ordered_rival` | Iterate through all rival countries ordered_rival = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value check… | country | → country |
| `ordered_road_type` | Iterate through all the road types ordered_road_type = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value ch… | none/any | → road_type |
| `ordered_royal_marriage` | Iterate through all royal married countries ordered_royal_marriage = { limit = { <triggers> } order_by = script_value position = int min = int max = s… | country | → country |
| `ordered_ruled_international_organization` | Iterate through IOs a character rules ordered_ruled_international_organization = { limit = { <triggers> } order_by = script_value position = int min =… | character | → international_organization |
| `ordered_ruler` | Iterate through all characters that have ever been rulers in a country, including the dead ordered_ruler = { limit = { <triggers> } order_by = script_… | country | → character |
| `ordered_ruling_countries` | Iterate through countries a character rules ordered_ruling_countries = { limit = { <triggers> } order_by = script_value position = int min = int max =… | character | → country |
| `ordered_sound_toll_in_country` | Iterate through all Sound Tolls in a country ordered_sound_toll_in_country = { limit = { <triggers> } order_by = script_value position = int min = int… | country | → location |
| `ordered_spouse` | Iterate through all spouses of a character ordered_spouse = { limit = { <triggers> } order_by = script_value position = int min = int max = script_val… | character | → character |
| `ordered_spy_network_built_in_us` | Iterate through all  countries building spy networks ordered_spy_network_built_in_us = { limit = { <triggers> } order_by = script_value position = int… | country | → country |
| `ordered_sub_continent` | Iterate through all existing sub_continents ordered_sub_continent = { limit = { <triggers> } order_by = script_value position = int min = int max = sc… | none/any | → sub_continent |
| `ordered_sub_continent_in_continent` | Iterate through all sub-continents in a continent ordered_sub_continent_in_continent = { limit = { <triggers> } order_by = script_value position = int… | continent | → sub_continent |
| `ordered_sub_continent_in_scripted_geography` | Iterate through all subcontinents in a scripted geography ordered_sub_continent_in_scripted_geography = { limit = { <triggers> } order_by = script_val… | scripted_geography | → sub_continent |
| `ordered_sub_unit` | Iterate through all subunits in a unit ordered_sub_unit = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value… | unit | → sub_unit |
| `ordered_subject` | Iterate through all subject countries ordered_subject = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value c… | country | → country |
| `ordered_subject_or_below` | Iterate through all subject countries and their subject countries, and so on ordered_subject_or_below = { limit = { <triggers> } order_by = script_val… | country | → country |
| `ordered_tolerated_culture` | Iterate through all Tolerated cultures in a country ordered_tolerated_culture = { limit = { <triggers> } order_by = script_value position = int min = … | country | → culture |
| `ordered_town_rights_in_country` | Iterate through all Town Rights in a country ordered_town_rights_in_country = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → town_rights |
| `ordered_town_rights_in_location` | Iterate through all Town Rights in a Location ordered_town_rights_in_Location = { limit = { <triggers> } order_by = script_value position = int min = … | location | → town_rights |
| `ordered_trade` | Iterate through all trades in a Country ordered_trade = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value c… | country | → trade |
| `ordered_trait` | Iterate through all traits of a character. ordered_trait = { limit = { <triggers> } order_by = script_value position = int min = int max = script_valu… | character | → trait |
| `ordered_union_partner` | Iterate through all countries which are in a personal union with the current country scope. ordered_union_partner = { limit = { <triggers> } order_by … | country | → country |
| `ordered_unit` | Iterate through all units in a country ordered_unit = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value che… | country | → unit |
| `ordered_unit_in_location` | Iterate through all units in a location ordered_unit_in_location = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | location | → unit |
| `ordered_valid_religion_for_aspect` | Iterate through all religion that an aspect can be for ordered_valid_religion_for_aspect = { limit = { <triggers> } order_by = script_value position =… | religious_aspect | → religion |
| `ordered_voter` | Iterate through all voters in an active resolution ordered_voter = { limit = { <triggers> } order_by = script_value position = int min = int max = scr… | active_resolution | → country |
| `ordered_war` | Iterate through all wars going on globally ordered_war = { limit = { <triggers> } order_by = script_value position = int min = int max = script_value … | none/any | → war |
| `ordered_war_participant` | Iterate through all participants of a war ordered_war_participant = { limit = { <triggers> } order_by = script_value position = int min = int max = sc… | war | → country |
| `ordered_weather_system_in_location` | Iterate through all weather systems in a location ordered_weather_system_in_location = { limit = { <triggers> } order_by = script_value position = int… | location | → weather_system |
| `ordered_west_of_province_definition` | Iterate through all province-definitions west of a province-definition ordered_west_of_province_definition = { limit = { <triggers> } order_by = scrip… | province_definition | → province_definition |
| `ordered_work_of_art` | Iterate through all WorkOfArts in the world ordered_work_of_art = { limit = { <triggers> } order_by = script_value position = int min = int max = scri… | none/any | → work_of_art |
| `ordered_work_of_art_by_creator` | Iterate through all work_of_art by a particular artist ordered_work_of_art_by_creator = { limit = { <triggers> } order_by = script_value position = in… | character | → work_of_art |
| `ordered_work_of_art_in_country` | Iterate through all work_of_art in a country ordered_work_of_art_in_country = { limit = { <triggers> } order_by = script_value position = int min = in… | country | → work_of_art |
| `ordered_work_of_art_in_location` | Iterate through all work_of_art in a location ordered_work_of_art_in_location = { limit = { <triggers> } order_by = script_value position = int min = … | location | → work_of_art |
| `pause_expedition` | Pauses an expedition for the given number of days. Usage: pause_expedition = 30 | expedition |  |
| `pay_off_loans` | pays off % of the current loans. Usage: pay_off_loans = { fraction = <script number> [payer = <country scope>] } | country |  |
| `pay_policy_price_effect` | executes the policy price effect on the current scope | country, international_organization | → policy |
| `pay_price` | Pays a Price from a country | country | → price |
| `pay_scaled_price` | Pays a fraction of a Price from a country. Usage: pay_scaled_price = { price = price:x scale = 0.2 reason = <loc key> } | country |  |
| `perform_diplomatic_action` | perform_diplomatic_action (type = <diploaction or country interaction type> actor/receipent/target/target_1... = country/province/location/character/w… | country |  |
| `post_audio_event` | Runs an audio even on a "persistent" audio object post_audio_event = { persistent_object = [object_name] # For example music_manager event = [audio_ev… | none/any |  |
| `press_estate_private_regiments` | Raises all unraised private regiment definitions for an estate into the country's royal army. estate = <type> | country |  |
| `propose_resolution` | proposes a new resolution. type = <resolution key> actor = <proposer country> recipient = <international organisation or situation> | none/any |  |
| `prospective_proximity_calculation` | Simulates the proximity calculation from the point of a country with capital in location. The distance are then output to a local variable map entitle… | country |  |
| `raise_all_levies` | The country scope will raise all its levies of the specified type. Usage: raise_all_levies = { type = army/navy [instant = yes] } | country |  |
| `raise_levies` | The province scope will raise levies of the specified type for the owner. Usage: raise_levies = { type = army/navy [instant = yes] } | province |  |
| `random` | run an effect depending on a random chance, do nothing otherwise. random = { chance = 0-100     # random chance in percent. can also be a script value… | none/any |  |
| `random_accepted_culture` | Iterate through all accepted cultures in a country random_accepted_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → culture |
| `random_active_disaster` | Iterate through all active disasters for a country random_active_disaster = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → disaster |
| `random_active_estate` | Iterate through all active estates (non-crown) random_active_estate = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → estate_type |
| `random_active_resolution` | Iterate through all currently active resolutions in an international organization or situation random_active_resolution = { limit = { <triggers> } (op… | international_organization, situation | → active_resolution |
| `random_adjacent_ports_to_area` | Iterate through all adjacent ports of an seazone area random_adjacent_ports_to_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | area | → location |
| `random_advance_definition` | Iterate through all advance definitions random_advance_definition = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → advance_type |
| `random_allowed_estate_in_heir_selection` | Iterate through all allowed estates a HeirSelection has random_allowed_estate_in_heir_selection = { limit = { <triggers> } (optional) weight = { mtth … | heir_selection | → estate_type |
| `random_ancestor` | Iterate through all ancestors (parents, grandparents etc) of a character random_ancestor = { limit = { <triggers> } (optional) weight = { mtth } <effe… | character | → character |
| `random_area` | Iterate through all existing areas random_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → area |
| `random_area_in_region` | Iterate through all areas in a region random_area_in_region = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | region | → area |
| `random_area_in_scripted_geography` | Iterate through all areas in a scripted geography random_area_in_scripted_geography = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | scripted_geography | → area |
| `random_area_with_core` | Iterate through all areas with cored locations in a country random_area_with_core = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → area |
| `random_area_with_owned_province` | Iterate through all areas with owned provinces in a country random_area_with_owned_province = { limit = { <triggers> } (optional) weight = { mtth } <e… | country | → area |
| `random_army` | Iterate through all armies in a country random_army = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → unit |
| `random_artist` | Iterate through all artists in a country random_artist = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → character |
| `random_attacker` | Iterate through all attackers of a war random_attacker = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | war | → country |
| `random_available_dynasty_member` | Iterate through adult dynasty members who are not a ruler or heir (cached) random_available_dynasty_member = { limit = { <triggers> } (optional) weigh… | dynasty | → character |
| `random_avatar_for_god` | Iterate through all Avatars of a God random_avatar_for_god = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | god | → avatar |
| `random_besieging_unit` | Iterate through all units participating in a siege random_besieging_unit = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | siege | → unit |
| `random_betrothal` | Iterate through all royal betrothal countries random_betrothal = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_border_location` | Iterate through all owned location in a country which border locations not owned by the current country scope. random_border_location = { limit = { <t… | country | → location |
| `random_buildable_building_type` | Iterate through all the building types a country can build random_buildable_building_type = { limit = { <triggers> } (optional) weight = { mtth } <eff… | country | → building_type |
| `random_building_owned_by_estate` | Iterate through all buildings that an estate has random_building_owned_by_estate = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | estate | → building |
| `random_building_type` | Iterate through all the building types random_building_type = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → building_type |
| `random_buildings_in_location` | Iterate through all buildings in a location random_buildings_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → building |
| `random_cabinet` | Iterate through all actions in a country's cabinet random_cabinet = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → cabinet |
| `random_cabinet_action` | Iterate through all actions in a country's cabinet actions random_cabinet_action = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → cabinet_action |
| `random_cabinet_character` | Iterate through all characters in a country that is in the cabinet random_cabinet_character = { limit = { <triggers> } (optional) weight = { mtth } <e… | country | → character |
| `random_calling` | Iterate through all callings random_calling = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → calling |
| `random_cardinal_in_country` | Iterate through all Cardinals in a country random_cardinal_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → character |
| `random_cardinal_in_religion` | Iterate through all Cardinals in a Religion random_cardinal_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → character |
| `random_casus_belli_on_us` | Iterate through all countries have a casus belli on us random_casus_belli_on_us = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_casus_belli_target` | Iterate through all countries we have a casus belli on random_casus_belli_target = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_center` | Iterate through all subunits on the center of a combat-side random_center = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | combat_side | → sub_unit |
| `random_character` | Iterate through all characters in a country random_character = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → character |
| `random_character_in_dynasty` | Iterate through all living characters in a Dynasty random_character_in_dynasty = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | dynasty | → character |
| `random_character_in_order` | Iterate through all living member characters of a religious order random_character_in_order = { limit = { <triggers> } (optional) weight = { mtth } <e… | religious_order | → character |
| `random_character_supporting_rebel` | Iterate through all characters supporting a rebel random_character_supporting_rebel = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | rebels | → character |
| `random_child` | Iterate through all children of a character random_child = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → character |
| `random_circle_member` | Iterate through all member countries of an Imperial Circle random_circle_member = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | imperial_circle | → country |
| `random_close_relative` | Iterate through all close relatives of a character random_close_relative = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → character |
| `random_coast_border_location` | Iterate through all bordering, or across one seazone of a location random_coast_border_location = { limit = { <triggers> } (optional) weight = { mtth … | location | → location |
| `random_colonial_charter` | Iterate through all colonial charters in a country random_colonial_charter = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → colonial_charter |
| `random_colonial_charter_goal` | Iterate through all colonial charter goal types random_colonial_charter_goal = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → colonial_charter_goal |
| `random_colonial_claim_province_definition` | Iterate through all province definitions with colonial claims from the scope country. Usage with country = <country> random_colonial_claim_province_de… | country | → province_definition |
| `random_colonial_country` | Iterate through all colonial countries in the world random_colonial_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → country |
| `random_colonial_overlord` | Iterate through all colonial overlord countries in the world random_colonial_overlord = { limit = { <triggers> } (optional) weight = { mtth } <effects… | none/any | → country |
| `random_colonial_top_overlord` | Iterate through all countries in the world that have a colonial country among their subjects or their subjects subjects and so on random_colonial_top_… | none/any | → country |
| `random_connected_location` | Iterate through all locations in the same country as the scope location that are connected by land or strait random_connected_location = { limit = { <… | location | → location |
| `random_construction_material_for_building_type` | Iterate through all goods required to construct a building type random_construction_material_for_building_type = { limit = { <triggers> } (optional) w… | building_type | → goods |
| `random_continent` | Iterate through all existing continents random_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → continent |
| `random_continent_in_scripted_geography` | Iterate through all continents in a scripted geography random_continent_in_scripted_geography = { limit = { <triggers> } (optional) weight = { mtth } … | scripted_geography | → continent |
| `random_controlled_location` | Iterate through all controlled location in a country random_controlled_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_core_in_location` | Iterate through all cores in a location random_core_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → country |
| `random_core_location` | Iterate through all core locations in a country random_core_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_country` | Iterate through all existing countries random_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → country |
| `random_country_annexing_us` | Iterate through all countries which are currently annexing the current country scope. random_country_annexing_us = { limit = { <triggers> } (optional)… | country | → country |
| `random_country_at_war_with` | Iterate through all countries at war with random_country_at_war_with = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_country_borrowed_from` | Iterate through all countries a country has borrowed from random_country_borrowed_from = { limit = { <triggers> } (optional) weight = { mtth } <effect… | country | → country |
| `random_country_in_culture` | Iterate through all countries with this primary culture random_country_in_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | culture | → country |
| `random_country_in_culture_group` | Iterate through all countries in a culture group. random_country_in_culture_group = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | culture_group | → country |
| `random_country_in_diplomatic_range` | Iterate through all countries in diplomatic range random_country_in_diplomatic_range = { limit = { <triggers> } (optional) weight = { mtth } <effects>… | country | → country |
| `random_country_in_dynasty` | Iterate through all countries in a Dynasty random_country_in_dynasty = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | dynasty | → country |
| `random_country_in_hierarchy` | Iterate through every country in the entire overlord/subject hierarchy, from the independent top overlord to the deepest subjects random_country_in_hi… | country | → country |
| `random_country_in_religion` | Iterate through all countries in a religion random_country_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → country |
| `random_country_in_religion_group` | Iterate through all countries in a religion group. random_country_in_religion_group = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | group | → country |
| `random_country_in_religious_order` | Iterate through all countries a religious order has a presence in random_country_in_religious_order = { limit = { <triggers> } (optional) weight = { m… | religious_order | → country |
| `random_country_in_religious_school` | Iterate through all countries within a school random_country_in_religious_school = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religious_school | → country |
| `random_country_including_inactive` | Iterate through all countries, including inactive random_country_including_inactive = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | none/any | → country |
| `random_country_lent_to` | Iterate through all countries a country has lent to random_country_lent_to = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_country_of_country_type` | Iterate through all countries of the specified type. Usage: country_of_country_type = { type = <country type> } random_country_of_country_type = { lim… | none/any | → country |
| `random_country_sub_unit` | Iterate through all subunits in all units in a country random_country_sub_unit = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → sub_unit |
| `random_country_supporting_rebel` | Iterate through all countries supporting a rebel random_country_supporting_rebel = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | rebels | → country |
| `random_country_that_can_be_called_defensively` | Iterate through all countries that may be called into a defensive war. random_country_that_can_be_called_defensively = { limit = { <triggers> } (optio… | country | → country |
| `random_country_that_can_be_called_offensively` | Iterate through all countries that may be called into an offensive war. random_country_that_can_be_called_offensively = { limit = { <triggers> } (opti… | country | → country |
| `random_country_together_in_war_with` | Iterate through all countries which are an ally in any of the country scope's wars random_country_together_in_war_with = { limit = { <triggers> } (opt… | country | → country |
| `random_country_wants_casus_belli_with` | all countries a country wants a casus belli with. Only for Ai random_country_wants_casus_belli_with = { limit = { <triggers> } (optional) weight = { m… | country | → country |
| `random_country_wants_military_access_in` | all countries a country wants military access through random_country_wants_military_access_in = { limit = { <triggers> } (optional) weight = { mtth } … | country | → country |
| `random_country_wants_to_attack` | all countries a country wants to attack random_country_wants_to_attack = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_country_wants_to_subjugate` | all countries a country wants to subjugate random_country_wants_to_subjugate = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_country_we_are_annexing` | Iterate through all countries which are currently annexed by the current country scope. random_country_we_are_annexing = { limit = { <triggers> } (opt… | country | → country |
| `random_country_with_capital_in_geography` | Iterate through all countries which have their capital in the specified geography random_country_with_capital_in_geography = { limit = { <triggers> } … | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `random_country_with_cardinals` | Iterate through all countries with cardinals in a religion random_country_with_cardinals = { limit = { <triggers> } (optional) weight = { mtth } <effe… | religion | → country |
| `random_country_with_chivalric_order` | Iterate through all countries with an active chivalric order random_country_with_chivalric_order = { limit = { <triggers> } (optional) weight = { mtth… | none/any | → country |
| `random_country_with_coalition_grade_antagonism_against_us` | Iterate through all countries who have coalition grade antagonism against us random_country_with_coalition_grade_antagonism_against_us = { limit = { <… | country | → country |
| `random_country_with_relation_that_can_be_annulled` | Iterate through all countries which have an annullable relation with the scope country. random_country_with_relation_that_can_be_annulled = { limit = … | country | → country |
| `random_country_with_special_status_of_type` | Iterate through all countries in the international organization which have the specified special status random_country_with_special_status_of_type = {… | international_organization | → country |
| `random_country_with_succession_law` | Iterate through all countries with a cached succession law (set cached = yes in the heir_selection to use this) random_country_with_succession_law = {… | none/any | → country |
| `random_culture` | Iterate through all cultures random_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → culture |
| `random_culture_group` | Iterate through all culture groups the culture is in. random_culture_group = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | culture | → culture_group |
| `random_culture_in_culture_group` | Iterate through all cultures in a culture group. random_culture_in_culture_group = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | culture_group | → culture |
| `random_current_avatars` | Iterate through all Avatars a country has random_current_avatars = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → avatar |
| `random_current_bureaucracy` | Iterate through all Bureaucracies a country has random_current_bureaucracy = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → bureaucracy |
| `random_current_bureaucracy_type` | Iterate through all Bureaucracy types a country has random_current_bureaucracy_type = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | country | → bureaucracy_type |
| `random_current_gods` | Iterate through all Gods a country worships random_current_gods = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → god |
| `random_current_law` | Iterate through all laws of a country. random_current_law = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → law |
| `random_current_law_in_international_organization` | Iterate through all laws that are codified in the international organization random_current_law_in_international_organization = { limit = { <triggers>… | international_organization | → law |
| `random_current_policy` | Iterate through all policies that are codified in the country random_current_policy = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | country | → policy |
| `random_current_policy_in_international_organization` | Iterate through all policies that are codified in the international organization random_current_policy_in_international_organization = { limit = { <tr… | international_organization | → policy |
| `random_current_reforms` | Iterate through all Government Reforms a country has random_current_reforms = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → government_reform |
| `random_current_war` | Iterate through all wars of a country random_current_war = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → war |
| `random_defender` | Iterate through all defenders of a war random_defender = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | war | → country |
| `random_descendant` | Iterate through all descendants (children, grandchildren etc) of a character random_descendant = { limit = { <triggers> } (optional) weight = { mtth }… | character | → character |
| `random_discriminated_culture` | Iterate through all discriminated cultures in a country random_discriminated_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects>… | country | → culture |
| `random_disloyal_subject` | Iterate through all loyal subject countries random_disloyal_subject = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_dynasty` | Iterate through all dynasties in a country random_dynasty = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → dynasty |
| `random_east_of_province_definition` | Iterate through all province-definitions east of a province-definition random_east_of_province_definition = { limit = { <triggers> } (optional) weight… | province_definition | → province_definition |
| `random_election_candidates` | Iterate through all election candidates of a country with elections! random_election_candidates = { limit = { <triggers> } (optional) weight = { mtth … | country | → character |
| `random_enemy` | Iterate through all Enemy countries random_enemy = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_enemy_war_leader` | Iterate through all countries which are leading a war against the scope random_enemy_war_leader = { limit = { <triggers> } (optional) weight = { mtth … | country | → country |
| `random_estate` | Iterate through all estates in a country random_estate = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → estate |
| `random_estate_privilege` | Iterate through all current estate privileges of a Country random_estate_privilege = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → estate_privilege |
| `random_estate_type_preferring` | Iterate through all estate types that a prefer a policy random_estate_type_preferring = { limit = { <triggers> } (optional) weight = { mtth } <effects… | policy | → estate_type |
| `random_estate_type_that_dislikes_bureaucracy` | Iterate through all estate types that do NOT prefer a bureaucracy random_estate_type_that_dislikes_bureaucracy = { limit = { <triggers> } (optional) w… | bureaucracy_type | → estate_type |
| `random_estate_type_that_likes_bureaucracy` | Iterate through all estate types that a prefer a bureaucracy random_estate_type_that_likes_bureaucracy = { limit = { <triggers> } (optional) weight = … | bureaucracy_type | → estate_type |
| `random_every_rented_out_mercenary` | Iterate through all lent units currently employed by another country, scoped from the lending country random_every_rented_out_mercenary = { limit = { … | country | → mercenary |
| `random_expedition` | Iterate through all expeditions for a country random_expedition = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → expedition |
| `random_expedition_type` | Iterate through all expedition types random_expedition_type = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → expedition_type |
| `random_exploration_from_country` | Iterate through all Explorations a country has random_exploration_from_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → exploration |
| `random_export` | Iterate through all exports in a market random_export = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | market | → trade |
| `random_export_from_location` | Iterate through all exports from location random_export_from_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → location |
| `random_food_goods` | Iterate through all food-goods random_food_goods = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → goods |
| `random_foreign_building_countries_in_location` | Iterate through all foreign building countries in a location random_foreign_building_countries_in_location = { limit = { <triggers> } (optional) weigh… | location | → country |
| `random_foreign_buildings_in_location` | Iterate through all foreign buildings in a location random_foreign_buildings_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effe… | location | → building |
| `random_fort_in_country` | Iterate through all Forts in a country random_fort_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_friendly_coast_border_location` | Iterate through all friendly bordering, or across one seazone of a location random_friendly_coast_border_location = { limit = { <triggers> } (optional… | location | → location |
| `random_friendly_country` | Iterate through all countries with relations marked as friendly random_friendly_country = { limit = { <triggers> } (optional) weight = { mtth } <effec… | country | → country |
| `random_friendly_or_high_opinion_country` | Iterate through all countries with relations marked as friendly or that we have a high opinion of set in defines random_friendly_or_high_opinion_count… | country | → country |
| `random_friendly_to_friendly_country` | Iterate through all friends of our friends random_friendly_to_friendly_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_friendly_to_hostile_country` | Iterate through all friends of our enemies random_friendly_to_hostile_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_god_in_religion` | Iterate through all Gods in a Religion random_god_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → god |
| `random_good_in_demand` | Iterate through all goods in a goods demand random_good_in_demand = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | demand | → goods |
| `random_goods` | Iterate through all types of goods random_goods = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → goods |
| `random_graphical_culture_in_culture` | Iterate through all graphical culture in a culture random_graphical_culture_in_culture = { limit = { <triggers> } (optional) weight = { mtth } <effect… | culture | → graphical_culture |
| `random_great_power` | Iterate through all great powers random_great_power = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → country |
| `random_heathen_location` | Iterate through all heathen locations in a country random_heathen_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_heretic_location` | Iterate through all Heretic locations in a country random_heretic_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_hired_mercenary` | Iterate through mercenaries a country has hired random_hired_mercenary = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → mercenary |
| `random_historical_enemy` | Iterate through all historical Enemy countries random_historical_enemy = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_historical_rival` | Iterate through all historical rival countries random_historical_rival = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_holy_site_in_country` | Iterate through all Holy Sites in a country random_holy_site_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → holy_site |
| `random_holy_site_in_religion` | Iterate through all Holy Sites in a Religion random_holy_site_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → holy_site |
| `random_hostile_country` | Iterate through all countries with relations marked as hostile random_hostile_country = { limit = { <triggers> } (optional) weight = { mtth } <effects… | country | → country |
| `random_hostile_or_low_opinion_country` | Iterate through all countries with relations marked as hostile or that we have a low opinion of set in defines random_hostile_or_low_opinion_country =… | country | → country |
| `random_hostile_to_friendly_country` | Iterate through all enemies of our friends random_hostile_to_friendly_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_hostile_to_hostile_country` | Iterate through all enemies of our enemies random_hostile_to_hostile_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_imperial_circle` | Iterate through all Imperial Circles in the scope international organization random_imperial_circle = { limit = { <triggers> } (optional) weight = { m… | international_organization | → imperial_circle |
| `random_imperial_circle_country_is_member_of` | Iterate through all Imperial Circles the scope country is a member of random_imperial_circle_country_is_member_of = { limit = { <triggers> } (optional… | country | → imperial_circle |
| `random_import` | Iterate through all imports in a market random_import = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | market | → trade |
| `random_import_from_location` | Iterate through all Imports from location random_import_from_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → location |
| `random_in_global_list` | Iterate through all items in global list. list = name or variable = name random_in_global_list = { limit = { <triggers> } (optional) weight = { mtth }… | none/any |  |
| `random_in_list` | Iterate through all items in list. list = name or variable = name random_in_list = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any |  |
| `random_in_local_list` | Iterate through all items in local list. list = name or variable = name random_in_local_list = { limit = { <triggers> } (optional) weight = { mtth } <… | none/any |  |
| `random_institutions_embraced` | Iterate through all institutions a country has embraced random_institutions_embraced = { limit = { <triggers> } (optional) weight = { mtth } <effects>… | country | → institution |
| `random_international_organization` | Iterate through all international organizations random_international_organization = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → international_organization |
| `random_international_organization_elector` | Iterate through all countries with an elector special status in the international organization random_international_organization_elector = { limit = {… | international_organization | → country |
| `random_international_organization_enemy` | Iterate through all countries that are enemies of the international organization random_international_organization_enemy = { limit = { <triggers> } (o… | international_organization | → country |
| `random_international_organization_member` | Iterate through all countries that are members of the international organization random_international_organization_member = { limit = { <triggers> } (… | international_organization | → country |
| `random_international_organization_owned_location` | Iterate through all locations that are owned by the international organization random_international_organization_owned_location = { limit = { <trigger… | international_organization | → location |
| `random_international_organization_owner` | Iterate through all international organizations which own the location scope random_international_organization_owner = { limit = { <triggers> } (optio… | location | → international_organization |
| `random_international_organization_parliament_opposers` | Iterate through all countries that have voted AGAINST the parliament issue in the in the parliament of the international organization and support the … | international_organization | → country |
| `random_international_organization_parliament_supporter` | Iterate through all countries that have voted FOR the parliament issue in the parliament of the international organization and support the current deb… | international_organization | → country |
| `random_international_organizations_member_of` | Iterate through all international organizations a country is a member of random_international_organizations_member_of = { limit = { <triggers> } (opti… | country | → international_organization |
| `random_international_organizations_target_of` | Iterate through all international organizations a country is a target of random_international_organizations_target_of = { limit = { <triggers> } (opti… | country | → international_organization |
| `random_invited_religious_figure` | Iterate through all invited religious figures in a Country random_invited_religious_figure = { limit = { <triggers> } (optional) weight = { mtth } <ef… | country | → character |
| `random_key_in_global_variable_map` | Iterate through all keys in a global variable map. key_in_global_variable_map = { variable = name <...> } random_key_in_global_variable_map = { limit … | none/any |  |
| `random_key_in_local_variable_map` | Iterate through all keys in a local variable map. key_in_local_variable_map = { variable = name <...> } random_key_in_local_variable_map = { limit = {… | none/any |  |
| `random_key_in_variable_map` | Iterate through all keys in a variable map. key_in_variable_map = { variable = name <...> } random_key_in_variable_map = { limit = { <triggers> } (opt… | none/any |  |
| `random_known_country` | Iterate through all known countries random_known_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_known_institution` | Iterate through all institutions a country knows of random_known_institution = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → institution |
| `random_left_flank` | Iterate through all subunits on the left-flank of a combat-side random_left_flank = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | combat_side | → sub_unit |
| `random_lent_loan` | Iterate through all loans that a country lent random_lent_loan = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → loan |
| `random_list` | randomly choose effects from a list random_list = { 10 = {                  # defines an option with base chance 10 trigger = { ... }    # optional tr… | none/any |  |
| `random_loan` | Iterate through all loans in a country random_loan = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → loan |
| `random_loan_lent_to_country` | Iterate through all loans a country has lent to the supplied borrower country random_loan_lent_to_country = { limit = { <triggers> } (optional) weight… | country | → loan |
| `random_location_in_area` | Iterate through all Locations in a area random_location_in_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | area | → location |
| `random_location_in_continent` | Iterate through all Locations in a continent random_location_in_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | continent | → location |
| `random_location_in_market` | Iterate through all locations in a market random_location_in_market = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | market | → location |
| `random_location_in_province` | Iterate through all Locations in a province random_location_in_province = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | province | → location |
| `random_location_in_province_definition` | Iterate through all Locations in a province definition random_location_in_province_definition = { limit = { <triggers> } (optional) weight = { mtth } … | province_definition | → location |
| `random_location_in_region` | Iterate through all Locations in a region random_location_in_region = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | region | → location |
| `random_location_in_religious_order` | Iterate through all locations (holdings) owned by a religious order random_location_in_religious_order = { limit = { <triggers> } (optional) weight = … | religious_order | → location |
| `random_location_in_scripted_geography` | Iterate through all locations in a scripted geography random_location_in_scripted_geography = { limit = { <triggers> } (optional) weight = { mtth } <e… | scripted_geography | → location |
| `random_location_in_sub_continent` | Iterate through all Locations in a sub-continent random_location_in_sub_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | sub_continent | → location |
| `random_location_in_the_world` | Iterate through all location random_location_in_the_world = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → location |
| `random_location_with_movement` | Iterate through all locations affected by the scope movement random_location_with_movement = { limit = { <triggers> } (optional) weight = { mtth } <ef… | movement | → location |
| `random_locations_with_town_rights_in_country` | Iterate through all locations with Town Rights in a country random_locations_with_town_rights_in_country = { limit = { <triggers> } (optional) weight … | country | → location |
| `random_log_scopes` | Log the current scope to the random log when this effect executes. Only use temprorarily for debugging purposes as it can introduce localized strings … | none/any |  |
| `random_loyal_subject` | Iterate through all loyal subject countries random_loyal_subject = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_maritime_area` | Iterate through all maritime areas for a country random_maritime_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → area |
| `random_market_center_in_country` | Iterate through all markets in a country which market centers are owned by the country random_market_center_in_country = { limit = { <triggers> } (opt… | country | → market |
| `random_market_in_world` | Iterate through all markets in the world random_market_in_world = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → market |
| `random_market_present_in_country` | Iterate through all markets in a country random_market_present_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → market |
| `random_market_with_merchants` | Iterate through all markets a country has active merchants random_market_with_merchants = { limit = { <triggers> } (optional) weight = { mtth } <effec… | country | → market |
| `random_mercenary` | Iterate through all mercenaries in the world random_mercenary = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → mercenary |
| `random_mercenary_sub_unit` | Iterate through all subunits in a Mercenary random_mercenary_sub_unit = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | mercenary | → sub_unit |
| `random_merchant_in_market` | Iterate through all merchants in a market random_merchant_in_market = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | market | → country |
| `random_movement` | Iterate through all movements random_movement = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → movement |
| `random_movement_in_country` | Iterate through all movements in a country random_movement_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → movement |
| `random_movement_in_culture` | Iterate through all movements in a culture random_movement_in_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | culture | → movement |
| `random_movement_in_religion` | Iterate through all movements in a religion random_movement_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → movement |
| `random_navy` | Iterate through all navies in a country random_navy = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → unit |
| `random_neighbor_area` | Iterate through all neighboring areas in a area random_neighbor_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | area | → area |
| `random_neighbor_country` | Iterate through all neighbour countries random_neighbor_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_neighbor_location` | Iterate through all neighbors of a location random_neighbor_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → location |
| `random_neighbor_province_definition` | Iterate through all neighboring ProvinceDefinitions in a ProvinceDefinition random_neighbor_province_definition = { limit = { <triggers> } (optional) … | province_definition | → province_definition |
| `random_new_world_goods` | Iterate through all newworld-goods random_new_world_goods = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → goods |
| `random_nomad_countries_in_location` | Iterate through all nomad pop countries in a location random_nomad_countries_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effe… | location | → country |
| `random_non_state_religion_location` | Iterate through all NonStateReligion locations in a country random_non_state_religion_location = { limit = { <triggers> } (optional) weight = { mtth }… | country | → location |
| `random_old_world_goods` | Iterate through all oldworld-goods random_old_world_goods = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → goods |
| `random_omen_in_country` | Iterate through all Omens active in a country random_omen_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → omen |
| `random_omen_in_god` | Iterate through all Omens associated with a God random_omen_in_god = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | god | → omen |
| `random_omen_in_religion` | Iterate through all Omens in a Religion random_omen_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | religion | → omen |
| `random_order_holding_in_country` | Iterate through all owned locations in a country that are holdings of a religious order random_order_holding_in_country = { limit = { <triggers> } (op… | country | → location |
| `random_other_core_country` | Iterate through all other countries which have a core on the current country random_other_core_country = { limit = { <triggers> } (optional) weight = … | country | → country |
| `random_other_country` | Iterate through all other countries random_other_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_other_great_power` | Iterate through all other great powers random_other_great_power = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_other_religion_in_same_group` | Iterate through all other religions that has the same group as Religion random_other_religion_in_same_group = { limit = { <triggers> } (optional) weig… | religion | → religion |
| `random_other_revolutionary` | Iterate through all other revolutionary countries random_other_revolutionary = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_overlord_or_above` | Iterate through your overlord, your overlord's overlord, and so on random_overlord_or_above = { limit = { <triggers> } (optional) weight = { mtth } <e… | country | → country |
| `random_ownable_location` | Iterate through all ownable location random_ownable_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → location |
| `random_ownable_location_in_area` | Iterate through all ownable Locations in an area random_ownable_location_in_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | area | → location |
| `random_ownable_location_in_continent` | Iterate through all ownable Locations in a continent random_ownable_location_in_continent = { limit = { <triggers> } (optional) weight = { mtth } <eff… | continent | → location |
| `random_ownable_location_in_province_definition` | Iterate through all ownable Locations in a province definition random_ownable_location_in_province_definition = { limit = { <triggers> } (optional) we… | province_definition | → location |
| `random_ownable_location_in_region` | Iterate through all ownable Locations in a region random_ownable_location_in_region = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | region | → location |
| `random_ownable_location_in_scripted_geography` | Iterate through all ownable Locations in a scripted geography random_ownable_location_in_scripted_geography = { limit = { <triggers> } (optional) weig… | scripted_geography | → location |
| `random_ownable_location_in_sub_continent` | Iterate through all ownable Locations in a sub continent random_ownable_location_in_sub_continent = { limit = { <triggers> } (optional) weight = { mtt… | sub_continent | → location |
| `random_owned_building` | Iterate through all the owned buildings in a country random_owned_building = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → building |
| `random_owned_foreign_building` | Iterate through all the owned foreign buildings in a country random_owned_foreign_building = { limit = { <triggers> } (optional) weight = { mtth } <ef… | country | → building |
| `random_owned_foreign_building_location` | Iterate through all the location of owned foreign buildings in a country random_owned_foreign_building_location = { limit = { <triggers> } (optional) … | country | → location |
| `random_owned_foreign_building_region` | Iterate through all the regions of owned foreign buildings in a country random_owned_foreign_building_region = { limit = { <triggers> } (optional) wei… | country | → region |
| `random_owned_location` | Iterate through all owned location in a country random_owned_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_owned_nomad_pop` | Iterate through all owned nomad pops in a country random_owned_nomad_pop = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → pop |
| `random_owned_non_rural_location` | Iterate through all owned non-rural locations in a country random_owned_non_rural_location = { limit = { <triggers> } (optional) weight = { mtth } <ef… | country | → location |
| `random_owned_rural_location` | Iterate through all owned rural locations in a country random_owned_rural_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_owner_in_region` | Iterate through all the countries that own locations in a region random_owner_in_region = { limit = { <triggers> } (optional) weight = { mtth } <effec… | region | → country |
| `random_parent` | Iterate through parents (order: father, mother) of a character. random_parent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → character |
| `random_participating_countries` | Iterate through all Countrys participating in 1 side of a combat random_participating_countries = { limit = { <triggers> } (optional) weight = { mtth … | combat_side | → country |
| `random_participating_units` | Iterate through all units participating in 1 side of a combat random_participating_units = { limit = { <triggers> } (optional) weight = { mtth } <effe… | combat_side | → unit |
| `random_past_court_dialect` | Iterate through all court dialects a country has had before random_past_court_dialect = { limit = { <triggers> } (optional) weight = { mtth } <effects… | country | → country |
| `random_past_liturgical_dialect` | Iterate through all liturgical dialects a country has had before random_past_liturgical_dialect = { limit = { <triggers> } (optional) weight = { mtth … | country | → country |
| `random_policy_in_law` | Iterate through all policies that are part of the law scope random_policy_in_law = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | law | → policy |
| `random_political_border_location` | Iterate through all owned location in a country which border another country. random_political_border_location = { limit = { <triggers> } (optional) w… | country | → location |
| `random_pop` | Iterate through all pops in a location or country random_pop = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location, country | → pop |
| `random_pop_in_religious_order_holding_location` | Picks a pop weighted by pop_size * owner-country's sway-over-estate across every holding location of the religious order, spawns a character there inh… | religious_order |  |
| `random_pops_supporting_rebel` | Iterate through all pops supporting a rebel random_pops_supporting_rebel = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | rebels | → pop |
| `random_port_in_country` | Iterate through all Ports in a country random_port_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_possible_disaster` | Iterate through all possible disasters for a country random_possible_disaster = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → disaster |
| `random_possible_parliament_issue` | Iterate through all possible parliament issues in a country's or an international organization's parliament random_possible_parliament_issue = { limit… | country, international_organization | → parliament_issue |
| `random_possible_policy` | Iterate through all possible policies of a Country that is not currently implemeted random_possible_policy = { limit = { <triggers> } (optional) weigh… | country | → policy |
| `random_possible_privilege` | Iterate through all possible & allowed estate privileges of a Country that is not currently implemeted random_possible_privilege = { limit = { <trigge… | estate | → estate_privilege |
| `random_possible_recruit_location` | Iterate through all possible recruit locations in a country random_possible_recruit_location = { limit = { <triggers> } (optional) weight = { mtth } <… | country | → location |
| `random_potential_expansion_target` | Iterate through all countries which are potential expansion targets for an AI country random_potential_expansion_target = { limit = { <triggers> } (op… | country | → country |
| `random_present_country` | Iterate through all countries in the specified geography random_present_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `random_present_culture_in_country` | Iterate through all cultures present in the country. random_present_culture_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effect… | country | → culture |
| `random_present_culture_in_location` | Iterate through all cultures present in the location. random_present_culture_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effe… | location | → culture |
| `random_present_overlord` | Iterate through all countries which have a subject in the specified geography random_present_overlord = { limit = { <triggers> } (optional) weight = {… | location, province_definition, area, region, sub_continent, continent, scripted_geography | → country |
| `random_present_religion_in_country` | Iterate through all religions present in the country. random_present_religion_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effe… | country | → religion |
| `random_present_religion_in_location` | Iterate through all religions present in the location. random_present_religion_in_location = { limit = { <triggers> } (optional) weight = { mtth } <ef… | location | → religion |
| `random_primary_or_accepted_culture` | Iterate through primary culture and all accepted cultures in a country. Primary is ordered first. random_primary_or_accepted_culture = { limit = { <tr… | country | → culture |
| `random_primary_or_accepted_or_tolerated_culture` | Iterate through primary culture and all accepted and all tolerated cultures in a country. Primary is ordered first. random_primary_or_accepted_or_tole… | country | → culture |
| `random_privateer` | Iterate through all privateers in the world random_privateer = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → privateer |
| `random_privateer_from_country` | Iterate through all privateers a country has random_privateer_from_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → privateer |
| `random_privateer_in_area` | Iterate through all privateers in a area random_privateer_in_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | area | → privateer |
| `random_production_method` | Iterate through all types of production methods. random_production_method = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → production_method |
| `random_production_method_of_building` | Iterate through all available production methods of the building. random_production_method_of_building = { limit = { <triggers> } (optional) weight = … | building | → production_method |
| `random_province` | Iterate through all provinces in a country random_province = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → province |
| `random_province_definition` | Iterate through all existing province_definition random_province_definition = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → province_definition |
| `random_province_definition_in_area` | Iterate through all province-definitions in an area random_province_definition_in_area = { limit = { <triggers> } (optional) weight = { mtth } <effect… | area | → province_definition |
| `random_province_definition_in_scripted_geography` | Iterate through all province_definitions in a scripted geography random_province_definition_in_scripted_geography = { limit = { <triggers> } (optional… | scripted_geography | → province_definition |
| `random_province_in_area` | Iterate through all provinces in an area random_province_in_area = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | area | → province |
| `random_province_in_province_definition` | Iterate through all provinces in a province-definition random_province_in_province_definition = { limit = { <triggers> } (optional) weight = { mtth } … | province_definition | → province |
| `random_rebel` | Iterate through all Rebels in a country random_rebel = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → rebels |
| `random_region` | Iterate through all existing regions random_region = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → region |
| `random_region_in_continent` | Iterate through all regions in a sub-continent random_region_in_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | sub_continent | → region |
| `random_region_in_scripted_geography` | Iterate through all regions in a scripted geography random_region_in_scripted_geography = { limit = { <triggers> } (optional) weight = { mtth } <effec… | scripted_geography | → region |
| `random_related_country` | Iterate through all related countries random_related_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_religion` | Iterate through all religions random_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → religion |
| `random_religion_for_god` | Iterate through all Religions of a God random_religion_for_god = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | god | → religion |
| `random_religion_in_religion_group` | Iterate through all religions in a religion group. random_religion_in_religion_group = { limit = { <triggers> } (optional) weight = { mtth } <effects>… | group | → religion |
| `random_religion_international_organization` | Iterate through all international organisations of a religion random_religion_international_organization = { limit = { <triggers> } (optional) weight … | religion | → international_organization |
| `random_religious_aspect` | Iterate through all religious aspects of a Country random_religious_aspect = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → religious_aspect |
| `random_religious_focus` | Iterate through all completed religious focuses of a Country random_religious_focus = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | country | → religious_focus |
| `random_religious_order` | Iterate through all religious orders random_religious_order = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → religious_order |
| `random_religious_order_in_country` | Iterate through all religious orders present in a country random_religious_order_in_country = { limit = { <triggers> } (optional) weight = { mtth } <e… | country | → religious_order |
| `random_religious_order_in_location` | Iterate through all religious orders present at a location random_religious_order_in_location = { limit = { <triggers> } (optional) weight = { mtth } … | location | → religious_order |
| `random_religious_order_in_religion` | Iterate through all religious orders of a religion random_religious_order_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effects… | religion | → religious_order |
| `random_religious_order_type` | Iterate through all religious order types random_religious_order_type = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → religious_order_type |
| `random_religious_school_in_religion` | Iterate through all Religious Schools in a Religion random_religious_school_in_religion = { limit = { <triggers> } (optional) weight = { mtth } <effec… | religion | → religious_school |
| `random_rented_out_mercenary` | Iterate through mercenaries a country has rented out to the market random_rented_out_mercenary = { limit = { <triggers> } (optional) weight = { mtth }… | country | → mercenary |
| `random_required_goods` | Iterate through all goods required by the scope production method. random_required_goods = { limit = { <triggers> } (optional) weight = { mtth } <effe… | production_method | → goods |
| `random_reserves` | Iterate through all subunits on the reserve of a combat-side random_reserves = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | combat_side | → sub_unit |
| `random_retreated` | Iterate through all subunits on the retreated of a combat-side random_retreated = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | combat_side | → sub_unit |
| `random_revolutionary` | Iterate through all revolutionary states random_revolutionary = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → country |
| `random_right_flank` | Iterate through all subunits on the right-flank of a combat-side random_right_flank = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | combat_side | → sub_unit |
| `random_rival` | Iterate through all rival countries random_rival = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_road_type` | Iterate through all the road types random_road_type = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → road_type |
| `random_royal_marriage` | Iterate through all royal married countries random_royal_marriage = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_ruled_international_organization` | Iterate through IOs a character rules random_ruled_international_organization = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → international_organization |
| `random_ruler` | Iterate through all characters that have ever been rulers in a country, including the dead random_ruler = { limit = { <triggers> } (optional) weight =… | country | → character |
| `random_ruling_countries` | Iterate through countries a character rules random_ruling_countries = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → country |
| `random_sound_toll_in_country` | Iterate through all Sound Tolls in a country random_sound_toll_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → location |
| `random_spouse` | Iterate through all spouses of a character random_spouse = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → character |
| `random_spy_network_built_in_us` | Iterate through all  countries building spy networks random_spy_network_built_in_us = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | country | → country |
| `random_sub_continent` | Iterate through all existing sub_continents random_sub_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → sub_continent |
| `random_sub_continent_in_continent` | Iterate through all sub-continents in a continent random_sub_continent_in_continent = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | continent | → sub_continent |
| `random_sub_continent_in_scripted_geography` | Iterate through all subcontinents in a scripted geography random_sub_continent_in_scripted_geography = { limit = { <triggers> } (optional) weight = { … | scripted_geography | → sub_continent |
| `random_sub_unit` | Iterate through all subunits in a unit random_sub_unit = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | unit | → sub_unit |
| `random_subject` | Iterate through all subject countries random_subject = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → country |
| `random_subject_or_below` | Iterate through all subject countries and their subject countries, and so on random_subject_or_below = { limit = { <triggers> } (optional) weight = { … | country | → country |
| `random_tolerated_culture` | Iterate through all Tolerated cultures in a country random_tolerated_culture = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → culture |
| `random_town_rights_in_country` | Iterate through all Town Rights in a country random_town_rights_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → town_rights |
| `random_town_rights_in_location` | Iterate through all Town Rights in a Location random_town_rights_in_Location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → town_rights |
| `random_trade` | Iterate through all trades in a Country random_trade = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → trade |
| `random_trait` | Iterate through all traits of a character. random_trait = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | character | → trait |
| `random_union_partner` | Iterate through all countries which are in a personal union with the current country scope. random_union_partner = { limit = { <triggers> } (optional)… | country | → country |
| `random_unit` | Iterate through all units in a country random_unit = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → unit |
| `random_unit_in_location` | Iterate through all units in a location random_unit_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → unit |
| `random_valid_religion_for_aspect` | Iterate through all religion that an aspect can be for random_valid_religion_for_aspect = { limit = { <triggers> } (optional) weight = { mtth } <effec… | religious_aspect | → religion |
| `random_voter` | Iterate through all voters in an active resolution random_voter = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | active_resolution | → country |
| `random_war` | Iterate through all wars going on globally random_war = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → war |
| `random_war_participant` | Iterate through all participants of a war random_war_participant = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | war | → country |
| `random_weather_system_in_location` | Iterate through all weather systems in a location random_weather_system_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> … | location | → weather_system |
| `random_west_of_province_definition` | Iterate through all province-definitions west of a province-definition random_west_of_province_definition = { limit = { <triggers> } (optional) weight… | province_definition | → province_definition |
| `random_work_of_art` | Iterate through all WorkOfArts in the world random_work_of_art = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | none/any | → work_of_art |
| `random_work_of_art_by_creator` | Iterate through all work_of_art by a particular artist random_work_of_art_by_creator = { limit = { <triggers> } (optional) weight = { mtth } <effects>… | character | → work_of_art |
| `random_work_of_art_in_country` | Iterate through all work_of_art in a country random_work_of_art_in_country = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | country | → work_of_art |
| `random_work_of_art_in_location` | Iterate through all work_of_art in a location random_work_of_art_in_location = { limit = { <triggers> } (optional) weight = { mtth } <effects> } | location | → work_of_art |
| `ransom_prisoners` | Prisoners are released back to their countries for a price | unit |  |
| `recall_lent_unit` | Returns a lent unit to its owner (lender) and ends the loan. Does nothing if the mercenary is not a lent unit. | mercenary |  |
| `redeem_bonds` | redeems the targeted government bond; country pays back face value | none/any | → loan |
| `refresh_map_colors` | Refreshes the map colours | none/any |  |
| `refund_price` | Refund a Price to a country | country | → price |
| `release_non_cores` | releases all non-core provinces | country |  |
| `relocate_market` | moves the market to the target location | market | → location |
| `remove_accepted_culture` | Removes an accepted culture from a country | country | → culture |
| `remove_additional_migration` | adds a new migration source to a colonial charter. usage: remove_additional_migration = <province> | colonial_charter |  |
| `remove_all_area_preferences` | Removes all area preferences from the AI of a country | country |  |
| `remove_all_casus_belli` | removes all casus belli on the target country | country | → country |
| `remove_all_casus_belli_of_type` | Removes the casus belli of the specified type against ALL other countries | country | → casus_belli |
| `remove_all_religious_orders` | remove_all_religious_orders = yes Detaches the current location from every religious order presently holding it, indiscriminately. | location |  |
| `remove_antagonism` | Removes an antagonism modifier, remove_antagonism = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `remove_area_preference` | Removes an area preference from the AI of a country | country |  |
| `remove_avatar` | removed an avatar from a country | country | → avatar |
| `remove_breach` | removes a breach from the current siege | siege |  |
| `remove_bureaucracy` | removes the supplied bureaucracy from the country's government. | country | → bureaucracy |
| `remove_casus_belli` | Removes casus_belli (target = x type = y <province = prov>} | country |  |
| `remove_character_allegiance` | Remove the character scope from the rebel group they are part of. | character |  |
| `remove_character_modifier` | Remove a modifier from a character remove_character_modifier = name | character |  |
| `remove_colonial_claim` | Removed the colonial claim in the target province_definition | none/any | → province_definition |
| `remove_commander` | removes the target character from his unit | none/any | → character |
| `remove_cooldown` | Removes a cooldown for a country, character or international organization. Usage: remove_cooldown = <cooldown token> | country, character, international_organization |  |
| `remove_core` | makes the location no longer a core of the target country | location | → country |
| `remove_country_from_circle` | Remove a country from an imperial circle. If the country was the leader, the leader is also cleared. | imperial_circle | → country |
| `remove_country_from_international_organization` | removes a country from an international organization | international_organization | → country |
| `remove_country_modifier` | Remove a modifier from a country remove_country_modifier = name | country |  |
| `remove_culture_bonus_advances` | Removes all bonus advances from a culture's definition from the country remove_culture_bonus_advances = scope:culture | country | → culture |
| `remove_discriminated_culture` | Removes a discriminated culture from a country | country | → culture |
| `remove_dynasty_modifier` | Remove a modifier from a dynasty remove_dynasty_modifier = name | dynasty |  |
| `remove_enemy_from_international_organization` | removes a country from an international organization | international_organization | → country |
| `remove_estate_private_regiment` | Removes one matching unraised private regiment definition from an estate. estate = <type> unit_type = <subunit_def> | country |  |
| `remove_extended_winter` | Removed extended winter to target area | none/any | → area |
| `remove_from_cabinet` | Removes the character from the cabinet | country | → character |
| `remove_from_global_variable_map` | Removes the target from a variable map remove_from_variable_map = { name = X key = Y } Where X is the name of the variable Where Y is an event target | none/any |  |
| `remove_from_international_organization` | remove a location from an international organization | location | → international_organization |
| `remove_from_list` | Removes the current scope from a named list remove_from_list = <string> | none/any |  |
| `remove_from_local_variable_map` | Removes the target from a variable map remove_from_variable_map = { name = X key = Y } Where X is the name of the variable Where Y is an event target | none/any |  |
| `remove_from_variable_map` | Removes the target from a variable map remove_from_variable_map = { name = X key = Y } Where X is the name of the variable Where Y is an event target | none/any |  |
| `remove_global_variable` | Removes the specified global variable remove_global_variable = variable_name | none/any |  |
| `remove_god` | removes a god from a country | country | → god |
| `remove_goods_import_trade_policy` | Removes the global import tariff/subvention policy for a good. Usage: remove_goods_import_trade_policy = { goods = <goods> } | country |  |
| `remove_historical_rival` | removes target country as an historical rival | country | → country |
| `remove_international_organization_modifier` | Remove a modifier from an international organization remove_international_organization_modifier = name | international_organization |  |
| `remove_law` | Makes a law get no options, which may make it dissapear | country | → law |
| `remove_law_from_international_organization` | remove a law from an international organization | international_organization | → law |
| `remove_list_global_variable` | Removes the target from a global variable list remove_list_global_variable = { name = X target = Y } Where X is the name of the variable Where Y is an… | none/any |  |
| `remove_list_local_variable` | Removes the target from a local variable list remove_list_local_variable = { name = X target = Y } Where X is the name of the variable Where Y is an e… | none/any |  |
| `remove_list_variable` | Removes the target from a variable list on the current scope remove_list_variable = { name = X target = Y } Where X is the name of the variable Where … | none/any |  |
| `remove_local_variable` | Removes the specified local variable remove_local_variable = variable_name | none/any |  |
| `remove_location_from_international_organization` | remove a location from an international organization | international_organization | → location |
| `remove_location_modifier` | Remove a modifier from a location remove_location_modifier = name | location |  |
| `remove_merchant_power` | Removes temporary power to a merchant (country = <country>  key = localisation} | market |  |
| `remove_migration` | removes a migration {  <from = provdef> <from_location = location> <to = provdef> <to_location = location> <culture = x> <religion = y> <type = z>  mo… | none/any |  |
| `remove_movement_modifier` | Remove a modifier from a movement remove_movement_modifier = name | movement |  |
| `remove_omen` | removes an omen from a country | country | → omen |
| `remove_opinion` | Removes an opinion modifier, remove_opinion = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `remove_policy` | Remove a policy from a country. | country | → policy |
| `remove_policy_from_international_organization` | remove a policy from an international organization | international_organization | → policy |
| `remove_pop_allegiance` | removed allegienace of a pop | pop |  |
| `remove_province_modifier` | Remove a modifier from a province remove_province_modifier = name | province |  |
| `remove_rebel_modifier` | Remove a modifier from a rebel remove_rebel_modifier = name | rebels |  |
| `remove_reform` | removes the government reform desired. | country | → government_reform |
| `remove_relation` | Removes a relation with the target country. Usage: remove_relation = { type = <scripted relation type key> first = <country> second = <country> } | none/any |  |
| `remove_religion_modifier` | Remove a modifier from a religion remove_religion_modifier = name | religion |  |
| `remove_religious_aspect` | removed a religious_aspect from a country | country | → religious_aspect |
| `remove_religious_focus` | Remove a completed religious focus from the country scope | country | → religious_focus |
| `remove_religious_order` | Detaches the current location from the supplied religious order — the location stops being a holding, leaving the order's location list and the locati… | location | → religious_order |
| `remove_rival` | removes target country as a rival | country | → country |
| `remove_ruler` | Removes the character as ruler from the target country | character | → country |
| `remove_scripted_proximity_from` | Removes scripted proximity from target location based on target = <location> and key = <string> | location |  |
| `remove_spreader` | Stops a character from spreading the movement. usage: remove_spreader = <character> | movement | → character |
| `remove_temporary_demand` | Remove a temporary demand in a market | market | → demand |
| `remove_tolerated_culture` | Removes an Tolerated culture from a country | country | → culture |
| `remove_trait` | removes a trait to a character | character | → trait |
| `remove_traits_of_category` | Removes all traits of a specified category. Supports the categories: ruler, general, admiral, artist, explorer, child, religious_figure | character |  |
| `remove_truce_with` | Removes the truce with the target country | country | → country |
| `remove_trust_equilibrium` | Removes a trust equilibrium modifier, remove_trust = { modifier = X  target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `remove_unit_modifier` | Remove a modifier from a unit remove_unit_modifier = name | unit |  |
| `remove_variable` | Removes the specified variable on the current scope remove_variable = variable_name | none/any |  |
| `remove_vfx` | Removes VFX effects on the map | location |  |
| `remove_vote` | Removes the vote for a country on a resolution in an international organization or situation. Usage: remove_vote = { voter = <country> resolution = <r… | international_organization, situation |  |
| `rename_location` | changes the name of a location | location |  |
| `request_ransom_prisoners` | An offer to ransom the unit's troops is sent to all original owners | unit |  |
| `reroll_parliament_agendas` | Re-roll the parliament agenda of every estate currently participating in the parliament. | country, international_organization |  |
| `rescue_sub_unit` | Rescues a sub unit | mercenary | → sub_unit |
| `research_advance` | Research a specific advance research_advance = name | country | → advance_type |
| `reset_character_location` | Moves a character back to their default location | character |  |
| `reset_character_religious_figure` | stops a character being a religious figure | character |  |
| `reset_regency` | performs the start_effect of a regency again | country |  |
| `reset_ruler_title` | resets a ruler's title so it gets generated again | character |  |
| `resume_expedition` | Resumes an expedition previously stalled by stall_expedition, so it can move again on its next tick. Usage: resume_expedition = scope:expedition | none/any | → expedition |
| `reverse_add_antagonism` | Adds a reverse antagonism modifier, reverse_add_antagonism = { modifier = X target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `reverse_add_opinion` | Adds an reverse opinion modifier, reverse_add_opinion = { modifier = X target = Z scale = {script value} } X is a scripted modifier name. | country |  |
| `reverse_add_trust_equilibrium` | Adds a reverse trust equilibrium modifier, reverse_add_trust_equilibrium = { modifier = X target = Z scale = {script value} } X is a scripted modifier… | country |  |
| `reverse_change_cultural_view` | Sets the targets cultural view of current culture.. { target = x change = relation_change } | culture |  |
| `reverse_change_religion_view` | Sets the targets religion view of current religion.. { target = x change = relation_change } | religion |  |
| `reverse_set_cultural_view` | Sets the targets cultural view of current culture.. { target = x to value = relation_level } | culture |  |
| `reverse_set_religious_view` | Sets the targets Religious view of current Religion.. { target = x to value = relation_level> | religion |  |
| `reverse_set_school_opinion` | Sets the targets school_opinion of current ReligiousSchool.. { target = x to value = relation_level> | religious_school |  |
| `revoke_estate_privilege` | revokes a special estate priviledge. | country | → estate_privilege |
| `revoke_town_rights` | Revoke a specific town rights | none/any | → town_rights |
| `revoke_town_rights_of_type` | revoke town-rights of a type from a location | location | → town_rights_type |
| `round_global_variable` | Rounds a global variable to the nearest specified value round_global_variable = { name = X nearest = Y } Where X is the name of the variable Where Y i… | none/any |  |
| `round_local_variable` | Rounds a local variable to the nearest specified value round_local_variable = { name = X nearest = Y } Where X is the name of the variable Where Y is … | none/any |  |
| `round_variable` | Rounds a variable to the nearest specified value on the current scope round_variable = { name = X nearest = Y } Where X is the name of the variable Wh… | none/any |  |
| `save_scope_as` | Saves the current scope as an arbitrarily-named target to be referenced later in the (unbroken) event chain save_scope_as = <string> | none/any |  |
| `save_scope_value_as` | Saves a numerical or bool value as an arbitrarily-named target to be referenced later in the (unbroken) event chain save_scope_value_as = { name = <st… | none/any |  |
| `save_temporary_scope_as` | Saves the current scope as an arbitrarily-named temporary target to be referenced later in the same effect save_temporary_scope_as = <string> | none/any |  |
| `save_temporary_scope_value_as` | Saves a numerical or bool value as an arbitrarily-named temporary target to be referenced later in the same effect save_temporary_scope_value_as = { n… | none/any |  |
| `sell_bonds` | issues a government bond; country receives gold and owes bond_interest monthly | country |  |
| `sell_goods_from_location` | Adds goods to a market's stockpile, selling them from a specific location, and the location's owner country selling them will get the sale value. Usag… | market |  |
| `sell_goods_to_market` | A country sells goods into a target market, receiving the sell price for its capital location in gold and adding the goods to the market stockpile. Ha… | country |  |
| `sell_prisoners_into_slavery` | Prisoners get sold as slaves | unit |  |
| `set_active_chivalric_order` | Unlocks the specified chivalric order for this country and refreshes the active order immediately. | country | → chivalric_order |
| `set_age_preference` | Sets age preference for this age for a country | country |  |
| `set_army_tradition` | Sets army tradition | country, international_organization |  |
| `set_art_owner` | Sets a new owner for the work of art | work_of_art | → country |
| `set_art_worth` | Sets the worth override of a work of art. -1 = priceless, 0 = use type default, positive = gold value. | work_of_art |  |
| `set_as_commander` | sets the commander of a unit to the specified character | unit | → character |
| `set_as_designated_heir` | Sets the target character as the designated heir for the scope country. Usage: set_as_designated_heir = <character> or set_as_designated_heir = { targ… | country | → character |
| `set_automated_system` | "Activate or deactivate the automation of a system for one country. Will only have an effect if this country is played or will be played by a player. … | country |  |
| `set_bankruptcy` | Make the country either bankrupt if set to yes, or get it out of its current bankruptcy if set to no. | country |  |
| `set_cabinet_action` | Sets cabinet action of this cabinet to <type> with scripted params | cabinet |  |
| `set_canal_open` | Opens or closes the canal passing through this location. Requires the location to be a canal through-location (registered in the adjacency CSV). | location |  |
| `set_canal_restricted` | Sets the restriction flag on the canal at this location. Restricted canals only allow allies / subjects / overlords through. | location |  |
| `set_capital` | Sets the target location as capital of the country | country | → location |
| `set_character_location` | Moves a character somewhere | character | → location |
| `set_character_religious_figure` | Makes a character a religious figure | character | → religious_figure |
| `set_child_education` | Set the education type of a child set_child_education = education | character | → child_education |
| `set_circle_leader` | Set the leader of an imperial circle. The country must already be a member. | imperial_circle | → country |
| `set_collection_pin` | Sets collection pin. | none/any |  |
| `set_colonial_charter_goal` | Sets the outcome goal for a colonial charter. Usage: set_colonial_charter_goal = { target = <charter> goal = <goal_key> subject = <country> } | none/any |  |
| `set_country_employment_system` | Sets the country's employment system | country | → employment_system |
| `set_country_military_stance` | Sets the country's military stance | none/any | → military_stance |
| `set_country_rank` | Sets the country rank | country | → country_rank |
| `set_court_language` | Sets the court_language to the target dialect | country | → dialect |
| `set_cultural_view` | Sets the cultural view of target.. { target = x to value = relation_level } | culture |  |
| `set_devotion` | Sets devotion | country, international_organization |  |
| `set_disease_presence` | sets the presence of a disease in a location or subunit. usage: set_disease_presence = { disease = <source disease outbreak> value = <script value> } | location, sub_unit |  |
| `set_doom` | Sets doom | country, international_organization |  |
| `set_dynasty_name_type` | changes dynasty to have a specific dynasty_name_type (default, location, patronym, descendant) | dynasty |  |
| `set_entrenchment` | Sets the entrenchment of a bureaucracy to the supplied value | bureaucracy |  |
| `set_ethnicity` | Set the ethnicity of a character | character | → ethnicity |
| `set_expedition_leader` | reassigns the leader of an active expedition, e.g. after the original leader's death | expedition | → character |
| `set_expedition_outcome` | Records a localization key describing how this expedition turned out, shown in the completed expeditions list. set_expedition_outcome = my_outcome_loc… | expedition |  |
| `set_expedition_status_to_deviation` | Sets the expedition's status to Deviation. Usage: set_expedition_status_to_deviation = yes (scope: expedition) | expedition |  |
| `set_expedition_status_to_in_port` | Sets the expedition's status to InPort. Usage: set_expedition_status_to_in_port = yes (scope: expedition) | expedition |  |
| `set_expedition_status_to_traveling` | Sets the expedition's status to Traveling. Usage: set_expedition_status_to_traveling = yes (scope: expedition) | expedition |  |
| `set_first_name` | Set the first name of a character to the defined string or scoped character | character |  |
| `set_garrison_size` | Sets the garrison size of the target location to the specified value. | location |  |
| `set_global_variable` | Sets a global variable set_global_variable = { name = X value = Y days = Z } Where X is the name of the variable used to then access it Where Y is any… | none/any |  |
| `set_gold` | Sets gold | country, international_organization |  |
| `set_goods_export_trade_policy` | Sets a global export tariff (positive modifier) or export subvention (negative modifier) on a good. Slider range -1.0 to 1.0. Replaces any existing ex… | country |  |
| `set_goods_import_trade_policy` | Sets a global import tariff (positive modifier) or import subvention (negative modifier) on a good. Slider range -1.0 to 1.0. Replaces any existing no… | country |  |
| `set_government_power` | Sets government power | country, international_organization |  |
| `set_harmony` | Sets Harmony | country, international_organization |  |
| `set_head_character` | Sets the religious order's head character to the supplied character scope (used inside on_leader_death blocks after create_character). | religious_order | → character |
| `set_honor` | Sets honor | country, international_organization |  |
| `set_horde_unity` | Sets horde_unity | country, international_organization |  |
| `set_inflation` | Sets inflation | country, international_organization |  |
| `set_international_organization_icon` | Sets the illustration/icon override for an international organization, looked up under international_organization_types/specific_ios/<icon>.dds. Usage… | international_organization |  |
| `set_karma` | Sets karma | country, international_organization |  |
| `set_leader_country` | set the leader country of an international organization | international_organization | → country |
| `set_legitimacy` | Sets legitimacy | country, international_organization |  |
| `set_liturgical_language` | Sets the Liturgical_language to the target language | country | → dialect |
| `set_loc_key` | changes the localisation key of a loan | loan |  |
| `set_local_variable` | Sets a local variable set_local_variable = { name = X value = Y days = Z } Where X is the name of the variable used to then access it Where Y is any e… | none/any |  |
| `set_locked` | sets the scope trade to be locked or not | trade |  |
| `set_lowborn` | The character gets removed from its current dynasty, making it a lowborn | character |  |
| `set_maintenance` | Sets the maintenance of a bureaucracy to the supplied value | bureaucracy |  |
| `set_manpower` | Sets Manpower | country, international_organization |  |
| `set_navy_tradition` | Sets navy tradition | country, international_organization |  |
| `set_needs_reform` | changes the needs_reform status of a religion | religion |  |
| `set_new_foreign_ruler` | Makes the target_character the new ruler without forcing them to move to the country | country | → character |
| `set_new_foreign_ruler_no_update` | Makes the target_character the new ruler without forcing them to move to the country, without updating any unions or fiefdom-like titles the character… | country | → character |
| `set_new_ruler` | Makes the target_character the new ruler | country | → character |
| `set_new_ruler_no_update` | Makes the target_character the new ruler, without updating any unions or fiefdom-like titles the character has | country | → character |
| `set_nickname` | Set the nickname of a character to the defined string | character |  |
| `set_parliament_active` | Set parliament active or not. The issue of the parliament will be removed without triggering any of its effects if set to no. | country, international_organization |  |
| `set_parliament_issue` | Set the specified issue as the topic of the parliament. | country, international_organization | → parliament_issue |
| `set_parliament_issue_support` | sets base support for the issue when calling a parliament | country, international_organization |  |
| `set_parliament_location` | Sets the parliament location. Use immediately before 'set_parliament_active = yes' | country, international_organization | → location |
| `set_parliament_type` | Set the specified parliament type for a country | country, international_organization | → parliament_type |
| `set_participated_in_parliament` | Mark the country as having participated in the parliament of the target international organization. Usage: set_participated_in_parliament = { internat… | country |  |
| `set_personality` | Sets the AI personality for a country | country | → ai_personality |
| `set_political_influence` | Sets political influence | country, international_organization |  |
| `set_prestige` | Sets prestige | country, international_organization |  |
| `set_production_method` | Sets the production method of a building | building | → production_method |
| `set_province_capital` | Locks the province capital to the target location, preventing automatic reassignment. | province | → location |
| `set_purity` | Sets purity | country, international_organization |  |
| `set_rebel_demands` | Sets the rebel demand for this rebel, overriding the trigger-based auto-resolve. Usage: scope:my_rebel = { set_rebel_demands = my_demand_key } | rebels |  |
| `set_regent` | Makes the target_character the acting regent | country | → character |
| `set_religious_influence` | Sets religious influence | country, international_organization |  |
| `set_religious_order_zeal` | Sets the religious order's global Zeal to the supplied value (clamped 0..100). Bypasses the auto-dissolve-at-zero rule used by change_religious_order_… | religious_order |  |
| `set_religious_school` | Sets the religious_school to the target school | country | → religious_school |
| `set_religious_view` | Sets the Religious view of target.. { target = x to value = relation_level> | religion |  |
| `set_republican_tradition` | Sets republican_tradition | country, international_organization |  |
| `set_revolt_target` | sets the original attacker of the revolt war | war | → country |
| `set_revolution` | Make the country either revolutionary if set to yes, or get remove its revoutionary status if set to no. | country |  |
| `set_revolution_target` | Make the country to the target of the revolution if set to yes, or remove it when set to no. | country |  |
| `set_righteousness` | Sets Righteousness | country, international_organization |  |
| `set_rite_power` | Sets rite power | country, international_organization |  |
| `set_sailors` | Sets sailors | country, international_organization |  |
| `set_school_opinion` | Sets the school_opinion of target.. { target = x to value = relation_level> | religious_school |  |
| `set_self_control` | Sets Self Control | country, international_organization |  |
| `set_show_stats` | Shows or hides the character's adm/dip/mil abilities in the UI. Hidden abilities still count for gameplay and AI | character |  |
| `set_societal_value` | Sets societal value | country |  |
| `set_stability` | Sets stability | country, international_organization |  |
| `set_subsidized` | change whether a building is subsidised or not | building |  |
| `set_target_of_international_organization` | Sets the target of an international organization | international_organization | → country |
| `set_to_limited_random_stat` | Sets the stats of the character scope to a random value between a min and max value. Usage: set_to_limited_random_stat = { ability = adm/dip/mil max =… | character |  |
| `set_tribal_cohesion` | Sets tribal_cohesion | country, international_organization |  |
| `set_tutorial_var` | Sets tutorial var. | none/any |  |
| `set_unit_size` | Sets the number of people in each subunit in the Unit to the given percentage of maximum | unit |  |
| `set_variable` | Sets a variable on the current scope set_variable = { name = X value = Y days = Z } Where X is the name of the variable used to then access it Where Y… | none/any |  |
| `set_vote` | Sets the vote for a country on a resolution in an international organization or situation. Usage: set_vote = { voter = <country> resolution = <resolut… | international_organization, situation |  |
| `set_war_exhaustion` | Sets WarExhaustion | country, international_organization |  |
| `set_war_leader` | Makes the country scope the new war leader for the target war. Country must already be an active participant. Usage: set_war_leader = { war = <war sco… | country |  |
| `set_yanantin` | Sets yanantin | country, international_organization |  |
| `show_as_tooltip` | Enclosed effects are only shown in tooltips (but are not actually executed) show_as_tooltip = { <more effects> } | none/any |  |
| `sort_global_variable_list` | Sorts a global variable list sort_global_variable_list = { name = X order = Y } Where X is the name of the variable Where Y is some math script to det… | none/any |  |
| `sort_local_variable_list` | Sorts a local variable list sort_local_variable_list = { name = X order = Y } Where X is the name of the variable Where Y is some math script to deter… | none/any |  |
| `sort_variable_list` | Sorts a variable list on the current scope sort_variable_list = { name = X order = Y } Where X is the name of the variable Where Y is some math script… | none/any |  |
| `spawn_army_levy_unit` | Spawn an army levy unit for the target country in the current location scope | none/any | → country |
| `spawn_disease` | Spawns a disease in a location or on a subunit. usage: spawn_disease = { disease = <disease> value = <script value> } | location | → disease_outbreak |
| `spawn_movement` | Spawns a movement in a location or on a subunit. usage: spawn_movement = { movement_definition = <movement_definition> supporters = <script value> } | location | → movement |
| `spawn_navy_levy_unit` | Spawn a navy levy unit for the target country in the current location scope | none/any | → country |
| `split_pop` | Splits a pop and makes changes to it { size = <fixed num> fraction = <percentage of size> religion = x type = poptype culture = y location = z } If bo… | pop |  |
| `spread_to_location` | Spreads the movement to a new location. usage: spread_to_location = { location = <location> [supporters_percent = <> or supporters = <>, pick one] } | movement |  |
| `stall_expedition` | Stalls an active expedition indefinitely until resume_expedition is called on it. Usage: stall_expedition = scope:expedition | none/any | → expedition |
| `stall_exploration` | enable/disable stall on exploration | exploration |  |
| `start_canal_construction` | Starts canal construction at the scoped location. The location must be a canal through-location. | location |  |
| `start_civil_war` | Make the rebels rise up against the country, starting a civil war. Has no effect if the country is already in a civil war. Will scope immediately into… | rebels | → country |
| `start_conquistador` | Creates a new conquistador = { area = <area> location = <location in area to make capital> character = <character> price = <price> price_modifier = <n… | country |  |
| `start_expedition` | starts an expedition of the given type along the given path | country |  |
| `start_expedition_wait_construction` | Creates a construction at the expedition's current location that the expedition waits on; on_construction_finished fires when it finishes. Usage: star… | expedition |  |
| `start_exploration` | Creates a new exploration = { area = <area> character = <character> price = <price> price_modifier = <number> } in the current country | country |  |
| `start_mission` | The country starts immediately the specified mission. Will not do anything if a mission is active already. start_mission = { mission = <mission scope>… | country |  |
| `start_periphora_towards` | Starts a periphora towards the target location. Art will move towards the destination slowly in a procession. | work_of_art | → location |
| `start_revolt` | Make the rebels rise up against the country in the form of a revolt. | rebels |  |
| `start_tutorial_lesson` | Starts the tutorial lesson with the given key. Does nothing if the tutorial is not running, the lesson is completed (or already running), or the lesso… | none/any |  |
| `start_weather_system` | Starts off a new weather system. Usage: start_weather_system = { width = <pixels> length = <pixels> strength = [0..1] speed = <pixels per day> type = … | none/any | → weather_system |
| `start_work_of_art` | Makes the target character start working on some art. start_work_of_art = { work_of_art_type = work_of_art_type:<type> (optional, will start a random … | character |  |
| `stop_annexing_country` | scope country stops annexing the target country | country | → country |
| `stop_cabinet_action` | Stop the cabinet doing stuff | cabinet |  |
| `stop_tutorial` | Stops the tutorial. | none/any |  |
| `support_rebel` | scope country starts supporting the target rebel | country | → rebels |
| `switch` | Switch on a trigger for the evaluation of another trigger with an optional fallback trigger. switch = { trigger = simple_assign_trigger case_1 = { <ef… | none/any |  |
| `take_over_all_wars` | takes over all wars where target country is warleader | country | → country |
| `test_log` | Log a string to the test log when this effect executes, test_log = message, the message can be a localization string with ROOT, SCOPE and PREV availab… | none/any |  |
| `transfer_gold_from_estate` | Country gains gold transferred from a named estate's pool; clamped to BURGHER_GRANT_MAX_FRACTION of the estate's gold | country |  |
| `transfer_location_occupation` | Transfers occupation of a Location to the target country | location | → country |
| `transfer_subject` | Copy the name and adjective of the target country and apply it to the current country scope | country | → country |
| `transfer_yearly_gold` | Transfers a proportion of your yearly gold to the target country | country |  |
| `transfer_yearly_manpower` | Transfers a proportion of your yearly Manpower to the target country | country |  |
| `transfer_yearly_sailors` | Transfers a proportion of your yearly sailors to the target country | country |  |
| `trigger_event_non_silently` | triggers an event or on_action, but shows the name of the event trigger_event_non_silently = { id = X days/months/years = Y } (for events) | none/any |  |
| `trigger_event_silently` | triggers an event or on_action trigger_event = { id = X days/months/years = Y } (for events) or trigger_event = { on_action = X days/months/years = Y … | none/any |  |
| `unlock_unit` | Unlocks a locked Unit | unit |  |
| `unset_participated_in_parliament` | Remove the mark of the country as having participated in the parliament of the target international organization. | country | → international_organization |
| `update_leadership` | Update the leadership panel of the target international organization. | none/any | → international_organization |
| `while` | Repeats enclosed effects while limit criteria are met or until set iteration count is reached while = { limit = { <triggers> } <effects> } while = { c… | none/any |  |
| `white_peace` | forces a white peace in the target war | none/any | → war |
