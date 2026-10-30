# 参考资料：common 目录 → 覆盖文档映射

> **用途**：想查某个 `common\` 类目本库哪篇讲过、或找哪些目录还没有专属字段档，看这张表。
> **来源**：`game\in_game\common` 133 个目录实查 + 本库 md 全文计数（排除生成的 name 清单），**EU5 1.4.5**，2026-10-30。
> **体量**：约 4 分钟

## 摘要

- 共 **133** 个目录；有专属 `fields\common-<目录>.md` 的 **73** 个，**无专属档 60** 个（多数被合并档覆盖，见下表「主要覆盖」列）。
- 「主要覆盖」为空或命中次数很低 = 真正的知识空白，优先补。

## 映射表

| 目录 | 档数 | KB | 专属 fields 档 | 主要覆盖档（命中次数） |
|---|---:|---:|:---:|---|
| `advances` | 227 | 829 | ✔ | vanilla-tech-and-age(15), common-advances(5) |
| `age` | 1 | 9 |  | modifiers-country(72), vanilla-tech-and-age(56) |
| `ai_diplochance` | 1 | 6 | ✔ | vanilla-ai(12), vanilla-combat(8) |
| `ai_personalities` | 1 | 5 | ✔ | vanilla-ai(15), common-ai_personalities(7) |
| `ai_scripted_expansion_score` | 3 | 2 |  | common-ai_scripted_expansion(3), INDEX(2) |
| `ai_scripted_expansion_target` | 4 | 4 |  | common-ai_scripted_expansion(3), vanilla-ai(2) |
| `alert_descriptions` | 1 | 21 | ✔ | vanilla-gui(4), INDEX(3) |
| `area_preferences` | 2 | 20 | ✔ | vanilla-colonization-and-exploration(6), INDEX(3) |
| `artist_types` | 2 | 4 |  | vanilla-character-dynasty-cabinet(6), common-artist(4) |
| `artist_work` | 2 | 7 |  | common-artist(7), vanilla-character-dynasty-cabinet(6) |
| `attribute_columns` | 69 | 97 | ✔ | vanilla-gui(17), gui-attribute_columns(9) |
| `auto_modifiers` | 9 | 61 | ✔ | INDEX(5), vanilla-pop(4) |
| `avatars` | 2 | 4 | ✔ | INDEX(2), common-avatars(2) |
| `biases` | 9 | 82 | ✔ | common-biases(3), INDEX(2) |
| `building_categories` | 1 | 1 |  | vanilla-production-and-buildings(4), INDEX(3) |
| `building_types` | 46 | 442 | ✔ | pitfalls(5), INDEX(4) |
| `bureaucracies` | 5 | 35 | ✔ | vanilla-government-and-reform(10), INDEX(4) |
| `cabinet_actions` | 74 | 97 | ✔ | INDEX(7), vanilla-tribes-and-tribal-estate(7) |
| `casus_belli` | 69 | 55 | ✔ | common-subject_types(11), vanilla-combat(9) |
| `character_interactions` | 31 | 76 | ✔ | vanilla-character-dynasty-cabinet(6), INDEX(3) |
| `child_educations` | 3 | 2 | ✔ | vanilla-character-dynasty-cabinet(7), INDEX(4) |
| `chivalric_orders` | 4 | 6 |  | vanilla-culture-and-religion(4), INDEX(3) |
| `climates` | 1 | 4 |  | vanilla-hazards-and-environment(6), systems-map(1) |
| `colonial_charter_goals` | 3 | 5 |  | vanilla-colonization-and-exploration(3), systems-map(1) |
| `country_description_categories` | 2 | 1 | ✔ | INDEX(2), common-country_description_categories(2) |
| `country_interactions` | 94 | 392 | ✔ | vanilla-diplomacy(8), INDEX(4) |
| `country_ranks` | 2 | 3 | ✔ | vanilla-diplomacy(5), vanilla-government-and-reform(5) |
| `culture_groups` | 1 | 5 |  | common-culture-religion(13), vanilla-culture-and-religion(12) |
| `cultures` | 52 | 552 |  | vanilla-culture-and-religion(17), common-culture-religion(8) |
| `customizable_localization` | 26 | 3897 | ✔ | vanilla-gui(7), INDEX(6) |
| `death_reason` | 3 | 5 | ✔ | common-death_reason(7), INDEX(6) |
| `decision_categories` | 1 | 1 |  | systems-map(2) |
| `decisions` | 24 | 258 |  | law-design(7), pitfalls(4) |
| `designated_heir_reason` | 1 | 0 |  | INDEX(4), common-death_reason(3) |
| `diplomatic_costs` | 2 | 1 |  | vanilla-diplomacy(6), INDEX(2) |
| `disasters` | 39 | 99 | ✔ | INDEX(5), common-disasters(3) |
| `diseases` | 9 | 46 | ✔ | vanilla-hazards-and-environment(11), INDEX(4) |
| `effect_localization` | 39 | 249 |  | common-trigger-effect-localization(6), pitfalls(5) |
| `employment_systems` | 2 | 5 | ✔ | INDEX(3), vanilla-pop(3) |
| `estate_privileges` | 8 | 129 | ✔ | vanilla-tribes-and-tribal-estate(6), vanilla-law-and-estate(5) |
| `estates` | 1 | 32 | ✔ | vanilla-law-and-estate(17), INDEX(11) |
| `ethnicities` | 54 | 629 |  | common-genes-ethnicities(9), vanilla-character-dynasty-cabinet(8) |
| `expedition_types` | 14 | 132 | ✔ | common-expedition_types(9), INDEX(2) |
| `formable_countries` | 2 | 81 | ✔ | common-formable_countries(3), INDEX(2) |
| `generic_action_ai_lists` | 96 | 25 | ✔ | vanilla-ai(4), INDEX(3) |
| `generic_actions` | 116 | 1032 | ✔ | vanilla-law-and-estate(16), INDEX(10) |
| `genes` | 10 | 422 |  | common-genes-ethnicities(32), common-persistent_dna(26) |
| `gods` | 12 | 111 | ✔ | common-gods(4), vanilla-culture-and-religion(4) |
| `goods` | 6 | 30 | ✔ | vanilla-production-and-buildings(34), modifiers-country(28) |
| `goods_demand` | 8 | 47 | ✔ | vanilla-culture-and-religion(9), common-goods_demand(8) |
| `goods_demand_category` | 1 | 0 |  | common-goods_demand(4), vanilla-production-and-buildings(3) |
| `government_reforms` | 7 | 157 | ✔ | vanilla-government-and-reform(5), INDEX(3) |
| `government_types` | 1 | 4 |  | vanilla-government-and-reform(7), vanilla-tribes-and-tribal-estate(2) |
| `hegemons` | 5 | 3 |  | vanilla-government-and-reform(5), INDEX(3) |
| `heir_selections` | 5 | 77 | ✔ | vanilla-character-dynasty-cabinet(10), INDEX(5) |
| `historical_scores` | 1 | 1 |  | INDEX(2), common-small-registries(2) |
| `holy_site_types` | 2 | 2 |  | common-holy_sites(5), INDEX(2) |
| `holy_sites` | 17 | 37 | ✔ | vanilla-culture-and-religion(5), common-holy_sites(4) |
| `industry_types` | 2 | 4 |  | systems-map(1) |
| `institution` | 7 | 26 | ✔ | vanilla-tech-and-age(24), common-scripted_triggers(19) |
| `insults` | 1 | 10 |  | vanilla-diplomacy(7), INDEX(4) |
| `international_organization_land_ownership_rules` | 9 | 9 |  | vanilla-international-organizations(5), common-international_organizations(1) |
| `international_organization_payments` | 9 | 25 |  | vanilla-international-organizations(5), vanilla-mandate-of-heaven(2) |
| `international_organization_special_statuses` | 13 | 35 |  | vanilla-international-organizations(5), INDEX(2) |
| `international_organizations` | 35 | 294 | ✔ | common-international_organizations(8), INDEX(7) |
| `join_war_rules` | 3 | 1 | ✔ | INDEX(4), vanilla-ai(4) |
| `language_families` | 1 | 3 |  | common-culture-religion(4), vanilla-culture-and-religion(4) |
| `languages` | 53 | 873 |  | vanilla-culture-and-religion(7), common-culture-religion(6) |
| `laws` | 31 | 602 | ✔ | law-design(22), INDEX(7) |
| `levies` | 9 | 22 | ✔ | vanilla-levies-and-manpower(40), modifiers-country(8) |
| `location_ranks` | 2 | 6 |  | INDEX(3), common-small-registries(3) |
| `mission_task_defs` | 0 | 0 |  | INDEX(2), systems-map(2) |
| `missions` | 12 | 142 | ✔ | INDEX(13), vanilla-events-and-missions(12) |
| `movements` | 5 | 27 | ✔ | vanilla-hazards-and-environment(10), vanilla-culture-and-religion(6) |
| `music_player_tracks` | 1 | 11 | ✔ | common-music_player_tracks(8), vanilla-audio-and-fonts(6) |
| `on_action` | 27 | 317 | ✔ | common-on_action(29), on-actions(29) |
| `parliament_agendas` | 10 | 103 |  | vanilla-law-and-estate(8), common-parliament(3) |
| `parliament_issues` | 10 | 164 |  | vanilla-law-and-estate(9), common-parliament(3) |
| `parliament_types` | 3 | 9 |  | vanilla-law-and-estate(5), common-parliament(4) |
| `peace_treaties` | 55 | 99 | ✔ | INDEX(6), common-casus_belli(6) |
| `persistent_dna` | 1 | 706 | ✔ | systems-map(4), INDEX(2) |
| `policies` | 0 | 0 |  | modifiers-country(12), vanilla-ai(4) |
| `pop_types` | 1 | 4 |  | vanilla-tribes-and-tribal-estate(8), vanilla-production-and-buildings(4) |
| `prices` | 10 | 28 | ✔ | INDEX(10), vanilla-production-and-buildings(7) |
| `production_methods` | 3 | 20 | ✔ | vanilla-production-and-buildings(8), INDEX(4) |
| `rebel_demands` | 2 | 36 | ✔ | vanilla-law-and-estate(6), common-rebel_demands(3) |
| `recruitment_method` | 3 | 0 | ✔ | INDEX(2), common-recruitment_method(2) |
| `regencies` | 19 | 27 | ✔ | vanilla-character-dynasty-cabinet(6), INDEX(4) |
| `religion_groups` | 1 | 3 |  | common-culture-religion(4), vanilla-culture-and-religion(4) |
| `religions` | 29 | 98 |  | vanilla-culture-and-religion(9), INDEX(6) |
| `religious_aspects` | 19 | 107 |  | common-culture-religion(8), vanilla-culture-and-religion(8) |
| `religious_factions` | 2 | 2 |  | common-religion(4), INDEX(2) |
| `religious_figures` | 3 | 0 |  | common-culture-religion(6), vanilla-culture-and-religion(4) |
| `religious_focuses` | 2 | 10 |  | common-religion(7), common-culture-religion(3) |
| `religious_order_callings` | 4 | 13 |  | vanilla-culture-and-religion(2) |
| `religious_order_types` | 2 | 3 |  | vanilla-culture-and-religion(2), systems-map(1) |
| `religious_order_zeal_modifiers` | 1 | 6 |  | vanilla-culture-and-religion(2), vanilla-dlc-and-assets(1) |
| `religious_schools` | 6 | 13 |  | common-culture-religion(5), vanilla-culture-and-religion(4) |
| `resolutions` | 25 | 131 | ✔ | vanilla-international-organizations(11), INDEX(3) |
| `rival_criteria` | 2 | 1 | ✔ | vanilla-ai(5), INDEX(4) |
| `road_types` | 2 | 3 | ✔ | INDEX(2), common-road_types(2) |
| `script_values` | 31 | 82 | ✔ | INDEX(11), common-subject_types(10) |
| `scriptable_hints` | 2 | 20 | ✔ | common-alert_descriptions(8), INDEX(4) |
| `scripted_country_names` | 1 | 8 |  | INDEX(2), common-small-registries(2) |
| `scripted_diplomatic_objectives` | 4 | 12 | ✔ | vanilla-ai(5), vanilla-diplomacy(5) |
| `scripted_effects` | 23 | 310 | ✔ | INDEX(6), scripting-core(5) |
| `scripted_geography` | 7 | 19 | ✔ | vanilla-map-and-geography(22), geography-areas(18) |
| `scripted_guis` | 1 | 17 | ✔ | vanilla-gui(14), INDEX(5) |
| `scripted_lists` | 3 | 1 |  | main_menu-game_rules(5), systems-map(3) |
| `scripted_modifiers` | 0 | 0 | ✔ | common-scripted_modifiers(3), INDEX(2) |
| `scripted_relations` | 37 | 105 | ✔ | vanilla-diplomacy(5), INDEX(4) |
| `scripted_rules` | 0 | 0 |  | systems-map(1), error-log-decoder(1) |
| `scripted_triggers` | 31 | 181 | ✔ | INDEX(5), common-missions(4) |
| `situations` | 23 | 253 | ✔ | INDEX(8), vanilla-colonization-and-exploration(7) |
| `societal_values` | 1 | 14 | ✔ | vanilla-government-and-reform(9), INDEX(5) |
| `subject_military_stances` | 2 | 8 | ✔ | INDEX(2), common-subject_military_stances(2) |
| `subject_types` | 19 | 84 | ✔ | common-subject_types(18), INDEX(4) |
| `tests` | 6 | 22 | ✔ | INDEX(5), common-tests(2) |
| `topography` | 1 | 11 |  | geography-areas(12), vanilla-hazards-and-environment(10) |
| `town_rights` | 12 | 32 | ✔ | vanilla-government-and-reform(19), modifiers-country(15) |
| `town_setups` | 1 | 32 |  | common-town_rights(5), vanilla-government-and-reform(5) |
| `trait_flavor` | 1 | 0 |  | common-traits(4), systems-map(2) |
| `traits` | 9 | 38 | ✔ | vanilla-character-dynasty-cabinet(13), INDEX(9) |
| `trigger_localization` | 46 | 271 |  | common-trigger-effect-localization(7), common-subject_types(5) |
| `tutorial_lesson_chains` | 1 | 0 |  | common-tutorial_lessons(5), INDEX(1) |
| `tutorial_lessons` | 4 | 85 | ✔ | common-tutorial_lessons(8), INDEX(2) |
| `unit_abilities` | 18 | 17 |  | common-unit(3), INDEX(1) |
| `unit_categories` | 11 | 7 |  | unit-attributes(20), vanilla-combat(11) |
| `unit_families` | 3 | 1 |  | systems-map(3), vanilla-dlc-and-assets(1) |
| `unit_formation_preference` | 3 | 1 |  | common-unit(3), INDEX(1) |
| `unit_types` | 32 | 130 | ✔ | unit-attributes(18), vanilla-combat(10) |
| `vegetation` | 1 | 3 |  | vanilla-hazards-and-environment(16), vanilla-dlc-and-assets(7) |
| `wargoals` | 2 | 21 | ✔ | vanilla-combat(6), INDEX(3) |
