# 原版解析：战斗与战争（vanilla combat & war）

> **一句话**：上篇讲怎么打赢（单位、`NCombat` 常数、地形、围城、战争目标），下篇讲战争本身（102 个 CB、战争分数热情、64 个和约条款、土地承诺）。
> **什么时候看**：调战斗与围城数值、写 CB 或和约条款、改战争分数与战争热情权重，或查宣战代价时翻这篇。
> **体量**：382 行 · 约 18 分钟通读

## 目录

- [一、部队组织](#一部队组织)
- [二、单位体系](#二单位体系)
  - [陆军 6 类（`common\unit_categories\`）](#陆军-6-类commonunit_categories)
  - [兵种模板与升级（`common\unit_types\`）](#兵种模板与升级commonunit_types)
- [三、战斗常数（`NCombat`）](#三战斗常数ncombat)
- [四、地形修正](#四地形修正)
- [五、围城（`NCombat` 后半段）](#五围城ncombat-后半段)
- [六、战争目标（`common\wargoals\`，readme 权威）](#六战争目标commonwargoalsreadme-权威)
- [七、宣战侧：宣战理由（`casus_belli`）与宣战代价](#七宣战侧宣战理由casus_belli与宣战代价)
  - [7.1 CB 系统（67 个 CB 文件 + 40 行 readme）](#71-cb-系统67-个-cb-文件--40-行-readme)
  - [7.2 宣战代价（`prices\00_hardcoded.txt`，全部 8 条）](#72-宣战代价prices00_hardcodedtxt全部-8-条)
- [八、战争状态机：战争分数 · 参与度 · 战争热情（`NWar` + `NDiplomacy`）](#八战争状态机战争分数--参与度--战争热情nwar--ndiplomacy)
  - [8.1 战争分数（warscore）](#81-战争分数warscore)
  - [8.2 参与度（participation）—— EU5 的"战争贡献账本"](#82-参与度participation-eu5-的战争贡献账本)
  - [8.3 战争热情（`WAR_ENTHUSIASM_*`，33 条）](#83-战争热情war_enthusiasm_33-条)
  - [8.4 逼降：Call for Peace 与无条件投降](#84-逼降call-for-peace-与无条件投降)
- [九、和约：条款系统与战争分数定价](#九和约条款系统与战争分数定价)
  - [9.1 条款系统（`peace_treaties\`，**64 个条款定义 / 53 数据档** + readme）](#91-条款系统peace_treaties64-个条款定义--53-数据档--readme)
  - [9.2 战争分数定价（`NDiplomacy` 的和约段，46 条）](#92-战争分数定价ndiplomacy-的和约段46-条)
  - [9.3 `WAR_WORTH`（战争价值）：和约里"这块地值多少"的底账](#93-war_worth战争价值和约里这块地值多少的底账)
- [十、盟友、土地承诺与战后](#十盟友土地承诺与战后)
  - [10.1 土地承诺与"分赃"（`NWar`，EU5 特色）](#101-土地承诺与分赃nwareu5-特色)
  - [10.2 战后：停战、复仇主义与强制和平](#102-战后停战复仇主义与强制和平)
- [十一、脚本钩子与 AI 常量](#十一脚本钩子与-ai-常量)
- [十二、Mod 改造建议](#十二mod-改造建议)
- [十三、中文检索键](#十三中文检索键)

版本基准：EU5 1.3.x。核心文件：`loading_screen\common\defines\00_defines.txt`（`NUnit` 325–399、`NCombat` 402–513、**`NWar` 2419–2479**、`NDiplomacy` 1992–2417 的战争段）、`common\unit_categories\`、`common\unit_types\`、`common\topography\`、`common\vegetation\`、`common\wargoals\`（readme 权威）、**`common\casus_belli\`（67 数据档 / 102 个 CB 定义 + 40 行 readme）**、**`common\peace_treaties\`（53 数据档 / 64 个条款定义 + readme）**、**`common\prices\00_hardcoded.txt`（宣战价格 8 条）**、`common\ai_diplochance\00_ai_diplochance.txt`（强制和平/威胁宣战/请求和平的权重表）。

> **本篇的分工**：§一–§六 = **怎么打赢**（部队、战斗、围城、战争目标）；**§七–§十 = 战争本身**（宣战 → 战争分数与热情 → 和约 → 战后）；§十一–§十三 = 钩子、Mod、检索键。
> 外交关系（好感/信任/条约/附庸/召唤参战的条约侧）见 `vanilla\vanilla-diplomacy.md`；地形与损耗见 `vanilla\vanilla-hazards-and-environment.md`。

## 一、部队组织

- 团（regiment）= `REGIMENT_SIZE = 1000` 人；每团有 **strength（兵力）** 与 **morale（士气）** 两条
- 基础士气 `LAND_MORALE = 3.0` / `NAVAL_MORALE = 3.0`
- 补充 `MONTHLY_REINFORCE = 0.25`（陆）/ `MONTHLY_REPAIR = 0.1`（海）
- 移动 `ARMY_MOVEMENT_SPEED = 0.13` / `NAVY_MOVEMENT_SPEED = 0.5`；规模影响移动（`SIZE_IMPACT_ON_MOVEMENT_SCALE = -0.02`，封顶 `-0.5`）
- 缺粮损耗 `ATTRITION_LACK_OF_FOOD = 5`、海上损耗 `ATTRITION_DAYS_AT_SEA = 0.02`、冻结损耗 `FROZEN_ATTRITION = 0.5`
- 宣战后征召兵补员加速：`WAR_DEC_LEVY_RECRUIT_SPEED_FOR_MONTHS = 6`（月）

## 二、单位体系

### 陆军 6 类（`common\unit_categories\`）

| 类别 | frontage | initiative | combat_speed | flanking | 特性 |
|---|---|---|---|---|---|
| 轻步兵 `00_army_light_infantry` | 1 | 5 | 3 | 1.1 | assault=yes；morale_damage_taken +0.10 |
| 重步兵 `01_army_heavy_infantry` | — | — | — | — | 近战主力 |
| 轻骑兵 `02_army_light_cavalry` | — | — | — | — | 侧翼 |
| 重骑兵 `03_army_heavy_cavalry` | — | — | — | — | 正面突破，革命时代后过时 |
| 炮兵 `04_army_artillery` | 1 | **1** | **1** | 1.0 | bombard=yes；`damage_taken = 1.25`、`attrition_loss = 0.5`、`food_consumption 0.66` |
| 辅助 `05_army_auxiliary` | — | — | — | — | 运粮 |

海军 4 类：galley / light_ship / heavy_ship / transport（属性 `cannons`、`hull_size`、`crew_size`、`maritime_presence`、`blockade_capacity`、`transport_capacity`）。

### 兵种模板与升级（`common\unit_types\`）

- `00_age_templates_land.txt`：6 个时代的模板，**数值随时代翻倍**

  | 时代 | 轻/重步兵 max_strength / combat_power | 炮兵 combat_power / bombard_efficiency / artillery_barrage |
  |---|---|---|
  | 1 traditions | 0.5 / 1 | 2 / 0.1 / 1 |
  | 2 renaissance | 1.0 / 1 | 3 / 0.125 / 3 |
  | 3 discovery | 1.5 / 1 | 4 / 0.15 / 4 |
  | 4 reformation | 2.0 / 1.5 | 5 / 0.20 / 5 |
  | 5 absolutism | 2.5 / 2.25 | 5.5 / 0.25 / 7 |
  | 6 revolutions | 3.0 / 3 | 7.5 / 0.30 / 9 |

- 海军模板 `00_age_templates_navy.txt`：重型船 hull 15→60、cannons 30→120；运输船 transport_capacity 0.25→1.5、food_storage 30→960
- 升级链 `upgrades_to`（如 `n_carrack → n_galleon → n_twodecker → n_threedecker`）；`copy_from` 继承
- 特殊兵种：`3_janissaries.txt`（每代 `strength_damage_taken = -0.10`、`morale_damage_taken = -0.10`，受 `janissary_unit_limit` 限制）、骑士 `0_knights.txt`、征服者、大象、卡瓦、齐兹尔巴什
- 征召兵 `levy = yes` → 战力 ×`LAND_LEVY_COMBAT_IMPACT = 0.75`
- 雇佣兵池 `mercenaries_per_location = { pop_type = X multiply = N }`（如 laborers 0.1、nobles 0.3）
- 全修饰符清单见 `unit_types\readme.txt`（frontage/initiative/combat_speed/flanking_ability/secure_flanks_defense/combat_power/max_strength/damage_taken…）

## 三、战斗常数（`NCombat`）

| 常量 | 值 | 说明 |
|---|---|---|
| `COMBAT_DICE_SIDE` | 10 | 10 面骰 |
| `COMBAT_BASE` / `COMBAT_MAX` | 5 / 15 | 骰值基准与上限 |
| `COMBAT_DAMAGE_MULT` | 0.01 | 伤害倍率 |
| `HOURS_PER_PHASE` | 5 | 每阶段 5 小时 |
| `MINIMUM_COMBAT_DURATION` | 24 | 陆战最少 24 小时 |
| `MINIMUM_NAVAL_COMBAT_DURATION` | 72 | 海战最少 72 小时 |
| `STRAIT_CROSSING_DICE` | −2 | 海峡 |
| `RIVER_CROSSING_DICE` | −1 | 渡河 |
| `SEA_LANDING_DICE` | −1 | 登陆 |
| `MAX_FRONTAGE_OVERSTACKING` | 1.25 | 侧翼可超编 25% |
| `MIN_FRONTAGE_AFTER_TERRAIN` | 2 | 地形削减后最低正面宽度（在 `NLocation`） |
| `MORALE_COLLAPSE_THRESHOLD` | 0.05 | 士气崩溃 |
| `COMBAT_HOURLY_MORALE_TICK` | 0.01 | 每小时士气流失 |
| `INITIATIVE_BASE_CHANCE / _EACH / _HOURS / _MAX` | 0.1 / 0.02 / 0.01 / 0.1 | 主动性决定接敌概率 |
| `COMBAT_SPEED_SCALE` | 0.05 | 战斗速度影响攻击频率与撤退 |
| `RETREAT_STRENGTH_DAMAGE` | 0.1 | 撤退损失 10% 兵力 |
| `LAND_EXPERIENCE_DAMAGE_REDUCTION` | 0.5 | 经验减伤上限 50% |
| `EXPERIENCE_GAIN` | 30 | 战斗经验 |
| `TRADITION_GAIN_LAND / _NAVAL` | 10 / 20 | 传统 |
| `COMBAT_IMPRISONED_UNIT_DEATH_RATE` | 0.4 | 俘虏死亡率 |
| `LAND_WAR_EXHAUSTION_FROM_LOSSES` | 1（海军 ×1.5），`MAX_WAR_EXHAUSTION_FROM_BATTLE` 5.0 | 厌战度（`ALERT_HIGH_WAR_EXHAUSTION = 10`、AI `SAFE_AMOUNT_OF_WAR_EXHAUSTION = 5`） |
| `PRESTIGE_FROM_LAND/_NAVAL`、`PRESTIGE_VS_RIVAL` | 0.5 / 0.5 / 1.5 | 威望（打宿敌 ×1.5） |
| 海战专属 | `NAVAL_MORALE_DAMAGE_MODIFIER 0.2`、`NAVAL_LOW_MORALE_THRESHOLD 1.5`、`NAVAL_COMBAT_SHIP_STR_SINK_THRESHOLD 0.1`、`NAVAL_RETREAT_CHANCE 10` | 海战士气伤害仅 20% |

## 四、地形修正

**骰子加成**（`topography`/`vegetation` 的 `defender` 字段）：

| 地形 | defender | frontage 惩罚 | 备注 |
|---|---|---|---|
| 山脉 mountains | **+2** | `local_frontage_allowed = -4` | `blocked_in_winter = yes` |
| 丘陵 hills | +1 | −3 | — |
| 森林 forest | +1 | −3 | — |
| 林地 woods | +1 | −2 | — |
| 丛林 jungle | +1 | −4 | — |

（另有湿地植被 +1；`local_frontage_allowed` 也出现在 location_modifier 中影响可用宽度。）

> **完整地形/植被数值表**（22 地形 + 7 植被的 `movement_cost`/`proximity`/`defender`/`vegetation_density`/人口容量/粮食/supply_limit/视野/海军损耗）见 `vanilla\vanilla-hazards-and-environment.md` §二；本篇只保留与战斗相关的 `defender` 与 frontage 两列。

**兵种地形修正**（unit 的 `combat = {}` / `impact = {}`）：骑兵模板自带 `jungle/wetlands/mountains = -0.10`（战斗伤害 −10%）、`impact` 同地形 +0.10（移动更慢）；还支持 `river`、`coastal`、`inland`、`climate` 键。

## 五、围城（`NCombat` 后半段）

| 常量 | 值 |
|---|---|
| `DAYS_PER_SIEGE_PHASE` | 30（无堡垒 `DAYS_PER_SIEGE_PHASE_WITHOUT_FORT = 15`，下限 `MIN_DAYS_PER_SIEGE_PHASE = 7`） |
| `SIEGE_WIN` | 20（阶段骰达 20 破城） |
| `MAX_BREACH` | 3；`BREACH_REPAIR_PER_DAY = 0.01` |
| `SIEGE_MEMORY` | 11 |
| 短缺惩罚 | 补给 −0.02 / 粮食 −0.03 / **水源 −0.05** / 守军逃亡 −0.1 / 破口 −0.05 |
| `SIEGE_DISEASE_IMPACT` | 0.05 |
| 轰炸 | `BOMBARD_BASE_CHANCE = 0.2`、`BOMBARD_HOURS = 5` |
| 强攻 | 攻方 `ASSAULT_ATTACKER_LOSS = 2.5`、士气 −3.0；守方 0.03 / 士气 −0.3；`ASSAULT_DICE_MODIFIER = 5`、`ASSAULT_WIDTH_LIMIT = 1` |
| 占领获得 | `GARRISON_AFTER_OCCUPATION = 0.010`、`FOOD_PERCENTAGE_LOST_AT_OCCUPATION = 0.9` |
| 炮兵门槛 | `SIEGE_REGIMENTS_FOR_ARTILLERY = 3`；`FORT_GARRISON_UPKEEP = 2`（NLocation） |

驻军强度脚本：`common\script_values\garrison.txt`（根为 location；`garrison_strength`、`combat_side_strength`、`besieger_strength`，输出守/攻比值用于出击判定）。

## 六、战争目标（`common\wargoals\`，readme 权威）

**9 种引擎类型**（`type` 是枚举，新增须复用现有类型）：`take_province`、`superiority`、`naval_superiority`、`defend_capital`、`enforce_military_access`、`independence`、`take_capital`、`take_border`、`take_country`。

`00_default.txt` 实例（`take_capital`，约 105–116 行）：attacker `conquer_cost = 0.5`、`subjugate_cost = 0.5`；`ticking_war_score = 0.5`（每月 +0.5 战争分数）。变体：`take_capital_sound_toll`（成本 1.5）、`take_capital_tributary`、`take_capital_subjugation`、`take_capital_imperial`。

**字段族**（readme）：`war_name`（本地化键）、`war_name_is_country_order_agnostic`（Eng→Fra 与 Fra→Eng 是否同一战争名）、`allow`、**`attacker {}` 与 `defender {}` 双方各一套**：`call_in_overlord` / `call_in_subjects`（是否拉宗主/拉附庸参战）、`conquer_cost`（割地战争分数系数）、`subjugate_cost`（收附庸系数）、`release_cost`（释放国家/地区系数）、**`antagonism`（整个和约的敌意系数）**、`allowed_locations`（**限定能割哪些地**，scope:winner/loser/war/location）、`allowed_subjugation`；以及 `ticking_war_score`（默认 1）。

## 七、宣战侧：宣战理由（`casus_belli`）与宣战代价

### 7.1 CB 系统（67 个 CB 文件 + 40 行 readme）

**三段门**（这是 EU5 CB 的核心设计——CB 不是"有/没有"，而是**逐月生成**的）：

| 字段 | 作用 |
|---|---|
| `create_visible` | 该国**能否看到**这个 CB（root = country，scope:target） |
| `create_enabled` | **能否开始生成**（对目标） |
| `declare_enabled` | 生成后**能否用它宣战** |
| **`speed`** | **每月生成进度**（百分点，100 = 生成完毕，可用它做"酝酿期"） |
| `province` | 若 CB 以省份为目标，校验该省是否可被选（root = province） |

**其它字段**：`no_cb`（唯一的"无 CB 的 CB"）、`trade`（是否贸易类）、`war_goal_type`（用这个 CB 时挂在哪个战争目标上）、`allow_separate_peace`（默认 yes）、`allow_white_peace`（默认 yes）、`can_expire`、`allow_wars_on_own_subjects`（**可对自家附庸用**）、`cut_down_in_size_cb`（仅 AI：更倾向释放条约）、`allow_ports_for_reach_ai`（十字军类：不在乎 AI 控制力）、`custom_tags` / `show_tags_in_ui`。

**三个战争热情钩子**：`additional_war_enthusiasm`（+ `_attacker` / `_defender` 变体）——**CB 直接决定参战方的战争热情**（scope:war / attacker / defender / target_*）。

**可逐 CB 覆盖的全局**：`days/weeks/months/years = <int>`（覆盖 `NDiplomacy::CASUS_BELLI_MONTHS = 120`）、`max_warscore_from_battles`（覆盖 `WARSCORE_MAX_FROM_BATTLES = 50`）。

**AI 三件套**：`ai_subjugation_desire`、`ai_cede_location_desire`、`antagonism_reduction_per_warworth_defender`；**`ai_will_do` 会覆盖默认的"按战争目标征服成本"计算**。
效果侧：`add_casus_belli` / `remove_casus_belli` / `remove_all_casus_belli_of_type`（`effect_localization\country_effects.txt`）。

### 7.2 宣战代价（`prices\00_hardcoded.txt`，全部 8 条）

| 价格键 | 代价 |
|---|---|
| `declaring_war` | **karma 10**（基础） |
| `war_no_cb` | **稳定 15 + 厌战度 1 + karma 10 + 正义 10** |
| `war_on_same_religion_no_cb` | **稳定 30 + 厌战度 2 + karma 20 + 正义 20** |
| `war_breaking_truce` | **稳定 50 + 厌战度 1** |
| `war_breaking_truce_with_guarantor` | 稳定 10 |
| `war_great_relations`（好感 ≥100 时宣战） | **稳定 20** |
| `war_good_relations`（好感 ≥50） | 稳定 10 |
| `war_when_military_acces`（对方有你的通行权） | **稳定 40** |
| `war_on_subject`（打别人的附庸） | 稳定 40 + 正义 40 |
| `war_on_same_religion_cb` / `war_on_different_religion` | **免费**（有对应 CB 时） |
| `honoring_alliance_call`（响应盟友召唤） | **karma −25**（负价 = 获得） |

**力量投射也影响宣战成本**：`NWar::ATTACKER_WAR_COST_POWER_PROJECTION_SCALE = 0.01` → **宣战成本乘数 = 1 −（攻守力量投射差 × 0.01）**。

## 八、战争状态机：战争分数 · 参与度 · 战争热情（`NWar` + `NDiplomacy`）

### 8.1 战争分数（warscore）

| 常量 | 值 | 说明 |
|---|---|---|
| `MAX_WAR_SCORE` | **100** | 上限 |
| `OCCUPATION_VALUE_SCALE` | **2.0** | "1.0 时需要占领 100% 土地才有 100 分；越高需要越少" |
| `MONTHS_BEFORE_TOTAL_OCCUPATION` | **60** | 开战未满 60 个月，**只占领战争领袖不能拿 100 分** |
| `WARSCORE_MAX_FROM_BATTLES` | **50** | 战斗最多贡献一半 |
| `BATTLE_RESULT_SCALE` / `BATTLE_RESULT_CAP` | 25 / 25 | 单场战斗结算 |
| `DEFAULT_WARGOAL_TICKINGWARSCORE_BONUS` | 2 | 战争目标每月 tick |
| `WARGOAL_MAX_TICKING_WAR_SCORE` / `WARGOAL_MAX_BONUS` | 25 / 50 | tick 上限 / 总上限 |
| `DEFAULT_WARGOAL_BATTLESCORE_BONUS` | 3 | 战争目标的战斗分加成 |
| `SUPERIORITY_WARGOAL_WARSCORE_THRESHOLD` | 10 | 制海/制陆权目标的战斗分门槛 |
| `MIN_WARSCORE_TO_DEMAND` | **10** | 低于此不能提要求 |
| `WAR_ENFORCE_DEMANDS_MAX_WARSCORE_FRACTION` / `..._COST_NEEDED` | 0.5 / 2 | 强制执行需求：需分数 ≥ 需求的 2 倍且不超过上限一半 |
| 效果 | `add_bonus_warscore` | 脚本直接加分（`war_effects.txt`） |

### 8.2 参与度（participation）—— EU5 的"战争贡献账本"

| 常量 | 值 | 说明 |
|---|---|---|
| `PARTICIPATION_SCORE_BLOCKADE` | **0.001** | 每点发展度 × 每船 × 每月 |
| `PARTICIPATION_SCORE_BATTLE` | **0.03** | 每个参战团或船 |
| `PARTICIPATION_SCORE_SIEGE` | **0.01** | 每个能推进围城的团 |
| `PARTICIPATION_SCORE_OVERSEAS_MULT` | **0.25** | 远离战争领袖作战打折 |
| `PARTICIPATION_SCORE_MERC_MULT` | **0.5** | 雇佣兵打折 |
| `PARTICIPATION_SCORE_UNFORTIFIED_MULT` | **0.1** | 围无堡垒地点几乎不算 |
| `TRANSFER_OCCUPATION_WAR_PARTICIPATION_BASE_SCORE` / `_MULTIPLIER` | 0.5 / 1 | 移交占领算谁的贡献 |
| **转人情** | `PARTICIPATION_SCORE_TO_FAVOURS_MULTIPLIER_{BLOCKADE,COMBAT,SIEGE}` 各 1、`..._JOINING_WAR = 20` | 参与度持续转成对受援国的人情，**参战入伙一次性 +20** |

### 8.3 战争热情（`WAR_ENTHUSIASM_*`，33 条）

| 组 | 常量 |
|---|---|
| 基础 | `BASE = 0.50`、`TIME_MONTHS = 24`（AI 顽固期）、`TIME_EARLY_FACTOR = 0.0075`、`TIME_LATE_FACTOR = 0.01` |
| 军事形势 | `ATTACKING_SIEGE = 0.25`、`DEFENDING_SIEGE = −0.5`、`UNIT_BALANCE = 0.5`、`MEN_LOSSES = −2.5`、`SHIP_LOSSES = −4.0`、`ONGOING_BATTLES = 10`、`MILITARY_STRENGTH_FACTOR = 0.1` |
| 目标与首都 | `WAR_GOAL = 0.05`、`CAPITAL = 0.05`、`INDEPENDENCE = 0.05` |
| 压力 | `CALL_FOR_PEACE = −0.01`、`WAR_EXHAUSTION = −0.02`、`REBEL_THREAT = −0.2`、`DESPERATION = −0.75`、`SEPERATE_PEACE = −0.1`、`COALITION_FACTOR = 0.3` |
| 走向 | `WAR_DIRECTION_FACTOR = 0.005`、`WAR_DIRECTION_WINNING_MULT = 5.0` |
| **盟友侧（独立倍率）** | `ALLY_MULT = 0.5`、`ALLY_BASE_RELUCTANCE_MULT = 1.5`、`ALLY_WAR_EXHAUSTION_MULT = 1`、`ALLY_TIME_MULT = 1`、`ALLY_CAPITAL_MULT = 1`、`ALLY_DESPERATION_MULT = 1`、`ALLY_REBELS_MULT = 1`、`ALLY_MILITARY_STRENGTH_MULT = 2`、`ALLY_WAR_DIRECTION_MULT = 0`、`ALLY_FORCE_BALANCE_MULT = 0`、`ALLY_WARGOAL_MULT = 0` |

### 8.4 逼降：Call for Peace 与无条件投降

| 机制 | 常量 |
|---|---|
| **Call for Peace** | `CALL_FOR_PEACE_THRESHOLD_MONTHS = 60`（打满 60 月触发）、`CALL_FOR_PEACE_WARSCORE_LIMIT = 67`（分数超过即不必再打）、`CALL_FOR_PEACE_FROM_UNCONDITIONAL_SURRENDER = 3` |
| **无条件投降** | `UNCONDITIONAL_SURRENDER_MONTHS = 2`（开始生效月数，负值关闭该功能）、`UNCONDITIONAL_SURRENDER_MIN_MONTHS = 12`（**开战不满 12 月不能投降**）、`UNCONDITIONAL_SURRENDER_WARSCORE_LIMIT = −90`、`MINIMUM_STRENGTH_TO_AVOID_UNCONDITIONAL_SURRENDER = 0.15`、**`UNCONDITIONAL_SURRENDER_HOPELESS_STRENGTH_COMPARISON = 20`（军力差 20 倍直接举白旗）** |
| 平衡参考 | `ACCEPTABLE_BALANCE_DEFAULT = 1.2` |
| **白和** | `YEARS_SINCE_LAST_WAR_ACTION_BEFORE_WHITE_PEACE = 3`（3 年无战事可白和）、`YEARS_BEFORE_WHITE_PEACE_ALERT_ADVANCE = 1`、`DURATION_BEFORE_PURGING_WARS_IN_DAYS = 365` |
| **单独媾和** | `PEACE_SEPERATE_BLOCK_MONTHS = 12`（前 12 月封锁）+ CB 的 `allow_separate_peace` |
| **自动执行** | `PEACE_AUTO_ENFORCE_BASE_DAYS = 365` + `PEACE_AUTO_ENFORCE_RANK_DAY = 180`（按国家等级每级再加 180 天） |
| 效果 | `white_peace`、`add_access_for_attackers` / `add_access_for_defenders`（`war_effects.txt`） |

## 九、和约：条款系统与战争分数定价

### 9.1 条款系统（`peace_treaties\`，**64 个条款定义 / 53 数据档** + readme）

**四作用域**（读 readme 第一行就会看到，这是最容易写错的地方）：

```
potential / allow / effect 里：
  scope:winner = 取方（taker）      scope:loser = 给方（giver）
  scope:war    = 所在战争           scope:target = 选中的 location/country/province
```

**字段族**：

| 组 | 字段 |
|---|---|
| 定价 | **`cost`**（战争分数，脚本值，四作用域均可用）、**`base_antagonism`**（最大敌意，会按各国因素调整）、`antagonism_type`（敌意 bias 类型键）、**`ai_desire`**（AI 多想加这条）、**`ai_force_add`**（AI 有机会就一定加） |
| 结构 | `blocks_full_annexation`（阻止目标被全吞）、`collate_targets`（目标能否从所有给方合并）、**`are_targets_exclusive`**（地点/省份目标互斥，不能与割地条约并存）、`category = country/location/province/area`（挂到哪一栏） |
| 目标选择 | `select_trigger`（同交互的完整字段族：`looking_for_a`/`source`/`source_flags`/`column`/`map_mode`/`pre_evaluation_*`/缓存三件套） |
| 标签 | `custom_tags`、`show_tags_in_ui` |

条款内容按情境分布：`hegemon_demands`、`rtr_rein_in_rebellion`、`scaligeri_peace_treaties`、`guelphs_and_ghibellines`、`dissolve_league`、`jurchen_confederation_treaties`、`force_convert`、`nanbokuchou_force_imperial_abdication`、`take_shogunate`、`execute_ruler`、`high_kingship_overthrow`、`expand_clan_influence`…

### 9.2 战争分数定价（`NDiplomacy` 的和约段，46 条）

| 类别 | 常量 |
|---|---|
| **割地与征服** | `PEACE_COST_EFFICIENCY_FOR_SAME_CULTURE = 0.10` / `_FOR_SAME_RELIGION = 0.10`（同文同教更便宜）、`_VS_RIVAL = 0.33`、`_FOR_REVOLT_WAR = 5.0`、`_UNOCCUPIED_FORT = −0.33`（未占堡垒更贵）、`_FOR_NON_TARGET = −0.5`、`_FOR_POWER_PROJECTION_DIFFERENCE = 0.001`；`PEACE_TREATY_PROVINCE_BONUS = 1.5` / `AREA_BONUS = 2`；`PEACE_REVOKE_CORE_MULT = 0.25`、`PEACE_RETURN_CORE_MULT = 0.5`、`PEACE_TREATY_RETURN_CORE_ANTAGONISM_PERCENT_REDUCTION = 0.75` |
| **金币** | `PEACE_MAX_WARSCORE_FOR_GOLD = 25`（最多用 25 分换钱）、`PEACE_LOAN_SIZE_MULTIPLIER_FOR_MAX_WARSCORE_GOLD = 5`、`PEACE_GOLD_AI_DESIRE = 0.1`、`PEACE_GOLD_MIN_ECONOMY = 20`、`PEACE_GOLD_COST_PER_MONTHLY_INCOME = 1`、`PEACE_GOLD_MAX_YEARLY_INCOMES = 2`、`PEACE_GOLD_STEP = 1`、**`INFLATION_FROM_PEACE_GOLD = 0.0002`**（每月份收入触发通胀） |
| **贵族献祭** | `PEACE_MAX_NOBLES_SACRIFICED_PERCENT = 0.25`、`PEACE_MAX_WARSCORE_FOR_SACRIFICE = 25`、`PEACE_NOBLES_SACRIFICED_PER_DOOM_POINT = 0.01`、`PEACE_SACRIFICE_AI_DESIRE = 2.0` |
| **附属国条款** | `PEACE_MAKE_SUBJECT_BASE_FACTOR = 0.8`、`..._OTHER_DOMINANT_CULTURE/RELIGION = 1.2`、`..._TOO_MUCH_TO_INTEGRATE = 1.5`、`..._OVER_LIMIT = 0.05`、`..._REVOLT_WAR = 0`、`..._AS_ALLY = 0.5`；`PEACE_CANCEL_SUBJECT_FACTOR = −0.2`、`PEACE_TRANSFER_SUBJECT_FACTOR = −0.1`、`PEACE_BECOME_SUBJECT_FACTOR = −0.1`、`PEACE_RELEASE_SUBJECT_FUTURE_CONQUEST_FACTOR = 0.1`、`PEACE_RELEASE_SUBJECT_CUT_RIVAL_FACTOR = 0.25`、`PEACE_RELEASE_SUBJECT_RIVAL_FACTOR = 1`、`PEACE_RELEASE_SUBJECT_STRENGTH_REQUIREMENT = 0.75` |
| **共主邦联 / 独立** | `PEACE_JUNIOR_PARTNER = 60`、`PEACE_GRANT_INDEPENDENCE = 60`、`PEACE_NON_UNION_ANTAGONISM_FACTOR = 0.33` |
| **条约类** | `PEACE_TREATY_FOOD_ACCESS = 0`、`MILITARY_ACCESS = 0`、`FLEET_BASING = 0`、`ANTI_PIRACY = 10`、`WAR_REPARATIONS = 10`（+ `WAR_REPARATIONS_FACTOR = 0.1`、`WAR_REPARATIONS_YEARS = 10`）、`ANNUL_ALL_TREATIES = 10`（+ `ANNUL_DURATION_MONTHS = 120`）、`PEACE_REMOVE_PROVINCE_FROM_INTERNATIONAL_ORGANIZATION = 0.25` |
| **迁移 / 革命者** | `PEACE_TREATY_FORCE_MIGRATE_COST = 10`（AE 0、规模 0.25、持续 120 月）、`PEACE_TREATY_ANNEX_REVOLTER_MAX_COST = 70`、`PEACE_TREATY_REVOLTER_SURVIVES_MAX_COST = 70` |
| **解放奴隶** | `LIBERATE_SLAVES_BASE_COST = 5`、`MAX_COST = 50`、`POPULATION_DIVISOR = 10000`、`MAX_POP_TRANSFER_FRACTION = 0.1`（单条约最多转移对方 10% 人口） |
| 杂项 | `PEACE_MAX_MONTHS_AT_WAR_BEFORE_START_DATE = 12` |

### 9.3 `WAR_WORTH`（战争价值）：和约里"这块地值多少"的底账

`NWar` 给每块地算一个 war worth，供 AI 与条约定价参考：

```
WAR_WORTH_BASE = 2                    + 税基 × 0.2              + 建筑数 × 0.025（上限 10）
+ 人口 × 0.02（上限 4）                + 国家首都 5              + 省份首都 1
+ 港口 1                              + 堡垒 0.5 + 堡垒等级 × 0.25
+ (该地发展度 − 取方平均发展度) × 0.05（区间 −1 .. +5）
```

## 十、盟友、土地承诺与战后

### 10.1 土地承诺与"分赃"（`NWar`，EU5 特色）

| 机制 | 常量 |
|---|---|
| 给地得好感/人情 | `FAVOR_GAIN_FOR_LAND = 10`、`FAVOR_GAIN_WARSCORE_FACTOR = 20`（按实际战争分数缩放，地越大越多人情） |
| **承诺落空的惩罚** | `TRUST_PENALTY_FOR_NO_LAND = 20`、`OPINION_PENALTY_FOR_NO_LAND = 1`、`PENALTY_FOR_NO_LAND_NOT_PROMISED_MULT = 0.5`（没被许诺过只罚一半） |
| **背信弃义（许诺过却没给）** | **`BROKE_LAND_PROMISE_YEARS = 30`**、`BROKE_LAND_PROMISE_IN_RANGE_TRUST_CHANGE = −5` |
| 期望值 | `MINIMUM_CONTRIBUTION_NOT_PROMISED_LAND = 0.2`、`MINIMUM_CONTRIBUTION_PROMISED_LAND = 0`、`EXPECTED_GAINS_PROMISED_LAND = 0.75`、`EXPECTED_GAINS_NOT_PROMISED_LAND = 0.5` |
| 拒绝召唤的代价 | `DECLINE_CANNOT_REJOIN_WAR_MONTHS = 6`（拒绝后 6 月内不能再入伙）；响应召唤 `honoring_alliance_call` = **karma −25（收益）** |

### 10.2 战后：停战、复仇主义与强制和平

| 机制 | 常量 / 入口 |
|---|---|
| **停战** | `TRUCE_YEARS = 5`、`SCALED_TRUCE_YEARS = 10`、`CASUS_BELLI_MONTHS = 120`（CB 存续 120 月，可被 CB 逐条覆盖）、**`REVANCHISM_MONTHLY_DECAY = 0.833`** |
| 破停战 | `war_breaking_truce` = 稳定 50 + 厌战度 1；`on_truce_broken` 钩子 |
| **强制和平 / 威胁宣战** | 交互：`subject_enforce_peace`（宗主对附庸，需 `overlord_can_enforce_peace_on_subject = yes`）、`union_enforce_peace`（需 `modifier:union_allowed_enforce_peace = yes`）；AI 接受度权重表在 `ai_diplochance\00_ai_diplochance.txt` |
| 干预附庸战争 | `intervene_in_subject_war` / `intervene_in_subject_civil_war` / `intervene_in_union_civil_war`（界面 `confirm_intervene_war_popup.gui`、`select_war_to_intervene.gui`） |
| 战争热度下降 | 打久了自动降温：`WAR_ENTHUSIASM_TIME_*`、白和 3 年规则、`CALL_FOR_PEACE_*` |

**AI 接受度权重实例**（`ai_diplochance\00_ai_diplochance.txt`，这是"逐项加权"的写法，可直接照抄）：

```
enforce_peace = { base = -25   diplomatic_reputation = 3   actor_is_rival = -100
                  negative_opinion = -5   war_exhaustion = 5   low_manpower = 10
                  recipient_occupied_beseiged_locations = 20   rank_difference = -10
                  competing_power = -100   relative_strength = 25   royal_ties = 10
                  border_distance = -0.25   lacks_border = -25   trust_in_actor = 0.25 }
threaten_war  = { yesman = 10000   relative_strength = 40   capital = -100
                  location_value = -1000   recipient_at_war = 10   base = -20 }
requestpeace  = { enforced_demand = 1   surrendered_to_other = -100   desperation = -20
                  call_for_peace = 1   war_enthusiam = -100   base = -25
                  warscore = 1   months_at_war = 0.5   peaceoffer = -1
                  peaceoffer_seek_white_peace = 50 }
```

## 十一、脚本钩子与 AI 常量

**on_action（`common\on_action\_hardcoded.txt`）**：

| 钩子 | 行号 | scope |
|---|---|---|
| `on_battle_won` / `on_battle_lost` | 约 2790 / 2916 | `root = actor`、`scope:actor` = 胜方单位、`scope:target` = 败方单位；浮点 scope：`scope:killed_land_units`、`killed_navy_units`、`lost_land_units`、`lost_navy_units`、`scope:war_score` |
| `on_great_battle_won` / `on_great_battle_lost` | 约 2609 / 2747 | 同上（大决战） |
| `in_battle` | 488–491 | `root = character`（最高指挥官），每 tick 触发 |
| `on_siege_won` / `on_siege_lost` | 278 / 351 | — |
| **`on_war_declared`** | **2055** | `root = country`、`scope:actor` = 宣战国、`scope:recipient` = 被宣国、`scope:war` |
| **`on_truce_broken`** | **5648** | `root = actor`、`scope:recipient`、`scope:war` |

原版用例：科索沃战役变量、帖木儿击杀计数、特殊单位经验（`grant_special_unit_experience`）、百年战争局势在 `on_war_declared` 里开局（判断 FRA/ENG 是否已开战）。

**AI 战斗常量（`NAI`）**：`BATTLE_WIN_CHANCE_GENERAL_MIL_FACTOR = 0.25`（100 军事 ≈ +25% 等效兵力）、`INITIATIVE_COMBAT_STRENGTH_FACTOR = 0.025`、`AI_FLANKING_COMBAT_STRENGTH_FACTOR = 0.3`、`AI_RECOVER_MORALE_THRESHOLD = 66`、`AI_RETREAT_DICE_MORALE_THRESHOLD = 0.45`、`AI_RETREAT_FLANK_MORALE_THRESHOLD = 0.40`、`AI_REINFORCE_BATTLE_DISTANCE_LIMIT = 3`。
**AI 战争倾向**：`AI_WAR_EXHAUSTION_EXPANSION_PENALTY = 0.1`（每点厌战度降低开新战概率）、`SAFE_AMOUNT_OF_WAR_EXHAUSTION = 5`。

## 十二、Mod 改造建议

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 兵种数值 | `common\unit_types\`（改 `copy_from` 的时代模板）或 `unit_categories\` | 模板改一处、全兵种跟着变 |
| 新兵种 | 新建 unit_types 文件 + `category` 指向现有类别 | `copy_from` 继承模板 |
| 地形战斗修正 | `topography`/`vegetation` 的 `defender`、`local_frontage_allowed`、`combat = {}` | — |
| 战斗全局数值 | `NCombat` / `NUnit` defines（或 game_rules 覆盖） | — |
| 战斗事件 | `on_battle_won/lost`、`in_battle`（写同名块追加） | on_action 是合并语义 |
| 战争目标 | `common\wargoals\` | **`type` 是引擎枚举**，新增须复用现有 9 种之一 |
| **新宣战理由** | `common\casus_belli\<新文件>.txt` | 三段门 + `speed` 进度；`war_goal_type` 必须指向存在的 wargoal；可用本地日期字段覆盖全局 CB 时限 |
| **新和约条款** | `common\peace_treaties\<新文件>.txt` | 四作用域（winner/loser/war/target）；`category` 决定 UI 归栏；`are_targets_exclusive` 防与割地冲突 |
| **改宣战代价** | `common\prices\00_hardcoded.txt`（`declaring_war` / `war_no_cb` / `war_breaking_truce` / `war_good_relations`…） | 也可用 `war_*_cost_modifier` 修正单国调整 |
| **改战争分数与热情** | `NWar` 全块 + `NDiplomacy` 的 `WAR_ENTHUSIASM_*` / `PEACE_*` | 全局生效；`MAX_WAR_SCORE = 100` 改动会影响所有和约定价 |
| 改停战/复仇主义 | `TRUCE_YEARS` / `SCALED_TRUCE_YEARS` / `REVANCHISM_MONTHLY_DECAY` / `CASUS_BELLI_MONTHS` | CB 可逐条覆盖存续期 |
| 强制和平与威胁宣战 | `ai_diplochance\00_ai_diplochance.txt` 的权重表 + `subject_enforce_peace` / `union_enforce_peace` 交互 | 权重表是"逐项加权"写法，照抄即可 |
| 驻军/围城脚本 | `common\script_values\garrison.txt`、`NCombat` 围城段 | — |

**硬编码**：骰子结算、接敌判定（主动性）、伤害公式主体、侧翼包抄逻辑、撤退判定、**战争分数的合成权重**（可改常量，公式在引擎）、**和约 AI 的最终接受判定**、`wargoals` 的 9 种 `type` 枚举。

## 十三、中文检索键

**战斗**：`morale`（士气）、`frontage`（正面宽度）、`initiative`（主动性）、`combat_speed`（战斗速度）；战争目标名在各 `*_l_simp_chinese.yml` 的 `war_goal_*` 键。

**战争（`game_concepts_l_simp_chinese.yml`）**：`game_concept_casus_belli` **宣战理由**（:1080）、`game_concept_truce` **停战**（:1421）、`game_concept_war_exhaustion`（厌战度）、`game_concept_war_score`（战争分数）、`game_concept_war_enthusiasm`（战争热情）、`game_concept_call_to_arms` 召唤参战（:1015）、`game_concept_coalition` 包围网（:2043）、`game_concept_great_power` 列强（:1163）、`game_concept_hegemony` 霸权（:1171）。

**界面**（`in_game\gui\`）：**`declare_war_lateralview.gui`（103KB）**、**`peace_offer_view.gui`（74KB）**、`war_lateralview.gui`（75KB）、`battle_lateralview.gui`（81KB）、`battle_result.gui`（60KB）、**`shared\combat_tooltips.gui`（155KB，全库最大单文件之一）**、`shared\war_tooltips.gui`（32KB）、`wars_ledger.gui`、`war_viewer.gui`、`threaten_war.gui`、`confirm_intervene_war_popup.gui`、`select_war_to_intervene.gui`、`attribute_columns\war.gui`；局势/灾难专属面板 `panels\situation\hundred_years_war|hussite_wars|italian_wars|war_of_religions.gui`、`panels\disaster\*civil_war*.gui`。
