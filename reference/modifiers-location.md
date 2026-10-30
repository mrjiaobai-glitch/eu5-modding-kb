# 修正名速查：location 类（地点修正）

> `category = location` 的修正键；地点/省份/建筑/阶层特权里的 local_* 类修正多属此类。
> **体量**：**检索用清单，不必通读** · 来源：本体 `main_menu\common\modifier_type_definitions\` 的 3 个注册表档 + `MODIFIER_TYPE_NAME_*` 本地化（**生成自 EU5 1.4.5**）

| 键 | 简中名 | 类型 | 标记 |
|---|---|---|---|
| `blockade_force_required` | 封锁力量需求 | number |  |
| `blocks_privateer_raids` | 免受私掠者掳掠奴隶 | boolean |  |
| `blocks_vision_from_land` | 遮挡陆地视野 | boolean |  |
| `blocks_vision_from_sea` | 遮挡海域视野 | boolean |  |
| `building_enslavement_power` | 每月人口奴役 | number | decimals=0 |
| `building_missionary_effort` | 每月人口皈依 | number | decimals=0 |
| `building_missionary_effort_scaled` | 每月人口皈依 % | percent |  |
| `building_owner_maritime_presence` | 给予所有者的海上存在 | number |  |
| `building_owner_overlord_maritime_presence` | 为所有者宗主国提供海上存在 | number |  |
| `can_build_ships_in_this_location` | 本地可建造船只 | boolean |  |
| `can_recruit_regiment_in_this_location` | 本地可招募军团 | boolean |  |
| `cannot_upgrade_location` | 无法升级地点 | boolean |  |
| `cap_maximum_population_growth_at_zero` | 人口增长中止 | boolean |  |
| `constructions_stalled` | 本地建设中止 | boolean |  |
| `fort_level` | 要塞等级 | number | decimals=0 |
| `free_building_levels` | 建筑等级上限 | number | decimals=0 |
| `free_capacity_attracts_pops` | 空余人口容量吸引移民 | boolean |  |
| `friendly_disembark_time_modifier` | 友军登陆时间修正 | percent |  |
| `friendly_movement_cost` | 友方移动耗时 | percent |  |
| `gives_cardinal` | 授予枢机主教 | boolean |  |
| `gold_to_building_owner` | 给予所有者的金钱 | number | 范围0~ |
| `gold_to_building_owner_overlord` | 为所有者宗主国提供金钱 | number | 范围0~ |
| `growth_is_primary_culture` | 仅允许主流/已接纳文化人口增长 | boolean |  |
| `harbor_suitability` | 港口容量 | number |  |
| `has_parliament_seat` | 议会席位 | boolean |  |
| `hostile_disembark_time_modifier` | 敌军登陆时间修正 | percent |  |
| `hostile_fleet_attrition` | 敌对海军损耗 | percent |  |
| `hostile_food_multiplier` | 食物总产量乘数 | percent |  |
| `hostile_movement_cost` | 敌人移动耗时 | percent |  |
| `local_alum_establishment_speed` | 本地$alum$投产速度 | percent |  |
| `local_alum_output_modifier` | 本地$alum$产出 | percent |  |
| `local_amber_establishment_speed` | 本地$amber$投产速度 | percent |  |
| `local_amber_output_modifier` | 本地$amber$产出 | percent |  |
| `local_army_attrition` | 陆军损耗 | percent |  |
| `local_army_levy_size_modifier` | 陆军征召规模 | percent |  |
| `local_beer_establishment_speed` | 本地$beer$投产速度 | percent |  |
| `local_beer_output_modifier` | 本地$beer$产出 | percent |  |
| `local_beeswax_establishment_speed` | 本地$beeswax$投产速度 | percent |  |
| `local_beeswax_output_modifier` | $beeswax$产出 | percent |  |
| `local_books_establishment_speed` | 本地$books$投产速度 | percent |  |
| `local_books_output_modifier` | 本地$books$产出 | percent |  |
| `local_bubonic_plague_growth_modifier` | 腺鼠疫增长 | percent |  |
| `local_bubonic_plague_impact_modifier` | 腺鼠疫影响 | percent |  |
| `local_bubonic_plague_resistance_modifier` | 腺鼠疫抵抗 | percent |  |
| `local_build_buildings_efficiency` | 建筑建造效率 | percent |  |
| `local_build_new_buildings_efficiency` | 新建筑建造效率 | percent |  |
| `local_building_establishment_reduction` | 建筑成熟度衰退 | number |  |
| `local_building_establishment_speed` | 建筑成熟度增长 | percent |  |
| `local_burghers_assimilation_blocked` | 禁止$burghers$同化 | boolean |  |
| `local_burghers_conversion_blocked` | 禁止$burghers$皈依 | boolean |  |
| `local_burghers_desired_pop` | $burghers$岗位容量 | number | decimals=0 |
| `local_burghers_desired_pop_scaled` | $per_1000_pops$的$burghers$岗位容量 | percent |  |
| `local_burghers_estate_power` | 本地市民阶层力量 | percent |  |
| `local_burghers_estate_unrest` | [ShowEstateTypeName('burghers_estate')]叛乱度 | percent |  |
| `local_burghers_food_consumption` | 本地市民食物消费 | percent |  |
| `local_burghers_max_literacy` | 本地市民最大识字率 | percent |  |
| `local_burghers_migration_allowed` | 允许$burghers$迁移 | boolean |  |
| `local_burghers_pop_growth` | $burghers$增长 | percent |  |
| `local_calvinism_movement_growth_modifier` | [ShowMovementDefinitionNameWithNoTooltip('calvinism_movement')]增长修正 | percent |  |
| `local_calvinism_movement_resistance_modifier` | [ShowMovementDefinitionNameWithNoTooltip('calvinism_movement')]抵抗修正 | percent |  |
| `local_camels_establishment_speed` | 本地$camels$投产速度 | percent |  |
| `local_camels_output_modifier` | 本地$camels$产出 | percent |  |
| `local_cannons_establishment_speed` | 本地$cannons$投产速度 | percent |  |
| `local_cannons_output_modifier` | 本地$cannons$产出 | percent |  |
| `local_chili_establishment_speed` | 本地$chili$投产速度 | percent |  |
| `local_chili_output_modifier` | $chili$产出 | percent |  |
| `local_clay_establishment_speed` | 本地$clay$投产速度 | percent |  |
| `local_clay_output_modifier` | 本地$clay$产出 | percent |  |
| `local_clergy_assimilation_blocked` | 禁止$clergy$同化 | boolean |  |
| `local_clergy_conversion_blocked` | 禁止$clergy$皈依 | boolean |  |
| `local_clergy_desired_pop` | $clergy$岗位容量 | number | decimals=0 |
| `local_clergy_desired_pop_scaled` | $per_1000_pops$的$clergy$岗位容量 | percent |  |
| `local_clergy_estate_power` | 本地教士阶层力量 | percent |  |
| `local_clergy_estate_unrest` | [ShowEstateTypeName('clergy_estate')]叛乱度 | percent |  |
| `local_clergy_food_consumption` | 本地教士食物消费 | percent |  |
| `local_clergy_max_literacy` | 本地教士最大识字率 | percent |  |
| `local_clergy_migration_allowed` | 允许$clergy$迁移 | boolean | decimals=0 |
| `local_clergy_pop_growth` | $clergy$增长 | percent |  |
| `local_cloth_establishment_speed` | 本地$cloth$投产速度 | percent |  |
| `local_cloth_guild_building_levels` | 纺织工行会建筑等级 | number | decimals=0 |
| `local_cloth_output_modifier` | 本地$cloth$产出 | percent |  |
| `local_cloves_establishment_speed` | 本地$cloves$投产速度 | percent |  |
| `local_cloves_output_modifier` | $cloves$产出 | percent |  |
| `local_coal_establishment_speed` | 本地$coal$投产速度 | percent |  |
| `local_coal_output_modifier` | 本地$coal$产出 | percent |  |
| `local_cocoa_establishment_speed` | 本地$cocoa$投产速度 | percent |  |
| `local_cocoa_output_modifier` | 本地$cocoa$产出 | percent |  |
| `local_coffee_establishment_speed` | 本地$coffee$投产速度 | percent |  |
| `local_coffee_output_modifier` | 本地$coffee$产出 | percent |  |
| `local_construction_speed` | 本地建造速度 | percent |  |
| `local_copper_establishment_speed` | 本地$copper$投产速度 | percent |  |
| `local_copper_output_modifier` | 本地$copper$产出 | percent |  |
| `local_cossacks_estate_power` | $cossacks_estate$力量 | percent |  |
| `local_cossacks_estate_unrest` | [ShowEstateTypeName('cossacks_estate')]叛乱度 | percent |  |
| `local_cotton_establishment_speed` | 本地$cotton$投产速度 | percent |  |
| `local_cotton_output_modifier` | 本地$cotton$产出 | percent |  |
| `local_crown_estate_power` | 本地王室力量 | percent |  |
| `local_crown_estate_unrest` | [ShowEstateTypeName('crown_estate')]叛乱度 | percent |  |
| `local_cultural_influence` | 本地文化影响力 | number |  |
| `local_cultural_tradition` | 文化传统 | number |  |
| `local_defensive` | 要塞防御 | percent |  |
| `local_devastation_recovery` | 本地繁荣度恢复 | percent |  |
| `local_dhimmi_estate_power` | 本地齐米阶层力量 | percent |  |
| `local_dhimmi_estate_unrest` | [ShowEstateTypeName('dhimmi_estate')]叛乱度 | percent |  |
| `local_disease_resistance` | 本地疫病抗性 | percent | decimals=0 |
| `local_distance_from_capital_speed_propagation` | 本地邻近度效率 | percent |  |
| `local_dyes_establishment_speed` | 本地$dyes$投产速度 | percent |  |
| `local_dyes_output_modifier` | 本地$dyes$产出 | percent |  |
| `local_egyptian_plague_growth_modifier` | 埃及瘟疫增长（本地） | percent |  |
| `local_egyptian_plague_impact_modifier` | 埃及瘟疫影响 | percent |  |
| `local_egyptian_plague_resistance_modifier` | 埃及瘟疫抵抗（本地） | percent |  |
| `local_elephants_establishment_speed` | 本地$elephants$投产速度 | percent |  |
| `local_elephants_output_modifier` | 本地$elephants$产出 | percent |  |
| `local_fiber_crops_establishment_speed` | 本地$fiber_crops$投产速度 | percent |  |
| `local_fiber_crops_output_modifier` | 本地$fiber_crops$产出 | percent |  |
| `local_fine_cloth_establishment_speed` | 本地$fine_cloth$投产速度 | percent |  |
| `local_fine_cloth_guild_building_levels` | 裁缝行会建筑等级 | number | decimals=0 |
| `local_fine_cloth_output_modifier` | 本地$fine_cloth$产出 | percent |  |
| `local_firearms_establishment_speed` | 本地$firearms$投产速度 | percent |  |
| `local_firearms_output_modifier` | 本地$firearms$产出 | percent |  |
| `local_fish_establishment_speed` | 本地$fish$投产速度 | percent |  |
| `local_fish_output_modifier` | 本地$fish$产出 | percent |  |
| `local_food_capacity` | 本地食物容量 | number |  |
| `local_food_capacity_modifier` | $MODIFIER_TYPE_NAME_local_food_capacity$ | percent |  |
| `local_food_decay` | 食物腐坏 | percent |  |
| `local_food_preservation_efficiency_modifier` | 食品保存效率 | percent |  |
| `local_food_production_mult` | 粮食产量倍率 | percent |  |
| `local_forced_attrition` | 不可避免损耗 | number |  |
| `local_fort_maintenance_efficiency` | 要塞维护效率 | percent |  |
| `local_frontage_allowed` | 最大战斗宽度 | number |  |
| `local_fruit_establishment_speed` | 本地$fruit$投产速度 | percent |  |
| `local_fruit_output_modifier` | 本地$fruit$产出 | percent |  |
| `local_fur_establishment_speed` | 本地$fur$投产速度 | percent |  |
| `local_fur_output_modifier` | 本地$fur$产出 | percent |  |
| `local_furniture_establishment_speed` | 本地$furniture$投产速度 | percent |  |
| `local_furniture_output_modifier` | $furniture$产出 | percent |  |
| `local_garrison_growth` | 驻军补充 | percent |  |
| `local_garrison_size` | 驻军规模 | number | decimals=0 |
| `local_gems_establishment_speed` | 本地$gems$投产速度 | percent |  |
| `local_gems_output_modifier` | 本地$gems$产出 | percent |  |
| `local_glass_establishment_speed` | 本地$glass$投产速度 | percent |  |
| `local_glass_output_modifier` | 本地$glass$产出 | percent |  |
| `local_goods_gold_establishment_speed` | 本地$goods_gold$投产速度 | percent |  |
| `local_goods_gold_output_modifier` | 本地$goods_gold$产出 | percent |  |
| `local_great_pestilence_growth_modifier` | 大瘟疫增长 | percent |  |
| `local_great_pestilence_impact_modifier` | 大瘟疫影响 | percent |  |
| `local_great_pestilence_resistance_modifier` | 大瘟疫抵抗 | percent |  |
| `local_heathen_pop_conversion_speed_modifier` | 本地异教人群皈依速度% | percent |  |
| `local_hellenism_religion_movement_growth_modifier` | 本地[ShowMovementDefinitionNameWithNoTooltip('hellenism_religion_movement')]运动增长修正 | percent |  |
| `local_hellenism_religion_movement_impact_modifier` | [ShowMovementDefinitionNameWithNoTooltip('hellenism_religion_movement')]影响修正 | percent |  |
| `local_hellenism_religion_movement_resistance_modifier` | 本地[ShowMovementDefinitionNameWithNoTooltip('hellenism_religion_movement')]运动抵抗修正 | percent |  |
| `local_heretic_pop_conversion_speed_modifier` | 本地异端人群皈依速度% | percent |  |
| `local_horses_establishment_speed` | 本地$horses$投产速度 | percent |  |
| `local_horses_output_modifier` | 本地$horses$产出 | percent |  |
| `local_hostile_attrition` | 敌军损耗 | percent |  |
| `local_incense_establishment_speed` | 本地$incense$投产速度 | percent |  |
| `local_incense_output_modifier` | 本地$incense$产出 | percent |  |
| `local_influenza_growth_modifier` | 流感增长 | percent |  |
| `local_influenza_impact_modifier` | 流感影响 | percent |  |
| `local_influenza_resistance_modifier` | 流感抵抗 | percent |  |
| `local_institution_growth_modifier` | 本地思潮增长 | percent |  |
| `local_integration_speed` | 本地整合速度 | number |  |
| `local_integration_speed_modifier` | 本地整合速度 | percent |  |
| `local_iron_establishment_speed` | 本地$iron$投产速度 | percent |  |
| `local_iron_output_modifier` | 本地$iron$产出 | percent |  |
| `local_ivory_establishment_speed` | 本地$ivory$投产速度 | percent |  |
| `local_ivory_output_modifier` | 本地$ivory$产出 | percent |  |
| `local_jewelry_establishment_speed` | 本地$jewelry$投产速度 | percent |  |
| `local_jewelry_guild_building_levels` | 珠宝行会建筑等级 | number | decimals=0 |
| `local_jewelry_output_modifier` | 本地$jewelry$产出 | percent |  |
| `local_laborers_assimilation_blocked` | 禁止$laborers$同化 | boolean |  |
| `local_laborers_conversion_blocked` | 禁止$laborers$皈依 | boolean |  |
| `local_laborers_desired_pop` | $laborers$岗位容量 | number | decimals=0 |
| `local_laborers_desired_pop_scaled` | $per_1000_pops$的$laborers$岗位容量 | percent |  |
| `local_laborers_food_consumption` | 劳工食物消费 | percent |  |
| `local_laborers_max_literacy` | 本地$laborers$最大识字率 | percent |  |
| `local_laborers_migration_allowed` | 允许$laborers$迁移 | boolean |  |
| `local_laborers_pop_growth` | $laborers$增长 | percent |  |
| `local_lacquerware_establishment_speed` | 本地$lacquerware$投产速度 | percent |  |
| `local_lacquerware_output_modifier` | 本地$lacquerware$产出 | percent |  |
| `local_lead_establishment_speed` | 本地$lead$投产速度 | percent |  |
| `local_lead_output_modifier` | 本地$lead$产出 | percent |  |
| `local_leather_establishment_speed` | 本地$leather$投产速度 | percent |  |
| `local_leather_output_modifier` | 本地$leather$产出 | percent |  |
| `local_legumes_establishment_speed` | 本地$legumes$投产速度 | percent |  |
| `local_legumes_output_modifier` | 本地$legumes$产出 | percent |  |
| `local_levy_recruitment_speed_modifier` | 本地征召速度 | percent |  |
| `local_levy_size_modifier` | 本地征召规模 | percent |  |
| `local_life_expectancy` | 角色预期寿命 | number | decimals=0 |
| `local_liquor_establishment_speed` | 本地$liquor$投产速度 | percent |  |
| `local_liquor_output_modifier` | 本地$liquor$产出 | percent |  |
| `local_livestock_establishment_speed` | 本地$livestock$投产速度 | percent |  |
| `local_livestock_output_modifier` | 本地$livestock$产出 | percent |  |
| `local_lumber_establishment_speed` | 本地$lumber$投产速度 | percent |  |
| `local_lumber_output_modifier` | 本地$lumber$产出 | percent |  |
| `local_lutheranism_movement_growth_modifier` | [ShowMovementDefinitionNameWithNoTooltip('lutheranism_movement')]增长修正 | percent |  |
| `local_lutheranism_movement_resistance_modifier` | [ShowMovementDefinitionNameWithNoTooltip('lutheranism_movement')]抵抗修正 | percent |  |
| `local_maize_establishment_speed` | 本地$maize$投产速度 | percent |  |
| `local_maize_output_modifier` | 本地$maize$产出 | percent |  |
| `local_malaria_growth_modifier` | 疟疾增长 | percent |  |
| `local_malaria_impact_modifier` | 疟疾影响 | percent |  |
| `local_malaria_resistance_modifier` | 疟疾抵抗 | percent |  |
| `local_manpower` | 每月人力 | number | decimals=0 |
| `local_manpower_modifier` | 每月人力 | percent |  |
| `local_marble_establishment_speed` | 本地$marble$投产速度 | percent |  |
| `local_marble_output_modifier` | $marble$产出 | percent |  |
| `local_maritime_presence` | 本地海上存在 | number |  |
| `local_market_access` | 市场接入度 | percent |  |
| `local_market_upgrade_building_levels` | 市场升级建筑等级 | number | decimals=0 |
| `local_marketplace_building_levels` | 集市建筑等级 | number | decimals=0 |
| `local_masonry_establishment_speed` | 本地$masonry$投产速度 | percent |  |
| `local_masonry_output_modifier` | $masonry$产出 | percent |  |
| `local_max_control` | 最大控制力 | percent |  |
| `local_max_literacy` | 本地最大识字率 | percent |  |
| `local_max_rgo_size` | 最大原产雇佣规模 | number | decimals=0 |
| `local_max_rgo_size_modifier` | 最大原产雇佣规模 | percent |  |
| `local_max_rural_control` | 乡村最大控制力 | percent |  |
| `local_max_urban_control` | 城镇最大控制力 | percent |  |
| `local_may_build_nahuatl_units` | 可招募雄鹰战士与美洲虎武士 | boolean |  |
| `local_may_build_north_american_units` | 可以组建北美战士 | boolean |  |
| `local_measles_growth_modifier` | 麻疹增长 | percent |  |
| `local_measles_impact_modifier` | 麻疹影响 | percent |  |
| `local_measles_resistance_modifier` | 麻疹抵抗 | percent |  |
| `local_medicaments_establishment_speed` | 本地$medicaments$投产速度 | percent |  |
| `local_medicaments_output_modifier` | 本地$medicaments$产出 | percent |  |
| `local_mercenaries_modifier` | 本地雇佣兵规模 | percent | decimals=0 |
| `local_merchant_capacity` | 贸易容量 | number |  |
| `local_merchant_capacity_modifier` | $MODIFIER_TYPE_NAME_local_merchant_capacity$ | percent |  |
| `local_merchant_power` | $game_concept_merchant_power$ | number |  |
| `local_mercury_establishment_speed` | 本地$mercury$投产速度 | percent |  |
| `local_mercury_output_modifier` | $mercury$产出 | percent |  |
| `local_migration_attraction` | 移民吸引力 | number |  |
| `local_migration_speed` | 人口迁徙速度 | number |  |
| `local_migration_speed_modifier` | 人口迁徙速度 | percent |  |
| `local_millet_establishment_speed` | 本地$millet$投产速度 | percent |  |
| `local_millet_output_modifier` | 本地$millet$产出 | percent |  |
| `local_mills_build_buildings_efficiency` | 本地磨坊建造效率 | percent |  |
| `local_monthly_control` | 每月控制力 | percent |  |
| `local_monthly_control_decline` | 每月控制力衰减 | percent |  |
| `local_monthly_development` | 本地每月发展度 | number | decimals=4 |
| `local_monthly_development_modifier` | 本地每月发展度增长 | percent |  |
| `local_monthly_food` | 本地食物产出 | number |  |
| `local_monthly_food_modifier` | 本地食物产出 % | percent |  |
| `local_monthly_literacy` | 本地每月识字率 | percent | decimals=3 |
| `local_monthly_prosperity` | 本地每月繁荣度 | percent |  |
| `local_monthly_rural_control` | 乡村每月控制力 | percent |  |
| `local_monthly_urban_control` | 城镇每月控制力 | percent |  |
| `local_naval_supplies_establishment_speed` | 本地$naval_supplies$投产速度 | percent |  |
| `local_naval_supplies_output_modifier` | 本地$naval_supplies$产出 | percent |  |
| `local_navy_attrition` | 海军损耗 | number |  |
| `local_navy_levy_size_modifier` | 海军征召规模 | percent |  |
| `local_nobles_assimilation_blocked` | 禁止$nobles$同化 | boolean |  |
| `local_nobles_conversion_blocked` | 禁止$nobles$皈依 | boolean |  |
| `local_nobles_desired_pop` | $nobles$岗位容量 | number | decimals=0 |
| `local_nobles_desired_pop_scaled` | $per_1000_pops$的$nobles$岗位容量 | percent |  |
| `local_nobles_estate_power` | 本地贵族阶层力量 | percent |  |
| `local_nobles_estate_unrest` | [ShowEstateTypeName('nobles_estate')]叛乱度 | percent |  |
| `local_nobles_food_consumption` | 本地贵族食物消费 | percent |  |
| `local_nobles_max_literacy` | 本地贵族最大识字率 | percent |  |
| `local_nobles_migration_allowed` | 允许$nobles$迁移 | boolean |  |
| `local_nobles_pop_growth` | $nobles$增长 | percent |  |
| `local_olives_establishment_speed` | 本地$olives$投产速度 | percent |  |
| `local_olives_output_modifier` | 本地$olives$产出 | percent |  |
| `local_paper_establishment_speed` | 本地$paper$投产速度 | percent |  |
| `local_paper_output_modifier` | 本地$paper$产出 | percent |  |
| `local_pearls_establishment_speed` | 本地$pearls$投产速度 | percent |  |
| `local_pearls_output_modifier` | 本地$pearls$产出 | percent |  |
| `local_peasant_enfranchisment` | 农民自主权 | percent |  |
| `local_peasants_assimilation_blocked` | 禁止$peasants$同化 | boolean |  |
| `local_peasants_conversion_blocked` | 禁止$peasants$皈依 | boolean |  |
| `local_peasants_desired_pop` | $peasants$岗位容量 | number | decimals=0 |
| `local_peasants_desired_pop_scaled` | $per_1000_pops$的$peasants$岗位容量 | percent |  |
| `local_peasants_estate_power` | 本地农民阶层力量 | percent |  |
| `local_peasants_estate_unrest` | [ShowEstateTypeName('peasants_estate')]叛乱度 | percent |  |
| `local_peasants_food_consumption` | 本地农民食物消费 | percent |  |
| `local_peasants_max_literacy` | 本地农民最大识字率 | percent |  |
| `local_peasants_migration_allowed` | 允许$peasants$迁移 | boolean |  |
| `local_peasants_pop_growth` | $peasants$增长 | percent |  |
| `local_pepper_establishment_speed` | 本地$pepper$投产速度 | percent |  |
| `local_pepper_output_modifier` | $pepper$产出 | percent |  |
| `local_pirate_spawn_chance` | 海盗出现概率 | percent |  |
| `local_pop_assimilation_speed` | 本地人口同化速度 | number |  |
| `local_pop_assimilation_speed_modifier` | 本地人口同化速度 | percent |  |
| `local_pop_conversion_speed` | 本地人口皈依速度 | number |  |
| `local_pop_conversion_speed_modifier` | 本地人口皈依速度 % | percent |  |
| `local_pop_demand` | 本地人群商品需求 | percent |  |
| `local_pop_demotion_speed` | 人口降级速度 | number |  |
| `local_pop_demotion_speed_modifier` | 人口降级速度 | percent |  |
| `local_pop_food_consumption` | 本地人口食物消耗 | percent |  |
| `local_pop_join_rebel_threshold` | 人口加入叛军的阈值 | percent |  |
| `local_pop_leave_rebels_threshold` | 人口脱离叛军的阈值 | percent |  |
| `local_pop_promotion_speed` | 本地人口升级速度 | number |  |
| `local_pop_promotion_speed_modifier` | 本地人口升级速度 % | percent |  |
| `local_pop_promotion_speed_scaled` | 本地人口升级速度 % | number |  |
| `local_population_capacity` | 本地人口容量 | number | decimals=0 |
| `local_population_capacity_modifier` | 本地人口容量 % | percent |  |
| `local_population_growth` | 本地人口增长 | percent |  |
| `local_porcelain_establishment_speed` | 本地$porcelain$投产速度 | percent |  |
| `local_porcelain_output_modifier` | 本地$porcelain$产出 | percent |  |
| `local_port_build_buildings_efficiency` | 港口建造效率 | percent |  |
| `local_port_cost_distance_impact` | 出入港口的邻近度影响 | percent |  |
| `local_possible_town_rights` | 城镇特权上限 | number | decimals=0 |
| `local_potato_establishment_speed` | 本地$potato$投产速度 | percent |  |
| `local_potato_output_modifier` | 本地$potato$产出 | percent |  |
| `local_pottery_establishment_speed` | 本地$pottery$投产速度 | percent |  |
| `local_pottery_output_modifier` | $pottery$产出 | percent |  |
| `local_production_efficiency` | 加工生产效率 | percent |  |
| `local_prosperity_decay` | 本地繁荣度衰减 | percent |  |
| `local_proximity_source` | 本地邻近度产生 | number |  |
| `local_proximity_source_modifier` | 本地邻近度来源百分比 | percent |  |
| `local_raw_material_output` | 原料生产产出 | percent |  |
| `local_repair_speed` | 本地海军修理速度 | percent |  |
| `local_rgo_build_time` | 原产扩建时间 | percent |  |
| `local_rice_establishment_speed` | 本地$rice$投产速度 | percent |  |
| `local_rice_output_modifier` | 本地$rice$产出 | percent |  |
| `local_road_building_time` | 道路建造时间 | percent |  |
| `local_roman_culture_movement_growth_modifier` | 本地[ShowMovementDefinitionNameWithNoTooltip('roman_culture_movement')]运动增长修正 | percent |  |
| `local_roman_culture_movement_resistance_modifier` | 本地[ShowMovementDefinitionNameWithNoTooltip('roman_culture_movement')]运动抵抗修正 | percent |  |
| `local_saffron_establishment_speed` | 本地$saffron$投产速度 | percent |  |
| `local_saffron_output_modifier` | $saffron$产出 | percent |  |
| `local_sailors` | 每月水手 | number | decimals=0 |
| `local_sailors_modifier` | 每月水手 | percent |  |
| `local_salt_establishment_speed` | 本地$salt$投产速度 | percent |  |
| `local_salt_output_modifier` | 本地$salt$产出 | percent |  |
| `local_saltpeter_establishment_speed` | 本地$saltpeter$投产速度 | percent |  |
| `local_saltpeter_output_modifier` | 本地$saltpeter$产出 | percent |  |
| `local_sand_establishment_speed` | 本地$sand$投产速度 | percent |  |
| `local_sand_output_modifier` | 本地$sand$产出 | percent |  |
| `local_separatism` | 本地分离主义 | percent |  |
| `local_ship_build_speed` | 本地舰船建造速度 | percent |  |
| `local_silk_establishment_speed` | 本地$silk$投产速度 | percent |  |
| `local_silk_output_modifier` | 本地$silk$产出 | percent |  |
| `local_silver_establishment_speed` | 本地$silver$投产速度 | percent |  |
| `local_silver_output_modifier` | 本地$silver$产出 | percent |  |
| `local_slave_pop_satisfaction` | 奴隶满意度 | percent |  |
| `local_slaves_assimilation_blocked` | 禁止$slaves$同化 | boolean |  |
| `local_slaves_conversion_blocked` | 禁止$slaves$皈依 | boolean |  |
| `local_slaves_desired_pop` | $slaves$岗位容量 | number | decimals=0 |
| `local_slaves_desired_pop_scaled` | $per_1000_pops$的$slaves$岗位容量 | percent |  |
| `local_slaves_food_consumption` | 本地奴隶食物消费 | percent |  |
| `local_slaves_goods_establishment_speed` | 本地$slaves_goods$投产速度 | percent |  |
| `local_slaves_goods_output_modifier` | 本地$slaves_goods$产出 | percent |  |
| `local_slaves_max_literacy` | 本地奴隶最大识字率 | percent |  |
| `local_slaves_migration_allowed` | 允许$slaves$迁移 | boolean |  |
| `local_slaves_pop_growth` | $slaves$增长 | percent |  |
| `local_smallpox_growth_modifier` | 天花增长 | percent |  |
| `local_smallpox_impact_modifier` | 天花影响 | percent |  |
| `local_smallpox_resistance_modifier` | 天花抵抗 | percent |  |
| `local_soldiers_assimilation_blocked` | 禁止$soldiers$同化 | boolean |  |
| `local_soldiers_conversion_blocked` | 禁止$soldiers$皈依 | boolean |  |
| `local_soldiers_desired_pop` | $soldiers$岗位容量 | number | decimals=0 |
| `local_soldiers_desired_pop_scaled` | $per_1000_pops$的$soldiers$岗位容量 | percent |  |
| `local_soldiers_food_consumption` | 士兵食物消费 | percent |  |
| `local_soldiers_max_literacy` | 本地$soldiers$最大识字率 | percent |  |
| `local_soldiers_migration_allowed` | 允许$soldiers$迁移 | boolean |  |
| `local_soldiers_pop_growth` | $soldiers$增长 | percent |  |
| `local_steel_establishment_speed` | 本地$steel$投产速度 | percent |  |
| `local_steel_output_modifier` | 本地$steel$产出 | percent |  |
| `local_stone_establishment_speed` | 本地$stone$投产速度 | percent |  |
| `local_stone_output_modifier` | 本地$stone$产出 | percent |  |
| `local_sugar_establishment_speed` | 本地$sugar$投产速度 | percent |  |
| `local_sugar_output_modifier` | 本地$sugar$产出 | percent |  |
| `local_supply_limit_modifier` | 补给上限 | percent |  |
| `local_tar_establishment_speed` | 本地$tar$投产速度 | percent |  |
| `local_tar_output_modifier` | 本地$tar$产出 | percent |  |
| `local_tea_establishment_speed` | 本地$tea$投产速度 | percent |  |
| `local_tea_output_modifier` | 本地$tea$产出 | percent |  |
| `local_tin_establishment_speed` | 本地$tin$投产速度 | percent |  |
| `local_tin_output_modifier` | 本地$tin$产出 | percent |  |
| `local_tobacco_establishment_speed` | 本地$tobacco$投产速度 | percent |  |
| `local_tobacco_output_modifier` | 本地$tobacco$产出 | percent |  |
| `local_tools_establishment_speed` | 本地$tools$投产速度 | percent |  |
| `local_tools_output_modifier` | 本地$tools$产出 | percent |  |
| `local_trade_center_power` | 市场吸引力 | percent |  |
| `local_trade_embark_disembark_efficiency` | 贸易出入港效率 | percent |  |
| `local_trade_protection_factor` | 本地市场保护 | percent |  |
| `local_trades_per_burgher` | $burghers_estate$贸易容量 | percent |  |
| `local_tribal_promotion` | $tribesmen$转为$peasants$ | percent |  |
| `local_tribes_estate_power` | $tribes_estate$阶层力量 | percent |  |
| `local_tribes_estate_unrest` | [ShowEstateTypeName('tribes_estate')]叛乱度 | percent |  |
| `local_tribesmen_assimilation_blocked` | 禁止$tribesmen$同化 | boolean |  |
| `local_tribesmen_conversion_blocked` | 禁止$tribesmen$皈依 | boolean |  |
| `local_tribesmen_desired_pop` | $tribesmen$岗位容量 | number | decimals=0 |
| `local_tribesmen_desired_pop_scaled` | $per_1000_pops$的$tribesmen$岗位容量 | percent |  |
| `local_tribesmen_food_consumption` | 部落民食物消费 | percent |  |
| `local_tribesmen_max_literacy` | 本地$tribesmen$最大识字率 | percent |  |
| `local_tribesmen_migration_allowed` | 允许$tribesmen$迁移 | boolean |  |
| `local_tribesmen_pop_growth` | $tribesmen$增长 | percent |  |
| `local_typhus_growth_modifier` | 伤寒增长 | percent |  |
| `local_typhus_impact_modifier` | 伤寒影响 | percent |  |
| `local_typhus_resistance_modifier` | 伤寒抵抗 | percent |  |
| `local_unrest` | 本地叛乱度 | percent |  |
| `local_upper_class_capacity_modifier` | 上层阶层人口容量 | percent |  |
| `local_war_score_efficiency` | 战争分数效率 | percent |  |
| `local_weaponry_establishment_speed` | 本地$weaponry$投产速度 | percent |  |
| `local_weaponry_output_modifier` | 本地$weaponry$产出 | percent |  |
| `local_wheat_establishment_speed` | 本地$wheat$投产速度 | percent |  |
| `local_wheat_output_modifier` | 本地$wheat$产出 | percent |  |
| `local_wild_game_establishment_speed` | 本地$wild_game$投产速度 | percent |  |
| `local_wild_game_output_modifier` | 本地$wild_game$产出 | percent |  |
| `local_wine_establishment_speed` | 本地$wine$投产速度 | percent |  |
| `local_wine_output_modifier` | 本地$wine$产出 | percent |  |
| `local_wool_establishment_speed` | 本地$wool$投产速度 | percent |  |
| `local_wool_output_modifier` | 本地$wool$产出 | percent |  |
| `manpower_to_building_owner` | 给予所有者的人力 | number | 范围0~ |
| `max_attrition` | 最大损耗 | number |  |
| `max_constructions_at_same_time` | 建筑建造队列 | number | decimals=0 |
| `max_regiments_trained_at_same_time` | 军团招募队列 | number | decimals=0 |
| `max_ships_built_at_same_time` | 舰船建造队列 | number | decimals=0 |
| `max_siege_memory` | 最大围攻进度 | number | decimals=0 |
| `maximum_stockpile_capacity` | 市场库存上限 | number | decimals=0 |
| `may_build_sofa_units` | 可以征募索法步兵 | boolean |  |
| `merchant_capacity_from_building` | 给予所有者的贸易容量 | number |  |
| `merchant_power_from_building` | 给予所有者的$game_concept_merchant_power$ | number |  |
| `minimum_fort_level` | 最低要塞等级 | number | decimals=0 |
| `movement_blocked` | 移动受阻 | boolean |  |
| `movement_cost` | 移动耗时 | percent |  |
| `natural_harbor_suitability` | 天然港口区位度 | percent |  |
| `occupation_time` | 占领时间 | percent |  |
| `owner_gets_vision_when_occupied` | 在被占领时不丢失视野 | boolean |  |
| `promote_institution_chance` | 思潮推广效率 | number |  |
| `propagating_zone_of_control` | 允许控制区 | boolean |  |
| `road_building_blocked` | 禁止建造道路 | boolean |  |
| `sailors_to_building_owner` | 给予所有者的税收 | number | 范围0~ |
| `scaled_gold_to_building_owner` | 给予所有者的税基 | percent | 范围0~ |
| `scaled_gold_to_building_owner_overlord` | 为所有者宗主国提供税基 | percent | 范围0~ |
| `supply_limit` | 补给上限 | number |  |
| `total_population_capacity_modifier` | 人口容量 | percent |  |
