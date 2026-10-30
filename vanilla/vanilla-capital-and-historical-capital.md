# 原版机制：首都与历史首都（capital / historical capital）

> **什么时候看**：想改首都加成、做"迁都后旧都仍保留部分效果"的机制、写 `is_capital` 类条件、或在建筑里用 `capital_modifier` 时翻这篇。
> **依据**：`main_menu\common\static_modifiers\location.txt`（capital/province_capital/historical_capital 三块）+ `main_menu\localization\english\game_concepts_l_english.yml` 的 `game_concept_historical_capital_desc` + `script_docs`（触发/效果/scope）+ 原版建筑用法。**EU5 1.4.5**，2026-10-30。
> **体量**：约 5 分钟

## 一、四个"首都"别混

| 概念 | 是什么 | 判定/入口 |
|---|---|---|
| **首都** `capital` | 国家当前首都，享受完整首都加成 | `is_capital`（location）；`set_capital`（country→location 效果） |
| **历史首都** `historical_capital` | **曾经做过"拥有它的国家"首都**的地点；一国**同时只有一个** | `is_historical_capital`（location） |
| **省都** `province_capital` | 省份内的行政中心（另一套静态修正） | `is_province_capital`（location） |
| **原始首都** `original_capital` | country→location 的 **scope link**，原版 advance 条件里在用（如"原始首都在欧洲"） | `original_capital`（scope link）；⚠️ 引擎文档写 "Unknown"，语义未确认 |

> 还有 `counts_as_capital`：**"是首都或历史首都"**——按"算不算首都"做判定时用它，别用 `is_capital` 硬套。

## 二、历史首都的规则（概念原文意译）
1. 定义：一个**曾经是**"拥有它的国家"首都的地点。
2. **唯一性**：一国同一时间只能有**一个**历史首都。
3. **失效**：首都再次迁移时，**上一个历史首都失去身份**（不是累积列表）。
4. **建筑保留**：任何"建在首都才有特殊效果"的建筑，在历史首都**继续生效**。
5. **小份加成**：历史首都保留**一部分**首都加成（见 §三）。
6. **proximity 不叠加**：如果该地已从**当前首都**获得更高 proximity，历史首都的 proximity 源**不生效**。

> 没有"历史首都列表"数据文件——身份由引擎在**换都时自动派生**；改它只能靠 `set_capital` 触发迁都。

## 三、三套静态修正对照（`static_modifiers\location.txt`）

| 字段 | 首都 `capital` | 历史首都 `historical_capital` | 省都 `province_capital` |
|---|---|---|---|
| `local_proximity_source` | **100** | **80** | — |
| `local_max_control` | **0.20** | **0.10** | — |
| `local_population_capacity_modifier` | **0.10** | **0.025** | 0.05 |
| `local_migration_attraction` | 0.025 | 0.01 | 0.01 |
| `local_nobles_desired_pop_scaled` / `local_clergy_desired_pop_scaled` | 0.002 / 0.002 | 0.0005 / 0.0005 | 0.001 / 0.001 |
| `local_fort_maintenance_efficiency` | **1.00** | **0.50** | — |
| `free_building_levels` | **5** | **2** | 2 |

**"小份"具体是**：proximity 80%（相对首都）、控制 50%、人口容量 **25%**、迁入吸引 40%、要塞维护 50%、免费建筑等级 40%。

> 这三个块都是 `game_data = { category = location }` 的**静态修正**；改它们 = 改全局首都表现，注意影响所有国家。

## 四、建筑侧怎么用
- 建筑 readme 的三个首都字段：`capital_modifier`（建在首都时给地点修正）、`capital_country_modifier`（给国家修正）、`capital_to_overlord_modifier`（给宗主，仅当建筑主是附庸）；都随**建筑等级 × 货物接入**缩放。
- "建在首都才生效"的效果会**延续到历史首都**（概念原文）；写自定义建筑时别只判 `is_capital`，否则迁都后失效——要"算首都"就用 `counts_as_capital`。
- 原版反例：`capital_buildings.txt` / `unique_buildings.txt` 的 `location_potential` 里同时写
  `is_capital = no` + `is_historical_capital = no` → 这类建筑**首都和历史首都都不能建**。

## 五、触发器 / 效果 / scope 速查
| 名称 | 类型 | 说明 |
|---|---|---|
| `is_capital` | trigger（location） | 是否当前首都 |
| `is_historical_capital` | trigger（location） | 是否历史首都（"of its owner"） |
| `counts_as_capital` | trigger（location） | 是否"首都或历史首都" |
| `is_province_capital` | trigger（location） | 是否省都 |
| `has_road_to_capital` | trigger（location） | 是否有通往首都的路（原版首都建筑前置） |
| `proximity` | trigger（location） | 到**拥有者首都**的 proximity 值（可比较） |
| `set_capital` | effect（country） | 把目标地点设为首都；**迁都→旧都降为历史首都** |
| `original_capital` | scope link（country→location） | 原始首都；⚠️ 语义未确认（引擎文档 "Unknown"） |

## 六、坑
1. **别找"历史首都列表"**：没有这个文件；它是换都时引擎派生的状态。
2. **`is_capital` ≠ `counts_as_capital`**：前者迁都后就 false，后者仍 true。
3. **历史首都唯一且会失效**：连续迁都 A→B→C 时，历史首都从 A 变成 B，A **失去**身份。
4. **proximity 有"取高不叠加"**：历史首都的 80 只在该地没有更高首都 proximity 时才有意义。
5. **`original_capital` 不是历史首都**：它是 scope link（原始首都），与"曾经是首都"是两套东西。

## 七、相关档
- 首都/省都相关建筑字段：`fields\main_menu-static_modifiers.md`、`fields\common-building_types.md`
- proximity / control / 地图：`vanilla\vanilla-map-and-geography.md`
- 建筑与生产方式：`vanilla\vanilla-production-and-buildings.md`
- 触发器总表：`reference\triggers.md`；效果总表：`reference\effects.md`；scope 链：`reference\scopes.md`
