# 原版解析：生产与建筑（vanilla production & buildings）

> **一句话**：讲原产 RGO 与建筑两条生产路径：74 种商品、45 档建筑类型、生产方式输入与 41 个建筑上限公式，并标出 `main_menu` 区的数值来源。
> **什么时候看**：加商品、建筑或生产方式，改建筑上限价格与建造需求，或遇到"改了没生效"要分清三区文件时翻这篇。
> **体量**：431 行 · 约 20 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、两条生产路径总览](#一两条生产路径总览)
- [二、原产（RGO）](#二原产rgo)
  - [2.1 定义与五种方法](#21-定义与五种方法)
  - [2.2 工人（`rgo_pops`）](#22-工人rgo_pops)
  - [2.3 扩张：价格、队列、批量 UI](#23-扩张价格队列批量-ui)
  - [2.4 上限从哪来](#24-上限从哪来)
  - [2.5 通胀与情报](#25-通胀与情报)
  - [2.6 ⚠️ 陷阱：`rgo_building_category` **不是**原产升级](#26-️-陷阱rgo_building_category-不是原产升级)
- [三、商品：两条路径的接口](#三商品两条路径的接口)
- [四、建筑（building）](#四建筑building)
  - [4.1 字段权威](#41-字段权威)
  - [4.2 实查字段 vs readme：**漏了 23 个**](#42-实查字段-vs-readme漏了-23-个)
  - [4.3 实例解剖：啤酒四级链（`production_beer.txt`，321 行）](#43-实例解剖啤酒四级链production_beertxt321-行)
  - [4.4 等级上限体系（`script_values\building_caps.txt`，1031 行 / 41 个公式）](#44-等级上限体系script_valuesbuilding_capstxt1031-行--41-个公式)
  - [4.5 价格与成本](#45-价格与成本)
  - [4.6 建造需求（`construction_demand`）](#46-建造需求construction_demand)
  - [4.7 位置等级门槛与"免费建筑位"](#47-位置等级门槛与免费建筑位)
- [五、生产方式（production_method）](#五生产方式production_method)
  - [5.1 字段权威](#51-字段权威)
  - [5.2 原版实际在用的额外字段](#52-原版实际在用的额外字段)
  - [5.3 结构：维护型 vs 生产型](#53-结构维护型-vs-生产型)
  - [5.4 啤酒的 7 种输入变体（`production_beer.txt:21–106`）](#54-啤酒的-7-种输入变体production_beertxt21106)
- [六、经济循环：投产度 / 雇佣 / 补贴 / 淘汰](#六经济循环投产度--雇佣--补贴--淘汰)
  - [6.1 投产度（establishment）—— 系统已定义但**默认关闭**](#61-投产度establishment-系统已定义但默认关闭)
  - [6.2 雇佣、裁员与补贴](#62-雇佣裁员与补贴)
  - [6.3 效果清单（mod 的写入接口）](#63-效果清单mod-的写入接口)
  - [6.4 AI 相关常量（改平衡时最容易漏）](#64-ai-相关常量改平衡时最容易漏)
- [七、建筑类别与施工音效](#七建筑类别与施工音效)
- [八、Mod 改造建议（可改 vs 硬编码）](#八mod-改造建议可改-vs-硬编码)
- [九、中文检索键](#九中文检索键)

版本基准：EU5 1.3.x。**注意分区**：建筑/生产方式/商品/上限公式都在 **`in_game\`** 区；而"数值默认值"（雇佣规模、建造时间、投产度目标、建筑价格档）在 **`main_menu\common\script_values\default_values.txt`**；修正名登记在 **`main_menu\common\modifier_type_definitions\00_modifier_types.txt`**；常量在 **`loading_screen\common\defines\00_defines.txt`**。改建筑时漏掉 main_menu 区是最常见的"改了没生效"。

| 路径 | 规模 | 作用 |
|---|---|---|
| `in_game\common\building_types\` | 45 文件（readme + 44 内容，含 20 个 `production_*`） | 建筑类型定义；`readme.txt` 48 行为字段权威 |
| `in_game\common\building_categories\00_default.txt` | 69 行 / 15 类别 | 建筑类别 + 施工音效桶 |
| `in_game\common\production_methods\` | 3 文件（`__readme.txt` 8 行 + `unsorted_building_inputs.txt` + `village_production_methods.txt`） | 共享生产方式库 |
| `in_game\common\goods\` | 5 文件 / 74 商品；`readme.txt` 23 行 | 商品定义（RGO 与建筑的接口） |
| `in_game\common\goods_demand\building_construction_costs.txt` | 777 行 | `construction_demand` 引用的建造需求表 |
| `in_game\common\goods_demand_category\00_default.txt` | 13 行 / 13 类别 | 需求分组 |
| `in_game\common\script_values\building_caps.txt` | 1031 行 / 41 个上限公式 | **建筑等级上限全部是脚本公式** |
| `main_menu\common\script_values\default_values.txt` | 1288 行 | 雇佣/建造时间/投产度目标/价格档（972–1134、1201–1230） |
| `in_game\common\prices\00_hardcoded.txt` | 26–44 行 | `expand_rgo_*` = 100 金 |
| `in_game\common\prices\01_buildings.txt` | 121 行 | 建筑价格档（普通/昂贵 × 6 时代） |
| `in_game\common\location_ranks\00_default.txt` | 4 等级 | 等级对 RGO 上限/免费建筑位的影响 |
| `in_game\common\effect_localization\`、`trigger_localization\` | — | 效果/触发器**登记表**（判断某效果是否存在的最快途径） |
| `gui\expand_raw_goods_lateralview.gui` | 42KB | 原产扩张界面（市场内排名 + 批量扩张） |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `rgo`（resource gathering operations） | **原产** | 官方的正式名是"原料生产"，原产是缩写 |
| `rgo_pops` | **劳工或奴隶** | 可从事原产采集的 pop 类型，只有 laborers + slaves |
| `raw_material` | 原材料 | 由 RGO 产出，**不需要建筑** |
| `produced_goods` | 制成品 | 由建筑里的 pop 产出 |
| `building_type` / `building` | 建筑类型 / 建筑 | 类型是蓝图，建筑是地点里的实例 |
| `building_category` | 建筑类别 | 主要用于区分相似建筑 |
| `production_method` | 生产方式 | 决定建筑的投入品与效率 |
| `employment_size` | 雇佣规模 | `1 = 1000 人`（readme 第 5 行） |
| `establishment` | **投产度** | 新建筑未投产；达阈值前吞吐缩水，超过后给产出加成 |
| `subsidy` | 补贴 | 亏损建筑由所有者每月补足 |
| `rgo_mining/farming/forestry/hunting/gathering` | 矿场/农场/林场/狩猎场/采集场 | 五种原产方法 |
| `rural_settlement/town/city/megalopolis` | 乡村/集镇/城市/大都市 | 四个 `location_rank`，是建筑的**可建造门槛** |
| `market_access` | 市场接入度 | 低准入同时惩罚**建筑等级上限**与 RGO 利润 |

## 一、两条生产路径总览

```
地点（location）
 ├─ 路径 A：原产 RGO —— 不需要任何建筑
 │    rgo_pops（劳工 laborers + 奴隶 slaves）→ 每月产出一种 raw_material
 │    等级 = 原产等级，每级至多 1000 工人；扩张花 gold（expand_rgo_*）
 │    上限 = 地点发展度 / 革新 / 修正 / 地点等级
 │
 └─ 路径 B：建筑生产 —— 需要建造与维护
      building_type 蓝图 → 地点里的 building 实例
      pop_type 雇佣 → production_method 决定投入品与产出
      等级上限 = script_values\building_caps.txt 的公式（不是字段硬值）
        ↓ 两条路径的产品都进市场定价后卖出（上游见 vanilla-trade-and-market.md）
```

**关键区别**：路径 A 改产出只要改 `goods` 的 `method`/`base_production` 与修正；路径 B 改产出要动建筑类型 + 生产方式 + 需求表三处。

## 二、原产（RGO）

### 2.1 定义与五种方法

- 每个 `location` 生产**一种** `raw_material`（`game_concept_rgo_desc`，`main_menu\localization\simp_chinese\game_concepts_l_simp_chinese.yml:640`）。
- 方法即商品的 `method` 字段：`mining / farming / hunting / gathering / forestry`，**默认 farming**（`goods\readme.txt:11`）。
- 原版 74 种商品的分布（实查）：

| 方法 | 数量 | 商品 |
|---|---|---|
| `farming` | 28 | horses, silk, dyes, incense, tea, cocoa, coffee, fiber_crops, wine, elephants, saffron, pepper, cloves, chili, wool, cotton, sugar, tobacco + 食物类 wheat, maize, rice, millet, legumes, potato, livestock, olives, fruit, beeswax |
| `mining` | 13 | coal, iron, copper, goods_gold, silver, stone, tin, lead, gems, saltpeter, alum, marble, mercury |
| `gathering` | 7 | clay, sand, salt, medicaments, pearls, amber, fish |
| `hunting` | 3 | ivory, wild_game, fur |
| `forestry` | **1** | lumber |
| 无 method（= 制成品） | 22 | tar…lacquerware（19）+ pottery, furniture + slaves_goods |

> `forestry` 只有 lumber 一种商品，`hunting` 只有 3 种 —— 想加"林业/狩猎"类 mod 内容时这两条方法几乎全是空白。

### 2.2 工人（`rgo_pops`）

- `game_concept_rgo_pops`（中文 1297–1298 行）明确："这些是可以通过工作收集原材料的 pop 类型"，只列 **`laborers` + `slaves`**。
- 每级原产至多雇 **1000 名** `rgo_pops`。
- 未被 RGO 或建筑雇佣的 `rgo_pops` 视为从事**自给农业**（`game_concept_subsistence_agriculture_desc`），食物产出效率低于正经食物原产。
- 原版 `pop_types\00_default.txt` 里 **laborers 与 slaves 都没有"可在 RGO 工作"的标记字段** → 这条绑定是引擎硬编码，不能靠 pop_types 改。能改的只有修正：`allow_rgo_slave_demand`、`*_estate_allowed_to_build_rgo`（8 个阶层）、`overlord_blocked_from_building_rgos`。

### 2.3 扩张：价格、队列、批量 UI

| 项 | 值 | 位置 |
|---|---|---|
| 扩张价格 | `expand_rgo_mining/farming/hunting/gathering/forestry` = **gold 100**（五种同价） | `prices\00_hardcoded.txt:26–44` |
| 打折修正 | `expand_rgo_*_cost_modifier`（五种各一） | `modifier_type_definitions\00_modifier_types.txt:2476–2524` |
| Ctrl 连点次数 | `RGO_QUEUE_CTRL_CLICK = 5`、`REDUCE_RGO_QUEUE_CTRL_CLICK = 5` | `defines:78, 83` |
| 基础建造时间 | `RGO_BASE_TIME = 180`（与百分比修正 `*_rgo_build_time` 配套，UI 走 `LocationView.GetUpgradeRGOBuildTimeBreakdown`；本体未标单位，按同族建造时间常量推断为天） | `defines:1693` |
| 时间修正 | `local_rgo_build_time`、`global_rgo_build_time`（如 `advances\ctype_location.txt:9` 给 −0.1） | 修正表 4928/4936 |
| 装填节奏 | `RGO_LOAD_TIME_FRACTION = 0.33`（"33% of cap, if pop can afford it."）、`RGO_LOAD_TIME_MAX_PEASANT_PERCENTAGE = 0.1`（"max % of peasants"） | `defines:1695–1696` |
| 成本公式常量 | `GOODS_RGO_BASE_COST = 0.5`、`GOODS_RGO_PRICE_SCALE = 0.25` | `defines:1858–1859`（引擎侧，UI 取 `Location.GetRGOCost`） |

> **地形/植被也在这条链上**：`local_rgo_build_time`（山地 +1.0、丛林 +0.5、丘陵/高原/湿地 +0.25、沙漠 +0.5）、`local_max_rgo_size_modifier`（农田 **+0.10**）、`local_road_building_time` 全部写在 `topography`/`vegetation` 的 `location_modifier` 里 —— 完整 22 地形 / 7 植被数值表见 `vanilla\vanilla-hazards-and-environment.md` §二。

**界面能力**（`gui\expand_raw_goods_lateralview.gui`）：

- 按**市场**分组的商品扩张榜（`ExpandRawGoodsLateralView.GetSelectedMarket` + `GetExpandRankingItems`），列出"最便宜/最赚"的地点，并给出 **3 年利润**预测（`EXPAND_RGO_IN_BEST_LOCATION_PROFIT_LABEL`）。
- **批量扩张**："Mass Expand RGO" / "Expand production in all possible locations"，Ctrl = 连做 N 次，Shift = 尽可能多做；对应的撤销入口是 "Cancel Latest RGO Expands"。
- **减少原产**：`LOCATION_VIEW_REDUCE_RGO_TITLE*`（单击/Ctrl/Shift 三档）。
- **自动化**：`AUTOMATED_SYSTEM_EXPANDRGO`（自动扩张已标记的原产，直到地点达到上限）、`AUTOMATED_SYSTEM_RGO`。
- 地点面板的原产条带：`location_rgo_button_tooltip`、`location_rgo_max_level_breakdown_tooltip`（上限明细是**引擎侧** `Location.GetMaximumMaxRGOWorkersInfoForUI`，脚本改不了）。

### 2.4 上限从哪来

- 官方词条只说"取决于地点发展度、革新和其他因素"。实查来源共四类：

| 来源 | 具体 | 位置 |
|---|---|---|
| **地点等级** | `rural_settlement` 给 `local_max_rgo_size_modifier = **+1.0**`；`megalopolis` 给 **−0.5**；`town`/`city` 无此项 | `location_ranks\00_default.txt:186 / :33` |
| **劳工识字率** | laborers 的 `literacy_impact` 给 `local_max_rgo_size_modifier = 0.1` | `pop_types\00_default.txt:130` |
| **革新** | 例：`0_age_of_discovery.txt:402` 乡村 +0.2、`:531` 全局 +0.10；`0_age_of_renaissance.txt:495` 非乡村 +0.1；`0_age_of_absolutism.txt:22/107` +0.10/+0.20；`0_age_of_revolutions.txt:463` +0.25 | `advances\` |
| **国家/文化修正** | 例：POL +0.2 & 农业扩张 −0.25、HUN +0.2、BUR `global_raw_material_output = 0.2` | `advances\country_*.txt`、`culture_*.txt` |

- 上限修正全套：`global_max_rgo_size_modifier`、`local_max_rgo_size_modifier`、`global_max_rgo_size_modifier_in_rural`、`global_max_rgo_size_modifier_in_non_rural`、`local_max_rgo_size`（**绝对值**，非百分比，`decimals=0`，格式 `FormatPopCaps`）。
- 输出侧修正：`local_raw_material_output` / `global_raw_material_output`（`raw_material_in_province_impact` 是省份层面的另一项）。
- 触发器：`rgo_level`、`rgo_workers`、`max_rgo_workers`（`trigger_localization\location_triggers.txt:160/167/173`）。
- 效果：`change_raw_material`（换商品！）、`change_max_raw_material_workers`（改上限）、`construct_rgo_upgrade`（**排一次原产升级**，`effect_localization\location_effects.txt:29/36/171`）。
  - `change_max_raw_material_workers` 原版在议会诉求与 `generic_actions\columbian_exchange.txt`、`scripted_effects\country_effects.txt:2495` 里用。
  - `construct_rgo_upgrade` **原版脚本零调用**（`in_game\common` 全目录只出现在 effect_localization 登记表里）→ 这是留给 mod 的干净接口。

### 2.5 通胀与情报

- 原产产出会推通胀：`INFLATION_RGO_IMPACT_FACTOR = 0.01`、`INFLATION_RGO_CAP = 0.5`、`INFLATION_RGO_INCOME_FACTOR = 2`（`defines:1934–1936`）。
- 低效建筑会拖累 RGO：`BUILDING_LOW_PRODUCTION_EFFICIENCY_IMPACT_ON_RGO = 5.0`（`defines:1300`）。
- 情报阈值：`INTEL_THRESHOLD_LOCATION_RGO_WORKERS = **15**`（`defines:2391`）——别国地点 RGO 工人数低于 15 时，界面显示为未知（`INTEL_FOG_LOC_RGO_WORKERS_UNKNOWN`），GUI 侧对应 `HasRgoWorkersIntelOn`。

### 2.6 ⚠️ 陷阱：`rgo_building_category` **不是**原产升级

`building_categories\00_default.txt:1–6` 的注释是官方自己写的警告：

```
rgo_building_category = {
	# Buildings classified here (mercury_patio, lumber_mill, stone_quarry, ...) are extractive
	# industry that gets built like any other building, so they map to the industry sound bucket.
	# True RGO upgrades take a separate code path (construction_rgo_<method>) that doesn't read this.
	audio_category = industry
}
```

- 中文里这个类别叫"**原材料生产**"，但里面的 `mercury_patio`（混汞提银场）、`lumber_mill`、`stone_quarry` 等**是普通建筑**：要 gold、要 pop 雇佣、有等级上限、能被拆除。
- 真正的原产升级走引擎侧代码路径 `construction_rgo_<method>`，它不读这个类别。该字符串在 `in_game\common` 与 `in_game\gui` 全目录**只此一处（注释）**，脚本里没有可覆盖的定义文件。
- 推论（写 mod 时最容易踩）：**想加"新的原产采集设施"有两种完全不同的做法**，选错会做出一个"看起来像 RGO 其实是建筑"的东西。
  - 想要真 RGO：只能改 `goods`（新增商品 + `method`）+ 修正/效果，RGO 本身没有可注册的类型清单。
  - 想要建筑式采掘业：加 `building_type` 并归进 `rgo_building_category`，但它就是建筑。

## 三、商品：两条路径的接口

字段权威 `goods\readme.txt`（23 行）：`is_slaves`、**`block_rgo_upgrade`**、`inflation`、`base_production`、`color`、`food`、`transport_cost`（默认 1）、`default_market_price`（默认 1）、`category`（`raw_material`/`produced`）、`method`、`ai_rgo_size_importance`、`demand_add`、`demand_multiply`、`location_potential`、`custom_tags`。

实查补充（readme 未记载但原版在用）：`ai_rgo_expansion_priority`（如 clay 0.025、lumber 0.025）、`sub_continent_demand_modifier`（按次大陆调整需求，如 `south_asia = 1.2`）、`origin_in_old_world`（如 horses），以及 `custom_tags` 里的 `old_world_goods` / `new_world_goods`。

**`block_rgo_upgrade` 原版零使用**（`goods\*.txt` 里只有 readme 自己提到）→ 干净的 mod 开关：设为 yes 就能让某种商品的原产无法被扩张（做成"矿脉枯竭/禁采"类机制）。

## 四、建筑（building）

### 4.1 字段权威

`building_types\readme.txt`（48 行）共登记 **44 个字段**。分组记忆：

| 分组 | 字段 |
|---|---|
| 成本/规模 | `build_time`、`price`、`destroy_price`、`increase_per_level_cost`、`max_levels`、`employment_size`、`output`、`expensive`(未登记) |
| 位置门槛 | `rural_settlement` / `town` / `city` / `megalopolis`（readme 写作 `<location rank>`）、`is_village`(未登记) |
| 雇佣 | `pop_type`、`always_add_demands`、`AI_ignore_available_worker_flag` |
| 生产 | `possible_production_methods`、`unique_production_methods`、`construction_demand`、`obsolete` |
| 可见/可建（三组 visible+enabled） | `allow`、`location_potential`、`country_potential`、`international_organization_potential` |
| 销毁 | `can_destroy`、`is_indestructible`、`remove_if` |
| 修正 | `modifier`（×等级×商品可得性）、`raw_modifier`（不缩放）、`capital_modifier`、`capital_country_modifier`、`capital_to_overlord_modifier`、`foreign_country_modifier`、`market_center_modifier` |
| 外国建筑 | `is_foreign`、`in_empty`、`stronger_power_projection`、`need_good_relation`、`pop_size_created` |
| IO | `international_organization_link` |
| 效果钩子 | `on_built`、`on_destroyed` |
| 其他 | `estate`、`conversion_religion`、`custom_tags`、`important_for_AI`、`important_for_UI`、`audio_category`、`audio_tier` |

> readme 的 `build_time: <integer> days` 其实不准确：原版写的是脚本值（`build_time = guild_build_time`，值 365），`max_levels` 也一律是脚本值。

### 4.2 实查字段 vs readme：**漏了 23 个**

对 44 个内容文件做"一个制表符缩进的顶层字段"统计：实际使用 **69 个**，其中 readme 未登记的：

| 字段 | 原版用例数 | 备注 |
|---|---|---|
| `is_special` | 224 | 事件/独特建筑标记，UI 与 AI 特殊处理 |
| `expensive` | 166 | **切价格档**：默认走 `p_building_age_N`，置 yes 走 `p_expensive_building_age_N` |
| `startup_ramp_target` | 110 | 投产度爬坡目标（**单位在任何文件里都没写**，见 §六） |
| `graphical_tags` | 51 | 如 `{ medium_religious }`、`{ palace }`、`{ small_city_walls city_walls }` |
| `forbidden_for_estates` | 47 | 禁止阶层建造 |
| `is_mill` | 18 | 工厂级标记（配合 `local_mills_build_buildings_efficiency`） |
| `ai_foreign_ignore_naval_range` | 8 | 外资建筑忽略海军范围 |
| `can_close` | 7 | 可关停 |
| `ai_forbid_shutdown` | 6 | 禁止 AI 关停 |
| `own_or_overlord_relation_needed` | 6 | 例：`= trade_access`（贸易公司建筑） |
| `want_foreign_pop_created` | 5 | 外资建筑拉 pop |
| `destroyable_building` | 4 | — |
| `is_village` | 4 | 村庄级 |
| `ai_ignore_maintenance` | 3 | — |
| `allow_wrong_startup` | 3 | — |
| `automation_build_allowed` | 3 | 首都建筑多设为 `no`，禁自动建造 |
| `convert_on_ownership` | 2 | 易主时转换 |
| `AI_optimization_flag_coastal` | 2 | AI 选址标记 |
| `on_construction_started` / `on_construction_ended` | 各 1 | **建造阶段钩子**（`trade_company_buildings.txt:67/76`） |
| `content_priority` | 1 | 内容优先级 |
| `lifts_fog_of_war` | 1 | `foreign_buildings.txt:95` |
| `ai_unique_location_list` | 1 | AI 专属选址清单 |

另有 readme 登记但**建筑层不用**的两个：`audio_category`（实际写在类别文件里）、`output`（实际只写在生产方式里）。

### 4.3 实例解剖：啤酒四级链（`production_beer.txt`，321 行）

| 项 | `brewery` 啤酒铺 | `beer_workshop` 啤酒工坊 | `brewery_manufactory` 啤酒发酵场 | `brewery_mill` 啤酒厂 |
|---|---|---|---|---|
| `max_levels` | `guild_max_level` | `workshop_max_level` | `manufactory_max_level` | `mills_max_level` |
| `employment_size` | `guild_employment` 0.1 | 0.1 | 0.2 | `mills_employment` 0.25 |
| `pop_type` | burghers | burghers | burghers | **laborers** |
| `build_time` | 365 | 365 | 450 | 730 |
| `startup_ramp_target` | 240 | 240 | 120 | 60 |
| 生产方式数 | **7 种输入变体** | 7 | 1 | 1 |
| `output` | 1 | 1.1 | 2 | **4** |
| `debug_max_profit` | `guild_profit_margin` 0.2 | 0.25 | 0.3 | 0.35 |
| `obsolete` | — | `brewery` | `beer_workshop` | `brewery_manufactory` |
| `custom_tags` | `{ guild }` | `{ workshop }` | `{ midgame_manufactory }` | `{ lategame_manufactory }` |
| `audio_tier` | 1 | 2 | 3 | 5 |
| 额外 | — | — | — | `is_mill = yes`、`raw_modifier = { local_burghers_desired_pop = 0.05 }` |

读法：**`obsolete` 串成升级链**——建了下一级，上一级被淘汰（`ESTABLISHMENT_UPGRADE_TRANSFER_FRACTION = 0.5` 让新建筑继承 50% 投产度）。`00_unique_buildings_to_make_obsolete.txt` 是专门给"独特建筑被通用建筑替代"用的文件（官方注释："so generic buildings (such as castles) can properly replace the unique ones"）。

### 4.4 等级上限体系（`script_values\building_caps.txt`，1031 行 / 41 个公式）

全部是**脚本公式**，不是硬编码 → mod 可以直接覆盖或新增。

| 上限 | 公式 |
|---|---|
| `guild_max_level` | 1 + `development`×0.1 + `population`×0.05 + 城市 5 + 大都市 10 |
| `workshop_max_level` | 1 + dev×**0.25** + pop×0.05 + 城市 10 + 大都市 20 |
| `manufactory_max_level` | **5** + dev×**0.5** + pop×0.1 + 城市 20 + 大都市 40 |
| `mills_max_level` | **5** + dev×**1.0** + pop×**0.25** + 城市 25 + 大都市 50 |
| `market_max_level` / `trade_office_max_level` | 同上骨架 ×(1 + `owner.modifier:market_building_levels`) |
| `market_warehouse_max_level` | dev×0.025 + 修正 |
| `manpower_max_level` | 1 + pop×0.05 |
| `plantation_cap` | **`max_rgo_workers`×2.0** |
| `rural_building_cap` | 1 + dev×0.1 + **`max_rgo_workers`×0.5** + 有河流 1 |
| `estate_building_stackable_level` | 1 + dev×0.1 + **`max_rgo_workers`×0.5**；`estate_building_single_level` 恒 1 |
| 特例 | `japanese_clan_building_cap`（×0.2）、`reformation_preachers_max_level`（按 `distance_to_rome` 1/2/3）、`bock_max_level`（按棱堡/星形要塞/要塞革新）、`janissary_barracks_size`（按时代）、`hre_imperial_armory_max_level` |

三条要点：

1. **产业升级链 = 同一块地能塞的等级跃升**（dev 系数 0.1 → 0.25 → 0.5 → 1.0），所以"工厂化"的本质是把同一块地的等级预算放大 10 倍。
2. **低市场接入度惩罚**：guild/workshop/manufactory/mills 四个上限都带
   `if = { limit = { market_access < 0.75 } multiply = { value = market_access add = 0.25 } }`，末了 `min = 1`。
   注意 `plantation_cap` / `rural_building_cap` / `estate_building_stackable_level` **没有**这一条（农村建筑不吃准入惩罚）。
3. **原产与建筑耦合**：种植园上限 = 原产工人上限×2，农村建筑上限/阶层建筑上限都含 `max_rgo_workers`×0.5 —— 想在农村刷建筑，先把 RGO 堆起来。

### 4.5 价格与成本

| 机制 | 值 | 位置 |
|---|---|---|
| 普通档（时代 1→6） | 50 / 100 / 200 / 400 / 800 / 1200 | `default_values.txt:1217–1222` |
| 昂贵档（`expensive = yes`） | 200 / 400 / 800 / 1600 / 3200 / 5000 | `default_values.txt:1225–1230` |
| 档位定义 | `p_building_age_N_*` / `p_expensive_building_age_N_*` | `prices\01_buildings.txt:81–121` |
| 自定义价格 | `price = <price 键>`，原版只有 11 个不同取值（`free_building`、`small/expensive_estate_building`、`orthodox/miaphysite_monastery_building`、`lutheran/calvinist_preachers_building`、`hre_army_building`、`expand_rgo_farming`、`build_hippodrome_price`、`merchant_guild_chapel_price`、`expand_aqueduct_system`） | — |
| 逐级涨价 | `increase_per_level_cost = 0.5`（每多一级贵 50%），原版仅 6 处；默认值 `production_per_level_cost = 0.1` | `town_buildings.txt:54`、`unique_buildings.txt:103` 等 |
| 升级折扣 | `UPGRADE_PRICE_MODIFIER = -0.25` | `defines:1888` |
| 拆除 | `BUILDING_DESTROY_GOLD_VALUE = 0.1`（返还成本的 10%）、`BUILDING_DESTROY_CHANCE = 50`、`BUILDING_DESTRUCTION_IMPACT = -0.20` | `defines:2477–2478, 1671` |
| 破产 | `CHANCE_FOR_BUILDING_REDUCTION_AT_BANKRUPTCY = 10` | `defines:1892` |
| 城市/集镇升级价 | `megalopolis_upgrade` 8000 / `city_upgrade` 2000 / `town_upgrade` 500 / `rural_settlement_downgrade` 100 | `prices\01_buildings.txt:1–19` |

### 4.6 建造需求（`construction_demand`）

- 字段指向 `goods_demand\building_construction_costs.txt`（777 行）里的键，如 `guild_construction`、`workshop_construction`、`manufactory_construction`、`mill_construction`、`soldier_building_construction`、`mercury_patio_construction`。
- 每条需求自带 `category = building_construction`（需求分组见 `goods_demand_category\00_default.txt` 的 13 类：`ship_maintenance/ship_construction/regiment_maintenance/regiment_construction/building_maintenance/guild_input/workshop_input/manufactory_input/mills_input/building_construction/government_activities/pop_needs/special_demands`）。
- 建筑的**日常维护**需求写在生产方式里（`category = building_maintenance`），**建造期**需求写在 `construction_demand` 里 —— 两者会一起进市场产生需求（`game_concept_building_desc`："大多数建筑的前期建造需要花费 gold，随后会在该地点的对应市场对某一商品产生额外的需求"）。
- `always_add_demands = yes`：即使没雇满工人也按全额吃需求。

### 4.7 位置等级门槛与"免费建筑位"

| 等级 | 中文 | 免费建筑等级 | 人口容量 | RGO 上限修正 | 工厂效率 |
|---|---|---|---|---|---|
| `rural_settlement` | 乡村 | — | — | **+1.0** | — |
| `town` | 集镇 | `free_building_levels = 25` | `local_population_capacity = 20` | — | — |
| `city` | 城市 | 100 | 100 | — | `local_mills_build_buildings_efficiency = 0.20` |
| `megalopolis` | 大都市 | 200 | 400 | **−0.5** | 0.33 |

（`location_ranks\00_default.txt:30/33/40/101/111/156/186`）

建筑用布尔字段 `rural_settlement/town/city/megalopolis = yes` 声明自己能在哪些等级建（原版用例数 249/425/438/419 —— 大多数建筑三个城市档全开）。

## 五、生产方式（production_method）

### 5.1 字段权威

`production_methods\__readme.txt`（全文 8 行）：

```
#name = {
#	<goods_input> = <amount>
#	produced = <goods>
#	output = <amount>
#   potential = { <triggers> }  #Country scope
# 	no_upkeep = yes/no  # blocks upkeep costs
#	allow = { <triggers> }	#Country scope
#}
```

readme 只登记 **5 个具名字段**（`produced` / `output` / `potential` / `no_upkeep` / `allow`），投入品则写成 `<商品名> = <数量>` 的任意键。而且 `potential` / `allow` 的作用域是**国家**（不是建筑、不是地点）——这是原版 readme 里少见的明确说明。

### 5.2 原版实际在用的额外字段

| 字段 | 含义 | 例 |
|---|---|---|
| `category` | **决定需求分组**（同时决定 UI 归类） | `guild_input` / `workshop_input` / `manufactory_input` / `mills_input` / `building_maintenance` |
| `debug_max_profit` | 调试用目标利润率，原版生产方式几乎都写（啤酒的 16 个变体全部有） | `debug_max_profit = guild_profit_margin`（0.2） |

### 5.3 结构：维护型 vs 生产型

- **维护型**：只有投入 + `category = building_maintenance`，无 `produced`。例：`estate_building_input`（空）、`incamisana_maintennace = { stone = 0.02 }`、`soldier_building_maintenance = { firearms 0.5, leather 0.5, cloth 0.10, paper 0.20, weaponry 0.5 }`。
- **生产型**：投入 + `produced = <商品>` + `output = <每级产量>`。
- 命名惯例：`<输入>_<建筑名>_maintenance`（如 `wheat_brewery_maintenance`），维护型与生产型混在同一个 `unique_production_methods` 块里。

### 5.4 啤酒的 7 种输入变体（`production_beer.txt:21–106`）

| 生产方式 | 主料 | 辅料 | output |
|---|---|---|---|
| `wheat_brewery_maintenance` 小麦 | wheat 0.9944 | lumber 0.2484 + tools 0.0999 | 1 |
| `millet_brewery_maintenance` 小米 | millet 0.9944 | lumber + tools | 1 |
| `fruit_brewery_maintenance` 水果 | fruit 1.0412 | lumber 0.208 + tools 0.1045 | 1 |
| `maize_brewery_maintenance` 玉米 | maize 1.3724 | **pottery** 0.2943 | 1 |
| `bavarian_brewery_maintenance` 巴伐利亚 | wheat 1.3345 | lumber + tools | **1.2** |
| `honey_brewery_maintenance` 蜂蜜 | beeswax 0.2603 + millet 0.5206 | lumber + tools | 1 |
| `rice_brewery_maintenance` 稻米 | rice 0.9944 | lumber + tools | 1 |

**这是原版最重要的可复用范式**：一个建筑给多个输入变体 → 玩家按本地 RGO/市场供给挑（availability 更高 → 效率更高），也就把"地域差异"写进了生产方式层。同一套 7 变体在工坊级只加了 1.1 倍 output、到工场/工厂级收敛回单一配方（output 2 → 4）。

生产方式本地化键**不在建筑文件里**，而在 `goods_l_<lang>.yml`：`wheat_brewery_maintenance: "小麦酿酒铺"`。

## 六、经济循环：投产度 / 雇佣 / 补贴 / 淘汰

### 6.1 投产度（establishment）—— 系统已定义但**默认关闭**

| 常量 | 值 | 说明 |
|---|---|---|
| `ESTABLISHMENT_SYSTEM_ENABLED` | **`no`** | **总开关：原版 1.3.x 是关的**。关时建筑开局即 100% 吞吐、无 PE 加成、无"拥挤"惩罚、无 AI 每地点上限 |
| `ESTABLISHMENT_THROUGHPUT_THRESHOLD` | 0.5 | 投产度低于阈值吞吐缩水，高于阈值给产出效率加成 |
| `STARTUP_MAX_PE_BONUS` | 0.30 | 满投产度最多 +30% 产出效率（词条里也直接引用这个 define） |
| `STARTUP_DECAY_RATE` | 1.0 | 建筑关停时每月流失的投产进度 |
| `ESTABLISHMENT_UPGRADE_TRANSFER_FRACTION` | 0.5 | 被淘汰建筑把 50% 投产度传给新建筑 |
| `ESTABLISHMENT_PROFIT_CAP_PER_LEVEL` | 3.0 | 金/月/级，达到即拿满投产加速 |
| `ESTABLISHMENT_MAX_PROFIT_BONUS` | 1.0 | 满利润最多把投产速度翻倍 |
| 逐建筑目标 | `startup_ramp_target`：行会 240 / 工坊 240 / 工场 120 / 工厂 60 / 农村 100（**单位未注明**：文件只写数字，本地化/GUI/defines 里都没有引用点，240/120/60 的量级更像"月"但无法从本体确认） | `default_values.txt:1085–1089` |
| 相关修正 | `local_building_establishment_speed` / `_reduction`、`global_building_establishment_speed` | 修正表 6696–6710 |

词条原文（中文 `game_concepts_l_simp_chinese.yml:1332`）："新建成的建筑处于未投产状态…建筑投产度每月自然累积，并于建筑关停时流失。"

⚠️ **三条要点必须分开记**：
1. 系统（字段 `startup_ramp_target`、7 个常量、3 个修正、`BUILDING_ESTABLISHMENT_*` 约 10 个本地化键、词条 `game_concept_establishment`）在 1.3.x 里**全部存在且完整**。
2. 但 `ESTABLISHMENT_SYSTEM_ENABLED = no` → 上述规则**当前不生效**。想启用只需把这一个 define 改 yes（改的是 `loading_screen\common\defines\`）。
3. 另有 `CONSTRUCTION_ESTABLISHMENT_CONFLICT_WARNING`（"同一地点同一时间只有一座建筑能以最高效率获得投产度"）——同样只在系统开启时才有意义。

### 6.2 雇佣、裁员与补贴

- 建筑只支持**一种** `pop_type`，最大雇佣量 = 建筑类型 × 等级（`game_concept_employ_desc`）。
- 亏损建筑会**逐月裁 10% 工人**：`UNPROFITABLE_BUILDING_WORKERS_LAID_OFF_PERCENTAGE = 10`；恢复盈利后再逐月补回 10%：`PROFITABLE_BUILDING_WORKERS_REHIRED_PERCENTAGE = 10`。
- 补贴（`subsidy`）：亏损建筑不买投入品也不产出任何东西；所有者可补贴它，**每月亏损直接从国库 balance 扣**（`game_concept_subsidization_desc`，中文 1969 行）。
- 效果：建筑作用域 `set_subsidized`（`effect_localization\building_effects.txt:19`）。注意本地化里存在 `SET_SUBSIDIZED_EFFECT` 与 **`UNSET_SUBSIDIZED_EFFECT`** 两组键，但 `building_effects.txt` 只登记了 `set_subsidized` → 取消补贴的写法未登记，用前先在 `scripted_tests` 里验一次。
- 建筑作用域触发器共 **25 个**（`trigger_localization\building_triggers.txt`）：`building_type`、`building_category`、`building_level`、`building_max_level`、`building_levels_under_construction`、`is_opened`、`is_at_max_level`、`is_full_capacity`、`is_subsidized`、`is_lacking_goods`、`is_building_owned_by`、`building_pop_type`、`employment_size`、`building_manpower_produced`、`building_sailors_produced`、`building_base_cost_in_gold`、`building_profit`、`building_potential_profit`、`building_index`、`building_produced_goods`、`building_goods_input`、`building_employment_size_amount`、`building_employed_amount`、`building_can_be_destroyed_by`、`building_can_be_upgraded_by`。

### 6.3 效果清单（mod 的写入接口）

| 作用域 | 效果 | 位置 |
|---|---|---|
| building | `change_building_level`、`change_building_owner`、`set_subsidized` | `building_effects.txt` |
| location | `construct_building`、`construct_estate_building`、`construct_rgo_upgrade`、`change_building_level_in_location`、`destroy_building`、`destroy_building_forcefully`、`destroy_all_buildings_of_type`、`create_building_country_in_location`、`change_raw_material`、`change_max_raw_material_workers` | `location_effects.txt` |

`change_building_owner` 与 readme 里的 `foreign_country_modifier` / `capital_to_overlord_modifier` 配套：外资建筑易主后，修正跟着新所有者走（readme 第 32–33 行专门解释这一点）。

### 6.4 AI 相关常量（改平衡时最容易漏）

`AI_BUILDING_PROFIT_THRESHOLD = 1.2`（非战略商品至少 20% 利润 AI 才扩）、`AI_ALLOWED_BUILDING_MAINTENANCE = 0.25`（维护费占收入上限，不含要塞）、`AI_BUILDING_MAINTENANCE_LEEWAY = 1.20`、`BUILDING_THRESHOLD_FACTOR = 1.2`、`AI_GLOBAL_BUILDING_COST_UTIL = −0.75` / `AI_GLOBAL_URBAN_BUILDING_COST_UTIL = −0.5`、`AI_UPGRADE_BUILDING_UTILITY = 0.001`、`AI_ALLOWED_PARALLEL_SAME_TYPE_CONSTRUCTION_RATIO = 0.02`、`AI_CONSTRUCTION_QUEUE_*`（1354–1381）、`PEASANT_BUILDING_IN_CITY_UTILITY_MULT = 0.5`、`CONSTRUCTION_GOODS_SHORTAGE_UTILITY_FACTOR = 0.66`。

## 七、建筑类别与施工音效

15 个类别（`building_categories\00_default.txt`）：

`rgo_building_category` 原材料生产 · `basic_industry_category` 基础工业 · `weapons_industry_category` 武器工业 · `consumer_goods_category` 消费品工业 · `government_category` 政府建筑 · `infrastructure_category` 基建建筑 · `religious_category` 宗教建筑 · `cultural_category` 文化建筑 · `trade_category` 贸易建筑 · `naval_category` 海军建筑 · `military_category` 军事建筑 · `defense_category` 防御建筑 · `village_category` 村庄建筑 · `colonial_category` 殖民建筑 · `estate_category` 阶层建筑

类别文件目前**只承载音效桶**（`audio_category = industry/economy/generic/religion/culture/military`）。建筑级 `audio_tier` 为 1–6：1 = 时代 1/基础，2 = 时代 2 默认，3 = 链条中段/首都升级，4 = 宫殿级全国修正，5 = 顶级稀有战略，6 = 时代 6 终局；越界会在 `PostReadInit` 报 ERRORLOG，使用时会 clamp 到 `CONSTRUCTION_AUDIO_MIN/MAX_TIER`（1/6），最终事件名 `construction_building_<audio_category>_<audio_tier>`。

## 八、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 新增建筑 | `in_game\common\building_types\<新文件>.txt` + `buildings_l_<lang>.yml` | 本地化键 = 建筑类型 id；描述键 `<id>_desc`（可选，不是每个建筑都有） |
| 新建类别 | `building_categories\` | 类别只影响音效与 UI 分组，不构成机制 |
| 改等级上限 | `script_values\building_caps.txt`（整块覆盖或新增公式） | 全是脚本公式，是最"软"的一层；注意低准入惩罚与 `min` |
| 改建造时间/雇佣/价格档 | **`main_menu\common\script_values\default_values.txt`**（972–1134、1201–1230） | 在这里改会同时影响所有引用该脚本值的建筑 |
| 改某个建筑的价格 | 建筑里 `price = <新 price 键>` + `prices\01_buildings.txt` | 或直接 `expensive = yes` 切档 |
| 加/改生产方式 | `production_methods\unsorted_building_inputs.txt`（共享库）或建筑内 `unique_production_methods` | 本地化键要写进 `goods_l_<lang>.yml` |
| 改建造期需求 | `goods_demand\building_construction_costs.txt` + 建筑 `construction_demand` | `category = building_construction` |
| 改需求分组 | `goods_demand_category\00_default.txt` | 分组名是 UI 归属，新增分组需要本地化 |
| 新增修正名 | **`main_menu\common\modifier_type_definitions\00_modifier_types.txt`**（17616 行） | 格式：`name={ percent=yes game_data={ category=location/country } }`；在 in_game 区加是无效的 |
| 改 RGO 价格 | `prices\00_hardcoded.txt` 的 `expand_rgo_*` + `*_cost_modifier` | 5 种方法各自独立 |
| 换某地原产商品 | 效果 `change_raw_material` | — |
| 改 RGO 工人上限 | 修正 `local_max_rgo_size`（绝对值）/ `*_max_rgo_size_modifier`（%）或效果 `change_max_raw_material_workers` | 与 `plantation_cap`/`rural_building_cap` 联动 |
| 启用投产度系统 | define `ESTABLISHMENT_SYSTEM_ENABLED = yes` | 同时要检查另 6 个配套常量与 `startup_ramp_target`（数值全在 `default_values.txt`，但目标单位未注明） |
| 建筑产物/投入 | 生产方式里 `produced` / `output` / `<goods> = <amount>` | 建筑的 `output` 字段在原版**不使用** |
| 建筑效果钩子 | `on_built` / `on_destroyed`（建筑）、`on_construction_started` / `on_construction_ended`（未登记） | 后两个原版各 1 处，谨慎复用 |

**硬编码边界**：

- RGO 只有 laborers + slaves 能工作；`pop_types` 里没有可注册的开关。
- RGO 的等级上限明细 UI（`GetMaximumMaxRGOWorkersInfoForUI`）与成本（`Location.GetRGOCost`）是引擎侧。
- 真正的原产升级走 `construction_rgo_<method>` 代码路径，脚本里无定义文件。
- 建筑升级链的"淘汰→继承"结算、补贴的每月扣款、亏损裁员/复雇节奏（10%）、破产降级概率都是引擎流程，只能改常量。
- `game\common` 三区的边界：建筑内容在 `in_game`，默认数值在 `main_menu`，define/常量在 `loading_screen`。

## 九、中文检索键

概念（`game_concepts_l_simp_chinese.yml`）：`game_concept_rgo`（原产）、`game_concept_resource_gathering_operations`（原料生产）、`game_concept_rgo_pops`（劳工或奴隶）、`game_concept_raw_material(s)`（原材料）、`game_concept_produced_goods`（制成品）、`game_concept_goods`（商品）、`game_concept_building`（建筑）、`game_concept_building_type`（建筑类型）、`game_concept_building_category`（建筑类别）、`game_concept_production_method`（生产方式）、`game_concept_employ/employment`（雇佣）、`game_concept_employment_system`（雇佣制度）、`game_concept_establishment`（**投产度**）、`game_concept_subsidy` / `game_concept_subsidization`（补贴）、`game_concept_output`（产出）、`game_concept_development`（发展度）、`game_concept_location_rank`（地点等级）、`game_concept_foreign_building`（外国建筑）。

RGO/等级/需求分组（`goods_l_simp_chinese.yml`）：`rgo_mining` 矿场、`rgo_farming` 农场、`rgo_forestry` 林场、`rgo_hunting` 狩猎场、`rgo_gathering` 采集场；`guild_input` 行会投入、`workshop_input` 工坊投入品、`manufactory_input` 工场投入、`mills_input` 工厂投入、`building_maintenance` 建筑维护、`building_construction` 建筑建造。

等级名（`economy_l_simp_chinese.yml`）：`megalopolis` 大都市、`city` 城市、`town` 集镇、`rural_settlement` 乡村。

界面：`build_location_lateralview.gui`（建造列表）、`building_view.gui`（建筑详情）、`production_lateralview.gui`（178KB，生产总览）、`goods_production_lateralview.gui`、`location_production_lateralview.gui`、`expand_raw_goods_lateralview.gui`（原产扩张）、`food_production_lateralview.gui`、`shared\production_method_details.gui`、`shared\production_tooltips.gui`、`attribute_columns\building.gui` / `goods.gui` / `production_method.gui`。

关键本地化键：`BUILDING_ESTABLISHMENT_*`（投产度面板，`interfaces_l_*` 318–330）、`PM_OUTPUT_LEVEL*`（生产方式产出明细，5193–5203）、`RAW_MATERIAL_LOCATION_DETAILS_TT`（原产地点提示，4317）、`RAW_MATERIAL_SIZE_TT*`（产出构成，4315–4316）、`PROFIT_EXPAND_RGO_TOOLTIP_*`（扩张利润预测，4322–4326）、`PROFIT_PER_RGO_*`（利润构成，1328–1333）、`LOCATION_VIEW_EXPAND_RGO_*` / `EXPAND_RGO_IN_BEST_LOCATION_*`（扩张按钮，1661–1686）、`AUTOMATED_SYSTEM_EXPANDRGO` / `AUTOMATED_SYSTEM_RGO`（自动化，871–882）、`CONSTRUCTION_ESTABLISHMENT_CONFLICT_WARNING`（3329）、`RAW_MATERIAL_BUILD_TIME_T`（4318）。
