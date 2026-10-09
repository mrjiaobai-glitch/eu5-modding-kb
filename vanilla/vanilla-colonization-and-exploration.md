# 原版解析：殖民与探索（vanilla colonization & exploration）

> **一句话**：梳理探索三动作、特许殖民地与迁徙执行器、征服者、殖民领附属国与殖民地联邦 IO，并给出 `NColony` 常量与区域偏好、CB 清单。
> **什么时候看**：做殖民或探索内容、改迁徙与殖民领规则、加殖民 CB 或调探索偏好与成本时翻这篇。
> **体量**：341 行 · 约 16 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、总览：四层结构](#一总览四层结构)
- [二、探索（exploration）](#二探索exploration)
  - [2.1 前置与游戏规则](#21-前置与游戏规则)
  - [2.2 三个动作与"三选"](#22-三个动作与三选)
  - [2.3 价格、工期与常量](#23-价格工期与常量)
  - [2.4 "发现"是**区域级**的](#24-发现是区域级的)
  - [2.5 月度脉冲与事件](#25-月度脉冲与事件)
- [三、特许殖民地（colonial charter）](#三特许殖民地colonial-charter)
  - [3.1 创建与放弃](#31-创建与放弃)
  - [3.2 目标选择与引擎候选](#32-目标选择与引擎候选)
  - [3.3 成本常量（`NColony`，defines 2746–2774）](#33-成本常量ncolonydefines-27462774)
- [四、迁徙（migration）——真正的执行器](#四迁徙migration真正的执行器)
  - [4.1 引擎效果](#41-引擎效果)
  - [4.2 三个来源（原版全部用法）](#42-三个来源原版全部用法)
  - [4.3 迁徙吸引力与其他迁移工具](#43-迁徙吸引力与其他迁移工具)
- [五、征服者（conquistador）](#五征服者conquistador)
- [六、政治层：殖民领 · 联邦 · 革命](#六政治层殖民领--联邦--革命)
  - [6.1 殖民领 `colonial_nation`](#61-殖民领-colonial_nation)
  - [6.2 四种"殖民/海外"附属国对比（原版实测）](#62-四种殖民海外附属国对比原版实测)
  - [6.3 殖民地联邦 IO（`international_organizations\colonial_federation.txt`）](#63-殖民地联邦-iointernational_organizationscolonial_federationtxt)
  - [6.4 殖民革命局势（`situations\colonial_revolution.txt`）](#64-殖民革命局势situationscolonial_revolutiontxt)
- [七、战争与外交层](#七战争与外交层)
  - [7.1 五个殖民/探索 CB（`casus_belli\`）](#71-五个殖民探索-cbcasus_belli)
  - [7.2 四个国家交互与两个和平条款](#72-四个国家交互与两个和平条款)
- [八、AI 与探索/征服偏好（`area_preferences\`）](#八ai-与探索征服偏好area_preferences)
- [九、耦合表（跨系统接口）](#九耦合表跨系统接口)
- [十、Mod 改造建议（可改 vs 硬编码）](#十mod-改造建议可改-vs-硬编码)
- [十一、中文检索键](#十一中文检索键)

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| `common\generic_actions\colonial_charters.txt` | **2 个动作**（建/弃特许殖民地）+ 156 行 | 无 readme |
| `common\generic_actions\explorers.txt` | **3 个动作**（开始/换人/取消探索）+ 269 行 | 无 readme |
| `common\generic_actions\conquistadors.txt` | **1 个动作**（`create_conquistador`）+ 171 行 | 无 readme |
| `common\cabinet_actions\` | **4 个殖民内阁行动**（`send_people_to_the_colonies` / `settle_the_frontier` / `settle_tribesmen` / `encourage_migration`） | `readme.txt` |
| `common\on_action\` | **3 条脉冲**（`colonial_charter_monthly` / `exploration_mission_monthly` / `settle_the_frontier_monthly`） | `on_actions.info` |
| `common\subject_types\` | **4 种殖民/海外附属国**（`colonial_nation` / `conquistador` / `dominion` / `trade_company`） | `subject_types\readme.txt` |
| `common\situations\colonial_revolution.txt` | 1 个局势 / 207 行 | `situations\readme.txt` |
| `common\international_organizations\colonial_federation.txt` | 1 个 IO / 118 行 | `international_organizations\readme.txt` |
| `common\laws\24_colonial_federation.txt` | 联邦法律 / 6.5 KB | `laws\readme.txt` |
| `common\area_preferences\` | **83 条区域偏好**（conquest **58** + exploration **25**）/ 2 文件 16.9 KB | 无 readme（文件头注释是权威） |
| `common\casus_belli\` | **5 个殖民/探索 CB** | `casus_belli\readme.txt` |
| `common\country_interactions\` | **4 个**（`merge_colonies` / `start_war_in_colony` / `take_colony_for_debt` / `invite_settlers`） | `country_interactions\readme.txt` |
| `common\peace_treaties\` | **2 个**（`abandon_colonies` / `abandon_colonial_claim`） | `peace_treaties\readme.txt` |
| `loading_screen\common\defines\00_defines.txt` | **`NColony`（2746–2774）** + 散布在 NCountry/NAI 的相关常量 | 逐条英文注释 |
| `common\advances\` | `colonial_nations.txt`（**10 条殖民领专属革新**）+ discovery 时代探索链 | `advances\readme.txt` |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `exploration` | **探索** | 「由一名探险家领导的任务，目的是发现**海军范围**内一片未测绘的海洋或陆地」 |
| `explorer` | **探险家** | 「具有有利于探索的特质的角色」 |
| `conquistador` | **征服者** | 「被指派征服异域土地，建立我们的**新附属国**」——既是动作也是附属国类型 |
| `colonial_charter` | **特许殖民地** | 「在**国家想殖民整个省份**时建立；特许殖民地将**迁徙**…」 |
| `colonial_range` | **殖民范围** | 「我们可以殖民的最远距离」 |
| `colony` / `colonization` | 殖民地 / 殖民 | — |
| `migration` | **迁徙** | POP 觉得别处生活更好就会迁移 |
| `migration_attraction` | **迁徙吸引力** | 「生活在**负**迁徙吸引力地点的 POP 会想迁到**同一市场内**吸引力最高的地点」 |
| `exploration_progress` | 探索进度 | 每个区域要满足一项成本才算完全探索 |
| `exploration_expense` | 探索花费 | 探索任务有维护费，可被革新/法律/特权影响 |

## 一、总览：四层结构

```
① 探索层   exploration        探险家 + 目标【区域 area】+ 出发港 → 记录"已发现区域"
② 特许层   colonial_charter   按【预设省份 province_definition】建立的殖民地实体（一省最多 3 个）
③ 迁徙层   add_migration      真正干活的东西：一条定向人口流（from 预设省份 → to 预设省份）
④ 政治层   殖民领 / 征服者 / 自治领 / 贸易公司 / 殖民地联邦 IO / 殖民革命局势
           ↕ 战争与外交层：5 个 CB · 4 个国家交互 · 2 个和平条款
```

**关键认知**：玩家点的是"特许殖民地"和"探索"，但**改变人口分布的只有 `add_migration`**；探索只负责"点名"（把区域标为已发现），特许殖民地负责"圈地"，真正把人搬过去的是迁徙流。

## 二、探索（exploration）

### 2.1 前置与游戏规则

- 国家门槛：`modifier:may_explore = yes`（由革新/特权给）
- 关键革新：`allow_open_sea_exploration = yes`（开远洋，discovery 时代）；`colonial_range` 由革新给 **+600**（首个探索革新）与 **+1500**（开远洋）
- 脚本判定 `country_can_start_exploration_trigger`（`scripted_triggers\exploration_triggers.txt`）：AI 额外要求 `country_tax_base > 30`
- **游戏规则**：`ai_exploration_rule` 默认 **`western_exploration_only`**（只有首都位于欧洲的国家——或宗主在欧洲的附属国——能探索，且排除教宗与带 `warfare` 偏好标签者）；另有 `all_countries_explore`（谁都能探索）

### 2.2 三个动作与"三选"

`generic_actions\explorers.txt`：`start_exploration` / `change_explorer` / `cancel_exploration`，全部 `player_automated_category = exploration`。

`start_exploration` 要连续选**三样**（顺序有意义，注释说明第二、三个选择器依赖前者）：

| 选择 | `looking_for_a` | 候选来源 |
|---|---|---|
| ① 目标区域 | `area` | `source_flags = possible_exploration_areas`（引擎给） |
| ② 出发港 | `location` | `source_flags = possible_launch_locations`；列显示名称/市场/到目标区距离 |
| ③ 探险家 | `character` | `source = actor`，`visible = { is_valid_for_exploration = yes }`，AI 用 `best_explorers` 覆盖 |

`change_explorer` 用于给正在探索的区域换人（`source_flags = areas_being_explored`）；`cancel_exploration` 的 `ai_tick = never`（**AI 永不取消**）。

### 2.3 价格、工期与常量

- **价格分两种**：目标区与"己方或附属国所有"相邻且**非海域** → `price:start_exploration_land`；否则 `price:start_exploration_sea`
- ⚠️ 动作标了 **`should_execute_price = no`**——价格只用于界面显示，因为探索会生成一个**建造项目**（有自己的价格信息，可取消并恢复价格）
- 常量（`NColony`）：

| 常量 | 值 | 含义 |
|---|---|---|
| `EXPLORATION_CONSTRUCTION_TIME` | **270 天** | 探索任务工期 |
| `CONQUISTADOR_CONSTRUCTION_TIME` | **90 天** | 征服者准备期 |
| `EXPLORATION_BASE_COST` | 10 | 基础花费 |
| `EXPLORATION_BASE_TIME` / `EXPLORATION_DISTANCE_TIME` | 3 / 9 | 基础时长与每单位距离的时长 |
| `EXPLORER_EXTRA_LIFE` | 15 | 探险家的额外寿命（在 NCharacter 区） |

### 2.4 "发现"是**区域级**的

发现记录的粒度是 `area`（不是省份/地点），判定统一写成 `has_discovered_area = area:<名>`。脚本里已有具名组合判定（`scripted_triggers\exploration_triggers.txt`）：

- `discovered_any_in_americas_trigger`——美洲 **12 个海域区**任一（加拿大沿岸 / 东海岸海域 / 巴哈马 / 安的列斯 / 圭亚那 / 巴西 / 拉普拉塔 / 安第斯 / 中美洲 / 阿里多美洲 / 卡斯卡迪亚 / 阿拉斯加）
- `discovered_route_to_india`——德干 / 西印度 / 孟加拉海域 **+ 南非海岸 + 非洲之角**
- `discovered_any_in_moluccas_trigger`——摩鹿加
- `capital_in_old_world_trigger`——首都位于欧/亚/非

### 2.5 月度脉冲与事件

- `on_exploration_monthly_pulse`（root = exploration）：**39 个随机事件**（`exploration_monthly.1–39`，缺 13/14/15）+ `chance_to_happen = 0.2`
- `on_exploration_success`（root = country，`scope:target` = area，`scope:actor` = character）：累加变量 `num_of_explorations`（缺则设为 1），另有 `exploration_success.1`（权重 10 / 触发概率 5）
- 相关动作触发器：`num_explorations_including_in_construction`、`is_being_explored`、`has_assigned_explorer`、`is_valid_for_exploration`；效果：`start_exploration = { area  location  character  price  price_modifier }`、`change_explorer`、`cancel_area_exploration`

## 三、特许殖民地（colonial charter）

### 3.1 创建与放弃

`generic_actions\colonial_charters.txt` 只有两个动作：

| 动作 | 要点 |
|---|---|
| `create_colonial_charter` | `type = owncountry`；`potential`：`num_provinces > 0`、非叛军国、非 `country_type = building`、**`modifier:can_colonize = yes`**、AI 需 `ai_country_should_colonize = yes`；`allow`：`total_population >= 5`（按 1 = 1000 人，即 **5000 人**）；`price = price:create_colonial_charter`；`player_automated_category = colonies`；AI `ai_tick = monthly × 6` |
| `abandon_colonial_charter` | 选中已有的特许殖民地并放弃（`looking_for_a = colonial_charter`、`visible = { owner = scope:actor }`）；AI `ai_tick = monthly × 9` |

**互斥规则**（写在 `enabled` 里）：同一预设省份上若已有 `settle_the_frontier` 内阁行动在跑，就不能再建特许殖民地。

### 3.2 目标选择与引擎候选

- `looking_for_a = province_definition`（**预设省份**，不是运行时 `province`）+ **`source_flags = possible_colonial_charters`**——候选由引擎筛，脚本侧用触发器 `is_valid_colonial_charter` 再校验
- 预筛排序：`pre_evaluation_sort_value = { value = "scope:actor.colonial_charter_distance(root)"  multiply = -1 }`（**越近分越高**），`pre_evaluation_number_to_evaluate_fully = 10`（前 10 个全量评估）
- 引擎提供的效用触发器（写在 `ai_will_do` 里）：`colonial_charter_utility(target)`、`colonial_charter_distance`；AI 只对"税基 > 100 / 本身是 `colonial_nation` 附属国 / SWE·POR·KUR"这类国家给正分，其余 `-1000`

### 3.3 成本常量（`NColony`，defines 2746–2774）

| 常量 | 值 | 含义 |
|---|---|---|
| `BASE_COST` | 1 | 基础成本 |
| `DISTANCE_COST_FACTOR` | 0.0005 | 每单位距离加价 |
| `POPULATION_COST_FACTOR` / `POPULATION_COST_CAP` | 0.25 / 3 | 人口加价与上限 |
| `SAME_AREA_COST_FACTOR` / `SAME_REGION_COST_FACTOR` | **−0.5** / −0.33 | 同地区 / 同大区**折扣** |
| `LOCATION_MAX_POP_ALLOWED_FOR_COLONY` | **50**（=5 万人） | 地点人口超过此值不能再作殖民地 |
| `DEVELOPMENT_AT_NEW_COLONY` | 5 | 新殖民地的发展度 |
| `MAX_CHARTERS_IN_PROVINCE` | **3** | 同一预设省份最多 3 个特许殖民地 |
| `POWER_PROJECTION_SPREAD` | 0.005 | 力量投射扩散系数 |
| `EXPEL_TRIBAL_BENEFIT_DURATION` | 120 | 驱逐部落的收益持续（月） |

**门槛常量**（NCountry 区）：

| 常量 | 值 | 含义 |
|---|---|---|
| `COLONIAL_POWER_PROJECTION_THRESHOLD_TO_START` | **25** | **力量投射不到 25 不能开始殖民** |
| `COLONIAL_CHARTER_MIN_POPS_TO_TAKE_LOCATION` / `MAX_...` | 1 / 5 | 拿下一块地点需要当地人口在 1000–5000 之间 |
| `COLONIAL_CHARTER_PP_POP_TARGET_IMPACT` | 0.04 | 力量投射对人口目标的影响 |
| `COLONIAL_MIGRATION_DISTANCE_FACTOR` | 0.0001 | 迁徙量随距离衰减 |

## 四、迁徙（migration）——真正的执行器

### 4.1 引擎效果

```
add_migration = {  owner = <国家>  from = <预设省份>  to = <预设省份>  amount = <脚本值>  months = <月数，-1 = 永久> }
remove_migration = { owner  from  to }
```

### 4.2 三个来源（原版全部用法）

| 来源 | 类型 | 迁徙参数 | 结束条件 |
|---|---|---|---|
| `send_people_to_the_colonies`（内阁 · **adm**） | 三选：母国 `province` → 殖民领 `country`（限 `is_subject_type = colonial_nation`）→ 目标 `province` | `amount = 0.100 × (1 + 内阁 effective_skill)`；`months = -1`（永久） | 目标省人口 ≥ 源省人口，或源省任一地点人口 < 1 |
| `settle_the_frontier`（内阁 · **adm** · **10 年**） | 三选：目标 `province_definition`（`source_flags = possible_colonial_charters_bordering_only`）→ 接壤的 `province` → **廷臣角色**当殖民者 | `amount = 源省人口 × 0.001`（上限 1，注释原文 "so we at most move out 15%"）；`months = 120` | 4 个分支：完全实施（`settle_the_frontier.100`）/ 殖民者死亡或易主（`.101`）/ 别人占了该预设省份（`.102`）/ 中途放弃 |
| 特许殖民地自身 | — | `add_additional_migration` / `remove_additional_migration` 效果 | — |

⚠️ `settle_the_frontier` 的殖民者必须：成年、`is_courtier = yes`、**非统治者/继承人/摄政、忠诚、且没有 `settling_no_mans_land` 角色修正**——并且会拿到变量 **`settler_character_truthfulness = 5`**，注释原文："**If that drops at or below 0, they form their own country**"（忠诚掉光他就自己建国）。

### 4.3 迁徙吸引力与其他迁移工具

- 引擎负责结算：POP 会从**迁徙吸引力低**的地点流向**同一市场内**吸引力最高的地点；mod 侧的抓手是 `local_migration_attraction` / `global_migration_speed_modifier` 等修正
- `encourage_migration`（内阁 · adm）：给目的地 `local_migration_attraction = +1`、给本国 `global_migration_speed_modifier = 0.5`；AI 侧用 `ai_best_proximity_candidate` 收窄候选
- `settle_tribesmen`（内阁 · **mil**）：给 `local_tribal_promotion = 5.0`，把部落民转成定居人口；完成条件 = 该省部落民归零
- 相关修饰与常量：`MONTHLY_MIGRATION_UTILITY_MODIFIER = 10.0`（NAI）、`COLONIAL_MIGRATION_DISTANCE_FACTOR = 0.0001`、`INTEL_THRESHOLD_LOCATION_MIGRATION = 5`

## 五、征服者（conquistador）

`generic_actions\conquistadors.txt` 的 `create_conquistador`（`player_automated_category = colonies`、`price = price:recruit_conquistador`、同样 `should_execute_price = no`）：

- 国家门槛：`modifier:allow_conquistadors = yes`
- **选人极严**：`is_valid_for_exploration = yes` + **未婚** + **不属于王室阶层** + **天主教**
- **选目标区**：`within_colonial_range_of = scope:actor` + **`continent = continent:america`** + 已被本国发现 + 非沿海海域 + 区内至少有一个地点"有主、不属于本国、也不属于本国附属" + 该地点**主流宗教组与本国有别**
- 结果：建立一个 `conquistador` **附属国**（见 §6.2），而不是殖民地——这是"不靠特许殖民地"的第二条扩张路线

## 六、政治层：殖民领 · 联邦 · 革命

### 6.1 殖民领 `colonial_nation`

| 字段 | 值 |
|---|---|
| 标记 | **`is_colonial_subject = yes`**、`subject_pays = subject_pays_colonial`、`level = 1` |
| 吞并 | **`can_be_annexed = no`**（吞不掉）；`subject_can_cancel = no` / `overlord_can_cancel = no` |
| 探索 | `shares_exploration_with_overlord = yes` + `overlord_share_exploration = yes`（**与宗主共享探索**） |
| 外交 | `diplomatic_capacity_cost_scale = 0.5`、`has_limited_diplomacy = yes`、`allow_declaring_wars = { colonial_nation_can_declare_wars_trigger }`、`allow_subjects = no`、`can_change_rank = yes`、**`can_change_heir_selection = no`** |
| 收益 | `great_power_score_transfer = **0.75**`、`merchants_to_overlord_fraction = **0.33**`、`strength_vs_overlord = −0.05` |
| 政体 | 固定 `government = republic`；制度双向传播 `monthly_institution_spread_severe` |
| 主体修正 | `global_population_capacity_modifier +0.10`、`allow_rgo_slave_demand = yes`、`loyalty_to_overlord +20` |
| 建国条件 | `subject_creation_enabled` / `release_country_enabled`：目标省份须 `is_overseas_for_owner = yes` |
| `on_enable` 自动流程 | 用 `order_by = best_capital_for_colony` 选首都 → `add_policy = policy:colonial_representation_law_no` → `setup_colonial_nation = yes` → 农村聚落升为城镇 → `cc_setup_new_town = yes` → `set_capital` |

### 6.2 四种"殖民/海外"附属国对比（原版实测）

| | `colonial_nation` 殖民领 | `conquistador` 征服者 | `dominion` 自治领 | `trade_company` 贸易公司 |
|---|---|---|---|---|
| level | 1 | 1 | **3** | 1 |
| 纳贡 | `subject_pays_colonial` | `subject_pays_vassal` | `subject_pays_vassal` | `subject_pays_trade_company` |
| 宗主统治者兼任 | no | no | **yes** | no |
| 可否吞并 | **no** | 可（20 年 + 好感 150） | — | **no** |
| 外交容量系数 | 0.5 | **0.1** | 0.5 | 0.5 |
| 列强分转移 | 0.75 | 0.5 | 0.5 | 0.75 |
| 贸易优势转宗主 | 0.33 | — | — | — |
| 政体 | republic | — | — | republic |
| 特色修正 | 人口容量 +10%、允许 RGO 奴隶、忠诚 +20 | 军事战术 +0.5、**最大控制 +0.5**、整合速度 +1.0、异文化征召 +0.5、`blocked_from_peace = yes` | 内阁效率 **−40%**、立法效率 +0.1、禁止改教 | 出口效率（小贸易加成） |

（`conquistador` 的修正注释原文吐槽其最大控制加成："YES, this is insane, but they need to raise some levies asap"。）

### 6.3 殖民地联邦 IO（`international_organizations\colonial_federation.txt`）

- **创建条件**：`is_colonial_subject = yes` **且** `is_situation_active = situation:colonial_revolution`（只有殖民革命爆发时才能拉帮结派）
- 有领袖国（`leader_type = country`、`leader_change_method = score`、**每 96 个月**轮换）、成员之间**禁止开战**、被攻击的成员之间自动踢人
- 法律槽：`mutual_defense_law` / `mutual_offense_law` 直接绑 `assured_defense_policy` / `assured_offense_policy`
- 修正按"与领袖国的人口对比"缩放（`total_population / −0.75 ÷ 领袖人口 + 1`），领袖另有 `monthly_prestige +0.05`
- 加入条件：是殖民附属国、局势激活、**与发起者同文化或同宗教**、且**同一次大陆**
- `subject_limited = no`——**即使是外交受限的附属国也能创建**

### 6.4 殖民革命局势（`situations\colonial_revolution.txt`）

- `can_start`（全部满足）：`current_age = age_6_revolutions` + 存在一个殖民国满足——`country_type = location`、宗主首都在、**首都与宗主首都不同大陆**、**`has_policy = colonial_representation_law_no`**、`is_disloyal_subject = yes`、**`has_embraced_institution = institution:enlightenment`**
- **`can_end = always = no`**——只能开始，不能结束
- `on_start`：给所有殖民国发 `colonial_revolution.1`、给所有宗主发 `.2`
- `on_monthly`：殖民地侧 `random_list`（1 / 1 / 1 / **300 空转**，两个条件分支按内阁人数）；宗主侧 `random_list`（6 项各 1 / **99 空转**），条件涉及 `mercantilism_vs_free_trade < 0`（重商侧）、`absolutism_vs_liberalism < 0`（自由主义侧）、附属国 `subject_loyalty < 60`；另有 `absolutism_vs_liberalism > 99` 时的 1/999 事件
- `map_color`：宗主视角 = 宗主国色；殖民国视角 = 不忠诚/已加入联邦时用自己颜色、忠诚时用 `top_owner` 颜色

## 七、战争与外交层

### 7.1 五个殖民/探索 CB（`casus_belli\`）

| CB | 谁用 | 条件 | 战争目标 |
|---|---|---|---|
| `cb_push_back_colonizers` | **部落** | 目标非部落、是邻居、**`has_colonial_charters = yes`** | `superiority_push_back_colonizers`（烧掉欧洲殖民地） |
| `cb_force_migration` | **部落** | 目标也是部落 | `superiority_force_migration` |
| `cb_colony_war` | 通用 | `create_visible/enabled = always = no`（纯脚本/事件给） | `take_capital_colony_war` |
| `cb_exploration` | 通用 | 需 `has_advance = exploration` 或 `chartered_companies`；目标首都**不同次大陆**且**力量投射低于本国** | `superiority` |
| `cb_colonial_conflict` | 通用 | 同上（`always = no`，脚本给） | `naval` |

### 7.2 四个国家交互与两个和平条款

- `merge_colonies`（**合并殖民领**）：type = subject，`price = price:merge_colonies_price`，要求宗主有 ≥2 个殖民附属国、非战时；两阶段选择（被吸收方先收到接受/拒绝弹窗）
- `start_war_in_colony`：把战争局限在殖民地
- `take_colony_for_debt`：以债换殖民地
- `invite_settlers`：邀请移民（带 `ai_will_do`）
- 和平条款：`abandon_colonies`（放弃殖民地）、`abandon_colonial_claim`（放弃殖民宣称）

## 八、AI 与探索/征服偏好（`area_preferences\`）

**83 条区域偏好**（`conquest_preferences.txt` **58** + `exploration_preferences.txt` **25**），文件头注释即权威：

```
<pref key> = {
    preference_type = exploration / conquest     # 100% 出现
    modifier = 2                                  # 100% 出现；可小于 1 表示"降低欲望"
    area = <区域>          # 57%
    region = <大区>        # 51%
    continent / sub_continent / formable_country   # 各 5%
    consider_capital_region = yes                  # 1%
    allow = { <trigger> }                          # 1%
}
```

- **纯数据，没有 country / allowed 字段**；用 `add_area_preference = <key>` 在 `on_game_start`、任务或事件里指派给国家
- 原版实例：`england_explore_usa`（大西洋北区 + 加拿大/东海岸大区，`modifier = 2`）、`england_explore_india`（西非→好望角→德干海域一串）、`france_explore_usa`、`france_explore_great_plains`…——**各国历史探索路线就是这么写死的**
- `modifier` 取值跨度很大：**−0.5**（抑制）到 **100**（狂热），最常见是 3 / 5 / 10 / 15 / 25

## 九、耦合表（跨系统接口）

| 系统 | 接口 |
|---|---|
| 角色（角色篇） | 探险家/征服者都是**角色**（`is_valid_for_exploration`、`explorer` 类特质、`EXPLORER_EXTRA_LIFE = 15`）；`settle_the_frontier` 用廷臣当殖民者并带"忠诚度变量" |
| 内阁（角色篇 §六） | 4 个殖民行动全走内阁（`ability = adm` ×3、`mil` ×1），效率受 `country_cabinet_efficiency` 影响 |
| 疾病（天灾篇） | `COLONY_DISEASE_UTILITY_PENALTY = 50`、`AI_MIGRATION_THRESHOLD_FOR_DISEASED_LOCATIONS = 0.05`——AI 会避开疟疾区 |
| 贸易市场（贸易篇） | `AI_COLONIAL_EXPORT_SCORE_BONUS = 3`（海外种植园商品出口加分）、殖民领 33% 贸易优势上交 |
| 生产建筑（生产篇） | 殖民领拿到 `allow_rgo_slave_demand`；`DEVELOPMENT_AT_NEW_COLONY = 5` 决定新殖民地起点 |
| 战争（战斗篇） | 5 个殖民 CB + `take_capital_colony_war` 战争目标 + 2 个放弃类和平条款 |
| 附属国（外交篇） | 4 种殖民附属国的纳贡/外交容量/参战字段（§6.2） |
| 法律阶层（法律篇） | `colonial_representation_law_no` 政策是**殖民革命的可选门槛之一**；联邦法律 6.5 KB |
| 革新（科技篇） | `colonial_range`、`allow_open_sea_exploration`、`exploration_mission_speed`、`exploration_maintenance_efficiency`、`chartered_companies`、**殖民领专属革新 10 条**（`colonial_nations.txt`，`potential = { is_subject_type = colonial_nation }`） |
| AI（AI 篇） | `EXPLORATION_UTILITY_SEA = 6` / `_LAND = 10`、`AI_BASE_COLONY_UTILITY = 3`、`AI_PERFORMANCE_SAMPLE_SIZE = 3`、`ai_country_should_colonize` |
| 规则（game_rules） | `ai_colonisation_rule`（默认 `all_countries_colonize`）、`ai_exploration_rule`（默认 `western_exploration_only`） |

## 十、Mod 改造建议（可改 vs 硬编码）

| 想改的东西 | 正规做法 |
|---|---|
| 让某国探索得更远/更快 | 加 `colonial_range` / `exploration_mission_speed` 革新，或改 `NColony` 的 `EXPLORATION_*` 常量 |
| 让某国"历史上"该往哪探索 | `common\area_preferences\` 加一条偏好，再用 `add_area_preference` 指派（on_game_start / 任务 / 事件） |
| 放宽/收紧殖民门槛 | `COLONIAL_POWER_PROJECTION_THRESHOLD_TO_START`、`NColony` 各成本常量、`modifier:can_colonize` |
| 改殖民速度 | 内阁行动的 `add_migration.amount`（脚本值，可引用内阁能力）、`COLONIAL_MIGRATION_DISTANCE_FACTOR`、`local_migration_attraction` |
| 加"边疆开拓"剧本 | 照 `settle_the_frontier` 写内阁行动 + 配 `on_action` 月度脉冲（原版 5 个事件） |
| 加殖民附属国类型 | `common\subject_types\<新>.txt`，记得 `is_colonial_subject = yes` 才会被联邦/革命/合并等逻辑认领 |
| 改殖民革命 | `situations\colonial_revolution.txt`（门槛在 `can_start`）+ `generic_actions\colonial_revolution.txt`（11.7 KB 的行动与决议） |
| 改殖民地联邦 | `international_organizations\colonial_federation.txt` + `laws\24_colonial_federation.txt` |

**硬编码边界**：迁徙吸引力的实际计算与月度迁徙结算、探索进度与"已发现"记录的合成、建造项目式探索的取消/恢复、`setup_colonial_nation` 的建国流程、殖民革命的触发判定入口、`possible_colonial_charters` / `possible_exploration_areas` 这类引擎候选表。

## 十一、中文检索键

**概念**（`game_concepts_l_simp_chinese.yml`）：`game_concept_colonial_range` **殖民范围**（:1199）、**`game_concept_colonial_charter` 特许殖民地**（:1266，desc :1268「在**国家想殖民整个省份**时建立。特许殖民地将迁徙…」）、`game_concept_colonization` 殖民（:1272）、`game_concept_colony` 殖民地（:1274）、**`game_concept_migration_attraction` 迁徙吸引力**（:1412）、`game_concept_migration` 迁徙（:1414）、`game_concept_exploration` 探索（:1804）、`game_concept_exploration_success` 探索成功（:1808）、`game_concept_exploration_progress` 探索进度（:1809）、`game_concept_explorer` **探险家**（:1821）、**`game_concept_conquistador` 征服者**（:1824，desc「被指派征服异域土地，建立我们的新附属国」）、`game_concept_exploration_expense` 探索花费（:1878）、`game_concept_explorer_trait` 探险家特质（:1436）。

**界面**（`in_game\gui\`）：**`expansion_lateralview.gui`（殖民/探索主面板，含 `colony_item_card_at_province_select`、`exploration_item_card_at_location_select`、`explorer_character_context_card`、`conquistador_character_top` 四个选择器卡片）**、`panels\situation\colonial_revolution.gui`（12 KB）、`panels\organization\colonial_federation.gui`。

**关联字段档**：`fields\common-area_preferences.md`（区域偏好）、`common-generic_actions.md`、`common-cabinet_actions.md`、`common-subject_types.md`、`common-casus_belli.md`、`common-country_interactions.md`、`common-peace_treaties.md`、`common-situations.md`、`common-international_organizations.md`、`common-advances.md`；常量索引见 `guides\defines.md`，AI 权重见 `vanilla\vanilla-ai.md`。
