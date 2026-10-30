# 军队移动速度（army movement）

> **一句话**：移速是**军团（army/stack）级**属性、不是每种兵种一个基速——由"基速（defines）× 军团级速度修正（`category=unit`）÷ 地点级移动成本（`category=location`）"三层合成，再叠加规模拖慢、低士气锁半速、道路与气候惩罚。
> **什么时候看**：调某支/某类军队的推进快慢、做地形或道路相关玩法、核对"为什么这支军走不动"时翻这篇。兵种战斗值请看 `fields\common-unit_types.md`。
> **体量**：107 行 · 约 5 分钟通读

来源：`loading_screen\common\defines\00_defines.txt`（`NUnit` 块）、`main_menu\common\modifier_type_definitions\00_modifier_types.txt`、`main_menu\common\static_modifiers\location.txt` + `in_game\common\` 全库实查。**基础地形成本烤在地图二进制里、不在可编辑文本，故本篇只覆盖文本可见项；合成算式属引擎侧（见末节）。**

## 术语对照

| 本文说法 | 内部名 | 说明 |
| --- | --- | --- |
| 军团 /  stack | `army` | 移速挂在军团上，一个军团可含多个团（regiment） |
| 团 / 编制单位 | `regiment` | `REGIMENT_SIZE = 1000`；兵种文件不含移速字段 |
| 移动速度 | `game_concept_movement_speed`（**界面简中="移动速度"**，`game_concepts_l_simp_chinese.yml`） | 玩家可见概念 |
| 士气 | `game_concept_morale`（简中="士气"） | 与移动互相影响（见下） |

> ⚠️ `ARMY_MOVEMENT_SPEED` / `MOVEMENT_COST` 等**修正键本身无简中本地化**——界面按"键名自动词条"显示，别去本地化文件里找它们的名字。

## 一、基速（defines，`NUnit` 块）

| 常量 | 值 | defines 注释原文 |
| --- | --- | --- |
| `ARMY_MOVEMENT_SPEED` | **0.9** | `# movement progress per day`（陆军每天推进量） |
| `NAVY_MOVEMENT_SPEED` | **3.525** | `# movement progress per day`（海军） |
| `EXPEDITION_TRAVEL_SPEED_LAND` | 10.8 | 远征陆行推进/天（见 `fields\common-expedition_types.md`） |
| `EXPEDITION_TRAVEL_SPEED_SEA` | 60.0 | 远征海行推进/天 |
| `MOVEMENT_LOCKED` | **0.5** | 士气过低 → 移速乘 0.5（锁半速） |
| `LOW_MORALE_THRESHOLD` | 0.5 | 士气低于此比例即触发上条 |
| `SIZE_IMPACT_ON_MOVEMENT_SCALE` | **−0.02** | 军团规模越大越慢 |
| `SIZE_IMPACT_ON_MOVEMENT_CAP` | **−0.5** | 规模拖慢**封顶 −50%** |
| `LOCATION_SCALE_ON_MOVEMENT_COST` | 1.33 | 地点大小放大移动成本 |
| `ARMY_MOVEMENT_SPEED_INTO_UNKNOWN_TERRITORY_MODIFIER` | 1 | 进入未知领土的额外惩罚（**当前=1＝无额外惩罚**） |
| `PROXIMITY_TERRAIN_SPEED_FLOOR` | 0.25 | 邻近地形采样速度的下限（`NCountry`） |

## 二、军团级"速度修正"（`category=unit`，`percent=yes`）

这些是可直接改一支军队速度的修正键（`00_modifier_types.txt` 行号）：

| 键 | 行 | 语义 |
| --- | --- | --- |
| `army_movement_speed` | L4697 | 陆军移速百分比修正（**全局主旋钮**） |
| `navy_movement_speed` | L4711 | 海军移速百分比 |
| `movement_speed_if_no_road` | L4718 | **无道路**地形上的速度损失减免（靠革新给） |
| `movement_speed_when_attached_to_another_unit` | L6107 | 附属于另一军团（登船/随军）时的速度 |
| `army_disembark_speed` | L4704 | 卸载/登陆速度 |
| `land_morale_movement_cost` | L4620 | 移动**消耗士气**的速率（`color=bad`） |
| `naval_morale_movement_cost` | L4628 | 同上，海军 |

## 三、地点级"移动成本"（`category=location`，速度是其倒数）

`movement_cost`（L4673）· `hostile_movement_cost`（L4681 敌境）· `friendly_movement_cost`（L4689 友境）；国家级另有 `capital_movement_cost_modifier`（L2916）、`global_distance_from_capital_speed_propagation`（L5460，离首都越远越慢）、`movement_blocked`（L5941，`boolean=yes`，完全断路）。

**实测：文本里的 `movement_cost` 全挂在气候/灾害静态修正上**（`main_menu\common\static_modifiers\location.txt`）：

| 静态修正块 | movement_cost |
| --- | --- |
| `winter_severe` | 0.25 |
| `snow_storm_in_location` | 0.5 |
| `encroaching_cold_modifier` | 0.5 |
| `floating_ices_modifier` | 0.33 |
| `high_winds_in_location` | 0.25 |
| `sandstorm_in_location` | 0.25 |

> ⚠️ **山地 / 森林 / 沼泽等基础地形成本不在文本里**——烤进地图二进制。文本能改的只有上面这些灾害成本、以及革新/建筑给的 `movement_cost`。

## 四、这些修正由谁提供（贡献源计数，2026-10 实查）

- **`army_movement_speed` 设置最多**：宗教 `in_game\common\religions\folk_peruvian.txt`（13 处）、`folk_aridoamerica.txt`（11）、静态修正 `main_menu\common\static_modifiers\country.txt`（7，即内阁/统治者/事件）、`character.txt`（5）、神祇 `gods\hindu.txt`（3）、社会价值观 `societal_values\00_default.txt`、宗教侧面 `religious_aspects\tonal.txt`、法律 `laws\02_country_specific.txt`。取值集中在 **0.1 ×48**，另有 0.05/0.15/0.2 及个别负值（−0.1、−0.25）。
- **`movement_speed_if_no_road`** 主要来自军事革新/学说：`advances\ctype_army.txt:6`（+0.10）、`advances\government_steppe_horde.txt:106`（+0.20，游牧）、`advances\country_kbo.txt:24`（+0.05）、各 `folk_*` 文化宗教。
- **`land_morale_movement_cost`** 见军事选择革新 `advances\4_choices_mil.txt:431`（−0.0007，降低移动耗士气）。
- 建筑层 `building_types\` 里有 4 处 `movement_cost`（要塞/据点类区域成本）。

## 五、海军

基速 `NAVY_MOVEMENT_SPEED = 3.525`；速度修正 `navy_movement_speed`、耗士气 `naval_morale_movement_cost`；天气成本 `high_winds_in_location`（0.25）与 `floating_ices_modifier`（0.33）走的是上面的 `movement_cost`；`NAVAL_SUPPLY_RANGE`、`OUTSIDE_OF_NAVAL_RANGE_ATTRITION` 影响能去哪、超出后转为损耗（海军机制本库暂无专篇）。

## 六、合成算式（引擎侧，未证）

可读出的近似关系：`每天推进 ≈ 基速 ×（1 + Σ 军团速度修正）÷（路径距离 ×（1 + Σ 地点 movement_cost））`，规模按 `SIZE_IMPACT_ON_MOVEMENT_*` 递减、士气低于 `LOW_MORALE_THRESHOLD` 再乘 `MOVEMENT_LOCKED = 0.5`。**但以下均不在 `common\` 文本里、属引擎硬编码，别当已证**：

- 各修正到底是 `Σ`（相加）还是 `Π`（相乘）合成、以及先加后除的确切顺序。
- `SIZE_IMPACT` 里的"规模"按**团数**还是**总兵力**计（`REGIMENT_SIZE = 1000` 只是显示换算）。
- `LOW_MORALE_THRESHOLD = 0.5` 比较的分母（相对 `LAND_MORALE = 3.0` 上限还是当前值）。
- 无简中本地化的这些键，在界面里到底以何文案呈现。

## 审查要点

- 移速改在**军团层**（`army_movement_speed`，`category=unit`），不是给每个兵种加基速——兵种文件里没有 speed 字段，别去 `unit_types\` 找。
- `movement_cost` 与 `army_movement_speed` 是**两条相反的轴**：一个加成本（变慢），一个加速度；调平衡时二者都会动，别重复惩罚。
- `movement_speed_if_no_road` 只在**无道路**时生效，给了革新才有；写"道路加成"要确认走的是这个键还是 `movement_cost`。
- `percent=yes` 的键填 `0.1` = **+10%**，不是 +0.1 个点；`MOVEMENT_LOCKED`/`SIZE_*` 是**小数乘子/增量**，量纲不同。
- 基础地形速度**改不了文本**——要改山/林成本得动地图数据，文本层只能改气候 `movement_cost` 与革新/建筑给的 `movement_cost`。

## 中文检索键

| 中文 | 内部名 |
| --- | --- |
| 移动速度 / 军队速度 | `ARMY_MOVEMENT_SPEED`（defines）/ `army_movement_speed`（修正）/ `game_concept_movement_speed` |
| 海军速度 | `NAVY_MOVEMENT_SPEED` / `navy_movement_speed` |
| 移动成本 / 地形成本 | `movement_cost` / `hostile_movement_cost` / `friendly_movement_cost` |
| 无道路 / 道路加成 | `movement_speed_if_no_road` |
| 士气 / 低士气锁速 | `game_concept_morale` / `MOVEMENT_LOCKED` / `LOW_MORALE_THRESHOLD` / `land_morale_movement_cost` |
| 规模拖慢 / 大军更慢 | `SIZE_IMPACT_ON_MOVEMENT_SCALE` / `SIZE_IMPACT_ON_MOVEMENT_CAP` |
| 登陆 / 卸载速度 | `army_disembark_speed` / `movement_speed_when_attached_to_another_unit` |
| 冬天 / 暴风雪 / 沙暴减速 | `winter_severe` / `snow_storm_in_location` / `sandstorm_in_location` / `high_winds_in_location` / `floating_ices_modifier` |
| 远征移动 | `EXPEDITION_TRAVEL_SPEED_LAND` / `_SEA`（见 `fields\common-expedition_types.md`） |
