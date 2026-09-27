# 原版解析：外交（vanilla diplomacy）

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

**核心文件**：`in_game\common\country_interactions\`（89 文件 / 346KB / **138 个交互**，`readme.txt` **238 行**为字段权威）、`common\scripted_relations\`（35 文件 / 94KB / **34 种条约关系**，readme 同为两百余行）、`common\subject_types\`（18 档 / 72KB / **20 个附属国定义**，readme 约 100 字段）、`common\diplomatic_costs\`（`00_hardcoded.txt` + `01_from_script.txt`）、`common\rival_criteria\`、`common\join_war_rules\`、`common\scripted_diplomatic_objectives\`、`common\insults\00_insults.txt`；常量在 **`loading_screen\common\defines\00_defines.txt` 的 `NDiplomacy`（1992–2417 行，426 行）**；等级相关在 `common\country_ranks\00_default.txt` 的 `rank_modifier`。

> ⚠️ **`ai_diplochance\ai_diplochance.info` 不是外交文档**——它写的是 **customizable localization** 的格式说明，放错目录了。真正的 AI 接受度键清单在 `country_interactions\readme.txt` 第 110–238 行（**128 个键**）。
> ⚠️ `diplomatic_costs` 两个文件**都没有 readme**（字段库亦无档）——本篇 §四.3 的字段表是实查结果。

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `diplomacy` | 外交 | 两个在**外交范围**内的国家之间的互动，由**外交官**推动 |
| `diplomat` / `diplomatic_capacity` | 外交官 / **外交容量** | 一段时间内能做多少外交；超出容量受声誉与威望惩罚 |
| `diplomatic_range` | 外交范围 | 按国家等级 |
| `opinion` | **好感** | −200..200；"高好感防止敌意，但需要信任才能合作" |
| `trust` | **信任度** | 0..100（均衡 50）；"长久协议的关键" |
| `antagonism` | **敌意** | 0..1000；EU5 版的"侵略扩张" |
| `favor` / `favors` | **人情** | 0..100；地下与常规外交的通用筹码 |
| `spy_network` | **间谍网** | 渗透程度；"地下外交行动"的货币 |
| `rival` | **宿敌** | 不双向；不接受友善条约 |
| `union` / `personal_union` | **共主邦联** | 因王室联姻共主形成的条约类型 |
| `royal_marriage` | 王室联姻 | 联姻数值见 §二 |
| `subject` / `overlord` | **附属国** / 宗主国 | 17 种 `subject_type`；允许长链 |
| `subject_loyalty` / `liberty_desire` | 忠诚 / **独立倾向** | −100..+100，每月向 0 衰减 |
| `coalition` | **包围网** | 敌意门槛 50 |
| `great_power` / `hegemony` | 列强 / 霸权 | 附属国不能成为列强 |
| `ai_disposition` | **国家态度** | AI 的威胁/机会/友好三态机 |

## 一、总览：资源侧 + 两套并列机制 + 一套 AI

```
[资源侧] 外交官(max_diplomats) · 外交容量(diplomatic_capacity) · 外交范围(diplomatic_range)
           ↓ 超出容量 → 外交声誉 + 威望惩罚
[机制 A] 国家交互 country_interactions（138 个：diplomacy 102 / subject 28 / union 8）
           → 一次性动作：提议、请求、宣战、间谍、赐爵…
[机制 B] 条约关系 scripted_relations（34 种：同盟、通行权、禁运、保障…）
           → 可开关的持续状态：占外交容量、带自动修正与观感、可双向转移贸易/金币/人情/思潮
[AI 层] 势力倾向(disposition：威胁/机会/友好) + diplo_chance(128 键接受度) + scripted_diplomatic_objectives
[粘合值] 好感 · 信任度 · 敌意 · 人情 · 间谍网
```

## 二、五个关系值（外交的"血条"）

| 值 | 范围 | 关键机制与常量（`NDiplomacy`） |
|---|---|---|
| `opinion` **好感** | **−200 .. 200** | `OPINION_GOOD_RELATIONS = 50`（宣战 −1 稳定）/ `OPINION_GREAT_RELATIONS = 100`（−2 稳定）/ **`OPINION_NO_WAR = 150`（AI 好感太高就不宣战）**；改善关系强度 `improve_relation_impact`（修正） |
| `trust` **信任度** | **0 .. 100**，`TRUST_EQUILIBRIUM = 50` | 月衰减 `−0.5` / 月恢复 `+0.05`（再按外交声誉 `+0.005`）；**宣战 −20**、破停战（防守方 **−30** / 范围内 −5）、**拒绝召唤参战 −25**；`IMMEDIATE_TRUST_FACTOR_FROM_PROFESS_TRUST = 0.5`、`PROFESS_TRUST_COOLDOWN_MONTHS = 24`；修正 `trust_recovery` / `trust_decay` |
| `antagonism` **敌意** | **0 .. 1000**（bias 下限 −100） | 见 §七；对好感影响 `ANTAGONISM_IMPACT_ON_OPINION = −4` |
| `favors` **人情** | **0 .. 100** | `CURRY_FAVORS = 0.5/月`（需好感 ≥ `CURRY_FAVORS_RELATION_REQUIRED = 10`，单次上限 `CURRY_FAVORS_MAX_FAVORS = 25`）；`GIFT_FAVORS = 5`（送礼）；拒绝消耗人情 → `FAVORS_REJECTED_TRUST_LOSS = 0.25`×用量、`FAVORS_REJECTED_DIP_REP_LOSS = 0.005`×用量×对方信任（持续 `FAVORS_REJECTED_DIP_REP_MONTHS = 24` 月/点） |
| `spy_network` **间谍网** | — | `MONTHLY_SPYNETWORK = 1`、`SPY_NETWORK_DECAY = −1`；**攻城 `SPY_NETWORK_SIEGE_EFFECT = 0.2`、降敌意 `SPY_NETWORK_AE_EFFECT = −0.1`**；被发现 `BASE_SPY_DISCOVERY_CHANCE_PER_YEAR = 0.25`（需网络 ≥ `MIN_SPY_NETWORK_SIZE_FOR_DISCOVERY = 25`）、冷却 `SPY_DISCOVERY_COOLDOWN_MONTHS = 12`；修正 `spy_network_construction` |

**王室联姻数值**：`ROYAL_MARRIAGE_BASE = 2` + 威望差 `×0.01` + 等级威望差 `×0.25`；**后续配偶收益 ×0.5**；统治者权重 `×2`、第 N 顺位 `×0.2`；另有权重常量 `ROYAL_MARRIAGE_RULER_WEIGHT = 20 / HEIR = 10 / OTHER = 10`。联姻的**角色侧**（婚姻与生育、继承法与 `heir_selection_score`、摄政 15 种、宗族 `dynasty`）见 `vanilla\vanilla-character-dynasty-cabinet.md`。

## 三、资源侧：外交官 · 外交容量 · 外交范围（按国家等级）

`common\country_ranks\00_default.txt` 的 `rank_modifier` 实查：

| 等级 | `max_diplomats` | `diplomatic_range` | `diplomatic_capacity` | `num_possible_rivals` | 允许同盟 / 保障独立 / 支持叛军 |
|---|---|---|---|---|---|
| `rank_empire` 帝国 | 2 | **1000** | **3** | 4 | ✓ / ✓ / ✓ |
| `rank_kingdom` 王国 | 1 | 500 | 2 | 3 | ✓ / ✓ / ✓ |
| `rank_duchy` 公爵领 | — | 200 | 1 | 2 | ✓ / ✓ / ✗ |
| `rank_county` 伯爵领 | — | — | — | 1 | ✓ / ✗ / ✗ |

**外交容量的官方解释**（`game_concept_diplomatic_capacity_desc` 原文）：

> "与所有人结盟就是不与任何人结盟。**维持比外交容量更多关系的国家将受到外交声誉和威望惩罚**。特定附属国类型也会占用**外交维持费**，这也会削弱可用的外交行动。成为某些国际组织的成员会占用外交容量，**你盟友的实力决定了该同盟占用的外交容量**。"

- 容量来源：**国家等级**（上表）+ **革新**（`diplomatic_capacity = 1` / `diplomatic_capacity_modifier = 0.1~0.2`，散落在 `advances\` 数十处）。
- 范围扩展：`diplomatic_range_modifier`（如 `0_age_of_discovery.txt:228` +0.25、`0_age_of_absolutism.txt:263` +0.33）。
- **外交官要跑路**：交互/条约的 `use_enroute = yes/no` + `TRAVEL_DAYS = 30`；`skip_diplomat_for_cancel` 让取消免跑。修正 `send_diplomat_cost_modifier`、`allow_cabinet_diplomatic_corps`（内阁"外交使团"）、`bias_for_diplomat_policies`。
- 维护成本：`diplomatic_upkeep_efficiency`、`diplomatic_spending_cost`、`diplomatic_annexation_efficiency`。

## 四、机制 A：138 个「国家交互」（`country_interactions`）

### 4.1 三种 `type` 与分布（花括号深度解析实查）

| `type` | 数量 | 内容抽样 |
|---|---|---|
| **`diplomacy`** | **102** | `improve_cultural_view`、`force_embargo`、`violate_sovereignty`、`annul_casus_belli`、`influence_nation`、`lend_unit_to_ally`、`transfer_occupation`、`surrender_civil_war`、间谍行动（`steal_technology` / `steal_maps` / `sabotage_reputation` / `scout_enemy_holdings` / `sabotage_clans_buildings` / `assassinate_character`）、借款三件套（`request_loan` / `renegotiate_loan` / `take_over_loan`）、艺术品（`sell_work_of_art` / `request_work_of_art_purchase` / `sell_icon`）、**HRE 全套**（`ask_to_vote_for_me_as_emperor` / `bestow_elector_status` / `bestow_free_city_status` / `appoint_imperial_circle_leader` / `call_imperial_circle_to_war` / `bolster_imperial_army` / `petition_imperial_diet` / `enforce_landfriede`…）、**教廷**（`excommunicate` / `lift_excommunication` / `place_interdict` / `ask_support_curia_proposal` / `bribe_voter_for_policy`）、`demand_conversion_to_islam`、`nusta_marriage`、`formalize_italian_state_relations` |
| **`subject`** | **28** | `transfer_subject`、`give_location_to_subject`、`give_province_to_subject`、`give_subject_location_to_other_subject`、`intervene_in_subject_war` / `_regular_war` / `_civil_war`、`change_subject_policy`、`change_subject_court_language`、`improve_cultural_view_subject`、`demand_additional_tribute`、`demand_silver_tribute`、`promote_member_to_celestial_governor`、`demote_celestial_governor_to_vassal`、`appanage_*`（征召/夺宫廷资源/召唤参战） |
| **`union`** | **8** | `break_union`、`propose_ruler`、`intervene_in_union_civil_war`、`take_over_seniority`、`rein_in_junior_diplomacy`… |

`type` **只有这三个合法值**——用"看文件第一行"的粗糙解析会被嵌套块（`ai_spam_protection_*` 之类）骗到。

### 4.2 交互字段族（`readme.txt` 238 行归纳）

| 组 | 字段 |
|---|---|
| 门 | `potential` / `allow`（**scope:actor = 发起国**）、`block_when_at_war`（默认 yes） |
| 价格 | `price = price:<id>` 或 `scope:target.price`、`price_modifier`（乘数）、**`payer` / `payee`**（默认发起国付、无人收 → "price disappears into the ether"） |
| 外交代价 | `diplomatic_cost = <diplomatic_cost_id>`（见 4.3）+ `diplomatic_cost_modifier` |
| 选目标 | `select_trigger`（可多个，依次落入 `scope:target` / `target_1`…）：`looking_for_a`、`source`（actor/recipient/rivals/subjects/atwar/inrange/knowncountries…）、`source_flags`（19 种性能滤镜：neighbor/border/adjacent_provinces/border_or_recipients_capital_area/same_international_organization…）、`source_global_list`、`interaction_source_list`、`ai_interaction_source_list`、`column`/`default_sort`/`map_mode`/`map_color`/`secondary_map_color`、`allow_null`/`allow_self`、`top_widget`/`bottom_widget` |
| AI 预筛 | `pre_evaluation_sort_value` + `pre_evaluation_number_to_evaluate_fully`（=只全量评估前 X 个）、`max_targets_for_ui`、**缓存三件套**（`cache_targets` / `cache_interaction_source_list` / `cache_order`） |
| AI 行为 | `accept`（脚本值）、`diplo_chance`（**128 个键**，见 4.4）、`ai_tick`（never/daily/monthly）、`ai_tick_frequency`（`monthly + 6` = 每 6 月检查）、**`ai_prerequisite`（只跑 scope:actor，纯性能用）**、`ai_will_do`、`ai_limit_per_check` |
| 结果 | `effect`、**`reject_effect`**、`cooldown = { type = <tag> days/weeks/months/years = N }`、`show_message` / `show_message_to_target` / `show_in_gui_list`（=no 时自建按钮）/ `should_execute_price` |

### 4.3 `diplomatic_costs`：两种代价货币（**无 readme，实查**）

```
使用格式：交互里写 diplomatic_cost = <id>，再在 common\diplomatic_costs\ 里给该 id 定价
```

| 货币 | 原版定价（`00_hardcoded.txt` + `01_from_script.txt`） |
|---|---|
| `spy_network` 间谍网 | `infiltrate_administration` 50、`scout_enemy_holdings` 50、**`steal_technology` 75**、`sabotage_clans_buildings` 70、`steal_maps` 40、`sow_discontent` 40、`support_rebels` 30、`agitate_for_liberty` 30、`casus_belli_creation` 20、`corrupt_officials` 20、`sabotage_reputation` 5 |
| `favors` 人情 | `anti_piracy_agreement` 50、`improve_cultural_view` 50、`ask_to_vote_for_me_as_emperor` 50、`ask_for_money` 25、`force_change_court_language` 20、`ask_join_war_for_favors` 10、教廷两案（支持/否决）10、`invite_settlers` 5、`military_access` / `fleet_basing_rights` 5、`break_others_alliance` 1（再乘被破坏的条约量）、`renegotiate_loan` 1 |
| 空价（由效果自行处理） | `sell_icon`、`subject_return_land`、`start_war_in_colony`、`knowledge_sharing`、`demand_additional_tribute` |

### 4.4 AI 接受度：`diplo_chance` 的 128 个键（外交与和谈共用同一套）

按用途分组（`country_interactions\readme.txt:110–238` 全清单）：

| 组 | 键 |
|---|---|
| 战争状态 | `at_war`、`recipient_at_war`、`actor_at_war`、`actor_civil_war`、`recipient_civil_war`、`multiple_offensive_wars`、`another_war`、`fighting_together`、`separate_peace`、`call_for_peace`、`war_enthusiam`、`war_exhaustion`、`warscore`、`war_balance`、`war_goal`、`making_gains`、`on_retreat`、`last_major_battle`、`has_truce`、`has_truce_with_target` |
| 关系值 | `opinion`、`positive_opinion`、`negative_opinion`、`target_opinion`、`trust_in_actor`、`trust_in_recipient`、`positive_trust_in_actor`、`negative_trust_in_actor`、`royal_ties`、`culture_view`、`religion_view`、`estates_like`、`estates_dislike`、`antagonism`、`too_much_antagonism`、`common_threat`、`competing_power` |
| 身份差异 | `same_religion` / `different_religion` / `different_religion_group`、`same_culture` / `different_culture`、`same_court_language` / `same_common_language`、`same_government_type` / `different_government_type`、`rank` / `rank_difference`、`same_international_organization` |
| 地理 | `capital`、`capital_distance`、`border_distance`、`province_distance`、`has_border` / `lacks_border`、`no_access`、`giving_them_access`、`location_value` / `base_location_value`、`interesting` / `vital` / `avoided` |
| 实力 | `current_strength`、`potential_strength`、`relative_strength`、`defeat`、`victory`、`desperation`、`low_manpower`、`in_debt`、`stability` / `positive_stability` / `negative_stability`、`tax_base`、`produced_goods` |
| 宿敌/冲突 | `actor_is_rival`、`recipient_is_rival`、`allied_to_enemy`、`betrayed_ally`、`conflicting_interests`、`conquer_desire`、`claim`、`revolter`、`enforced_demand` |
| 条约/容量 | `max_relations`、`actor_max_relations`、`few_relations`、`overlord`、`disloyal_subject`、`junior_to`、`belongs_to_international_organization`、`giving_defensive_support` / `receiving_defensive_support` |
| 人情/贷款 | `using_favors`、`unbalanced_favors`、`need_loan`、`interest_rate_too_high`、`good_interest_rate`、`existing_loans_from_country`、`too_many_loans`、`loan_is_insignificant`、`loan_ends_too_soon` / `loan_ends_too_late` |
| 和谈专用 | `peaceoffer`、`peaceoffer_most_of_wanted`、`best_possible_offer`、`want_more`、`want_something_else`、`demands_made`、`promised_land`、`substantial_land_lost`、`my_proposal` |
| 杂项 | `price`、`cost`、`price_percentage_of_treasury_funds`、`months_at_war`、`diplomatic_reputation`、`culture_war`、`planning_demise`、`heir`、`ai_setting`、`yesman`（调试）、`tutorial`、`no_action`、`base`、`strategic_interest` |

## 五、机制 B：34 种「条约关系」（`scripted_relations`）

**这是 EU5 最有特色的设计**——同盟/通行权这类"关系"不是硬编码系统，而是**可开关的关系对象**：

**原版 34 种**：`alliance`、`military_access`、`fleet_basing_rights`、`food_access`、`guarantee`、`guarantee_vassal_independence`、`trade_access`、`embargo_nation`、`sound_toll_exemption`、`exclusive_trade_rights_with_isolated`、`anti_piracy_agreement`、`anti_coalition_treaty`、`block_foreign_buildings`、`deny_market_access`、`divert_trade`、`forced_divert_trade`、`fondaco_rights`、`knowledge_sharing`、`send_officers`、`military_sponsorship`、`support_military`、`support_heir`、`support_loyalists`、`strengthen_family_ties`、`scutage`、`rein_in_junior_diplomacy`、`grant_administrative_autonomy`、`agitate_for_liberty`、`sow_dislcontent`、`corrupt_officials`、`wako_sponsorship`、`union_of_crowns_pact`、`italian_wars_forced_access`、`curb_pronoia_military_action`。

### 5.1 字段族（readme 归纳）

| 组 | 字段 |
|---|---|
| 身份 | `type`（diplomacy/subject/union）、**`relation_type = oneway/mutual`**（单向 = 给予方/接受方）、**`uses_diplo_capacity = none/mutual/giving/receiving`** + `diplomatic_capacity_cost = <script value>` |
| 断链 | `block_when_at_war`、`break_on_war`、`break_on_becoming_subject`、**`break_on_not_spying`**、`annulled_by_peace_treaty` + **`annullment_favours_required`**（用人情解除） |
| 效果开关 | `disallow_war`（**禁止宣战**）、`embargo`、`military_access`、`fleet_basing_rights`、`food_access`、`is_exempt_from_sound_toll`、`is_exempt_from_isolation`、`block_building`、**`lifts_fog_of_war`**、**`lifts_trade_protection`**（取消市场保护主义） |
| 参战 | **`called_in_defensively` / `called_in_offensively` = none/mutual/giving/receiving** |
| 双向转移 | `trade_to_first/second`（市场吸引力）、`gold_to_first/second`、`favors_to_first/second`、**`institution_spread_to_first/second`**（思潮传播） |
| 价格 | `diplomatic_cost`、`war_declaration_cost`（**该关系存在时宣战要付的代价**）、`buy_price`（未写 = 不可买）、`monthly_ongoing_price_first_country` / `_second_country`（持续维护费） |
| 四组门 | `visible` + `offer_visible` / `request_visible` / `cancel_visible` / `break_visible`；`offer_enabled` / `request_enabled` / `cancel_enabled` / `break_enabled`；`will_expire_trigger`（自动过期）、`should_ai_offer_trigger` |
| AI 双轨 | **`wants_to_give` / `wants_to_receive` / `wants_to_keep`（ai evaluation）** 与 **`wants_to_give_diplo_chance` / …（diplo evaluation）**——两套并存，作用域与用途不同；`show_break_alert` |
| 效果钩子 | `offer_effect`、`request_effect`、`cancel_effect`、`break_effect`、`offer_declined_effect`、`request_declined_effect`、`expire_effect` |
| 持续行动区 | **`is_ongoing = yes` + `texture_file` + `concept` + `progress`（0–100）**——外交面板"进行中"进度条（吞并、传教等用它显示进度） |
| 其它 | `sound`、`mutual_color` / `giving_color` / `receiving_color`（外交地图模式配色）、`giving_modifier_scale` / `receiving_modifier_scale` / `mutual_modifier_scale`（**脚本数学缩放修正**） |

### 5.2 自动挂载的修正与观感（命名约定）

```
<key>            / giving_<key>          / receiving_<key>            → 自动国家修正
opinion_<key>    / opinion_giving_<key>  / opinion_receiving_<key>   / opinion_decline_<key>   → 自动好感
trust_<key>      / trust_giving_<key>    / trust_receiving_<key>     / trust_decline_<key>     → 自动信任
```

即：**给关系定义加几个键，双方好感/信任/修正就自动生效**——不需要写 effect。这是做"友好条约体系"最省力的入口。

## 六、20 个附属国定义（`subject_types`，17 个数据档）

**20 个定义**（分布在 17 个数据档里；`pronoia` 来自 DLC 档 `D008_pronoia.txt`）：`vassal`、`march`、`fiefdom`、`appanage`、`dominion`、`tributary`、`tusi`、`uc_bey`、`samanta` / `maha_samanta` / `pradhana_maha_samanta`（三个同档 `samanta.txt`）、`pronoia`、`hanseatic_member`、`state_bank`、`trade_company`、`colonial_nation`、`conquistador`、`secessionists`、`imperial_free_city` / `direct_imperial_free_city`（两个同档 `hre.txt`）。

> ⚠️ **别按文件数数这里**：旧档写"17 种"是把 17 个**数据档**当成了类型数——`samanta.txt` 含 3 个、`hre.txt` 含 2 个。

### 6.1 关键字段（readme ~100 字段中最重要的几组）

| 组 | 字段 |
|---|---|
| **`level` 语义** | readme 原文："**level 3 是用于吞并的附属国，level 0 的附属国只是名义上的**"；`type = location / pop / building / army`（后三类是**非主权附属**，见 `non_sovereign_subjects`） |
| 四组门 | `visible` / `enabled`（+ `visible_through_diplomacy` / `enabled_through_diplomacy`）、**`visible_through_treaty` / `enabled_through_treaty`**（和约里能否出现）；`creation_visible`、`subject_creation_enabled`、`release_country_enabled` |
| 建立条件 | `minimum_opinion_for_offer`、`can_attack` / `can_rival` / `can_marry`、`government`（成立时政体）、`on_enable` / `on_disable` / **`on_monthly`**（root = subject） |
| 修正 | `overlord_modifier` / `subject_modifier`、`great_power_score_transfer`（GP 分上缴比例） |
| **吞并** | `can_be_annexed`、`annexation_speed`（月进度）、`annexation_min_opinion`（默认 190，IO 150）、`annexation_stall_opinion`（低于即停滞）、`annexation_min_years_before`（默认 10 年） |
| 贡赋 | `subject_pays = <price>`；`monthly_favor_gain`（附带人情产出） |
| **参战六件套** | `join_{offensive,defensive}_wars_{always,auto_call,can_call}`（自动加入 / 自动收到召唤 / 允许召唤；scope:actor=caller, recipient=callee） |
| 同化条款 | `has_overlords_ruler`（**用宗主的君主，会使附属国脱离共主邦联**）、`has_overlords_religion`、`only_overlord_culture` / **`only_overlord_or_kindred_culture`**（亲缘文化）、`only_overlord_court_language`、**`use_overlord_laws`**（共用法律政策）、`use_overlord_map_color` / `use_overlord_map_name` |
| 宗主权限 | `can_overlord_recruit_regiments` / `build_ships` / `build_roads` / `build_buildings` / `build_rgos`、`overlord_share_exploration`、`overlord_can_enforce_peace_on_subject`、**`allow_declaring_wars`** |
| 保护 | `overlord_protects_external`（默认 yes）/ `overlord_protects_other_subjects`（默认 no）/ `counts_as_external` |
| 解链 | `subject_can_cancel` / `overlord_can_cancel`、`annulled_by_peace_treaty`（默认 yes）、`annullment_favours_required`、`can_be_force_broken_in_peace_treaty`、**`will_join_independence_wars`** |
| **长链** | **`on_overlord_becomes_a_subject = cancel_subjects / transfer_subjects / nothing`**（默认 nothing → 允许"附属国-宗主国"长链，与词条原文一致） |
| 外交占用 | **`diplomatic_capacity_cost_scape = <float>`**（乘在外交容量公式上）、`has_limited_diplomacy`、`can_change_rank`、`can_change_heir_selection` |
| 战争相关 | `war_score_cost`（和约里建立该附属的战争分数）、`base_antagonism`（建立时的敌意上限，0 或负值 = 用引擎默认） |
| AI | `diplo_chance_accept_subject` / `diplo_chance_accept_overlord`（**按标签配权重**，如 `border_distance = -0.1`）、`ai_wants_to_be_overlord` / `ai_wants_to_be_subject` |
| 思潮 | `institution_spread_to_overlord` / `institution_spread_to_subject` |

### 6.2 维持费、忠诚与独立倾向（`NDiplomacy`）

| 机制 | 常量 |
|---|---|
| **附属维持费** | `SUBJECT_UPKEEP_BASE = 0.1` + 经济体量比 `×2.0`（`SUBJECT_UPKEEP_ECONOMY_MIN = 0.1`）；修正项：等级 `×0.2`、**共同方言 −0.1 / 共同语言 −0.05 / 共同宗教 −0.1 / 共同文化组 −0.05** |
| 力量阈值 | `SUBJECTS_STRENGTH_THRESHOLD = 0.25`、`SUBJECTS_STRENGTH_SCALE = −2` |
| **独立倾向** | `LIBERTY_DESIRE_MONTHLY_DECAY = −0.5`、**`LIBERTY_DESIRE_FROM_FORCED_IN_WAR = 50`**；独立运动强度影响忠诚（`LOYALTY_FROM_INDEPENDENCE_MOVEMENT_SCALE = 10`，上限 20） |
| 忠诚来源 | `LOYALTY_FROM_OPINION = 0.15`（对宗主好感）、`LOYALTY_FROM_TRUST = 0.1` |
| 不忠惩罚 | `TOO_DISLOYAL_THRESHOLD = 50`、`DISLOYAL_REVENUE_SCALE = −0.02` |
| 吞并相关 | `MULTIPLE_ANNEX_PENALTY = −0.5`、`SUBJECT_LOYALTY_ANNEXATION_SPEED_FACTOR = 0.2`、`DEFAULT_ANNEX_MIN_RELATION = 190`（IO 150）、`DEFAULT_ANNEX_MIN_YEARS = 10`、`MAX_ANNEX_SIZE = 2` |
| 反哺宗主 | `great_power_score_transfer`；**附属国与独立战争中都不能成为列强**（词条原文） |

## 七、敌意 `antagonism`：EU5 版的"侵略扩张"

`NDiplomacy:2069–2123` 全套：

| 类别 | 常量 |
|---|---|
| 基础（每地点） | `ANTAGONISM_BASE_PER_LOCATION = 0.4`（固定）+ 发展 `×0.04` + `log2(发展) × 0.4` |
| 大国因素 | 发起国 GP 分数 `×0.01`（上限 +0.25）、**防守方 GP 分数同理但为减免（上限 −0.50）**、非主目标国 `×0.15` |
| 情境修正 | 我方是 BBC −0.85、我方是宗主 −0.9、我方是目标 +0.5、震中距离 `×0.75/100 距离` |
| 加成（通用） | 同文化 **+0.4**、同文化组 +0.2、同宗教 **+0.35**、**异教 −0.35**（"我们不在乎那些异教徒"）、同方言 +0.25、同语言 +0.2、同语言族 +0.05、同政府 +0.05、异政府 −0.05、**外交声誉 −0.04**、既有敌意惩罚 +0.005/点 |
| 静态值 | 同文化 −2 / 同文化组 0 / **异文化组 +4**；同宗教 −2 / 同宗教组 0 / **异教 +10**；同政府 −1 / 异政府 +1；**社会价值差异 ×25**；同方言 −2 / 同语言 −1 / 同语言族 0 / **外语族 +2** |
| 对好感的影响 | 基础 `−4`，再乘：邻居 −0.33、超范围 −0.25、依附 −0.9、共主邦联 −0.9、同阵营 −0.8、受防御支持 −0.75、王室联姻 −0.2、互相 IO −0.2 |
| **包围网门槛** | **警告 `40` / 成立 `50` / 退出 `30`** |
| 修正钩子 | `antagonism_received_modifier`、`antagonism_culture_influence` / `_societal_value_` / `_religion_` / `_government_type_` / `_language_influence`、`antagonism_peace_treaty_demands_giving_modifier`、`antagonism_taking_land_giving_modifier`、`antagonism_breaking_truce_giving_modifier`、`antagonism_declared_war_no_cb_giving_modifier`、`antagonism_monthly_change_modifier`、`antagonism_development_impact` |

**包围网完整条件**（词条原文）：对目标敌意 > 50、好感 < −50、**处于外交范围内**，且不与目标处于战争/停战/联合统治、不是其附属国（**朝贡国除外**）；目标攻击任一成员 → 全体防御并可集体宣战；战争期间成员不得退出，和平且敌意 < 30 自动脱离；领袖退出或目标灭亡 → 立即解散。

## 八、势力倾向（`ai_disposition` 国家态度）：AI 的外交大脑

`NDiplomacy:2000–2015`：

| 机制 | 常量 |
|---|---|
| **威胁三轴** | 经济 / 军事 / 人口各归一化，**每轴上限 `THREAT_COMPONENT_CAP = 1.5`**（"没有任何单一比较能单独把国家推入警戒"）；分母下限 `THREAT_COMPONENT_DENOMINATOR_FLOOR_FRACTION = 0.5`（防小国除爆） |
| 距离与规模 | `THREAT_DISTANCE_FACTOR_FOR_RIVAL = 0.002`；**规模差越大距离衰减越狠**（`THREAT_DISTANCE_SIZE_SCALING = 0.1`，上限 ×5）；边界比 `THREAT_BORDER_PERCENTAGE_FACTOR_FOR_RIVAL = 0.1`；降级时 `THREAT_FACTOR_WHEN_REDUCING = 0.25` |
| **状态阈值（带滞回）** | 威胁 **1.0 = 警惕 / 2.0 = 警戒**（滞回 `0.15`）；**机会/竞争门槛 0.5**（滞回 `0.1`）；**友好 = 好感 ≥ 75 或信任 ≥ 75**（滞回 `5`） |
| 提示节制 | `DISPOSITION_MESSAGE_RELEVANCE_RATIO = 2.0`（非邻国且无关系时，经济体量需 ≥ 玩家 2 倍才出现在"态度变化"摘要）；`NOTABLE_DISPOSITIONS_MAX = 5`、`DISPOSITION_BREAKDOWN_MAX_LOCATIONS = 5` |

## 九、其它外交子系统

| 系统 | 要点 |
|---|---|
| **宿敌 rival** | **不双向**（被宣宿敌不必也不自动回宣）；宿敌**不接受同盟等友善条约**；**拥有共同宿敌的更容易签约**；只有 **GP 分数接近且好感低**者才能宣；**上限取决于国家等级**（4/3/2/1）；`REMOVE_RIVAL_COOLDOWN = 60 月`、`RIVAL_OPINION_THRESHOLD = 100`、`RANGE_MULTIPLIER_FOR_RIVALRY = 0.5`、修正 `replace_rival_cost_modifier` / `num_possible_rivals` |
| **共主邦联 union** | 因**王室联姻**共主形成的条约类型；类似共同防御同盟（一方受攻击召唤全体）；**运作方式可通过法律改变**；有 **senior partner / junior partners**；8 个 union 交互；修正 `union_unlock_rein_in_junior_diplomacy` |
| **间谍网与情报迷雾** | 网络值决定你能看到对方多少数据：**`INTEL_THRESHOLD_*` 共 40+ 项，分 5 / 15 / 25 三档**（统治者属性/合法性/修正/阶层力量/地点明细 = 5；收入/税基/人力/水手/稳定 = 15；军队规模/levy/海军/堡垒数/传统/战斗修正/地点驻军 = 25）；`INFILTRATE_ADMINISTRATION_DURATION = 24` 月 |
| **AI 外交目标** | `scripted_diplomatic_objectives`：`ai_tick_frequency`、`actor_trigger`、`recipient_trigger`、**`recipient_priority`**（跨目标比较）、`recipient_list_builder`、`pause_trigger` / `cancel_trigger`、行为开关 `improve_relation(+_limit)` / `defensive_support` / **`antagonise`** / **`destroy`**、`country_interactions {}` / `country_relations {}`（逐项 yes/no）、`time_limit`、`max_allowed` |
| **参战规则** | `join_war_rules`：`join_war_disabled_trigger`（root = 加入国；scope:war / first_leader / second_leader；本地化键用规则名） |
| **宿敌准则** | `rival_criteria`：按 tag 写 `enabled = { <country triggers> }`，限定该国 AI 允许宿敌谁 |
| **侮辱** | `insults\00_insults.txt`：`insult_default` 1–4，每条只有 `trigger`（`insult_default2` 限定基督教组），供侮辱交互抽取文案/条件 |
| **礼物与出售核心** | `SEND_GIFT_COOLDOWN = 120 月`、`GIFT_FAVORS = 5`；出售核心 `SELL_CORE_STABILITY_COST = −20` + `SELL_CORE_REBEL_MODIFIER_YEARS = 10` |
| **对高好感国家宣战加价** | `war_good_relations_cost_modifier`、`war_great_relations_cost_modifier`（对应 `OPINION_GOOD/ GREAT_RELATIONS` 的稳定度代价） |
| 外交解锁开关 | `allow_diplomacy_force_embargo`、`allow_diplomacy_violate_sovereignty`、`allow_diplomacy_force_divert_trade`、`allow_diplomacy_force_change_court_language`、`allow_diplomacy_influence_nation`、`unlock_improve_relations_member`、`disallow_diplomatic_subjugation` |

## 十、外交修正速查（mod 可改点）

| 想改什么 | 修正键 |
|---|---|
| 外交范围 | `diplomatic_range`（绝对值）/ `diplomatic_range_modifier`（%） |
| 外交容量 | `diplomatic_capacity`（+N 槽）/ `diplomatic_capacity_modifier`（%）/ `diplomatic_upkeep_efficiency` |
| 外交官 | `max_diplomats`、`monthly_diplomats`、`send_diplomat_cost_modifier` |
| 声誉与关系 | `diplomatic_reputation`、`improve_relation_impact`、`subject_opinions`、`pop_countries_opinions`、`ai_opinion_bias`、`diplomacy_importance_modifier` |
| 信任 | `trust_recovery`、`trust_decay` |
| 敌意 | §七 的 13 个 `antagonism_*` 键 |
| 间谍 | `spy_network_construction`、`spy_network_*`（define 侧） |
| 宿敌 | `num_possible_rivals`、`replace_rival_cost_modifier` |
| 附庸 | `diplomatic_annexation_efficiency`、`hostile_diplomatic_annexation_efficiency`、`disallow_diplomatic_subjugation` |
| 具体交互的开关 | `allow_diplomacy_*` 五连 |

## 十一、与其它篇的耦合

| 接到 | 具体 |
|---|---|
| 贸易（贸易篇） | 条约关系能开关：贸易准入、禁运、通行费豁免、孤立豁免、**市场保护主义取消**（`lifts_trade_protection`）、双向市场吸引力（`trade_to_first/second`） |
| 文化宗教（文化与宗教篇） | `culture_view` / `religion_view` 与 `same_culture/court_language/common_language` **直接进 AI 接受度**；敌意的来源项就是文化·宗教·语言·政府·社会价值；王室联姻与共主邦联 |
| 战争 | 宣战理由（宿敌/间谍网造 CB）、停战（`TRUCE_YEARS = 5` / `SCALED_TRUCE_YEARS = 10`）、召唤参战（条约的 `called_in_*` + 附属六件套）、战争热情（盟友倍率）；**战争侧完整机制（CB 三段门与生成进度、宣战代价 8 条价格、战争分数/参与度/热情、和约 64 个条款定义（53 数据档）与 46 条定价、WAR_WORTH、土地承诺与分赃、无条件投降）见 `vanilla\vanilla-combat.md` §七–§十** |
| IO / 灾难局势 | IO 成员占外交容量；HRE 全套交互（选举/赐爵/自由市/帝国圈/帝国军）；教廷（绝罚/停圣事/贿选）；`belongs_to_international_organization` 进接受度 |
| 科技时代 | 革新给外交容量/范围；条约可传思潮（`institution_spread_to_*`） |
| POP / 阶层 | 出售核心掉稳定 + 10 年叛乱增长；`estates_like` / `estates_dislike` 进 AI 接受度 |
| 情报 | 间谍网 = 情报迷雾阈值（40+ 项 `INTEL_THRESHOLD_*`）的唯一开关 |

## 十二、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 新增外交交互 | `common\country_interactions\<新文件>.txt` + 本地化 | `type` 只能填 diplomacy/subject/union；`accept` 与 `diplo_chance` 二选一或并用；AI 用 `ai_prerequisite` 省性能 |
| 给交互定价 | `common\diplomatic_costs\`（spy_network / favors）+ 交互里 `diplomatic_cost` | 该目录**无 readme**，照 `00_hardcoded.txt` / `01_from_script.txt` 的样例写 |
| 新增条约关系 | `common\scripted_relations\<新文件>.txt` | 记得配 `uses_diplo_capacity` + `diplomatic_capacity_cost`，否则不占容量；用 `opinion_*` / `trust_*` 自动挂观感最省力 |
| 新增附属国类型 | `common\subject_types\<新文件>.txt` | 重点关注 `level` / `type` / 吞并四件套 / 参战六件套 / `on_overlord_becomes_a_subject` |
| 改容量与范围 | 修正 `diplomatic_capacity(_modifier)` / `diplomatic_range(_modifier)` + 等级 `rank_modifier` | 等级表在 `country_ranks\00_default.txt` |
| 改敌意平衡 | `NDiplomacy` 的 `ANTAGONISM_*`（40 余条） | 全局生效；单国差异用 `antagonism_*_influence` 修正 |
| 改 AI 态度阈值 | `NDiplomacy` 的 `DISPOSITION_*` / `THREAT_*` | 带滞回，改阈值时留意滞回值 |
| 改 AI 外交行为 | `scripted_diplomatic_objectives\` + `rival_criteria\` + `join_war_rules\` | 影响 AI 而非玩家 |
| 改侮辱文案 | `insults\00_insults.txt` + 本地化 | — |

**硬编码边界**：好感/信任/敌意/人情/间谍网的**数值结算与衰减**（只能改常量）、AI 接受度的最终合成公式、外交官寻路（`TRAVEL_DAYS` 之外的路径计算）、情报迷雾的阈值判定逻辑（阈值可调，判定在引擎）、`country_ranks` 的等级数量与升级条件结构。

## 十三、中文检索键

**概念**（`game_concepts_l_simp_chinese.yml`）：`game_concept_diplomacy` 外交（:67）、`game_concept_diplomat` **外交官**（:957）、`game_concept_diplomatic_capacity` **外交容量**（:1191）、`game_concept_diplomatic_reputation` **外交声誉**（:1195）、`game_concept_opinion` **好感**（:959）、`game_concept_trust` **信任度**（:1154）、`game_concept_antagonism` **敌意**（:942）、`game_concept_favor(s)` **人情**（:1150/1152）、`game_concept_spy_network` **间谍网**（:1144）、`game_concept_rival(s)` **宿敌**（:966）、`game_concept_royal_marriage` 王室联姻（:980）、`game_concept_union` / `personal_union` **共主邦联**（:987/993）、`game_concept_alliance` 同盟（:1003）、`game_concept_call_to_arms` 召唤参战（:1015）、`game_concept_guarantee` 保障独立（:1026）、`game_concept_military_access` 军事通行权（:1201）、`game_concept_fleet_basing_rights` 驻港权（:1203）、`game_concept_truce` 停战（:1421）、`game_concept_casus_belli` 宣战理由（:1080）、`game_concept_subject` 附属国（:1504）、`game_concept_overlord` 宗主国（:1512）、`game_concept_liberty_desire` **独立倾向**（:1516）、`game_concept_improve_relations` 改善关系（:1935）、`game_concept_isolation` 孤立（:2022）、`game_concept_coalition` **包围网**（:2043）、`game_concept_great_power` 列强（:1163）、`game_concept_hegemony` 霸权（:1171）、`game_concept_ai_disposition` **国家态度**（:2105）、`game_concept_prestige` 威望（:796）。

**界面**（`in_game\gui\`）：`diplomacy_lateralview.gui`（127KB）、**`foreign_country_lateralview.gui`（145KB，单国全景）**、`diplomacy_macrobuilder_lateralview.gui`（67KB）、`manage_subjects_lateralview.gui`、`create_subjects_lateralview.gui`、`select_country_diplomacy_lateralview.gui`、`select_subject_type_lateralview.gui`、`shared\diplomacy_tooltips.gui`（29KB）、`ai_diplomatic_objectives_viewer.gui`（调试用，看 AI 目标）、`panels\organization\foreign_league_*.gui`。

**关联字段档**：`fields\common-country_interactions.md`、`fields\common-scripted_relations.md`、`fields\common-subject_types.md`、`fields\common-join_war_rules.md`、`fields\common-rival_criteria.md`、`fields\common-scripted_diplomatic_objectives.md`、`fields\common-country_ranks.md`（`diplomatic_costs` 与 `insults` 无 readme、亦无字段档，以本篇 §4.3 / §九 为准）。
