# 原版解析：AI（vanilla AI）

> **一句话**：把 AI 拆成性格、权重、节拍、常量与脚本钩子五层，含 `NAI` 段 746 个常量、16 张外交接受度表与难度攻击性，并标注可改点。
> **什么时候看**：改 AI 性格与接受度权重、调难度或攻击性、排查 AI 为何不打不签不建，或接 AI 脚本钩子时翻这篇。
> **体量**：411 行 · 约 19 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、总览：AI 是"六层"结构](#一总览ai-是六层结构)
- [二、性格层（`common\ai_personalities\`，8 种）](#二性格层commonai_personalities8-种)
- [三、权重层：AI 想做某件事的评分](#三权重层ai-想做某件事的评分)
  - [3.1 `ai_will_do`（**660 处**）——AI 评分的主力](#31-ai_will_do660-处ai-评分的主力)
  - [3.2 其余权重键](#32-其余权重键)
  - [3.3 `ai_diplochance\00_ai_diplochance.txt`——**16 张外交接受度表**](#33-ai_diplochance00_ai_diplochancetxt16-张外交接受度表)
- [四、节拍层：AI 多久想一次](#四节拍层ai-多久想一次)
- [五、常量层：`NAI` 段 746 个常量（515–1444 行）](#五常量层nai-段-746-个常量5151444-行)
  - [5.1 战争平衡与胜率（决定"敢不敢打"）](#51-战争平衡与胜率决定敢不敢打)
  - [5.2 扩张与征服欲望（`CONQUER_DESIRE_*` 27 个）](#52-扩张与征服欲望conquer_desire_-27-个)
  - [5.3 战争与和平 AI](#53-战争与和平-ai)
  - [5.4 军事 AI（`HUNT_ARMIES_` / `CARPET_SIEGE_` / 撤退）](#54-军事-aihunt_armies_--carpet_siege_--撤退)
  - [5.5 外交、威胁与结盟](#55-外交威胁与结盟)
  - [5.6 经济、建造与殖民](#56-经济建造与殖民)
  - [5.7 储蓄模式（破产前 AI 的"过冬"状态）](#57-储蓄模式破产前-ai-的过冬状态)
- [六、脚本钩子层（原版用量极少，但都是 mod 的正规入口）](#六脚本钩子层原版用量极少但都是-mod-的正规入口)
- [七、难度与攻击性（`main_menu\common\static_modifiers\difficulty.txt`）](#七难度与攻击性main_menucommonstatic_modifiersdifficultytxt)
- [八、AI 的认知边界（"看得见"与"记得住"）](#八ai-的认知边界看得见与记得住)
- [九、Mod 改造建议（可改 vs 硬编码）](#九mod-改造建议可改-vs-硬编码)
- [十、中文检索键](#十中文检索键)

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| **`NAI` 常量段**（`loading_screen\common\defines\00_defines.txt`） | **515–1444 行 / 930 行内 746 个常量** | 本体逐条英文注释 |
| `in_game\common\ai_personalities\` | **8 种国家性格** | 文件头注释（**无 readme**） |
| `main_menu\common\modifier_type_definitions\00_modifier_types.txt` | **2393 个修正类型中 48 个带 `ai=yes`**（目录合计 **2,436**：主档 2,393 + `01_byz` 29 + `02_generic_bureaucracies` 14）；性格实际用到 **35 个** | 字段权威见 `fields\main_menu-modifier_type_definitions.md` |
| `in_game\common\ai_diplochance\` | **16 张外交接受度权重表** | **无有效 readme**：同目录的 `ai_diplochance.info` 其实是 `customizable_localization` 的格式文档（放错位置） |
| `in_game\common\ai_scripted_expansion_score\` | **2 条**（都在 `guelphs_and_ghibellines.txt`） | `readme.txt` |
| `in_game\common\ai_scripted_expansion_target\` | **2 条**（同上文件） | `readme.txt` |
| `in_game\common\scripted_diplomatic_objectives\` | **5 条**（`hre_emperorship.txt` 2 / `japanese_clan_spying.txt` 3） | `readme.txt` |
| `in_game\common\rival_criteria\` | **6 条**（`europe.txt`） | `readme.txt` |
| `in_game\common\join_war_rules\` | **1 条**（`teu_crusade.txt`） | `readme.txt` |
| `in_game\common\generic_action_ai_lists\` | **93 条 / 85 文件** | `readme.txt` |
| `in_game\common\traits`、`advances`、`goods`、各类 `*_interactions` | 权重键撒在各处（见 §三） | 各自 readme |
| `main_menu\common\static_modifiers\difficulty.txt` | 5 个玩家难度 + **5 个 AI 难度** + **3 档攻击性** | 文件头注 `# Hardcoded` |
| `main_menu\common\game_rules\00_game_rules.txt` | `ai_personalities` 规则（4 个选项，默认 `ai_personalities_historical`） | `_game_rules.info` |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `ai_personality` | **国家性格** | 8 种；驱动外交与军事行动的基本特性（有独立地图模式） |
| `ai_disposition` | **国家态度** | 概念层另有此条：该国**目前对你的看法**（受相对实力、对你的土地有无宣称、是否正式宿敌影响，会不断重估） |
| `aggressiveness_modifier` | 侵略性 | AI 修正；正数更爱开战 |
| `carefulness_modifier` | 谨慎度 | 正数更谨慎 |
| `win_war_chance_threshold` | **开战胜率门槛** | 需要多高胜算才打 |
| `ai_months_between_wars` | 两次战争间隔（月） | 负值 = 更频繁 |
| `ai_will_do` | **AI 意愿值** | 评分块（脚本值 DSL），也是可读的 trigger |
| `ai_tick` / `ai_tick_frequency` | AI 决策节拍 / 间隔 | `daily`/`monthly`/`never` × 每 N 拍一次 |
| `ai_weight` / `ai_desire` | AI 权重 / AI 欲望 | 科技、和平条款等专用评分 |
| `ai_diplochance` | 外交接受度权重 | 16 张表（联姻、贷款、求和…） |
| `subjugation_preference_modifier` | 臣服偏好 | 倾向附庸而非吞并 |
| `dynastic_acquisition_preference_modifier` | 王朝获取偏好 | 联姻/联统倾向 |
| `war balance` | 战争平衡 | AI 对双方实力的估值（WAR_BALANCE_* 权重） |
| `carpet siege` | 铺开围城 | 多路小队同时围城 |
| `hunt armies` | 猎杀敌军 | 追击敌方主力的目标系统 |
| `saving mode` | 储蓄模式 | 高负债/低政府权力时的紧缩状态 |
| `ai_spam_protection` | AI 刷屏保护 | on_action 冷却类型 |

## 一、总览：AI 是"六层"结构

读 AI 相关文件前先定位层级，改错层是白费功夫：

| 层 | 载体 | 原版规模 | 改它等于改什么 |
|---|---|---|---|
| ① **性格层** | `common\ai_personalities\` + 48 个 `ai=yes` 修正类型 | 8 种性格 / 35 个修正 | 一个国家的"人格底色" |
| ② **权重层** | 各类目里的 `ai_will_do` / `ai_weight` / `ai_desire` / `ai_prerequisite` + `ai_diplochance\` | **660 / 163 / 63 / 46** 处 + 16 张表 | 具体某个行动 AI 想不想做 |
| ③ **节拍层** | `ai_tick` + `ai_tick_frequency` + `AI_PERFORMANCE_*` | 424 + 376 处 + 30 余个常量 | AI 多久想一次（性能与反应速度） |
| ④ **常量层** | `defines` 的 `NAI` 段 | **746 个常量** | AI 的"世界知识"与阈值 |
| ⑤ **脚本钩子层** | `ai_scripted_expansion_*`、`scripted_diplomatic_objectives`、`rival_criteria`、`join_war_rules`、`generic_action_ai_lists` | 2 / 2 / 5 / 6 / 1 / **93** | 给 AI 加"剧本级"偏好 |
| ⑥ **难度层** | `main_menu\common\static_modifiers\difficulty.txt` | 5 + 5 + 3 档 | 全局强弱与攻击性 |

> ⚠️ **原版的 AI 主要活在 ④（746 个 defines）和 ②（上千个权重块）里**，⑤ 层几乎没怎么用（扩展目标 2 条、外交目标 5 条、宿敌标准 6 条、参战规则 1 条）。想"改 AI"先问自己改的是哪一层。

## 二、性格层（`common\ai_personalities\`，8 种）

文件头注释给出三层语义，这是理解所有 AI 修正的钥匙：

- **Willingness 意愿**：多 eager 地找仗打（`aggressiveness_modifier`、`carefulness_modifier`、`win_war_chance_threshold`、`ai_months_between_wars`、`ai_require_cb_for_war`）
- **Tolerance 忍耐**：为了目标肯吃多少痛（`war_declaration_stab_hit_tolerance`、`war_declaration_war_exhaustion_tolerance`、`coalition_strength_tolerance`、`expected_warscore_modifier`、`peace_offer_fairness`）
- **Priorities 优先**：看重什么资源与结果（各类 `*_importance_modifier`、`bias_for_*_policies`、`ai_opinion_bias`、`dynastic_acquisition_preference_modifier`、`subjugation_preference_modifier`）

| id | 中文 | 关键数值（节选） |
|---|---|---|
| `ai_balanced` | **平衡**（`default = yes`） | 只放锚点：`carefulness +0.05`、`win_war_chance_threshold +0.05`、`ai_months_between_wars = 6`（其余交给特质与社会价值） |
| `ai_aggressive` | **侵略** | `aggressiveness +0.3`、`carefulness −0.2`、**间隔 −12 月**、`coalition_strength_tolerance +0.25`、`ai_force_annexation +0.5`、`dynastic_acquisition −0.7` |
| `ai_expansionist` | **扩张** | `aggressiveness +0.2`、间隔 −6、`subjugation_preference +0.15`、`bias_for_colonialist_policies +0.15` |
| `ai_defensive` | **防御** | `aggressiveness −0.2`、`carefulness +0.25`、`defence_importance +0.25`、`ai_stability_target +0.1` |
| `ai_cautious` | **谨慎** | `carefulness +0.3`、`win_war_chance_threshold 0.15`、间隔 +18、**`ai_require_cb_for_war = yes`**、`war_declaration_stab_hit_tolerance −2` |
| `ai_opportunistic` | **投机** | `gold_importance +0.2`、`trade_importance +0.15`、`peace_offer_negotiation_power +0.15`、`ai_opinion_bias −0.15` |
| `ai_isolationist` | **孤立** | `aggressiveness −0.25`、**间隔 +24**、`diplomacy_importance −0.2`、`bias_for_isolationist_policies +0.3`、`dynastic_acquisition +0.5` |
| `ai_friendly` | **友善** | `peace_offer_fairness +0.25`、`diplomacy_importance +0.25`、`ai_force_annexation −0.5`、**`dynastic_acquisition_preference_modifier = 1.0`** |

**分配规则**（`main_menu\common\game_rules\00_game_rules.txt:467`）：`ai_personalities` 规则，默认 `ai_personalities_historical`；可选 `ai_personalities_random`、`..._random_per_age`、`..._random_per_ruler`（后者 = 每换一任统治者重掷性格）。除 `ai_balanced` 外都带 `flag = flavour_rule`。

概念词条原文佐证（`game_concepts_l_simp_chinese.yml:2097`）：「国家性格反映了该国的基本特性和治理方略，无论它倾向于征服、谨慎、外交还是孤立。**性格在游戏开局时设置，只能通过事件或游戏规则进行更改。**」——即性格**不是**每局随机，mod 想改只能走游戏规则或事件效果。

**性格是修正，不是硬编码**：8 种性格合计使用 **35 个修正类型**（全部定义在 `modifier_type_definitions\00_modifier_types.txt`），且与**特质、社会价值观的 AI 修正叠加**（文件头原文："These stack additively with trait and societal value modifiers"）。所以同一个性格在不同政体/价值下表现不同。

**AI 专用修正类型共 48 个**（`game_data` 里标 `ai=yes`，主档 2,393 个修正中占比极小；目录合计 2,436）：

```
win_war_chance_threshold / win_war_chance_lower_limit / unintegrated_land_expansion_penalty_modifier
war_declaration_stab_hit_tolerance / war_declaration_war_exhaustion_tolerance / coalition_strength_tolerance
peace_offer_fairness / peace_offer_negotiation_power / expected_warscore_modifier
aggressiveness_modifier / carefulness_modifier / ai_months_between_wars / ai_force_annexation_modifier
subjugation_preference_modifier / dynastic_acquisition_preference_modifier / reject_subjugation_reasons
ai_conquer_desire_religion_mult / ai_amount_of_parallel_charters / mercenary_units_preference_modifier
ai_stability_target_modifier / ai_government_power_target_modifier / language_change_threshold_modifier
gold_importance_modifier / stability_importance_modifier / manpower_importance_modifier / control_importance_modifier
diplomacy_importance_modifier / religious_unity_importance_modifier / defence_importance_modifier
institution_importance_modifier / trade_importance_modifier / societal_value_importance_modifier
bias_for_{administrative|balanced|colonialist|diplomat|militarist|capitalist|tolerant|spiritualist|scholar|isolationist|patron_of_arts}_policies
court_language_is_{liturgical|common|market}_language_importance_modifier
revoke_privileges_importance_modifier / revoke_privileges_stability_tolerance
```

另有 2 个**行为型修正没打 `ai=yes` 标签**但只对 AI 有意义：`ai_require_cb_for_war`（boolean，`00_modifier_types.txt:18`）与 `ai_opinion_bias`（:24）——查"是不是 AI 修正"不能只看 `ai=yes`，还要看命名族（`ai_*` 共 8 个）。

## 三、权重层：AI 想做某件事的评分

### 3.1 `ai_will_do`（**660 处**）——AI 评分的主力

它是**脚本值 DSL**，不是单个数：支持 `add = { desc = "..." value = ... }` 与 `if = { limit = { ... } add = ... }`，`desc` 会作为界面上的"为什么"（原版 `generic_actions` 里成篇的 `VOTE_BASE_VALUE` 之类）。

按类目分布（`in_game\common` 内 `ai_will_do =` 计数）：

| 类目 | 处数 | 类目 | 处数 |
|---|---|---|---|
| `generic_actions` | **418** | `resolutions` | 21 |
| `country_interactions` | **105** | `unit_abilities` | 13 |
| `character_interactions` | 33 | `parliament_agendas` | 11 |
| `casus_belli` | 27 | `cabinet_actions` | 10 |

### 3.2 其余权重键

| 键 | 处数 | 主要归属 |
|---|---|---|
| `ai_weight` | 163 | **`advances` 149**（AI 选科技）、`unit_categories` 10、`estate_privileges` 4 |
| `ai_desire` | 63 | **`peace_treaties` 63**（和约条款欲望） |
| `ai_prerequisite` | 46 | 国家交互（AI 预筛，见外交篇） |
| `ai_desire_to_join` / `ai_desire_to_allow_new_member` | 各 35 | `international_organizations` |
| `ai_preference_tags` | 29 | `advances`（AI 科技偏好标签） |
| `ai_rgo_size_importance` | 15 | `goods`（哪些商品值得扩 RGO） |
| `ai_months_between_wars` | 14 | `ai_personalities` 7 + `international_organizations` 7 |
| `ai_issue_voting_bias` | 4 | `international_organizations` |
| `ai_rule` / `ai_level` | 5 / 4 | `rival_criteria` / `country_ranks` |
| `ai_force_annexation_modifier`、`ai_opinion_bias`、`ai_stability_target_modifier`、`ai_advance_preference_tags`、`ai_require_cb_for_war`、`ai_conquer_desire_religion_mult`、`ai_religious_conversion_rule`、`ai_succession_disaster_targets` … | 各 1–5 | 政府改革、灾难、法律等（从 `main_menu\common` 与 `in_game\common` 两处读） |

### 3.3 `ai_diplochance\00_ai_diplochance.txt`——**16 张外交接受度表**

每张表 = 一个交互类型，表内每个键是一个**已命名的评估项**，值是该评估项的权重（正=更容易接受）。原版 16 张：

```
royal_marriage                # 联姻：不同宗教 −100 / 不同宗教组 −200 / 年龄(女) −10 / 联姻期望 +1…
demand_become_subject_action  # 要求臣服：base −50、负债 +1.0、负稳定 +0.2、厌战度 +0.5
offermilaccess / buy_milaccess
offerloan                     # 一堆 −1000 硬否决：贷款太小 / 到期太早 / 太晚 / 利率太高 / 已有太多贷款
sell_location / buy_location  # 注意两表符号相反（卖看 location_value +100，买看 capital −99999）
invite_to_international_organization / ask_to_join_international_organization / create_international_organization_targetting
enforce_peace / threaten_war / requestpeace / ransom_subunits
callaction_defensive / callaction_offensive   # 响应召唤：enforced_demand 10000（强制必应）、背叛盟友 −1000
```

值得注意的原版细节：`yesman = 10000`（用于强制必应）、`requestpeace` 里的 `victory = 1000`（打不下去了）、`callaction_offensive` 的 `promised_land = 20` 与 `betrayed_ally = -1000`；说明**这些表既能表达软倾向，也能做硬否决**。

## 四、节拍层：AI 多久想一次

`ai_tick`（节拍）+ `ai_tick_frequency`（每 N 拍一次）成对出现：

| `ai_tick` 值 | 处数 | 含义 |
|---|---|---|
| `monthly` | **238** | 按月 |
| `daily` | **131** | 按日 |
| **`never`** | **55** | **AI 永不评估**（纯玩家/脚本用） |

出现位置：`generic_actions` 348、`country_interactions` 37、`character_interactions` 34、`resolutions` 5。
`ai_tick_frequency` 常见取值：**6（80 处）/ 12（71）/ 80 / 120 / 40 / 3 / 180 / 1**……

**`AI_PERFORMANCE_*`：把"AI 想得多慢"写成常量**（这一族是 EU5 AI 的关键设计，改大会明显影响帧率与 AI 反应）：

| 常量 | 值 | 含义 |
|---|---|---|
| `AI_PERFORMANCE_CABINET_ACTION_MONTHS_BETWEEN_UPDATES` | **36** | 内阁行动换人/换事每 36 个月才重估一次 |
| `AI_PERFORMANCE_REFORMS_MONTHS_BETWEEN_UPDATES` | 24 | 政府改革 |
| `AI_PERFORMANCE_BUREAUCRACY_MONTHS_BETWEEN_UPDATES` | 24 | 官僚制增删 |
| `AI_PERFORMANCE_ESTATE_PRIVILEGES_MONTHS_BETWEEN_UPDATES` | 12 | 阶层特权 |
| `AI_PERFORMANCE_POLICIES_MONTHS_BETWEEN_UPDATES` | 6 | 法律政策 |
| `AI_PERFORMANCE_SCHOLARS_MONTHS_BETWEEN_UPDATES` | 6 | 宗教学者 |
| `AI_PERFORMANCE_CABINET_ACTION_{LOCATION|PROVINCE|AREA}_SAMPLE_SIZE` | 20 / 5 / **1** | 内阁行动取样上限（省与地区只抽 5 个 / 1 个） |
| `AI_PERFORMANCE_EVALUATE_MARKET_RATIO_PER_BUILDING` | 0.042 | 每类建筑每月只抽查市场 1/24 的地点，**至少 20 个**，下月换另一批 |
| `AI_PERFORMANCE_EVALUATE_CITY_UPGRADES_RATIO` | 0.017 | 升城市每 5 年才轮一遍（至少 20 个） |
| `AI_ALLOWED_PARALLEL_TOTAL_CONSTRUCTION_RATIO` | 0.05 | 每月最多在 5% 的地点开新工地 |
| `AI_ALLOWED_PARALLEL_SAME_TYPE_CONSTRUCTION_RATIO` / `_MINIMUM` | 0.02 / 3 | 同类建筑并行上限 |
| `AI_MAX_NEW_BUILDINGS_PER_LOCATION` | 3 | 同地点在途建筑上限 |
| `AI_DEFAULT_COUNTRY_INTERACTION_FREQUENCY` / `AI_DEFAULT_CHARACTER_INTERACTION_FREQUENCY` | 30 / 180 天 | 交互检查默认间隔 |
| `AI_PEACE_CHECK_DAYS` / `AI_LOAN_CHECK_DAYS` / `AI_REDEEM_BONDS_UPDATE_MONTHS` | 90 / 3 / 3 | 求和、贷款、赎债节奏 |
| `AI_PERFORMANCE_PATHFIND_DISTANCE_THRESHOLD` / `AI_NAVAL_...` | 100 / **2500** | 超过此距离改用粗略寻路（海军阈值更高，否则会逆风横渡大西洋） |

> 读懂这一族的用处：**"AI 明明能做却不做"多数是节拍与采样问题，不是权重问题**（比如内阁行动 36 个月才重估一次、地区只抽 1 个样本）。

## 五、常量层：`NAI` 段 746 个常量（515–1444 行）

930 行里 **746 个常量**、154 个空行、仅 27 行纯注释——这是全游戏最"自解释"的一段 defines。前缀分布：`AI_` **393**、`PEACE_` **59**、`CONQUER_` 27、`HUNT_` 16、`WAR_` 11、`COLONY_` 9……（唯一的嵌套块是 `CARPET_SIEGE_SEARCH_DEPTHS = { 10 8 6 5 4 3 2 2 2 2 1 1 1 1 1 1 1 1 }`，18 个值对应"第 N 大部队能看多远"。）

### 5.1 战争平衡与胜率（决定"敢不敢打"）

| 常量 | 值 | 含义 |
|---|---|---|
| `WAR_BALANCE_REGULARS_IMPORTANCE` | **3** | 常备军权重最高 |
| `WAR_BALANCE_LEVIES_IMPORTANCE` / `MANPOWER_IMPORTANCE` | 1 / 0.5 | |
| `WAR_BALANCE_LEVY_BOATS_IMPORTANCE` | **10** | 征召船被估得极重 |
| `WAR_BALANCE_TAXBASE_IMPORTANCE` | 0.05 | 税基只是零头 |
| `WAR_WIN_CHANCE_SENSITIVITY` / `BATTLE_WIN_CHANCE_SENSITIVITY` | 8 / 16 | 胜率曲线陡度 |
| `BATTLE_WIN_CHANCE_ENEMY_BIAS` | 1.1 | 系统性高估敌人 10% |
| `BATTLE_WIN_CHANCE_GENERAL_MIL_FACTOR` | 0.25 | **100 军事 ≈ 多 25% 兵力** |
| `INITIATIVE_COMBAT_STRENGTH_FACTOR` / `AI_FLANKING_COMBAT_STRENGTH_FACTOR` | 0.025 / 0.3 | |
| `HEAVILY_OUTNUMBERED_RATIO` | 0.40 | 实力比低于此进入"被压制"行为 |
| `OUTNUMBERED_BATTLE_ADVANTAGE` | 0.60 | 被压制时只追打实力 ≤ 60% 的敌队 |

### 5.2 扩张与征服欲望（`CONQUER_DESIRE_*` 27 个）

| 常量 | 值 | 含义 |
|---|---|---|
| `DEFAULT_CHANCE_OF_EXPANSION` | **0.25** | 每月启动一次战争计划的概率 |
| `CHANCE_OF_EXPANSION_AGGRESSIVENESS_SCALE` | 10 | 侵略性修正的缩放 |
| `AI_NUM_EXPANSION_TARGETS` | 20 | 每月评估的候选目标数上限 |
| `EXPANSION_TARGET_SCORE_NEEDED_TO_KEEP` / `_TO_PICK` | 15 / 20 | 保留 / 采纳阈值 |
| `AI_UNINTEGRATED_LAND_EXPANSION_PENALTY` | 2 | 未整合土地每多一个百分点 → −2% 开战概率（≥5 个地点才生效） |
| `SAFE_AMOUNT_OF_ANTAGONISM` / `SAFE_AMOUNT_OF_WAR_EXHAUSTION` | 30 / 5 | 自设上限（**不是**求和目标） |
| `CONQUER_DESIRE_BASE_SCORE` | 20 | 每个候选地点底分 |
| `CONQUER_DESIRE_POPULATION_FACTOR` | 0.025 | 人口（千为单位）：20k=0.5 / 100k=2.5 / **1M=25** |
| `CONQUER_DESIRE_TAX_BASE_FACTOR` | 0.2 | 税基：20g=4 / 100g=20 / 350g=70 |
| `CONQUER_DESIRE_CORE_BONUS` / `CB_BONUS` / `EXCLAVED_BONUS` | 100 / 20 / 50 | 核心 / 有 CB / 被包围的飞地 |
| `CONQUER_DESIRE_AREA_COMPLETE_BONUS`（`_RAMP_START` 0.6） | 40 | 地区凑整（已占 60% 起线性给分） |
| `CONQUER_DESIRE_REGION_BONUS`（`_RAMP_START` 0.5） | 30 | 大区凑整 |
| `CONQUER_DESIRE_HERETIC_BONUS` / `HEATHEN_BONUS` | 10 / 20 | 异端 / 异教 |
| `CONQUER_DESIRE_DISTANCE_TAIL_DECAY` | 0.5 | 超出可达距离后每步分数减半 |
| `CONQUER_DESIRE_DYNASTIC_{UNION|MARRIAGE}_MULT` | 0.4 / 0.75 | 有联统 / 有联姻时"够得着"的距离倍率 |
| `AI_DYNASTIC_RESTRAINT_{HEIRLESS|SHARED_HEIR}_MULTIPLIER` | **0.05 / 0.25** | 对方无继承人 / 双方继承人同宗族时**几乎不动手**（等联统） |
| `AI_USE_CONQUER_CB_FACTOR` | 25 | 有便宜 CB 时的开战分倍率 |
| `MONTHS_TO_WAIT_FOR_CASUS_BELLI` | 24 | 等脚本给 CB 的耐心 |
| `AI_CONQUER_DESIRE_MAX` | 100 | 征服欲望上限 |

### 5.3 战争与和平 AI

| 常量 | 值 | 含义 |
|---|---|---|
| `PEACE_OFFER_WAR_ENTHUSIASM_THRESHOLD` / `WAR_SCORE_THRESHOLD` / `ACCEPTANCE_THRESHOLD` | 0.6 / 50 / −20 | AI 主动求和的三道门 |
| `EARLY_PEACE_TIME_MONTHS` / `_BASE` / `_FACTOR` | 4 / 10 / 1.5 | 开战 4 个月内的"早和惩罚" |
| `MINIMUM_WIN_CHANCE_TO_CONSIDER_DEFEAT` / `MINIMUM_WAR_SCORE_TO_CONSIDER_DEFEAT` | 0.25 / −50 | 认输线 |
| `PEACE_KNAPSACK_EXTRAS` / `_MAX` | 0 / 3 | 和约条款背包算法尝试次数 |
| `IDEAL_WARSCORE_DIFFERENCE_THRESHOLD_TO_CONSIDER_PEACE` | 20 | 距理想和约 20 分内就谈 |
| `AI_CALL_FOR_PEACE_CHANCE_MULTIPLIER` | 0.25 | 呼吁和平的频率倍率 |
| `PEACE_MAKE_SUBJECT_*` | 0.8 起，含 `TOO_MUCH_TO_INTEGRATE 1.5`、`OVER_LIMIT 0.05` | 附庸化条款的战争价值倍率 |
| `PEACE_RELEASE_COUNTRY_*` | `RIVAL_FACTOR 1`、`GORE_FACTOR −3` | 释放国家条款（切对手、按碎地扣分） |
| `PEACE_OFFER_ALLY_DEFAULT_FAIRNESS` / `PEACE_DEAL_PROMISED_LAND_GUARANTEED_PARTICIPATION` | 0.40 / 0.75 | 盟友分赃 |
| `MONTHS_TO_WAIT_BEFORE_DECLARING_IO_WARS` / `POWERBALANCE_NEEDED_FOR_IO_WAR` | 4 / 1.1 | 国际组织（包围网）宣战 |

### 5.4 军事 AI（`HUNT_ARMIES_` / `CARPET_SIEGE_` / 撤退）

| 常量 | 值 | 含义 |
|---|---|---|
| `HUNT_ARMIES_MAX_RADIUS` / `AI_MAX_DISTANCE` | 20 / 3 | 猎杀目标的作用半径 |
| `HUNT_ARMIES_CONFRONT_FACTOR` | 2.0 | 逼战倾向（越大概率越高） |
| `HUNT_ARMIES_WIN_CHANCE_SPLIT` / `MAX_SPLINTERS` | 0.99 / 6 | 胜率高于 99% 就分兵，最多 6 路 |
| `HUNT_ARMIES_DESIRED_STRENGTH_MULT` | 1.5 | 想要 1.5 倍于敌的兵力 |
| `HUNT_ARMIES_AI_MAX_NUM_OBJECTIVES` | 5 | 同时猎杀目标上限（软上限） |
| `CARPET_SIEGE_REQUIRED_WIN_CHANCE` / `CONFRONT_FACTOR` | 0.8 / 0.5 | 铺开围城的门槛（比猎杀保守） |
| `CARPET_SIEGE_MAX_ARMIES` | 10 | 最多拆成 10 路 |
| `AI_SUPPLY_LIMIT_TARGET` / `RATIO_OVER_SUPPLY_TARGET_TO_SPLIT` | 0.95 / 1.3 | 维持补给上限 95% 的堆叠 |
| `AI_RECOVER_MORALE_THRESHOLD` | 66 | 士气低于 66% 就停下来恢复 |
| `AI_RETREAT_DICE_MARGIN`（围城姿态 2） | 3 | 骰子劣势 3 点且士气低 → 撤退 |
| `AI_RETREAT_DICE_MORALE_THRESHOLD` / `_FLANK_` | 0.45 / 0.40 | 士气门槛 |
| `AI_TERRAIN_RETREAT_THRESHOLD` | 0.80 | 地形效能低于 0.8 → 撤 |
| `AI_NAVAL_RETREAT_STRENGTH_RATIO` / `_MORALE_THRESHOLD` | 2.0 / 1.5 | 海军撤退（敌 2 倍实力且我士气 <1.5） |
| `AI_REINFORCE_BATTLE_DISTANCE_LIMIT` / `_STRENGTH_TRESHOLD` | 3 / 0.55 | 增援距离与"我已占优就不再增援" |
| `AI_ASSAULT_FORT_NO_BREACH_STRENGTH_NEEDED` / `_BREACH_` | 3 / 1.5 | 强攻要塞所需兵力倍数 |
| `AI_AVOID_ATTRITION_MAX_PATH_FIND_DISTANCE` | 7 | 找避损点的搜索深度 |

### 5.5 外交、威胁与结盟

| 常量 | 值 | 含义 |
|---|---|---|
| `THREAT_CALCULATION_BASE_VALUE` / `OPINION_DIVISOR` / `ANTAGONISM_DIVISOR` | 10 / 50 / 5 | 威胁值算法 |
| `THREAT_DANGEROUS_SCORE` | 2.5 | 超过就要找强援 |
| `AI_RIVAL_THREAT_THRESHOLD` / `AI_RIVAL_WARY_THRESHOLD` / `_SCORE_MULTIPLIER` | 0.6 / 0.3 / 0.3 | 宿敌选择（威胁太高反而不选） |
| `AI_THREAT_STRENGTH_THRESHOLD` / `AI_THREAT_TOO_HIGH_THRESHOLD` | 0.50 / 0.875 | 找盟友要补到目标实力的多少；超过 0.875 直接放弃 |
| `MAX_ALLIES_TO_PICK_AGAINST_ENEMIES` / `AI_TOO_STRONG_ALLY` / `AI_TOO_WEAK_ALLY` | 2 / 1.25 / 0.33 | 同时拉拢数、太强/太弱不结盟 |
| `AI_MAX_BUILD_OPINION_AT_ONCE`（建筑型国家 16） / `..._IMPROVE_UNTIL` | 2 / 100 | 同时改善关系数、改到 100 好感为止 |
| `AI_HIGH_OPINION_THRESHOLD` / `AI_LOW_OPINION_THRESHOLD` | 100 / −100 | 特定交互的好感门槛 |
| `ROYAL_MARRIAGE_RULER_WEIGHT` / `HEIR` / `OTHER` | 20 / 10 / 10 | 联姻对象权重（见外交篇 §二） |
| `DYNASTIC_HEIRLESS_MARRIAGE_FACTOR` | 2.0 | 性格×王朝偏好下，"对方无嗣"的联姻分可翻到 3 倍 |
| `UNBALANCED_FAVORS_ACCEPTANCE_MULT` | 200 | 人情不对等时的接受度倍率 |
| `AI_KEEP_SPY_NETWORK_*` / `AI_DIPLOMAT_UTILITY` | 1.2 / 0.1 / 1；0.0075 | 间谍网与外交官价值 |

### 5.6 经济、建造与殖民

| 常量 | 值 | 含义 |
|---|---|---|
| `AI_PROFIT_MARGIN_TARGET` / `AI_GOLD_COST_UTIL_FROM_LOW_PROFIT_MARGIN` | 0.25 / 3 | 想维持 25% 利润率，越偏离越在意 |
| `AI_BUILDING_PROFIT_THRESHOLD` / `BUILDING_THRESHOLD_FACTOR` | 1.2 / 1.2 | 至少 20% 盈利才扩建 |
| `AI_HEAVY_DEBT_THRESHOLD` / `AI_HIGH_DEBT_RATIO_TO_TAX_BASE` | 0.75 / 10 | 重债线；债务 > 10 倍税基进入高储蓄模式 |
| `MAX_LOAN_FOR_TROOPS` / `MAX_LOANS_FOR_MERCS` | **0** / 3 | **原版绝不为买兵借钱**；雇雇佣兵最多背 3 笔 |
| `AI_BUFFER_TAX_BASE_FACTOR` | 3 | 留 3 倍税基的现金缓冲 |
| `MAX_ALLOWED_INFLATION`（储蓄模式 ×3 / ×10） | 0.02 | 平时容忍 2% 通胀 |
| `AI_EARLY_GAME_MONTHS` | 36 | 开局 36 个月优先发育（暂缓征兵） |
| `AI_BASE_COLONY_UTILITY` / `COLONY_DISEASE_UTILITY_PENALTY` / `COLONY_POP_THRESHOLD` | 3 / 50 / 20 | 殖民基础欲望；疫病区 −50；人口阈值 20 |
| `AI_MIGRATION_THRESHOLD_FOR_DISEASED_LOCATIONS` | 0.05 | 月迁移量够高才敢去疟疾区 |
| `COLONY_NEIGHBORING_UTILITY_BONUS` / `COLONY_REGION_COMPLETION_UTILITY_BONUS` | 10 / 5 | 贴着己方/凑齐大区 |
| `EXPLORATION_UTILITY_SEA` / `_LAND` / `_DISTANCE_BASE` / `ONGOING_..._PENALTY` | 6 / 10 / 1000 / 0.5 | 探索偏好 |
| `AI_RGO_SIZE_PRICE_UTIL` / `AI_RGO_FALLING_PRICE_UTILITY_FACTOR` | 0.02 / 0.4 | RGO 扩建看价格，跌价趋势打折 60% |
| `ADJUST_TRADE_ROUTE_CHANCE` / `AI_MINIMUM_TRADE_SIZE` | 0.5 / 0.5 | 贸易路线调整概率（防全 AI 挤同一条） |
| `AI_MARKET_LOW_FOOD_THRESHOLD` / `_CRITICAL_` | 0.5 / 0.1 | 粮食库存告警线 |
| `AI_ALLOWED_FORT_BUDGET` / `AI_ALLOWED_BUILDING_MAINTENANCE` | 0.1 / 0.25 | 要塞与建筑维护预算占收入比 |

### 5.7 储蓄模式（破产前 AI 的"过冬"状态）

`AI_SAVING_MODE_*` 一族：低/高两档，触发条件是债务 > `AI_HIGH_DEBT_RATIO_TO_TAX_BASE`（10 倍税基）；目标是 60 个月内还清债务、稳定度目标 0.1、政府权力分档（<40 → 月增 0.3；<85 → 0.15；否则 0.05）、殖民地/探索/文化维护费一律砍到 0.5/0.5/0.25、允许通胀上限放大 3–10 倍。

## 六、脚本钩子层（原版用量极少，但都是 mod 的正规入口）

| 目录 | 原版条数 | 用途 |
|---|---|---|
| `ai_scripted_expansion_score\` | **2** | 给"AI 评估未来战争"的目标加减分：`attacker_potential` / `target_trigger` / `score`（加）/ `multiplier`（乘）/ `never_attack`（归零） |
| `ai_scripted_expansion_target\` | **2** | 让 AI 考虑本会忽略的国家：`candidate_list`（effect 填 `add_to_list = source`）/ `casus_belli` / `ignore_antagonism` / `score` / `sort_value` |
| `scripted_diplomatic_objectives\` | **5** | 长期外交目标：`ai_tick_frequency`、`recipient_trigger`、`recipient_priority`、`improve_relation(_limit)`、`defensive_support`、`antagonise`、`destroy`、`country_interactions{...}`、`country_relations{...}`、`time_limit`、`max_allowed` |
| `rival_criteria\` | **6**（`europe.txt`） | 宿敌标准：`enabled = { … }`（**只有 AI 遵守**） |
| `join_war_rules\` | **1**（`teu_crusade.txt`） | 参战规则：`join_war_disabled_trigger`（root=参战国，scope:war / first_leader / second_leader） |
| `generic_action_ai_lists\` | **93 / 85 文件** | 给通用行动分组排序（`global_list`、`military_list`、`parliament_list`、各灾难/IO 专属表…） |

原版把 2 条扩张分与 2 条扩张目标**全部用在圭尔夫与吉伯林（`guelphs_and_ghibellines.txt`）**，外交目标只用在 **HRE 选帝权**与**日本战国谋报**；也就是说：**想让 AI 有"剧本级"偏好，这些文件是唯一正规入口，但原版几乎没参考样例**——写之前务必先读 readme（6 个目录都有）与 `fields\common-scripted_diplomatic_objectives.md` 等字段档。

另有 **on_action 里的 `ai_spam_protection` 冷却**：原版在 1338 年前给每个 AI 国家 `random = { chance = 90 }` 加 `add_cooldown = { type = ai_spam_protection years = 4 }`（`on_action\_hardcoded.txt:32-44`），用来抑制 AI 的事件刷屏。

## 七、难度与攻击性（`main_menu\common\static_modifiers\difficulty.txt`）

**玩家难度**（`difficulty_player_*`）：`very_easy` 给一堆 +25~30%（陆军/海军士气 +0.25、税收效率 +0.25、内阁效率 +0.25、立法效率 +0.25、+0.25 王室阶层权力、每月自满 −0.025）→ `normal` 空块 → `hard`/`very_hard` **只减阶层目标满意度**（−0.05 / −0.1）。

**AI 难度**（`difficulty_ai_*`）：

| 档 | 数值 |
|---|---|
| `very_easy` | 陆/海军士气 **−0.50** |
| `easy` | 陆/海军士气 −0.25 |
| `normal` | 小贸易效率加成、阶层目标满意 +0.05 |
| `hard` | 纪律 +0.1、海军伤害 +0.1/−0.1、税收效率（大加成）、研究 +0.1、阶层满意 +0.1、人口增长 +0.0005、食物消费 −0.1 |
| `very_hard` | 纪律 +0.2、海军伤害 +0.2/−0.2、税收 +0.2、研究 +0.2、阶层满意 **+0.25**、人口增长 +0.001、食物消费 −0.2 |

**攻击性三档**（独立于难度）：

| 档 | 数值 |
|---|---|
| `low_aggression` | `ai_require_cb_for_war = yes`、`carefulness +0.25`、`ai_months_between_wars = 60` |
| `normal_aggression` | `ai_months_between_wars = 24` |
| `high_aggression` | `aggressiveness_modifier = 1.0`、`ai_opinion_bias = −50`、`ai_months_between_wars = 6`、`carefulness −0.25` |

## 八、AI 的认知边界（"看得见"与"记得住"）

AI 不是全知的，这一层决定了它的"失误"是否符合预期：

| 机制 | 常量 | 值 |
|---|---|---|
| 迷雾中遗忘单位 | `FOG_OF_WAR_FORGET_CHANCE` | 1（进雾就可能忘） |
| 忘记追击目标 | `AI_FOW_FORGET_CHASE_ARMY_CHANCE` | 0.33 |
| 忘记被强拆的单位 | `AI_GROUP_RECENTLY_REMOVED_UNIT_FORGET_CHANCE` | 0.25 |
| 得知对方性格所需情报 | `NDiplomacy` 的 `INTEL_THRESHOLD_AI_PERSONALITY`（loc 引用） | 见外交篇情报阈值三档 |
| 重复报价抑制 | `OFFER_MINIMUM_MONTHS` / `MIN_MONTHS_BETWEEN_REJECTED_OFFERS` | 18 / 48 月 |
| 寻路降级 | `AI_PERFORMANCE_PATHFIND_DISTANCE_THRESHOLD` / 海军 | 100 / 2500 |
| 建筑与城市评估抽样 | 见 §四 | 1/24 市场、1/56 城市、至少 20 个 |

## 九、Mod 改造建议（可改 vs 硬编码）

| 想改的东西 | 正规做法 |
|---|---|
| 让某国更凶 / 更爱联姻 | 加 `ai_personalities` 修正（特质、社会价值、政府改革、法律都能挂这些修正） |
| 让某类行动 AI 更愿意做 | 在对应类目写 `ai_will_do`（脚本值，带 `desc` 便于解释），并设 `ai_tick` / `ai_tick_frequency` |
| 让 AI 优先研究某条革新 | `advances` 的 `ai_weight` / `ai_preference_tags` |
| 让 AI 更看重某商品 | `goods` 的 `ai_rgo_size_importance`（可再乘 `AI_RGO_SIZE_PRICE_UTIL`） |
| 让 AI 对某交互更容易点头 | `ai_diplochance\<表>` 加权重项（或在该交互文件里写 `ai_prerequisite`） |
| 给 AI 编"剧本偏好" | ⑤ 层六个目录（expansion score/target、diplomatic objectives、rival criteria、join war rules） |
| 整国层面的 AI 强弱 | `difficulty_ai_*` 静态修正、`game_rules` 的 `ai_personalities` 选项 |
| 全局节奏与性能 | `NAI` 段 + `game_rules` 的 `defines = { NAI = { … } }`（推荐用规则覆盖，见 `guides\defines.md`） |

**硬编码边界**：AI 的实际决策循环（何时调用哪个评估器）、效用函数如何合成、寻路与战场模拟、背包式和约算法、性格的随机分配时机、地图模式的着色——都在引擎里。defines 与权重只调"输入"，不换"算法"。

## 十、中文检索键

**概念**（`game_concepts_l_simp_chinese.yml`）：`game_concept_ai_personality` **国家性格**（:2097，alias 含 `country_personality`；`_upper_desc` :2099 原文见 §二）、`game_concept_ai_disposition` **国家态度**（:2105，alias `country_disposition`；另有 `game_concept_ai_dispositions` 复数键 :2106，desc 用 `[ShowValues('ai_disposition')]` 列出全部态度）；`00_game_concepts.txt:3386` / :3391 为两者的概念定义（各带 `texture = map_modes/country_personalities_bg` / `country_dispositions_bg`）。

**界面**（`main_menu\localization\simp_chinese\ai_personalities_l_simp_chinese.yml`，**169 行**）：8 个性格键 `ai_balanced` 平衡 / `ai_aggressive` 侵略 / `ai_expansionist` 扩张 / `ai_defensive` 防御 / `ai_cautious` 谨慎 / `ai_opportunistic` 投机 / `ai_isolationist` 孤立 / `ai_friendly` 友善（各带 `_desc`，:3–19）；`AI_PERSONALITY_MODIFIERS` 行为修正（:23）；`ai_personality_mapmode` **国家性格**地图模式（:166，按性格给国家上色）；筛选器 `CUSTOM_SEARCH_FILTER_AI_PERSONALITY_CATEGORY_NAME` 国家性格（:21）。外交侧的 `INTEL_FOG_AI_PERSONALITY_UNKNOWN` 在 `diplomacy_l_simp_chinese.yml:2946`。

**trigger 词条**：`ai_will_do` 可作为 trigger 读取（`trigger_localization\common_triggers.txt:123`，键 `AI_WILL_DO_TRIGGER`）。

**关联字段档**：`fields\common-ai_personalities.md`、`fields\common-ai_diplochance.md`、`fields\common-ai_scripted_expansion.md`、`fields\common-scripted_diplomatic_objectives.md`、`fields\common-rival_criteria.md`、`fields\common-join_war_rules.md`、`fields\common-generic_action_ai_lists.md`；常量索引见 `guides\defines.md`，权重键分布见 `guides\systems-map.md`。

**AI 与治理的交叉点**（详见 `vanilla\vanilla-government-and-reform.md`）：`ai_government_power_target_modifier`（政体资源目标）、`AI_PERFORMANCE_REFORMS_MONTHS_BETWEEN_UPDATES = 24` / `AI_PERFORMANCE_BUREAUCRACY_MONTHS_BETWEEN_UPDATES = 24`（改革与官僚每 24 个月才重估）、`AI_GRANT_BUREAUCRACY_THRESHOLD` / `AI_REMOVE_BUREAUCRACY_THRESHOLD = 5`、`AI_REVOKE_PRIVILEGE_STABILITY_THRESHOLD = −50`、`AI_GOVERNMENT_SIZE_UTILITY = 0.5`。

**AI 与殖民/探索的交叉点**（详见 `vanilla\vanilla-colonization-and-exploration.md`）：`EXPLORATION_UTILITY_SEA = 6` / `_LAND = 10` / `ONGOING_EXPLORATION_UTILITY_PENALTY = 0.5`、`AI_BASE_COLONY_UTILITY = 3`、`AI_COLONIAL_MIGRATION_SPEED_UTILITY = 0.05`、`AI_COLONY_COMPETING_CHARTER_UTILITY_PENALTY = 0.25`、`COLONY_DISEASE_UTILITY_PENALTY = 50`、`AI_MIGRATION_THRESHOLD_FOR_DISEASED_LOCATIONS = 0.05`、`AI_PERFORMANCE_SAMPLE_SIZE = 3`（削减特许殖民地测试）；**"历史上该往哪探索"由 `common\area_preferences\` 的 83 条区域偏好决定**（须用 `add_area_preference` 指派才生效）。
