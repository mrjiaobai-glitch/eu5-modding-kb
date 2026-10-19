# 领土整合与核心（integration status / cores）

> **一句话**：一个**有主权的地点**在本国行政体系里有四种整合状态、排成一条链 `殖民地 colonized → 未整合 conquered → 已整合 integrated → 核心 core`；靠内阁动作 `integrate_province/area` 推进 `integration_progress`，`integrated → core` 卡在文化与宗教的 `CORE_THRESHOLD = 0.5`。核心分两类：**文化转正的核心（永久）**与**效果 `add_core` 给的核心（带 100 年倒计时，到期时文化仍不对就消失）**。
> **什么时候看**：做领土/占领玩法、调整合速度与核心门槛、用 `integration_level`/`is_core_of` 写触发器、用 `add_core` 造自定义核心获取途径，或区分"地点整合"与"附庸吞并"这两套易混系统时翻这篇。
> **体量**：208 行 · 约 10 分钟通读

来源：`loading_screen\common\defines\00_defines.txt`（`NLocation` / `NCulture` / `NReligion` / `NAI` / `NDiplomacy` / `NWar`）、`in_game\common\cabinet_actions\integrate_*.txt`、`trigger_localization\location_triggers.txt`、`effect_localization\location_effects.txt`、`main_menu\common\static_modifiers\integration.txt`、`main_menu\gui\messagetypes.txt`、`in_game\gui\location_window.gui`、`main_menu\common\modifier_type_definitions\00_modifier_types.txt`、`main_menu\game_concepts`。**本体无 readme**，全为 2026-10 实查。合成公式（进度阈值等）属引擎侧，见末节。§八 的到期结论为 **B 级（运行时存档实测）**，解码法见 `tools\save-decoding.md`。

## 术语对照

| 中文（本体词条） | 内部名 | 是什么 |
| --- | --- | --- |
| 整合状态 | `game_concept_integration_status` | 地点在本国行政体系里的整合程度 |
| 殖民地 | `colonized` | 四态之一（海外/殖民定居所得） |
| 未整合 | `conquered` | 四态之一（新征服、尚未消化） |
| 已整合 | `integrated` | 四态之一（消化完成，但还不是核心） |
| 核心 | `core` / `game_concept_core` | 四态顶端，权利最全 |
| 整合进度 | `integration_progress` | 0–1 的数值（内阁地图着色 `lerp factor` 用它） |

> ⚠️ **两套"整合/吞并"别混**：本篇是**地点级** `integration_level`（province/location 的消化）。附庸变成本国领土走的是**另一套**——附庸 `can_be_annexed` / `annexation_speed`，见 `fields\common-subject_types.md`「附庸吞并」。二者数值、修正、触发器都不同名。

## 一、四态链

```
colonized（殖民地） ─┐
                     ├─→  conquered（未整合） ──内閣整合──→  integrated（已整合） ──文化&宗教达标──→  core（核心）
（新征服的本土）  ───┘
```

- `integration_level` 既是**地点字段**也是**地点触发器**（`location_triggers.txt:257`，`INTEGRATION_LEVEL_TRIGGER`），取值为上述四个词。
- 全库使用频次：`core` ×241、`conquered` ×66、`integrated` ×65、`colonized` ×7。
- `game_concept_integration_status_desc` 原话：**拥有主权的所有地点都属于以下四种状态之一**。

### 各态含义与门槛（依 `integration_status` 概念描述 + 实测常量）

| 态 | 怎么进入 | 与上一态的关键差 |
| --- | --- | --- |
| `colonized` | 殖民/割附路径（`on_colonize_annex` / `on_colonize_annexed`，`on_action\_hardcoded.txt:722/1021`）；海外定居所得 | 与 `conquered` 平级、非"征服消化"路线 |
| `conquered`（未整合） | 新征服/割让的本土初始态 | 会随时间/失控**衰减**（见 §四） |
| `integrated`（已整合） | 经内阁 `integrate_province`/`integrate_area` 把 `integration_progress` 推满 | 权利提升，但**还不能算核心** |
| `core`（核心） | `integrated` + **主流/已接纳文化 POP 与国教占比各达 `CORE_THRESHOLD = 0.5`** | 概念原话：未整合可经内阁动作整合，但**成为核心需要主流或已接纳文化 POP 占很大比例** |

## 二、脚本判定与切换（可 mod 的接口）

| 用途 | 内部名 | 作用域 / 出处 |
| --- | --- | --- |
| 判地点处于哪态 | `integration_level = <colonized\|conquered\|integrated\|core>` | 地点，`location_triggers.txt:257` |
| 判是否本国核心 | `is_core_of = <country>` | 地点，`location_triggers.txt:276` |
| 判国家是否正在整合 | `is_integrating` | 国家，`country_triggers.txt:2668` |
| 改整合态 | `change_integration_level = <integrated\|core\|…>` | 效果，`location_effects.txt`（`_hardcoded.txt:2209/4973` 在用） |
| 直接加/去核心 | `add_core` / `remove_core` | **地点作用域**，`effect_localization\location_effects.txt:192` / `:199` |
| 以国家为主语给核心 | `add_location_as_core` | **国家作用域**（目标是地点），`effect_localization\country_effects.txt:1329` |
| 释放非核心地 / 用我方境内的核心建国 | `release_non_cores` / `create_country_from_cores_in_our_locations` | 国家，同上 `:2454` / `:1572` |
| 判国家是否有某核心 | `has_core` | 国家，`trigger_localization\country_triggers.txt:1964` |
| 读整合进度 | `integration_progress`（脚本值） / `province_average_integration` | 内阁着色/进度条用 |

> ⚠️ 效果侧**没有**任何"读/改核心到期时间"的接口：`on_action\on_actions.info` 搜 `core` **0 命中**，也没有 `is_scripted_core` / `core_expiry` 类触发器 ⇒ 到期判定与清除全在引擎侧，脚本只能"给"和"删"。唯一能**看见**计时器的是界面：`in_game\gui\location_window.gui:8244` `Location.ShouldShowScriptedCoreExpiry(Country.Self)` ＋ `:8246` `Location.GetScriptedCoreExpiryDate(Country.Self)`（词条 `LOC_CORE_TT_EXPIRES: "Expires on $DATE|V$"`，`interfaces_l_english.yml:505`）⇒ **地点面板悬停核心图标**就是免费探针，不必等 100 年。

## 三、内阁动作（执行器）

**`integrate_province`**（`cabinet_actions\integrate_province.txt`，71 行，基础款）：`ability = mil`、`allow_multiple = yes`；`potential` = 有地点且**尚未**取得 `integrate_area_advance`；选择列表只列含 `integration_level = conquered` 地点的省（列头 `integration`）；进行中给省挂 `province_modifier { province_integration_speed = 1.0 }`；进度取 `province_average_integration`；`is_finished` = 省内**所有**地点都已 `integrated`/`core`/`colonized`。

**`integrate_area`**（`integrate_area.txt`，77 行，进阶款）：同族但**要求先有革新 `integrate_area_advance`**（`advances\3_cabinet_actions_unlocks.txt`），按 **area 批量**处理（`select_trigger` 同时接受 `integration_level = conquered` 与 `colonized`）。

> 二者是"分配内阁成员去消化占领地"的玩法；`integrate_province` 是未解锁进阶革新时的单体动作，`integrate_area` 是解锁后的批量版。

## 四、速度常量与衰减（`NLocation`，真值）

| 常量 | 值 | defines 注释 |
| --- | --- | --- |
| `CONQUERED_LOCATION_INTEGRATION_DECAY` | **−0.1 / 月** | 未整合地点**不在征服者控制下**时每月掉的整合度 |
| `CONQUERED_OCCUPIED_LOCATION_INTEGRATION_DECAY` | **−0.025 / 月** | 同样失控、但**被占领**时掉得慢 |
| `CONQUERED_LOCATION_INTEGRATION_REBELLION_HIT` | **−10** | 一发生叛乱，地点整合度**立即**扣这么多 |
| `MAX_NUM_LOCATIONS_FOR_INTEGRATE_SLOWDOWN` | 5 | 一次整合超过 5 个地点则放缓 |
| `CORE_THRESHOLD` | **0.5** | `NCulture:1630` 与 `NReligion:2803` 各一处——升核心要求主流/已接纳文化 POP 与国教占比各过半 |
| `SCRIPTED_CORE_DURATION_YEARS` | 100 | 脚本 `add_core` 给的核心持续年数；**到期时文化仍不对才真的清除**（实测见 §8.4） |
| `REBEL_NON_INTEGRATED_FACTOR` | 0.01（`NCountry`） | 未整合土地对叛乱的放大系数 |

## 五、四个整合速度修正（作用域实测）

| 修正 | 行 | 作用域 |
| --- | --- | --- |
| `province_integration_speed` | L6253 | `province` |
| `local_integration_speed` | L6259 | `location`（percent） |
| `local_integration_speed_modifier` | L6265 | `location`（percent） |
| `global_integration_speed_modifier` | L6272 | `country`（percent） |

## 六、AI 侧（`NAI`，真值）

`AI_MAX_PROVINCES_TO_INTEGRATE_PER_CABINET_MEMBER = 4`（每个内阁成员最多整合 4 省）· `AI_INTEGRATION_IMPORTANCE_MULT = 0.075` · `AI_INTEGRATION_ACCEPTED_CULTURE_MULT = 2`（已接纳文化地区更值得整合）· `AI_INTEGRATION_POPULATION_SHARE_MULT = 0.01` · `AI_INTEGRATION_TAXBASE_SHARE_MULT = 0.01` · `AI_UNINTEGRATED_LAND_EXPANSION_PENALTY = 2`（有未整合地就压低进一步扩张欲）· `AI_GLOBAL_INTEGRATION_SPEED_UTILITY = 1` · 和平侧 `PEACE_MAKE_SUBJECT_TOO_MUCH_TO_INTEGRATE = 1.5`、`PEACE_DEAL_INTEGRATION_LIMIT_MULTIPLIER = 2` · 情报侧 `INTEL_THRESHOLD_LOCATION_INTEGRATION = 5`。

## 七、相邻系统（耦合点）

- **文化/宗教**：`integrated → core` 由 `CORE_THRESHOLD`（文化 0.5、宗教 0.5）把关 → 见 `vanilla\vanilla-culture-and-religion.md` 与本篇 §一。
- **殖民**：`colonized` 态来自殖民/割附；与 `vanilla\vanilla-colonization-and-exploration.md`、`subject_types\colonial_nation` 相接。
- **和平条款**：`PEACE_RETURN_CORE_MULT` / `PEACE_REVOKE_CORE_MULT` / `ANTAGONISM_BASE_*_GREAT_POWER_SCORE` 等按"核心"计战损与恶名（`NWar` / `NDiplomacy`）。
- **卖核心**：`SELL_CORE_STABILITY_COST` / `SELL_CORE_REBEL_MODIFIER_YEARS`（`NDiplomacy`）。
- **附庸吞并（≠ 本篇）**：省/国消化走 `integration_level`；**附庸变领土**走 `can_be_annexed`/`annexation_speed`，见 `fields\common-subject_types.md`。

## 八、核心的两类来源与"掉核心"的三条路径（2026-10 存档实测）

### 8.1 四档状态各自的静态修正（`main_menu\common\static_modifiers\integration.txt`，40 行）

| 块 | 行 | 关键键（数值 = 增量，不是绝对值） |
| --- | --- | --- |
| `integration_none` | 1–5 | 只有 `game_data.category = location` ⇒ 状态枚举**可以**有零效果键的块，但**别推成"零键的修正也能显示"**（见下方 ⚠️，实测踩到） |
| `integration_conquered` | 7–16 | `local_max_control −0.10`、`local_separatism 0.5`、同化/改宗各 `−0.02`、`local_garrison_growth −0.005` |
| `integration_colonized` | 17–22 | `local_monthly_control 0.001` |
| `integration_integrated` | 23–30 | `local_separatism **+0.1**`、`local_pop_assimilation_speed −0.01`、`local_max_control +0.05` |
| `integration_core` | 31–39 | `local_monthly_control 0.001`、`local_max_control **+0.20**`、`growth_is_primary_culture = yes`、`owner_gets_vision_when_occupied = yes` |

- ⚠️ `integration_integrated` **自带 `local_separatism = +0.1`** ⇒ 拿"分离度 ≤ 0"当"已整合地点"的门槛会**永远不成立且不报错**（实测坑，见 `pitfalls.md`）。
- ⚠️ `growth_is_primary_culture = yes` 就是核心 tooltip 里"仅允许主流/已接纳文化人口增长"那一行的来源；删掉它 = 核心地所有文化都能自然增长（界面会少一行）。
- ⚠️ **⚠️ 更正（2026-10-12 实测）：不要照 `integration_none` 做"零效果键的显示块"**。
  `integration_none` 是**状态枚举的内部标记**，它本来就不需要在修正列表里渲染成一行。
  反例（本次踩到）：自定义 `zzz_core_progress` 块只有 `game_data`，用
  `add_location_modifier = { modifier = zzz_core_progress size = {…} }` 挂上——
  **存档侧确实挂上了**（该键在值位命中数千次），图标也注册了，但**地点面板整条不出现**；
  面板是按"这个块有哪些修正键"来列行的，没键就没东西可列。
  ⇒ **闭环已证实**（2026-10-12）：给块加一个真键 `local_max_control = 0.01` 之后，
     "核心进度"这一行立刻出现在地点面板，并显示"最大控制力 +N.00%"。
  对照：本体的 `isolating_cities`（`static_modifiers\location.txt:917-925`）带 **4 个真实效果键**；
  它的 `size = {…}`（`generic_actions\black_death.txt:149-167`）是**把值乘进块内键**
  （`size` = 乘数，实机确认：好感达标的地点逐年 +1%），不是"没有键也能显示"。
  ⇒ 想做纯显示读数必须给块至少一个真键；而既然那个键是**真效果**，就要处理
  "进度累计值在达成瞬间被终态值接住"的问题（详见 `pitfalls.md` §四 的"奖励悬崖"条）。
- ⚠️ `static_modifiers` 是**注册语义**：换文件名不会覆盖同名块 ⇒ 改这几块要么**同名整份替换**（并把该文件当作"原版副本"维护，本体更新时必须重新 diff），要么用 INJECT。

### 8.2 两类核心

| 来路 | 触发 | 到期计时 |
| --- | --- | --- |
| **文化转正**（原生） | `integrated` + 主流/已接纳文化 POP 占比（与国教占比）达 `CORE_THRESHOLD = 0.5` ⇒ 引擎自动给核心，弹 `NEW_CORE` | **无**，永久 |
| **效果授予** | `add_core` / `add_location_as_core`（事件、决策、外交行动、和约、局势） | **有**：`SCRIPTED_CORE_DURATION_YEARS = 100` |

存档侧是**逐条核心记账**的（地点记录内）：

```
cores = { <国家id> … }                                    ← 当前核心持有者
scripted_cores = { { scripted_core_country = <国家id>
                     scripted_core_grant_date = <日期> } … } ← 存"授予日"，不是到期日
```
日期整数 = 自 -5000 年起的小时数（`((年+5000)*365 + 儒略日)*24`，不计闰年）。`1337.4.1`（= `55514280`）是历史核心的**哨兵值**，不是真实年份。

**本体自己给效果核心的位置**（可当写法范本；注意 `country_effects.txt:794-795` 那对行）：
`scripted_effects\country_effects.txt:794`（`add_core = prev` **紧跟** `:795 change_integration_level = core`）、`:822`；`formable_countries\00_formable_countries.txt:1119`/`:1234`；`decisions\mallorca_decisions.txt:86`/`:99`；`country_interactions\sell_corsica.txt:69`/`:85`；`generic_actions\golden_age_of_piracy.txt:521`；`religious_effects.txt:510-511`（`remove_core = owner` + `add_core = c:PAP`）；`situation_effects.txt:185`。
⇒ **`add_core` 与 `change_integration_level = core` 要成对写**：只写一行会出现"地点状态与国家核心列表不一致"的静默半授予。

### 8.3 掉核心的三条独立路径

| 路径 | 条件（本体文案原文） | 消息块 | 时效 |
| --- | --- | --- | --- |
| `LOST_CORE` | 该地主导文化在我国**连 `tolerated_culture` 都不是** | `messagetypes.txt:831`，文案 `messages_l_english.yml:1715-1723`（中文 `messages_l_simp_chinese.yml:1483-1492`） | **立刻** |
| `LOST_CORE_DECLINING` | 受认可文化 POP **跌破阈值**（`NCulture.CORE_THRESHOLD`） | `messagetypes.txt:841`，文案 `:1725-1733` | **立刻** |
| **效果核心到期** | 授予满 100 年 **且届时文化仍不对** | 无专属消息块 | **100 年** |

`NEW_CORES`/`NEW_CORE`（`messagetypes.txt:801`/`:811`）与 `LOST_CORES`（`:821`，多块地同时掉）也在同一族。

### 8.4 实测：到期规则**真的兑现**，且按文化筛选

方法：解码一份 1444→1657（213 年）的玩家存档（只读，法见 `tools\save-decoding.md`）。全档 `scripted_core_grant_date` 命中 9,927 条，其中 9,837 条可按地点侧布局核对。按【授予年 × 该地点 `cultural_unity`】分三组各随机抽 45 条：

| 组 | 定义 | 总体 | `cultural_unity < 0.5` |
| --- | --- | --- | --- |
| A | 效果授予、**已跨过 100 年** | 1,129 | **2%**（1/45，最低 0.472） |
| B | 效果授予、**未到 100 年** | 1,294 | **16%**（7/45，最低 0.292） |
| C | 历史/哨兵（授予年 <1444） | 7,414 | **0%**（45/45，最低 0.555） |

⇒ 结论：文化不对的效果核心**撑不过 100 年**；到期前文化爬上来的即"转正"，此后与原生核心一样不受计时影响。
⚠️ **方法学**：只统计"老核心还在不在"会得出**相反**结论——还在的那些恰恰是文化已转正的（幸存者偏差）。判据必须是与"未到期的同期群体"做**分组对照**。

## 审查要点

- 别把**地点整合**（`integration_level` / `province_integration_speed`）和**附庸吞并**（`annexation_speed` / `can_be_annexed`）混为一谈——同名"整合/吞并"、两套数值两套触发器。
- `integration_level` 的取值只有 `colonized / conquered / integrated / core` 四个词；写别的（如 `territory`/`state`）静默不匹配。
- 升 `core` 不是纯内阁动作能强推的——`CORE_THRESHOLD = 0.5` 是文化+宗教双门槛；人口结构不达标，`integrated` 就卡在非核心。
- `add_core` 脚本给的核心有期限：`SCRIPTED_CORE_DURATION_YEARS = 100`，不是永久——长局/事件给的核心要留意过期。
  **已实测确认会兑现**（§8.4）：跨过 100 年线的效果核心里，文化不对的那批从 16% 塌到 2%。⇒ 若你的 mod 用效果**不看文化**地发核心，那些核心就是"100 年租约"；要么接受并写进解释文本，要么覆盖 define（会一并作用于原版那 6 处效果核心）。
- **验证到期不必等 100 年**：地点面板悬停核心图标，看有没有"Expires on …/到期于…"那一行（由 `ShouldShowScriptedCoreExpiry` 控制）——有行 = 该核心带计时，无行 = 已转正。
- 别把 `integration_progress` 或 `local_separatism` 当自定义仪表：前者驱动地图着色（`integrate_province.txt:36-42` 的 `lerp factor`），后者被 `integration_integrated` 自身 +0.1 污染。
- `game_concept_core_desc` 的**第四句（效果核心 100 年消失）在各语言里详略不一**：英文 `game_concepts_l_english.yml:1612` 有，**中文 `…simp_chinese.yml:1328` 根本没有**。做覆盖文本时**逐语言核对**，不要假设本体某语言有全四句；改机制更要同步改这些散文（数值 tooltip 自动读修正，只有成因散文会过期）。
- 失控就掉：被夺回/未拥有时 `conquered` 每月 −0.1（被占 −0.025）、叛乱立即 −10——只加速度修正而不控住地盘是白干。

## 引擎侧未证（别当已证）

- `integration_progress` 到多少自动升下一态的确切阈值、以及能否跳级。
- `province_average_integration` 与各速度/衰减常量的**合成公式**（相加/相乘/上限）。
- `colonized` 与 `conquered` 之间是否可互转、各自能享哪些权利的全表（概念只给了概述）。
- `integrate_area_advance` 具体在哪个时代/类别解锁（本篇只确认革新键存在于 `3_cabinet_actions_unlocks.txt`）。
- ~~效果核心到期是否真的执行~~ —— **已由存档实测定性**（§8.4，B 级）：会执行，且只在到期时文化仍不对才清除；但"文化正确"的**精确判据**（`cultural_unity` 数值 vs "主导文化是否主流/已接纳/容忍"）仍未钉死，A 组里留有 unity 0.472 的边缘样本。

## 中文检索键

| 中文 | 内部名 |
| --- | --- |
| 整合状态 / 整合程度 | `integration_status` / `integration_level` |
| 殖民地 / 未整合 / 已整合 / 核心 | `colonized` / `conquered` / `integrated` / `core` |
| 整合进度 | `integration_progress` / `province_average_integration` |
| 整合（动作/效果） | `integrate_province` / `integrate_area` / `change_integration_level` |
| 加核心 / 去核心 | `add_core` / `remove_core` / `is_core_of` |
| 整合速度 | `province_integration_speed` / `local_integration_speed` / `global_integration_speed_modifier` |
| 核心门槛（文化/宗教过半） | `CORE_THRESHOLD`（0.5） |
| 失控掉整合 / 叛乱重创 | `CONQUERED_LOCATION_INTEGRATION_DECAY` / `…_REBELLION_HIT` |
| 卖核心 / 退还核心 / 废除核心 | `SELL_CORE_*` / `PEACE_RETURN_CORE_MULT` / `PEACE_REVOKE_CORE_MULT` |
| 效果核心（带 100 年计时，存档侧） | `scripted_cores` / `scripted_core_country` / `scripted_core_grant_date` |
| 掉核心消息 | `LOST_CORE` / `LOST_CORE_DECLINING` / `LOST_CORES` / `NEW_CORE` |
| 核心到期显示 | `ShouldShowScriptedCoreExpiry` / `GetScriptedCoreExpiryDate` / `LOC_CORE_TT_EXPIRES` |
| 状态静态修正块 | `static_modifiers\integration.txt`：`integration_none` / `_conquered` / `_colonized` / `_integrated` / `_core` |
