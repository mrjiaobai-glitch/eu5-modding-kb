# 原版解析：贸易与市场（vanilla trade & market）

版本基准：EU5 1.3.x。核心文件：`common\goods\`（6 文件，74 种商品）、`common\prices\`（8 文件）、`common\goods_demand\`（7 文件）、`common\production_methods\`（3 文件）、`common\generic_actions\markets.txt`（550 行，市场与贸易操作）、`loading_screen\common\defines\00_defines.txt` 的 **`NMarket`（1754–1837）** 与 **`NEconomy`（1839–1989，含 `TRADE_PATH_*`）**。机制描述引自游戏内百科词条（`game_concepts_l_simp_chinese.yml`）。

> **上游见生产端篇**：商品从哪来（原产 RGO vs 建筑生产）、建筑等级上限、生产方式与建造需求，见 `vanilla\vanilla-production-and-buildings.md`；本篇只讲商品进入市场**之后**的定价、流动与分配。

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `market` | **市场** | 商品买卖场所，以市场中心为基础 |
| `market_center` | 市场中心 | 集中经济行为的地点，**所有者可控制入市资格** |
| `market_access` | 市场准入 | 地点到市场中心的贸易便利度 |
| `merchant_capacity` | **商人容量** | 能在市场内进出口多少商品 |
| `merchant_power` | **商人力量** | 供给有限时决定出口交割优先顺序 |
| `trade_profit` / `trade_income` | 贸易利润 / 贸易收入 | 价差收益 / 卖货收入 |
| `stockpile` | 库存 | 市场储备的商品量 |
| `protectionism` | 保护主义 | 提高本地控制度，抵消市场吸引力 |
| `sound_toll` | 通行费 | 特定海峡控制者向穿行贸易收费 |
| `maritime_presence` | 海事存在 | 沿岸海区的海上影响力，商人力量重要来源 |
| `trade_orders` | 贸易指令 | 不指定目的地的自动进出口 |
| `trade_company` | （贸易公司） | 附庸类型 `subject_types\trade_company.txt` |

## 一、双层模型总览

```
市场（market）= 市场中心 + 按「市场准入」与「保护主义」覆盖的地点
   ├─ 市场内：供需决定价格，库存（stockpile）缓冲
   └─ 市场间：用「商人容量」运货赚「进出口价差」；供给有限时按「商人力量」排队交割
        ↓
   贸易收入 → 按阶层权力分配给王室与阶层（仅王室份额入国库）
```

⚠️ 与 EU4 的根本差异：**没有贸易节点/流向**，而是"市场内定价 + 市场间点对点商品流动"。

## 二、商品（`common\goods\`，74 种）

| 文件 | 数量 | 内容 |
|---|---|---|
| `00_raw_materials.txt` | 36 | horses / clay / sand / coal / iron / copper / goods_gold / silver / stone / tin / lead / silk / dyes / incense / tea / cocoa / coffee / fiber_crops / ivory / lumber / salt / medicaments / gems / pearls / amber / saltpeter / alum / wine / elephants / marble / mercury / saffron / pepper / cloves / chili / wool |
| `01_plantation_goods.txt` | 3 | cotton / sugar / tobacco |
| `02_produced_goods.txt` | 21 | tar / porcelain / naval_supplies / firearms / cannons / weaponry / glass / steel / cloth / fine_cloth / liquor / beer / paper / books / jewelry / leather / tools / masonry / lacquerware / pottery / furniture |
| `03_food.txt` | 13 | wild_game / fur / fish / wheat / maize / rice / millet / legumes / potato / livestock / olives / fruit / beeswax |
| `04_special.txt` | 1 | slaves_goods |

**字段**（权威：`goods\readme.txt`）：`is_slaves`、`block_rgo_upgrade`、`inflation`、`base_production`、`color`、**`food`**（提供多少食物）、**`transport_cost`**（默认 1）、**`default_market_price`**（默认 1）、**`category`**（raw_material/produced）、**`method`**（mining/farming/hunting/gathering/forestry）、`ai_rgo_size_importance`、`demand_add`、`demand_multiply`、`location_potential`（setup 时为假会报错）、`custom_tags`。

## 三、市场（Market）

### 结构与准入

- **市场中心**：官方词条强调"该地点的所有者将从中获得一些优势，其中最重要的是，**该国能控制谁有资格进入该市场**"。
- **市场准入（market_access）**：取决于**地点到市场中心的距离**。三大作用：
  1. 接入度高的市场**吸引力更高** → 地点倾向加入最近的市场经济
  2. 各地**按其市场准入度依次**从市场购买商品与食物（交割顺序）
  3. 低接入度**降低建筑产出与 RGO 利润**（损害本地产业）
- 提高准入：海洋与河流天然加成；**修建道路、海军（naval category）、新建市场**。

### 四类市场（按你与它的关系）

| 类型 | 条件 | 能力 |
|---|---|---|
| **国内市场** `domestic` | 涵盖你直接管辖的地点 | 统筹治下所有必需品（政府建造、建筑维护、POP 需求、粮食）；**关键商品短缺 → 建筑停工、招募中断** |
| **贸易市场** `trading` | 你无领土但有**外资建筑**提供的商人容量 | 无需管该市场需求/粮食/人口，可用其商人容量**盈利或供应国内市场** |
| **外国市场** `foreign` | 无领土、无商人容量 | 不能直接贸易，但可借**贸易范围内其他市场**的容量与它贸易 |
| **遥远市场** `distant` | 在所有国内市场的**贸易范围（`trade_range`）**之外 | 完全无法贸易 |

### 市场操作（`generic_actions\markets.txt` + `prices\00_hardcoded.txt`）

| 操作 | 价格 | 条件要点 |
|---|---|---|
| `create_market` 建立市场 | **gold 250 + scaled_gold 5** | `should_execute_price = no` |
| `relocate_market` 迁移市场 | **gold 800** | 所选市场中心须属于你；目标须为**城市**、属于该市场、属于你、非现中心 |
| `destroy_market` 销毁市场 | **stability 50 + prestige 25** | — |
| `create_trade` 建立贸易 | — | 选 from/to 市场 + **须 `can_find_trade_route` 找得到路线**；受 `is_embargoed_by`、`trade_isolation`（可 `gives_isolation_exemption_to` 豁免）限制 |

四者均 `ai_tick = never`（AI 不主动执行；`automation_tick_frequency = 12`）。

### 市场归属竞争（`NMarket` 吸引力清单）

| 因素 | 加成 | 备注 |
|---|---|---|
| 同省份 / 同地区 | **+0.2 / +0.1** | 同区域及以上为 0；不叠加，取**最小地理单位** |
| 国家通用语言 / 同语系 | +0.05 / +0.01 | `COUNTRY_COMMON_LANGUAGE_*` |
| 地点主导语言 / 同语系 | +0.05 / +0.01 | `LOCATION_DOMINANT_LANGUAGE_*` |
| 语言力量 | +0.1 | `MARKET_LANGUAGE_POWER_ATTRACTION` |
| 保护主义 | 负向 | 与市场吸引力**相互抵消** |

**市场语言**：市场中心大多数**市民（burghers）**使用的语言；说当地市场语言的市民**不会同化为其他文化**（贸易系统与文化同化的挂钩点）。

### 枯萎机制（withering）

地点数 **≤ 5**（`MARKET_WITHERING_LOCATION_THRESHOLD`）且持续 **24 个月**（`MARKET_WITHERING_GRACE_MONTHS`）、并且 **50% 地点被别的市场超越**（`MARKET_WITHERING_OUTCLASSED_FRACTION`）→ 市场被标记枯萎：AI 获得销毁倾向，玩家收到 `on_market_withering_started` 事件。

其他相关常量：`MARKET_CREATION_MONTHS = 3`（建立耗时）、`MARKET_TRADE_CHANGE_PER_CLICK = 1`、`MARKET_BASE_ACCESS = 1`（100%）、`MARKET_BASE_DISTANCE_FACTOR = 0.005`（每像素损失）、`MARKET_OPEN_SEA_DISTANCE_FACTOR = 1.5`（公海差）、`MARKET_SEA_DISTANCE_FACTOR = 0.75`（海上传播更好）、`MARKET_SEA_TO_LAND_DISTANCE_FACTOR = 0.5`、`MARKET_DOWNSTREAM_FACTOR = 0.5` / `MARKET_UPSTREAM_FACTOR = 0.9`、`MARKET_NO_PORT_EXTRA_DISTANCE = 0.1` / `MARKET_PORT_EXTRA_DISTANCE = 0.02`。

## 四、价格、供需与库存

**供给（supply）**（官方词条）= 建筑生产 + 原材料采集 + **从其他市场进口**
**需求（demand）** = 建筑投入 + POP 生活需求 + 各类建造 + **出口到其他市场**
**库存（stockpile）** = 市场储备量；供给 > 需求时补充，反之消耗；市场内**建筑/建造/单位/POP 都用库存满足需求**。

价格常量（`NMarket`）：

| 常量 | 值 | 含义 |
|---|---|---|
| `MONTHLY_PRICE_CHANGE` | 0.05 | 每月向目标价移动差值的 5% |
| `MIN_PRICE_IMPACT` / `MAX_PRICE_IMPACT` | −0.33 / **3.0** | 价格影响上下限（最高涨到基准 3 倍） |
| `DEMAND_ELASTICITY_COEFFICIENT` | 0.40 | 价格高于基准 100% → 需求降 40% |
| `DEMAND_ELASTICITY_FLOOR` | 0.35 | 需求不低于基准的 35% |
| `BURGHER_TRADE_IMPACT_ON_SUPPLY_SCALE` / `TRADE_IMPACT_ON_SUPPLY_SCALE` | 0.1 / 0.75 | 贸易对供给的影响权重（市民贸易按 0.1 缩放） |
| `BURGHER_TRADE_IMPACT_ON_DEMAND_SCALE` / `TRADE_IMPACT_ON_DEMAND_SCALE` | 0.25 / 0.75 | 贸易对需求的影响权重 |
| `SUPPLY_AND_DEMAND_STABILITY_OFFSET_CONSTANT`（在 `NEconomy`） | 1 | 计算市价时加到供需上，越大价格越稳 |
| `PRICE_SCALE_FROM_ECO_BASE`（`NEconomy`） | 0.1 | 价格随经济基础缩放 |
| `MARKET_MIN_STOCKPILE_TO_ALLOW_EXTRA_TRADE` / `MARKET_STOCKPILE_FULL_EXTRA_TRADE_THRESHOLD` / `MARKET_STOCKPILE_PERCENTAGE_FOR_EXTRA_TRADE` | 0.5 / 0.75 / 0.05 | 库存溢出转化为额外贸易供给的三段阈值 |
| `STOCKPILE_TRADE_IMPACT_ON_SUPPLY_SCALE` | 0 | 库存溢出对**价格形成**的影响（0 = 不影响价格，只喂贸易路由） |
| `MARKET_WASTED_MANUAL_TRADE_CAPACITY_TRESHOLD` | 0.1 | 手动贸易容量的浪费阈值 |

## 五、贸易（市场间流动）

官方词条原文要点：

- **贸易 = 一系列商品由一个市场到另一市场的流动**；商品在不同市场间买入卖出，为发起者带来**贸易利润**；市场间距离与经手数量共同决定所需**商人容量**。
- **进口** = 商品转入某市场 → **增加该市场供给**。
- **出口** = 商品转出某市场 → **增加该市场需求**；多项贸易尝试出口同一商品时，**按商人力量交付**。
- **贸易利润** 主要取决于商品在**出口市场与进口市场间的价格差**；维护成本削减利润，部分贸易还须缴 **sound_toll（通行费）**。
- **商人力量**：同一市场内各国商人力量决定**谁优先获得出口权**；市场内商品供给有限时**按商人力量依次交割**。
- **贸易指令（trade orders）**：分配商人容量自动进出口特定商品、**无需指定目的地**；**AI 每天选择最优路线**。

贸易路径成本（`NEconomy`）：

| 常量 | 值 | 含义 |
|---|---|---|
| `TRADE_PATH_UPSTREAM_COST_MULTIPLIER` | −0.2 | 逆流而上仍优于走陆路 |
| `TRADE_PATH_DOWNSTREAM_COST_MULTIPLIER` | −0.5 | 顺流而下最便宜 |
| `TRADE_PATH_RIVER_CROSSING_COST_MULTIPLIER` | +0.3 | 渡河加价 |
| `TRADE_SEA_MULTIPLIER` / `TRADE_SEA_MARITIME_IMPACT` | 0.3 / −0.5 | 海运便宜，但受海事存在影响 |
| `TRADE_PORT_COST` | 5 | 装卸固定成本 |
| `TRADE_PATH_TRAVEL_COST_MULT` | 10 | 路程成本倍数 |
| `TRADE_IMPACT_ON_SUPPLY_AND_DEMAND` | yes | 贸易是否影响供需 |
| `TRADE_BURGHER_PROFIT_SCALE` | 0.1 | 市民利润缩放 |

## 六、商人容量与力量的来源

| 来源 | 修正键 | 原版取值示例 |
|---|---|---|
| 建筑（本地） | `local_merchant_capacity` / `local_merchant_power` | 容量 1~5；力量 0.05~2 |
| 建筑（按类） | `merchant_capacity_from_building` / `merchant_power_from_building` / `merchant_capacity_from_maritime` | 0.1~1.0 / 0.2 |
| **外资建筑**（`building_types\foreign_buildings.txt`） | `merchant_capacity_from_building` + `merchant_power_from_building` | 0.1~0.3 / 0.2 —— **"贸易市场"的来源** |
| **海事存在** | `local_/global_maritime_presence_modifier`、`merchant_power_from_maritime(_modifier)` | 官方明言"市场内沿岸海区的海事存在是**商人力量的重要来源**" |
| 市民 POP | `local_trades_per_burgher` | 0.5（`pop_types`） |
| 修正 | `local_merchant_capacity_modifier`、`global_merchant_capacity_modifier`、`global_merchant_power` | — |

海事存在补充：和平时期靠**拥有港口**随时间增长；**封锁**快速削减；**海区中的海军**快速重建；高海事存在还降低 proximity 衰减（间接影响控制度）。

## 七、贸易收入分配

**贸易收入**按**阶层权力**分配给王室与各阶层；**只有属于王室的那一份（按王权比例）进入国库**。
**贸易支出** = 从市场购买商品 + **商人维护费**（`merchant_maintenance_efficiency` 可降低）。

相关常量：`ECONOMICAL_BASE_FROM_TRADE_VALUE = 0.2`、`ECONOMICAL_BASE_FROM_TRADE_PROFIT = 0.2`（`NEconomy`）。

## 八、贸易战与限制手段

| 手段 | 机制 | 关键键 |
|---|---|---|
| **封锁 blockade** | 敌方海军停在港口外海区；**所需舰船数取决于受影响港口的人口与发展度**；效果：降低粮食产出、海事存在、发展度、控制度、繁荣度 | `blockade_force_required`、`blockade_efficiency`、`blockade_capacity`（单位属性） |
| **私掠者 privateer** | "对任何潜在敌对者的海事存在制造严重破坏" | `hire/dismiss_privateer_cost_modifier`、`privateer_maintenance_cost_modifier`、`privateer_durability`、`can_hire_privateers`；`NEconomy` 的 `PRIVATEER_STEAL_MONEY_FACTOR = 0.01`、`PRIVATEER_OPINION_*` |
| **禁运 embargo** | `force_embargo` / `subject_embargo` 交互；被禁运方无法与你贸易（贸易可见性条件直接查 `is_embargoed_by`） | `common\country_interactions\force_embargo.txt` |
| **通行费 sound_toll** | 世界特定海峡（**厄勒海峡 oresund、伊兹米特湾 gulf_izmit** 等）的控制者可向穿行贸易收费；须控制关联陆地地点，收入受**控制度**影响 | `SOUND_TOLL_FACTOR = 0.10`（`NEconomy`）、`sound_toll_exemptions_broken` CB；**海峡清单定义在 `map_data\default.map` 的 `sound_toll` 段（原版 3 个：`oresund = helsingor` / `gulf_izmit = constantinople` / `strait_hormuz = hormuz`），见 `vanilla\vanilla-map-and-geography.md` §三** |
| **市场保护主义** | `local/global_trade_protection_factor` — **提高本地控制度**，并与市场吸引力相互抵消 | — |
| **贸易孤立** | 禁止外国人贸易的修正，可给特定国家豁免 | `trade_isolation`、`gives_isolation_exemption_to` |
| **贸易公司** | 附庸类型，宗主支付其成本 | `common\subject_types\trade_company.txt`、`subject_pays_trade_company_cost_modifier` |
| **贸易效率** | 陆运/海运/经领土过境/外资出口 | `trade_land_efficiency`、`trade_sea_efficiency`、`global_trade_through_owned_territory_efficiency`、`foreign_export_from_market_efficiency`、`local_trade_embark_disembark_efficiency` |

> **上面这些开关有一半挂在"条约关系"上**：禁运（`embargo_nation`）、通行费豁免（`sound_toll_exemption`）、孤立豁免（`exclusive_trade_rights_with_isolated`）、**取消市场保护主义（`lifts_trade_protection`）**、双向市场吸引力（`trade_to_first/second`）、禁止对方在你境内建外国建筑（`block_foreign_buildings`）、拒入市场（`deny_market_access`）、转口贸易（`divert_trade` / `forced_divert_trade`）**全都是 `common\scripted_relations\` 里的条约定义**——见 `vanilla\vanilla-diplomacy.md` §五。

## 九、粮食的三级体系（特殊机制）

官方词条把食物分三层（`game_concept_food_desc`）：

1. **local_food（本地）**：地点内生产与消费，盈余/短缺上交省级
2. **provincial_food（省级，最重要）**：省份产不足则**贮藏下降**；贮藏耗尽则**全省饥荒**；在此之前省份会尝试**从所属市场买粮**
3. **market_food（市场级）**：省内所有省份满贮藏时**卖粮**、贮藏低时**买粮**；只要市场粮食足够就无人饿死

价格随市场库存量变动；盈余省份给所有者带来收入，赤字省份产生支出。可**建立食物原材料贸易在市场间运粮**。

常量：`FOOD_PRICE = 0.05`、`FOOD_PRICE_IMPACT_ON_PRICES = 0.25`、`FOOD_CAPACITY_FACTOR = 1.0`、`FOOD_PROPORTION_TO_PRESERVE_IN_MARKET_IF_POSSIBLE = 0.25`、`FOOD_PROVINCE_LOCAL_RESERVE_RATIO = 0.75`、`FOOD_PASS2_MAX_MONTHS_OF_DEFICIT = 3.0`、`MARKET_FOOD_STOCKPILE_TRESHOLD_MONTHS = 24.0`。

## 十、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 商品本身 | `common\goods\`（`food`/`transport_cost`/`default_market_price`/`category`/`method`/`demand_add`） | `location_potential` 为假会在 setup 报错 |
| 商品需求 | `common\goods_demand\`（pop/army/navy/building_construction 各文件） | 需求分组在 `goods_demand_category\` |
| 价格与弹性 | defines `NMarket`（`MONTHLY_PRICE_CHANGE`、`MAX_PRICE_IMPACT`、弹性系数） | 全局生效，影响所有市场 |
| 市场归属/吸引力 | `NMarket` 的 `*_MARKET_ATTRACTION` 系列、`MARKET_BASE_DISTANCE_FACTOR` | 与保护主义相互抵消 |
| 贸易成本/路径 | `NEconomy` 的 `TRADE_PATH_*`、`TRADE_SEA_*`、`TRADE_PORT_COST` | — |
| 商人容量/力量来源 | 建筑（`local_merchant_capacity`/`power`、`merchant_*_from_building`）、`maritime_presence` 相关修正 | 外资建筑决定"贸易市场" |
| 市场操作花费 | `common\prices\00_hardcoded.txt`（create/relocate/destroy_market） | 也可用 `*_cost_modifier` 修正 |
| 贸易行动 | `common\generic_actions\markets.txt` | `ai_tick = never`（AI 不自主执行） |
| 贸易公司 | `common\subject_types\trade_company.txt` | — |
| 粮食体系 | `NMarket` 的 `FOOD_*` 系列、`goods` 的 `food` 值 | 三级分配是硬编码流程 |

**硬编码**：供需结算与价格公式主体、市场归属最终裁决、贸易寻路（`can_find_trade_route`）、**按市场准入的交割顺序**、**按商人力量的出口排队**、粮食三级分配流程。

## 十一、中文检索键

概念：`game_concept_market`（市场）、`market_center`（市场中心）、`market_access`（市场准入）、`market_attraction`（市场吸引力）、`protectionism`（保护主义）、`merchant_power`（商人力量）、`merchant_capacity`（商人容量）、`trade`（贸易）、`trade_profit`（贸易利润）、`trade_income`（贸易收入）、`trade_orders`（贸易指令）、`import` / `export`（进口/出口）、`supply` / `demand`（供给/需求）、`stockpile`（库存）、`market_balance`（供需结余）、`domestic/trading/foreign/distant_market`（四类市场）、`market_language`（市场语言）、`maritime_presence`（海事存在）、`blockade`（封锁）、`privateer`（私掠者）、`sound_toll`（通行费）、`market_food`（市场食物）。
界面：`markets_overview.gui`、`selected_market_view.gui`、`trade_overview.gui`、`trade_details_lateral_view.gui`、`goods_overview.gui`、`goods_details.gui`、`import_export_lateralview.gui`。
