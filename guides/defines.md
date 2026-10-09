# Defines 体系（defines）

> **一句话**：引擎常量文件 00_defines.txt 的 N 块索引与实查数值（战斗、AI、POP、殖民等），并给出直接覆盖与游戏规则覆盖两种改法。
> **什么时候看**：要改任何引擎常量前，先用这张索引定位它属于哪个 N 块、大致行号与现值。
> **体量**：84 行 · 约 4 分钟通读

位置：`loading_screen\common\defines\`（三处）：
- `00_defines.txt`（2836 行）——全部引擎常量
- `graphic\00_graphics.txt`——图形常量
- `jomini\`——引擎层：00_tooltips.txt / adjacencies.txt / fog_of_war.txt / icons.txt / mapeditor.txt / rivers.txt / roads.txt

## 00_defines.txt 的 N 块索引（实查）

| 块 | 行区间（约） | 内容 |
|---|---|---|
| NGame | 1–27 | 时间（START_DATE "1337.4.1" / END_DATE "1836.12.31"）、HOUR_TICK=2、游戏速度表 |
| NCityAudio / NAlertAudio / NJominiMap / NGUI / NText | 28–124 | 音频/地图/GUI/文本 |
| NCountry | 125–333 | 国家级（稳定度、威望、厌战度等） |
| NMercenary | 334–360 | 雇佣兵（MERCENARY_PRISONER_MAX_MORALE=0.6） |
| **NUnit** | 361–444 | 单位：REGIMENT_SIZE=1000、LAND_MORALE=3.0、NAVAL_MORALE=3.0、MONTHLY_REINFORCE=0.25、MONTHLY_REPAIR=0.1、ATTRITION_LACK_OF_FOOD=5、LEVY_MAINTENANCE_FACTOR=0.01、ARMY_MOVEMENT_SPEED=0.13、NAVY_MOVEMENT_SPEED=0.5 |
| **NCombat** | 445–561 | 战斗：COMBAT_DICE_SIDE=10、COMBAT_BASE=5、COMBAT_MAX=15、COMBAT_DAMAGE_MULT=0.01、HOURS_PER_PHASE=5、MINIMUM_COMBAT_DURATION=24、MINIMUM_NAVAL_COMBAT_DURATION=72、STRAIT_CROSSING_DICE=-2、RIVER_CROSSING_DICE=-1、SEA_LANDING_DICE=-1、MAX_FRONTAGE_OVERSTACKING=1.25、LAND_LEVY_COMBAT_IMPACT=0.75、INITIATIVE_*、ASSAULT_*、SIEGE_*、DAYS_PER_SIEGE_PHASE=30、MAX_BREACH=3、TRADITION_GAIN_LAND=10 |
| **NAI** | **562–1592** | AI：**1031 行内约 839 个常量**（`AI_` 前缀 393 / `PEACE_` 59 / `CONQUER_` 27 / `HUNT_` 16 / `WAR_` 11 / `COLONY_` 9…）——战争平衡（`WAR_BALANCE_REGULARS_IMPORTANCE=3`、`WAR_BALANCE_LEVY_BOATS_IMPORTANCE=10`）、胜率（`BATTLE_WIN_CHANCE_SENSITIVITY=16`、`BATTLE_WIN_CHANCE_ENEMY_BIAS=1.1`、100 军事≈+25% 兵力）、扩张（`DEFAULT_CHANCE_OF_EXPANSION=0.25`/月、`AI_NUM_EXPANSION_TARGETS=20`、`CONQUER_DESIRE_*` 27 个）、和平（`PEACE_OFFER_*` 59 个）、军事 AI（`HUNT_ARMIES_` 16、`CARPET_SIEGE_` 4、撤退阈值）、AI 性能节拍（`AI_PERFORMANCE_*_MONTHS_BETWEEN_UPDATES`：内阁 36 / 改革 24 / 政策 6 / 特权 12）、储蓄模式（`AI_SAVING_MODE_*`）。**逐条自带英文注释，是全游戏最自解释的一段 defines；详见 `vanilla\vanilla-ai.md`** |
| NCulture / NCharacter / NPortrait | 1593–1741 | 文化；角色（MAX_ABILITY_VALUE=100、LEADER_COMBAT_TRAIT_GAIN_CHANCE=0.5、ASSIMILATION_* 同化修正表 1487–1506） |
| **NPop** | 1752–1824 | POP：MIN_REBEL_POP_SIZE_THRESHOLD=5、DISPLAY_SIZE=1000（显示=千）、BASE_SATISFACTION=0.2、主文化+0.25/接纳+0.10/容忍+0.05、pop_join_rebel_threshold 相关 REBEL_JOIN_MIN_THRESHOLD=0.05、POP_MINORITY_SIMILAR_THRESHOLD=4/8、YEARLY_POP_SATISFACTION_FROM_EVENTS_DECAY=0.01 |
| NEstate | 1825–1891 | 阶层 |
| **NLocation** | 1892–1956 | 地点：SUPPLY_LIMIT_OWNER=0.25、FORT_GARRISON_UPKEEP=2、MIN_FRONTAGE_AFTER_TERRAIN=2、NORMAL_LOCATION_SIZE=25、MONTHLY_CONTROL_DECAY=0.01 |
| NMarket / NEconomy | 1957–2207 | 市场/经济：MONTHLY_PRICE_CHANGE=0.05、GROWTH_FROM_FOOD_MULTIPLIER_MAX=10、LOCATION_DEPOPULATION_ALERT_MONTHS=60 |
| NDiplomacy / NWar | 2208–2702 | 外交/战争：DEFAULT_WARGOAL_BATTLESCORE_BONUS=3、WARSCORE_MAX_FROM_BATTLES=50、BATTLE_RESULT_SCALE=25、PARTICIPATION_SCORE_BATTLE=0.03 |
| NWorkOfArt / NDynasty / NColony | 2703–2775 | 艺术品/王朝/殖民 |
| NInternationalOrganization / NImperialCircle | 2776–2787 | 国际组织/帝国圈。⚠️ **`NInternationalOrganization` 全文件出现两次**：`2776–2779`（仅 `MONTHS_TO_DISBAND_FROM_BIAS = 3`）与 **`2830–2836`**（议会四常量 `PARLIAMENT_REQUEST_ISSUE_SUPPORT_NEEDED=0.5` / `PARLIAMENT_ISSUE_THRESHOLD=0.5` / `PARLIAMENT_DURATION_DAYS=365` / `PARLIAMENT_AVAILABLE_AGENDAS=5`，在文件最末、`NDisease` 之后）——**引用该段常量必须带行号** |
| **NWeather** | 2788–2798 | 天气：*_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY=125、PERCENT_DIFFERENCE_BETWEEN_GOOD_AND_BAD_SIDE_OF_CYCLONE=50 |
| NReligion / NSpreadable / NDisease | 2799–2829 | 宗教/传播/疾病 |

（⚠️ **行区间已按测试版 2026-10-01 build 重算**；beta 仍会再排，定稿前一律以 grep 定位为准。新增块 `NPortraitAssetShowcase`（L1742）与两处 `NInternationalOrganization`（L2776 / L2830）。）

**行号口径与体检结论（2026-09 全量复核）**：

- 上表与各篇引用的区间口径是 **`起始行 – 结束行`，结束行 = 该段 `}` 所在行**（个别处多算一行空行）。精确定位一律用 `grep -n '^NXxx = {'`，别信区间。
- **顶层段落共 30 个**（去重后；`NInternationalOrganization` 重名两次，见上表）。
- 复核通过：`NAI` **746 个常量** ✔（930 行）、`NCharacter` 起 1661 ✔、`NCombat` 起 445 ✔、`NWar` 起 2640 ✔、`NDiplomacy` 起 2208 ✔、`NColony` 起 2746 ✔、`NWeather` 起 2788 ✔、`NDisease` 起 2592 ✔、`NMarket` 起 1754 ✔、`NEconomy` 起 1839 ✔、`NImperialCircle` 起 2556 ✔。文件总行数 **2,609**。

## 修改方式

1. **直接覆盖**：mod 里建 `loading_screen\common\defines\00_defines.txt`，只写要改的块（同名块合并，键级覆盖）。
2. **游戏规则覆盖**（推荐，玩家可选）：`main_menu\common\game_rules\00_game_rules.txt` 的 setting 里写 `defines = { NCombat = { COMBAT_BASE = 6 } }`（格式见 `_game_rules.info`），并可配 flag 阻止成就。
3. **图形常量**：`graphic\00_graphics.txt`。

## 关键事实

- `HOUR_TICK = 2`：游戏按 2 小时 tick；战斗阶段 `HOURS_PER_PHASE = 5`。
- `START_DATE = "1337.4.1"`、`END_DATE = "1836.12.31"`：事件时间窗（dynamic_historical_event 的 from/to）应落在此区间。
- 修改 defines 会**改变存档兼容性与多人哈希**，平衡性改动优先用修正（static_modifiers/auto_modifiers）而非 defines。

## 散落在其它 N 块里的"政府/改革"常量（2026-09 补）

| 常量 | 值 | 所在块（行） | 含义 |
|---|---|---|---|
| `SOCIAL_VALUE_REQUIREMENT_FOR_REFORM` | **50** | NCountry（155） | 政府改革要求的社会价值位置 |
| `GOVERNMENT_REFORM_SOCIETAL_VALUE_REQUIREMENT_FORECAST_IN_MONTHS` / `..._CUTOFF_IN_MONTHS` | 12 / 24 | NCountry（101–102） | 界面"还要多久够格"的预告窗口 |
| `BUREAUCRACY_ENTRENCHMENT_YEARS_PER_PHASE` / `..._QUOTE_PER_PHASE` | **100** / 50 | NMercenary 之后（295–296） | 官僚部门"根深蒂固"每阶段年数与每阶段份额 |
| `ESTATE_SATISFACTION_BUREAUCRACY` | 0.01 | NEstate 区（230） | 官僚部门对阶层满意度的系数 |
| `DEFAULT_CURRENCY_GOVERNMENT_POWER` / `DEFAULT_CURRENCY_RIGHTEOUSNESS` | 95 / 90 | NDiplomacy 区（1971/1974） | 货币偏置基准值 |
| `HEGEMONY_LOST_MONTHS` / `HEGEMONY_GRACE_PERIOD` / `HEGEMONY_ACTIVE_UNTIL_REPLACED` / `HEGEMONY_MONTHLY_PROGRESS` | 120 / 12 / yes / 1 | 霸权区（2273–2276） | 霸权的失去、宽限与易主规则 |

详见 `vanilla\vanilla-government-and-reform.md`。

## 殖民与探索常量（2026-09 补）

| 常量 | 值 | 所在块（行） | 含义 |
|---|---|---|---|
| `BASE_COST` / `DISTANCE_COST_FACTOR` / `POPULATION_COST_FACTOR`（上限 3） | 1 / 0.0005 / 0.25 | **NColony（2746–2774）** | 特许殖民地的基础/距离/人口成本 |
| `SAME_AREA_COST_FACTOR` / `SAME_REGION_COST_FACTOR` | **−0.5** / −0.33 | NColony | 同地区 / 同大区**折扣** |
| `LOCATION_MAX_POP_ALLOWED_FOR_COLONY` / `DEVELOPMENT_AT_NEW_COLONY` | 50（=5 万）/ 5 | NColony | 可殖民地点人口上限 / 新殖民地发展度 |
| **`MAX_CHARTERS_IN_PROVINCE`** | **3** | NColony | 同一预设省份最多几个特许殖民地 |
| `EXPLORATION_CONSTRUCTION_TIME` / `CONQUISTADOR_CONSTRUCTION_TIME` | **270 天** / **90 天** | NColony | 探索任务 / 征服者工期 |
| `EXPLORATION_BASE_COST` / `_BASE_TIME` / `_DISTANCE_TIME` | 10 / 3 / 9 | NColony | 探索花费与时长 |
| `POWER_PROJECTION_SPREAD` / `EXPEL_TRIBAL_BENEFIT_DURATION` | 0.005 / 120 | NColony | 力量投射扩散 / 驱逐部落收益持续月数 |
| **`COLONIAL_POWER_PROJECTION_THRESHOLD_TO_START`** | **25** | NCountry（238） | **力量投射不到 25 不能开始殖民** |
| `COLONIAL_CHARTER_MIN/MAX_POPS_TO_TAKE_LOCATION` | 1 / 5（1000–5000 人） | NCountry（239–240） | 拿下一块地点所需当地人口 |
| `COLONIAL_MIGRATION_DISTANCE_FACTOR` | 0.0001 | NCountry（235） | 迁徙量随距离衰减 |
| `EXPLORER_EXTRA_LIFE` | 15 | NCharacter 区（1536） | 探险家的额外寿命 |
| `INTEL_THRESHOLD_LOCATION_MIGRATION` | 5 | NDiplomacy 区（2383） | 情报门槛 |

详见 `vanilla\vanilla-colonization-and-exploration.md`。
