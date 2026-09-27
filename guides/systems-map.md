# common 类目地图（systems-map）

`in_game\common\` 全部类目（实查 ~160 个），按功能分组。**每类目制作流程**：看本表找入口文件 → read 原版样例 → 加载 `fields\` 对应字段文档核对字段 → 照抄改。标注 `[有readme]` 的类目其 `readme.txt` 是官方字段说明（73 个 total）。

## 经济与生产

| 类目 | 用途 | 入口文件（原版） |
|---|---|---|
| goods `[有readme]` | 商品定义（category/method 枚举、demand） | 00_default.txt |
| goods_demand / goods_demand_category | 商品需求分组（**355 条**；`<商品> = 值` + `category`，13 个分组；字段档 `fields\common-goods_demand.md`） | 建筑维护如 `infantry_maintenance` 定义处 |
| prices `[有readme]` | 价格定义（min/min_scale/max_scale） | 00_default.txt |
| production_methods `[有readme]` | 生产方式（goods_input/produced） | 00_default.txt（另有 `__readme.txt`） |
| building_categories / building_types `[有readme]` | 建筑类别（**15 条，只有 `audio_category`= 音效桶**；字段档 `fields\common-small-registries.md`）/ 建筑（level/employment/cost/effect） | building_types\00_default.txt、town_buildings.txt、rural_buildings.txt 等 |
| employment_systems `[有readme]` | 就业系统（平等/先来先得/资本主义） | 00_default.txt |
| town_rights `[有readme]` | 城镇权利（区域特色建筑/修正） | 00_traditions.txt、10_country_specific.txt… |
| town_setups | 城镇初始布局 | 00_default.txt |

## 军事

| 类目 | 用途 | 入口文件 |
|---|---|---|
| unit_categories `[有readme]` | 单位类别（基础属性：frontage/initiative/combat_speed 等） | 00_army_light_infantry.txt … 04_army_artillery.txt、10_navy_galley.txt… |
| unit_types `[有readme]` | 兵种（copy_from 模板、age、upgrades_to、combat/impact 地形） | 00_age_templates_land.txt、2_unlocked_through_tech.txt、3_janissaries.txt |
| unit_abilities `[有readme]` | 部队能力（drill_army、scorch_earth、ransom_prisoners…） | 每能力一个文件 |
| unit_formation_preference `[有readme]` | 阵型偏好 | 00_default.txt |
| levies `[有readme]` | 征召（**特化单位必须放文件顶部**） | 00_default.txt |
| recruitment_method `[有readme]` | 招募方式（strength/experience/build_time） | 00_default.txt |
| subject_military_stances `[有readme]` | 附庸军事姿态（AI 优先级 float） | 00_default.txt |

## 人口与阶层

| 类目 | 用途 | 入口文件 |
|---|---|---|
| pop_types `[无readme]` | POP 类型（promote_to/assimilation_conversion_factor/literacy_impact） | 00_default.txt（nobles/clergy/burghers/laborers/soldiers/peasants/tribesmen/slaves） |
| estates `[无readme]` | 阶层（power_per_pop/tax_per_pop/satisfaction/power 块） | 00_default.txt（1275 行） |
| estate_privileges `[有readme]` | 阶层特权 | nobles_estate.txt、peasants_estate.txt… |

## 政府与政治

| 类目 | 用途 | 入口文件 |
|---|---|---|
| government_types | 政体类型（**5 个**：monarchy/republic/theocracy/steppe_horde/tribe；每个定义**政府影响力名**、继承法白名单、角色默认阶层） | 00_default.txt |
| government_reforms `[有readme]` | 政府改革（**328 项**，major **58 项排他**；**改革无价格**，靠槽位 + 社会价值 50） | common.txt、country_specific.txt、monarchy.txt… |
| laws `[有readme]` | 法律与政策（law 容器） | 00_common.txt、01_military_laws.txt… |
| bureaucracies `[有readme]` | 官僚部门（**25 个**；三态修正按 `scope:maintenance` 缩放；三个价格只在 `prices\05_byz.txt`） | generic.txt、byz.txt、china.txt |
| country_ranks `[有readme]` | 国家等级（**4 级**：帝国/王国/公国/伯国；升级 1000/250/100 金 + 缩放金） | 00_default.txt |
| regencies `[有readme]` | 摄政（**15 种**，一文件一摄政；`interregnum` 为兜底） | `zz_default.txt`、`1_nobles_regency.txt`…`12_lordship_of_ireland_regency.txt`、`00_*_election.txt` |
| policies | **空壳·勿删**——政策现在**由法律间接生成**，目录为空但删掉会 assert（info 原文） | 无数据文件 |
| country_description_categories `[有readme]` | 国家简介分类（loc 前缀固定） | 00_default.txt |
| designated_heir_reason / heir_selections / death_reason | 继承法（**44 种**）/ 死因（**51 个**）/ 指定继承人理由（7 个，空块纯 loc 键） | `heir_selections\monarchy.txt`…、`death_reason\00_hardcoded.txt`+`01_content.txt`+`02_life_expectancy.txt`、`designated_heir_reason\00_standard.txt`（**均无 `00_default.txt`**） |
| cabinet_actions `[有readme]` | 内阁行动 | 00_default.txt、promote_religion.txt… |
| chivalric_orders | 骑士团 | 00_default.txt、02_german_societies.txt |
| hegemons | 霸权（**5 种**：经济/海军/军事/外交/文化；`gain`/`lose`/`modifier` 各 5；军事霸权 lose 用 `×1.1` 滞回，`HEGEMONY_LOST_MONTHS = 120`；字段档 `fields\common-small-registries.md`） | `0_economic_hegemon.txt`…`4_cultural_hegemon.txt` |
| town_rights `[有readme]` | 城镇特权（**50 项**，授予 **政府影响力 5** / 撤销 稳定 10；`kept_at_conquest` 64%） | 00_traditions.txt、11_scandinavian.txt…、15_iberia.txt |
| town_setups | 城镇建筑模板（**117 个**，用 `copy_from` 继承） | 00_default.txt |
| generic_actions\government_conversions.txt | 换政体动作（**8 条**；稳定 50 + 正统性 25、**20 年冷却**） | government_conversions.txt |

> **政体 · 改革 · 官僚 · 社会价值观 · 国家等级 · 霸权**的完整机制（政府影响力五种形态与全部价格、328 项改革与槽位来源、25 个官僚部门的三态修正与 `entrenchment`、17 条社会价值轴与 30 个 `societal_value_push_*`、城镇特权与等级升级价）详见 `vanilla\vanilla-government-and-reform.md`；议会与叛乱诉求见 `vanilla\vanilla-law-and-estate.md`。

> **角色 · 王朝 · 内阁**的完整机制（147 个特质 / 44 种继承法 / 15 种摄政 / 73 个内阁行动 / 34 个角色交互 / `NCharacter` 常量；**角色与王朝本体是开局数据**，在 `main_menu\setup\start\04_dynasties.txt` + `05_characters.txt`）详见 `vanilla\vanilla-character-dynasty-cabinet.md`。

## 文化与人口属性

| 类目 | 用途 | 入口文件 |
|---|---|---|
| cultures / culture_groups | 文化与文化组（`culture_groups` 是**文化内字段**，不是独立目录） | 53 文件按地区分：`alaska.txt`、`arabia.txt`、`argentinian.txt`…（示例见 `00_cultures.info`，**无 `00_default.txt`**） |
| languages / language_families | 语言与语系 | `languages\00_<地区>.txt`（53 文件，语言条目直接写顶层）+ `language_families\00_language_families.txt`（**56 个语系**，只有 `color`） |
| ethnicities | 族群（**60 个**；`template` 带引号指向同目录其它族群） | `00_ethnicities.txt`（基模板 `ethnicity_template`）、`00_ethnicities_<地区>.txt`（54 文件） |
| traits / trait_flavor | 特质（**147 个 / 9 类别**：ruler/cabinet/general/admiral/health/artist/child/religious_figure/explorer） | traits 侧 `00_ruler.txt`…`08_health.txt`（9 文件）；flavor 侧 `trait_flavor\00_default.txt`（4 种颜色分组） |
| genes / persistent_dna | 角色外貌基因（6 个顶层容器：color/morph/accessory/special_genes、decal_atlases、age_presets）/ **`persistent_dna`（105 条固定外貌 DNA；⚠️ 别用它强制穿戴，字段档 `fields\common-persistent_dna.md`）** | genes 侧 `00_genes_color.txt`、`01_genes_morph.txt`、`02…09_genes_*.txt`；`persistent_dna\custom_characters.txt` |
| child_educations `[有readme]` | 儿童教育（**6 种**；⚠️ 名字与 `=` 一行、`{` 另起一行） | `00_default.txt`（5 个）、`D008_orthodox_education.txt`（1 个，DLC） |
| avatars `[有readme]` | 神祇化身 | 00_default.txt |

## 宗教

| 类目 | 用途 | 入口文件 |
|---|---|---|
| religions / religion_groups | 宗教与宗教组 | christian.txt、muslim.txt、00_religion_groups.txt |
| religious_aspects / religious_factions / religious_focuses / religious_figures / religious_schools | 宗教五件套 | common.txt、calvinist.txt、sunni.txt… |
| holy_sites / holy_site_types `[有readme]` | 圣地与类型 | 00_default.txt |
| gods `[有readme]` | 神祇（religion/group 两种写法） | hellenism.txt… |

## 外交与战争

| 类目 | 用途 | 入口文件 |
|---|---|---|
| casus_belli `[有readme]` | 战争理由 | 00_default.txt |
| wargoals `[有readme]` | 战争目标（attacker/defender 块） | 00_default.txt |
| peace_treaties `[有readme]` | 和平条约 | 00_default.txt |
| diplomatic_costs | 外交行动成本 | 00_default.txt |
| join_war_rules `[有readme]` | 参战规则 | 00_default.txt |
| rival_criteria `[有readme]` | 宿敌标准（仅 AI） | 00_default.txt |
| subject_types `[有readme]` | 附庸类型 | 00_default.txt、conquistador.txt、appanage.txt |
| country_interactions `[有readme]` | 国家交互（diplo_chance 键全清单） | 00_default.txt、hre.txt… |
| character_interactions `[有readme]` | 角色交互 | 00_default.txt |
| scripted_relations `[有readme]` | 脚本化关系 | 00_default.txt |
| scripted_diplomatic_objectives `[有readme]` | 脚本化外交目标 | 00_default.txt |
| insults | 羞辱文本（**73 条**，全部只有 `trigger`；字段档 `fields\common-small-registries.md`） | 00_default.txt |
| international_organizations `[有readme]` | 国际组织（**36 个 / 34 文件 / 282.7 KB**；`hre.txt` 29.9 KB 最大） | 按 IO 分文件：`hre.txt`、`middle_kingdom.txt`、`union.txt`、`catholic_church.txt`…（**无 `00_default.txt`**） |
| international_organization_special_statuses / _payments / _land_ownership_rules `[有readme]` | IO 三个子类目（地位含 `special_status_power`；付款 payer/payee **可为阶层**；土地规则含 `removed_by_peace_treaty`） | `hre.txt`、`middle_kingdom.txt`、`tithe.txt`… |
| resolutions `[有readme]` | IO 决议（**18 个 / 24 文件 / 130 KB**；投票引擎：`votes`/`total_votes_needed`/`should_finalize_vote`） | `policy_vote.txt`、`hre_election.txt`、`high_kingship_election.txt`… |
| generic_actions\io_parliament / io_parliament_bribes / international_organizations* | IO 议会（**60 个月间隔**、会址须为 IO 拥有地点）、行贿、IO 政策议题 | 各同名文件 |
| parliament_agendas / parliament_issues / parliament_types `[有readme]` | 议会三件套 | 00_default.txt、01_country_specific_parliament_issues.txt |
| hegemons / formable_countries `[有readme]` | 霸权（见"政府与政治"组）/ 可成立国家 | 00_default.txt |

> **IO 深挖**（36 个 IO 字段对照、领袖的三种触发×五种方法、议会与决议引擎、三个子类目、帝国圈常量、**天朝 IO 案例**）详见 `vanilla\vanilla-international-organizations.md`；IO 与灾难/局势的耦合见 `vanilla\vanilla-disaster-and-situation.md`。

## 殖民与探索

| 类目 | 用途 | 入口文件 |
|---|---|---|
| generic_actions\colonial_charters.txt | **特许殖民地**（建/弃；目标 = **预设省份**，候选由引擎给） | 本文件（2 个动作） |
| generic_actions\explorers.txt | 探索（开始/换探险家/取消；三选 = area + 出发港 + 角色） | 本文件（3 个动作） |
| generic_actions\conquistadors.txt | 征服者（建立 `conquistador` 附属国） | 本文件（1 个动作） |
| cabinet_actions\send_people_to_the_colonies / settle_the_frontier / settle_tribesmen / encourage_migration `[有readme]` | 殖民四内阁行动（`add_migration` 的三个来源） | 各同名文件 |
| on_action\colonial_charter_monthly / exploration_mission_monthly / settle_the_frontier_monthly | 三条脉冲（探索月度 **39 事件 × 0.2**） | 各同名文件 |
| subject_types\colonial_nation / conquistador / dominion / trade_company `[有readme]` | 四种殖民/海外附属国 | 各同名文件 |
| situations\colonial_revolution `[有readme]` | 殖民革命局势（`can_end = always = no`） | colonial_revolution.txt |
| international_organizations\colonial_federation `[有readme]` + laws\24_colonial_federation | 殖民地联邦 IO 与联邦法律 | 各同名文件 |
| casus_belli\（push_back_colonizers / force_migration / colony_war / exploration / colonial_conflict） `[有readme]` | 五个殖民/探索 CB | 各同名文件 |
| country_interactions\merge_colonies / start_war_in_colony / take_colony_for_debt / invite_settlers `[有readme]` | 殖民外交交互 | 各同名文件 |
| peace_treaties\abandon_colonies / abandon_colonial_claim `[有readme]` | 放弃殖民地/宣称 | 各同名文件 |

> **完整机制**（探索三选与价格、特许殖民地成本常量与"力量投射 ≥ 25"、`add_migration` 三来源、征服者硬条件、四种殖民附属国对比、殖民革命与联邦、83 条区域偏好）详见 `vanilla\vanilla-colonization-and-exploration.md`；常量见 `guides\defines.md` 的 `NColony` 表。

## 内容系统（事件/任务/灾难）

**⚠️ 事件本体不在 `common\` 而在 `in_game\events\`**：**349 档 / 10.52 MB / 7,470 个事件 / 361 个 namespace**（权威：`events\readme.txt` 6.9 KB）；`DHE\` 159 档 / 6.2 MB / 4,145 事件（占 55.5%）、根目录散档 26 档 435 事件（`earthquake_events.txt` 137 KB 最大）、`missionevents\` 3 档 40 事件。

| 类目 | 用途 | 入口文件 |
|---|---|---|
| on_action `[有 info]` | 触发钩子（**21 个数据档 + 1 个 .info / 279 KB / 顶层 216 条**；`_hardcoded.txt` 134 KB 装 132 个引擎钩子，含 6 个 `on_mission_*`） | `_hardcoded.txt`、`country_yearly.txt`、`location_pulses.txt`（天气/火山/地震）、`religion_flavor_pulse.txt`… |
| scripted_effects `[有readme]` | 效果宏（$参数$） | 00_default.txt、country_effects.txt… |
| scripted_triggers `[有readme]` | 条件宏 | 00_default.txt、war_triggers.txt… |
| script_values `[有 info]` | 数值与公式（**info 是完整数学 DSL 文档**；与 `guides\scripting-core.md` §一 分工） | 26 文件（`define_values.txt`、`unit_values.txt`…；**无 `00_default.txt`**） |
| scripted_lists（main_menu） `[有 info]` | 自定义脚本列表（`base` 只写 `any_/random_/every_/ordered_` **之后**的部分 + `conditions`） | `main_menu\common\scripted_lists\` |
| scripted_modifiers `[有 info]` | **权重片段**（modifier / opinion_modifier / compare_modifier；**不是国家修正**） | `common\scripted_modifiers\` |
| scripted_rules | **空壳（info 仅 3 字节）** | 无数据文件 |
| scripted_country_names / scripted_guis / scriptable_hints | 国名生成（**12 条**，`country_trigger` vs `capital_trigger`；字段档 `fields\common-small-registries.md`）/ 脚本化 GUI / 界面提示（后两者见「界面层」组） | 各同名文件 |
| scripted_geography `[有 info]` | 脚本化地理集合（见「地图与地理」组） | `common\scripted_geography\` |
| missions / mission_task_defs | 任务链（**11 条链 / 108 个节点 / 142 KB**；链 = 候选池 `visible`+`chance`，节点 = 限时任务 `duration`；`____Info.txt` 61 行是字段权威，**没有 readme.txt**）；**`mission_task_defs` 是空壳·勿删**（任务项在 missions 里，删目录会 assert） | `missions\generic_*_mission_pack.txt`（11 档） |
| disasters `[有readme]` | 灾难 | 00_default.txt、decline_of_majapahit.txt |
| situations `[有readme]` | 局势 | 00_default.txt、little_ice_age.txt、black_death.txt |
| diseases `[有readme]` | 疾病 | bubonic_plague.txt |
| movements `[有readme]` | 思潮（**4 条**；与疾病同一套传播模型：`r0`/`calc_interval_days`/`location_spread_threshold` + 相同的"4 不传播 / 4 传播目标"规则） | lutheranism_movement.txt、calvinism_movement.txt… |
| rebel_demands | 叛乱诉求（**13 条顶层**：4 类别 + 7 阶层 + 兜底 + 国别；**文件名 `000_`–`998_` 前缀 = 加载优先级**） | 999_default_rebel_demands.txt、900_country_specific… |
| institution `[有readme]` | 制度 | 00_default.txt |
| historical_scores | 历史评分（**21 条**：`tag` + `score`；字段档 `fields\common-small-registries.md`） | 00_default.txt |
| age `[无readme]` | 时代（age_1_traditions…） | 00_default.txt |
| advances `[有readme]` | 科技（**改革槽位与官僚槽位都在这里**：`government_reform_slots` 14 条、`global_max_bureaucracy_slots` 5 条） | 0_age_of_traditions.txt…、2_army_unlocks.txt |
| societal_values | 社会价值轴（**17 条**，内部名 `A_vs_B`；推动靠 `main_menu\common\static_modifiers\country.txt` 的 **30 个 `societal_value_push_*`**） | 00_default.txt |
| customizable_localization | 可定制本地化（动态文本） | 00_default.txt |
| effect_localization / trigger_localization | 效果/触发自定义显示名 | 各 *_effects.txt、*_triggers.txt |

> **事件 · 任务完整机制**（7,470 个事件的目录/字段/取值实测与"文档有、原版零用"清单、四种触发方式用量、DHE 布局、任务链候选池与 6 个 `on_mission_*` 钩子、默认游戏规则关任务包、任务 loc 子目录）详见 `vanilla\vanilla-events-and-missions.md`；字段权威见 `fields\common-events.md`、`fields\common-missions.md`。

## 地图与地理

| 类目 | 用途 | 入口文件 |
|---|---|---|
| topography `[无readme]` | 地形（movement_cost/defender/weather 衰减/local_frontage_allowed） | 00_default.txt |
| vegetation `[无readme]` | 植被（woods/forest/jungle/desert…） | 00_default.txt |
| climates `[无readme]` | 气候带（winter 等级/has_precipitation/location_modifier） | 00_default.txt |
| road_types `[有readme]` | 道路 | 00_default.txt |
| area_preferences | 区域偏好（**83 条**：exploration 25 / conquest 58；**纯数据，须用 `add_area_preference` 指派**） | `exploration_preferences.txt`、`conquest_preferences.txt`（**无 `00_default.txt`**） |
| location_ranks | 地点等级（**4 级**，每级一组 `local_<pop>_desired_pop`；**顺序即等级**；字段档 `fields\common-small-registries.md`） | 00_default.txt |

**⚠️ 地图本体数据不在 `common\` 而在 `in_game\map_data\`**（改地图层级/地点属性时别在 common 里找）：

| 文件 | 用途 |
|---|---|
| `map_data\definitions.txt` | **五级静态层级树**：continent → sub_continent → region → area → province_definition → location（缩进即层级） |
| `map_data\location_templates.txt` | **地点属性表**（28573 条）：topography / vegetation / climate / culture / religion / raw_material / natural_harbor_suitability |
| `map_data\default.map` | 地图总声明（7 个数据文件 + equator_y/wrap_x）+ 7 个特殊地点区（sea_zones / lakes / impassable_mountains / non_ownable / volcanoes / earthquakes / sound_toll） |
| `map_data\ports.csv` / `adjacencies.csv` | 港口（4421）/ 跨海连接（184） |
| `map_data\locations.png` + `named_locations\00_default.txt` | 地点位图与其颜色对照（键 = 颜色） |
| `common\scripted_geography\` | 自定义地理包（可混任意层级；`scripted_geography:` 作用域 + 5 种专属脚本列表） |
| `main_menu\setup\start\` | **开局数据层（全 25 档，编号 02–27、缺 01/17）**：`06_pops.txt` 5 MB 逐地点 `define_pop`、`05_characters.txt` **104,547 行**、`10_countries.txt` 1.58 MB 领土+`include`、`08_institutions` 662 KB、`07_cities_and_buildings` 257 KB、`04_dynasties` 238 KB、`15_IO` 67 KB、`12_diplomacy` 50 KB、`18_opinions` / `20_rivals` / `23_colonies` / `24_town_rights` / `25_area_preferences` / `26_ai_personalities` / `27_armies` / `19_diseases` / `22_situations` / `21_locations`（**本轮全部解析**）。**完整逐档表与"改开局动哪档"决策表见 `vanilla\vanilla-setup-data.md`** |
| **`main_menu\setup\templates\`** | ★ **开局模板层（205 档）**：`include = "<模板名>"`（`10_countries.txt` 里 **5,256 次 / 192 去重**）拼开局——`government` 168 档（含继承法/议会/13 条社会价值观初值）、`starting_technology_level` 123、`discovered_regions/areas/provinces` 39/33/25、`court_language` 14、`country_rank` 10，**模板可嵌套**（57 档）。命名 `expl_<地区>` 与 `<宗教/文化区>_<政体>[_no_coast|_not_present|_no_censor…]`。**端到端用法见 `guides\new-country-tutorial.md` §2b** |

详见 `vanilla\vanilla-map-and-geography.md`。

## AI 与其他

| 类目 | 用途 | 入口文件 |
|---|---|---|
| ai_personalities | AI 国家性格（**8 种**，`ai_balanced` 为 default；**无 readme**） | `00_ai_personalities.txt`（**没有** `00_default.txt`） |
| ai_diplochance | AI 外交接受度权重（**16 张表 / 91 个去重评估键**（使用 168 处）；`ai_diplochance.info` 是放错位置的 customizable_localization 文档） | `00_ai_diplochance.txt` |
| ai_scripted_expansion_score / ai_scripted_expansion_target `[有readme]` | AI 扩张评分与目标（原版各 **2 条**，全在 `guelphs_and_ghibellines.txt`） | `guelphs_and_ghibellines.txt` |
| scripted_diplomatic_objectives `[有readme]` | 脚本化外交目标（**5 条**：HRE 选帝权 / 日本战国谋报） | `hre_emperorship.txt`、`japanese_clan_spying.txt` |
| rival_criteria `[有readme]` | 宿敌标准（**6 条**，只有 AI 遵守） | `europe.txt` |
| join_war_rules `[有readme]` | 参战规则（**1 条**） | `teu_crusade.txt` |
| generic_actions `[有readme]` / generic_action_ai_lists `[有readme]` | 通用行动与 AI 分组（AI 表 **93 条 / 85 文件**；行动用 `ai_tick` + `ai_tick_frequency` 定节拍） | 00_default.txt、`global_list.txt`… |
| alert_descriptions | 警报定义（title/texture/priority） | 00_default.txt |
| attribute_columns | 列表视图列定义 | 13_combat.txt、05_location.txt |
| auto_modifiers `[有readme]` | 自动修正（country.txt 1555 行：country_base_values 等） | country.txt、location.txt |
| biases | 观感来源注册表（**1,140 条**；`00_opinion_hardcoded` 头注"removing may cause problems"；字段档 `fields\common-biases.md`） | 01_opinion_scripted_diplomacy.txt… |
| artist_types / artist_work `[有readme]` | 艺术家 | 00_default.txt |
| music_player_tracks `[有 info]` | 音乐（条目名 = **wwise 事件键**；composer/performer/soloist 是 loc 键） | `00_music_player_tracks.txt`（**无 `00_default.txt`**） |
| tests `[有readme]` | 游戏自带测试用例 | 00_default.txt |
| tutorial_lessons / tutorial_lesson_chains `[有 info]` | 教程（chain → lesson → step；`save_progress_in_gamestate` 是暂停/effect 的总开关） | 4 个 `00_tutorial_lesson_*.txt`（**无 `00_default.txt`**） |
| country_description_categories | 见政府组 | — |

> **AI 的六层结构**（性格层 / 权重层 / 节拍层 / 常量层 / 脚本钩子层 / 难度层）、`NAI` 段 746 个常量与权重键分布，详见 `vanilla\vanilla-ai.md`。改 AI 前先定位层级：原版 AI 主要活在 **defines 的 `NAI` 段**与各处的 **`ai_will_do`（660 处）**里，上表这些"脚本化 AI 钩子"原版用得极少（2 / 2 / 5 / 6 / 1 条）。

## 界面层（`gui\` + 配套数据目录）

| 类目 | 用途 | 入口文件 |
|---|---|---|
| `in_game\gui\`（**387 个 .gui / 408 文件 / 9.4 MB**） | 游戏内界面；`types <集合>` + `type 新 = 基` + `block`/`blockoverride` + `using` | `ui_library.gui`（588 KB 控件库，`types UiLibrary`）、`*_lateralview.gui`（41 个）、`panels\`（9 主题组）、`shared\`（tooltip 库） |
| `gui\filters\` `[有readme]` | 列表筛选器（**GUI 层唯一有实质文档的目录**，2,212 B） | `06_country.txt`… + `readme.txt` |
| `gui\attribute_columns\` + `common\attribute_columns\` `[有readme]` | 列表列（gui 管外观 / common 管数据与排序） | `character.gui`…；`common\attribute_columns\readme.txt` 2,142 B |
| `gui\sort_keys\` | 排序键（icon/tooltip） | `00_sort_keys.txt` |
| `common\scripted_guis\` `[有 info]` | **脚本化 GUI**（玩家能点、AI 也能点） | `economy_satisfaction_target.txt` 17.8 KB + `scripted_guis.info` |
| `common\customizable_localization\` `[有 info]` | 动态文本（`[<scope>.Custom('key')]`） | `00_customizable_localization.txt` + info 604 B |
| `main_menu\gui\messagetypes.txt` | **消息类型 1,348 条**（log/onmap/popup/idle/option/pausepopup/message_category） | `messagetypes.txt` 177 KB |
| `main_menu\gui\scripted_widgets\` `[有 info]` | 声明式挂载控件（`gui/x.gui = 名`；⚠️ `visible = yes` 无效） | `_scripted_widgets.info` |
| `common\alert_descriptions\` | **警报 133 条**（title/texture/priority 必填 + hint 38 / game_concept 15） | `00_default.txt` |
| `common\scriptable_hints\` `[有 info]` | **提示 93 条**（`needed` 每帧求值；与警报用 `hint =` 挂钩） | `scripted_hints.txt`、`____info.txt` |
| `common\trigger_localization\` / `effect_localization\` `[trigger 有 info]` | **tooltip 文本权威**（人称/时态/否定/比较运算条目） | 各 44 / 36 文件 |
| `loading_screen\gui\` | 字体模板 / 文本格式 / tooltip / sounds / defaults。⚠️ **文本样式在 `textformatting.gui`（704 行）**，`fonttemplates.gui` 只 1.5 KB 且多为注释示例 | `fonttemplates.gui`、`textformatting.gui` |
| **`main_menu\notifications\game.txt`** | **通知/对话框（原版 7 条，2 KB）**：`level = dialog\|alert` + `category` + `window_file`/`window_name` + 4 个 loc 键（`title_text`/`text`/`accept_text`/`decline_text`）；文件头两个 `@default_window_*` 宏指向 `main_menu\gui\notifications\jomini_message.gui`。**与 `messagetypes`（消息）是两码事**；mod 侧入口 = SGUI 的 `notification_key`（原版**零使用**） | `vanilla\vanilla-gui.md` §6.5 |
| **`content_source\map_objects\`** | **地图对象生成层（14 档 / 20 MB）**：`generators\vegetation_generators.txt`（`layer` + `max_density` + `mask` + 加权 `meshes`，**low/medium/high 三件套靠 `@density_factor_*` 宏**，官方注明"密度最低者优先占位"）+ `ambience_generators`（`entity` + `topography`/`vegetation`/`climate`/`raw_material`/`avoid_sea`）+ `masks\*.png` 12 张 20 MB | `vanilla\vanilla-dlc-and-assets.md` §六 |
| **`loading_screen\sound\`**（**音频层**） | 1,267 档 / 2.3 GB：`audio_settings.txt`（引擎/6 档案/VCA 总线）、`banks\windows\`（**10 个语义化 `.bnk`** + `Media\*.wem` **1,225 个（名=Wwise ID）** + `Init.txt` 644 行映射表）、`map\ambience\`（11 文本档 + 官方 `sensorgen.py`）、`persistent_objects\`。曲目表在 `in_game\common\music_player_tracks\`（**82 曲**）、按文化配乐在 `main_menu\music\audio_culture_types\`。**替换曲目可行；新增曲目必须回 Wwise 重打包** | `vanilla\vanilla-audio-and-fonts.md` §一 |
| **字体层 `loading_screen\fonts\`** | **289 档 / 541 MB**（`.ttf` 250 + `.otf` 24，13 字族各带 `OFL.txt`）+ **6 个 `.font` 定义跨三区**（`loading_screen_fonts` / `cw_fonts` / headers ×2 / `in_game_fonts` / `main_menu_fonts`）：`fontfiles` 声明语言→字体文件（**有序=回退链**），`font` 声明样式名→字体组 | `vanilla\vanilla-audio-and-fonts.md` §二 |
| `main_menu\gfx\interface\`（**资产主干**） | **icons 6,705 档 / 112 个主题目录**（modifier_types 1,377 · buildings 473 · flat_icons 461 · government_reforms 337 · religion 294 …）、**illustrations 1,065 档 / 25 个主题目录**（units 293 · location 241 · event 220 · disaster 32 · missions 11 …）、**advance 1,634 档**（革新图标，**任务节点图标也取自这里 108/108**） | `icons\<主题>\<名>.dds`、`illustrations\<主题>\<名>.dds` |
| `in_game\gfx\`（**3D 资产**） | **22,369 档 / 7.6 GB**：models 17,327、terrain2 4,545、map 348（1.4 GB）、city_materials 37、compound_nodes 82 | `.mesh` + `.asset` 成对 |
| `dlc\`（**DLC 层**） | 4 个包：`D000_shared` 24 档（3 个 DLC 定义 + 11 语言文案 + 图标）、`D008` **1,141 档 / 539 MB**（`in_game` 765 / `main_menu` 350 / `loading_screen` 23）、`D015` 107、`D017` 90；**只新增不覆盖**（与基础包 0 路径碰撞）；门控 `has_dlc = "<dlc 名>"`（**44 个基础包文件**在用） | `dlc\<包>\<区>\…` + `<包>.dlc.json` |

> **DLC 层与美术资源层的完整机制**（DLC 定义 13 字段实查表、`.dlc.json` 与 mod `metadata.json` 的关系、**图片路径解析实测**：`image` 是区根相对路径且跨基础包+DLC 找、`illustration_tags` 是**加权块** `{ 10 = happy }` 及其词汇表、单位外观三份官方 `.info` 与 9 级命名优先级、可改点表）详见 `vanilla\vanilla-dlc-and-assets.md`。⚠️ 尚未成档：`main_menu\common\flag_definitions`（187 KB）与 `coat_of_arms`（15 档）。

> **界面层完整机制**（四层构成、语法实测计数、`GetVariableSystem`、数据函数 top20、SGUI 全字段、四个数据目录、三条硬规则与三个有官方原文的坑）详见 `vanilla\vanilla-gui.md`。

## main_menu\common（13 类目）

achievements `[有readme]`（成就）、**coat_of_arms（纹章本体 + 随机池 + 图集，9 档 / 4,566 个 COA 键 + 5 池档 + `atlases.txt`；字段档 `fields\main_menu-coat_of_arms.md`）**、**flag_definitions（旗帜规则，259 列表 / 1,133 定义，**文件头自带官方 schema**；字段档 `fields\main_menu-flag_definitions.md`）**、**game_concepts（概念词条 696 条；字段档 `fields\main_menu-game_concepts.md`）**、game_rules（游戏规则，`_game_rules.info` 权威）、**modifier_icons（修正图标映射 2,427 条；字段档 `fields\main_menu-modifier_icons.md`）**、**modifier_type_definitions（修正键注册表，目录合计 2,436 键；字段档 `fields\main_menu-modifier_type_definitions.md`）**、**named_colors（具名色板 4,103 色；`setup\countries` 的 `color = map_*` 用了 1,004 次；字段档 `fields\main_menu-named_colors.md`）**、**scenarios（推荐开局卡 10 个；`flag` 是 **COA 键不是 tag**；字段档 `fields\main_menu-scenarios.md`）**、scripted_lists / scripted_triggers / script_values（主菜单域脚本）、static_modifiers（静态修正）。

> **纹章与旗帜完整机制**（五层模型、拿旗流程与 `priority` 竞争、`subject_canton` 与宗主角标、四个加权池与条件子池、图集四档尺寸、贴图 4,138 档 ≈519 MB、"想改什么动哪层"表）详见 `vanilla\vanilla-heraldry-and-flags.md`。

**⚠️ 基础的 `main_menu\common\` 里没有 `dlc\`**——`common\dlc\<pack>.txt`（13 字段）只存在于 `dlc\D000_shared\`，且 mod 无法据此注册 DLC（见 `vanilla\vanilla-dlc-and-assets.md` §二）。

## loading_screen\common\defines

`00_defines.txt` + `graphic\00_graphics.txt` + `jomini\`（tooltips/adjacencies/fog_of_war/icons/mapeditor/rivers/roads）——见 `defines.md`。
