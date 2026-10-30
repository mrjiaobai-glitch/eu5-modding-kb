# 征召兵与人力（levies & manpower）

> **一句话**：EU5 里**征召兵(levy)**和**人力(manpower)**是两套并行资源——征召兵是"召集 POP 直接凑成的军事单位(army_levy/navy_levy)、战力打七五折"，人力是"供养**正规军团(regiments)**的额度、正规更强"；征召规模由**士兵 POP 供人力 + 八个阶层各出一份额(×`*_estate_levy_size`) + 文化/全国百分比修正**层层缩放，用完按 240 个月恢复。
> **什么时候看**：调某国能拉多少兵、正规 vs 征召兵取舍、写 `raise_levies`/阶层"要额外征召兵"动作、或核对某个 levy 修正该挂 country/location/unit 哪一层时翻这篇。
> **体量**：105 行 · 约 5 分钟通读

来源：`loading_screen\common\defines\00_defines.txt`（`NCountry`/`NUnit`/`NCombat`/`NPop`/`NAI`）、`main_menu\common\modifier_type_definitions\00_modifier_types.txt`、`in_game\common\pop_types\00_default.txt`、`generic_actions\estate_emergency_actions.txt`、`effect/trigger_localization`、`main_menu\game_concepts`。**本体无 levy 专 readme**，全为 2026-10 实查；闭合公式属引擎侧（见 §七）。

## 一、先分清三样（本体概念原文）

| 概念 | 官方简中定义 |
| --- | --- |
| **征召兵** `game_concept_levy` | "征召兵是通过召集 [pop] 进行组织军事单位。可以是 [army_levy] 或 [navy_levy]。" |
| **人力** `game_concept_manpower` | "人力代表了一个国家可用于供养常备 army 的闲散人员的限度。训练和维持**正规 regiments** 需要消耗人力。通常这一军种比 army_levies **更具战斗力**。通过建造人力 buildings 或研发特定 advances 可提高获取人力的速度。" |
| **军团** `regiments` | 常备正规军基本编制；`REGIMENT_SIZE = 1000`（显示=千） |
| **海军征召兵** `navy_levy_desc` | "直接从 [pops] 征用的运输船；组建时带走该船 pops 与船员、解散时幸存者回到 pops；比从 sailors 招募的 navy **更脆弱**，应由配炮正规船护卫。" |

**关系**：正规军团消耗**人力**、战力满档；征召兵是从 POP 直接动员的**次等兵源**（陆战仅 75% 战力，见 §五）。二者不是一回事。

## 二、征召规模从哪来（三条来源线，全是"百分比缩放"）

本体**没有** EU4 那种"文化静态 `levies=`"。最终征召量 = 各来源基数 × 下列 percent 修正：

1. **阶层(estate)** —— 八个阶层各一个 `*_estate_levy_size`（**category=country, percent**）：`crown`(L1996)/`dhimmi`(L2069)/`nobles`(L2259)/`clergy`(L2342)/`burghers`(L2425)/`peasants`(L2508)/`tribes`(L14268)/`cossacks`(L14357)。被**革新/文化/骑士团**加值（实测 0.1–0.5，如 `advances\country_fra.txt:18 nobles_estate_levy_size=0.3`、`culture_group_german.txt:148 =0.5`、`chivalric_orders\01_historical_orders.txt =0.1`）。阶层能出多少还挂它的 **`estate_tax_base`** 与**满意度**（`ESTATE_LEVY_SATISFACTION_THRESHOLD = 0.25`）。
2. **士兵 POP → 人力** —— `pop_types\00_default.txt` 里**只有** `soldiers` 带 `local_manpower_modifier = 0.1`（人力主要来自士兵 POP）、`peasants` 带 `local_levy_recruitment_speed_modifier = 0.2`（平民加快征召速度）。其余 POP 不直接给征召。
3. **文化与全局缩放** —— `wrong_culture_levy_size`（**category=country**, percent，⚠️ 国家级不是逐省）：未接纳文化的征召打折；`local_levy_size_modifier`/`local_army_levy_size_modifier`/`local_navy_levy_size_modifier`（**location**, percent）；`global_levy_size_modifier`/`global_army_levy_size_modifier`（**country**, percent）。

**能不能出兵**由布尔开关决定：`always_allow_levies`/`always_allow_army_levies`(L10819)/`always_allow_navy_levies`(L10812)（country boolean）、`allow_paik_levies`(L13669, 印度)、`can_raise_crusader_levies`(L17186, **category=all**)。

## 三、关键常量（`00_defines.txt`，真值）

| 常量 | 值 | 含义 |
| --- | --- | --- |
| `MAX_MANPOWER_YEARS` | 5 | 人力储备上限（年数口径） |
| `MONTHLY_MANPOWER_PER_BASE_TAX_REFERENCE` | 0.04 | 每月人力参考（随基础税基） |
| `ARMY_LEVY_RECRUITMENT_TIME` / `NAVY_…` | 15 | 征召成军时间 |
| `LEVY_RECRUIT_THRESHOLD` | **0.20** | 一次可征召的比例闸（精确套用见 §七） |
| `LEVY_RECOVERY_MONTHS` | **240（20 年）** | 征召兵用完的恢复基准时长 |
| `ESTATE_LEVY_SATISFACTION_THRESHOLD` | **0.25** | 阶层满意度低于此→影响其征召贡献 |
| `LEVY_MAINTENANCE_FACTOR` | 0.01 | 征召兵维持费 |
| `LAND_LEVY_COMBAT_IMPACT` | **0.75** | 陆军征召兵战力＝正规的 75% |
| `NAVAL_LEVY_IMPACT` | 0.05 | 海军征召兵影响 |
| `ARMY_MANPOWER_LOSS_IMPACT` / `GARRISON_…` | 1.0 / 0.5 | 掉人力：野战全、围城驻军半 |
| `REGIMENT_SIZE` | 1000 | 一军团人数 |

## 四、玩家能动它的动作（本体用例）

- **效果**：`raise_levies`（**省**作用域，`province_effects.txt`）/ `raise_all_levies`、`raise_all_levies_instant`（国家）。瞬发版 `raise_levies_instant`。
- **触发器**：`has_levies`（军团作用域，`unit_triggers.txt`）；`has_raised_levies`（状态位，本体用 `= no` 卡"这次不能再抽"，如 `generic_actions\D008_fate_of_the_phoenix_actions.txt:677`、`events\italian_wars.txt:566`）。
- **`ask_for_extra_levies`（阶层应急动作，`estate_emergency_actions.txt:1`）**：向某阶层讨额外征召兵——`potential` 需有 `estate_tax_base > 0.01` 的阶层；**冷却 5 年**；`price:ask_for_extra_levies`（**防御战 ×0.5 半价**）；效果给国家挂 `estate_emergency_extra_levies` 修正 **12 个月**（`mode = add_and_extend`）。成本再被 `ask_for_extra_levies_cost_modifier`(country percent) 缩放；日本幕府另有 `increase_levies_from_shogun_court_cost_modifier`(L516)。
- **`appanage_request_levies`（`country_interactions\appanage_request_levies.txt`）**：宗主向封邑要兵。
- **恢复**：抽过的征召兵按 `LEVY_RECOVERY_MONTHS`(240)/`levy_recovery_modifier`(**category=unit**, percent) 回复。

## 五、levy / manpower 修正全表（含真实 category —— 挂错层会静默无效）

| 键 | 行 | category | 量纲 |
| --- | --- | --- | --- |
| `local_levy_size_modifier` | L6853 | **location** | percent |
| `global_levy_size_modifier` | L6860 | country | percent |
| `wrong_culture_levy_size` | L6846 | **country**（非逐省） | percent |
| `*_estate_levy_size` ×8 | 见 §二 | country | percent |
| `local_army_levy_size_modifier` / `local_navy_levy_size_modifier` | L6891 / L6905 | location | percent |
| `global_army_levy_size_modifier` / `global_navy_levy_size_modifier` | L6898 / L6912 | country | percent |
| `levy_combat_efficiency_modifier` | L6875 | **unit** | percent |
| `levy_recovery_modifier` | L6867 | **unit** | percent |
| `levy_maintenance_modifier` | L6883 | country | percent |
| `local_levy_recruitment_speed_modifier` / `global_…` | L6919 / L6926 | location / country | percent |
| `ask_for_extra_levies_cost_modifier` | L660 | country | percent |
| `always_allow_levies` / `_army_levies` / `_navy_levies` | L14459/10819/10812 | country | boolean |
| `can_raise_crusader_levies` | L17186 | **all** | boolean |
| `allow_paik_levies` | L13669 | country | boolean |

> ⚠️ 三个易挂错层的：`wrong_culture_levy_size` 是 **country**（不是逐省）；`levy_combat_efficiency_modifier` 与 `levy_recovery_modifier` 是 **unit**（要挂军团修正，不是国家/地点）。

## 六、与 manpower（人力池）的联动

人力是供养正规军团的额度：`MONTHLY_MANPOWER_PER_BASE_TAX_REFERENCE=0.04`（每月随基础税基回）、`MAX_MANPOWER_YEARS=5`（储备年数上限）、`ARMY_MANPOWER_LOSS_IMPACT=1.0`/`GARRISON…=0.5`（掉人力）。人力建筑/`advances` 提速获取。AI 侧：`WAR_BALANCE_MANPOWER_IMPORTANCE`、`AI_REQUIRED_MANPOWER_SURPLUS_MARGIN`、`AI_UNLOCK_LEVY_SCORE`、`DEFAULT_LEVY_ARMY_PREFERENCE`/`LEVY_ARMY_PREFERENCE_FACTOR`（AI 用征召 vs 正规的比例偏好）等（`NAI`）。

## 七、引擎侧未证（别当已证）

- 从 POP/阶层/税基到**最终征召团数的闭合公式**；`LEVY_RECRUIT_THRESHOLD = 0.20` 的精确套用位置（对税基？对 POP 上限？）。
- `local_manpower_modifier`/`*_estate_levy_size` 之间的**合成顺序**（相加/相乘/夹取）。
- `army_levy` 与 `regiments` 在编制层的换算（一"征召"折多少团）。
- `can_raise_crusader_levies` 的 `category=all` 具体涵盖哪些对象层。

## 审查要点

- **别把征召兵和人力混为一谈**：征召兵(`levy`)是次等动员兵、人力(`manpower`)是养正规军的池子；`land_levies`≠正规军团战力（`LAND_LEVY_COMBAT_IMPACT=0.75`）。
- **修正别挂错作用域**：`wrong_culture_levy_size`=country、`levy_combat_efficiency_modifier`/`levy_recovery_modifier`=unit、`local_*`=location——写进错误的 `add_country_modifier`/unit/location 块会静默失效。
- `raise_levies` 是**省**作用域、`raise_all_levies` 是**国家**；两者都有 `_instant` 瞬发版；用 `has_raised_levies` 防重复抽。
- 征召规模想放大优先动 `*_estate_levy_size`（阶层）与 `global_levy_size_modifier`（国家），而非逐省——注意阶层份额还受满意度阈值(0.25)与 `estate_tax_base` 约束。
- 恢复很慢（基准 240 月），`levy_recovery_modifier` 是 **unit 级**，不是国家一键加速。

## 中文检索键

| 中文 | 内部名 |
| --- | --- |
| 征召兵 / 陆军征召 / 海军征召 | `levy` / `army_levy` / `navy_levy`（`raise_levies`） |
| 人力 / 月度人力 | `manpower` / `monthly_manpower` / `local_manpower_modifier` |
| 军团（正规） | `regiments` / `REGIMENT_SIZE` |
| 征召规模 / 未接纳文化打折 | `*_levy_size_modifier` / `*_estate_levy_size` / `wrong_culture_levy_size` |
| 征召效率 / 恢复 / 维持 | `levy_combat_efficiency_modifier` / `levy_recovery_modifier` / `levy_maintenance_modifier` |
| 向阶层要额外征召兵 | `ask_for_extra_levies`（阶层应急）/ `estate_emergency_extra_levies` |
| 向附庸要兵 | `appanage_request_levies` |
| 允许征召开关 | `always_allow_levies` / `can_raise_crusader_levies` / `allow_paik_levies` |
