# 原版机制：贸易公司（trade companies）

> **什么时候看**：改贸易公司、写公司相关事件、调 `trade_company_buildings` 或 `subject_types\trade_company` 时。
> **依据**：`common\subject_types\trade_company.txt`、`common\building_types\trade_company_buildings.txt`（541 行 / 6 建筑）、`common\advances\0_age_of_reformation.txt`、本地化。**EU5 1.4.5**，2026-10-30。
> **体量**：约 5 分钟

## 一、贸易公司是什么
- **依附型主体（subject）**，`subject_types\trade_company.txt`：
  ```
  trade_company = {
  	type = building          # ★ 建筑型主体（由建筑创建）
  	color = subject_trade_company ; level = 1
  	subject_pays = subject_pays_trade_company
  	has_overlords_ruler = no ; can_be_annexed = no
  	subject_can_cancel = no ; overlord_can_cancel = yes
  	fleet_basing_rights = yes ; has_limited_diplomacy = yes
  	government = republic ; minimum_opinion_for_offer = 100
  	strength_vs_overlord = -0.1 ; diplomatic_capacity_cost_scale = 0.5
  	great_power_score_transfer = 0.75
  	institution_spread_to_overlord = monthly_institution_spread_severe
  	institution_spread_to_subject  = monthly_institution_spread_severe
  	subject_modifier = { export_efficiency = tiny_trade_efficiency_bonus  max_tariff = 0.05 }
  }
  ```
- **不能外交建立**（`visible_through_diplomacy = always = no`），也**不能吞并**——只能由建筑创建、由宗主取消。

## 二、怎么成立（建总部，不是签条约）
1. **解锁**：时代 4（宗教改革）的革新 `unlock_subject_type = trade_company`（`0_age_of_reformation.txt:211`）。
2. **建总部** `trade_company_headquarters`（中文 **"贸易公司总部"**）：
   - `country_potential`：本国**还没有**总部（`total_effective_building_levels:trade_company_headquarters < 1`）、`NOT has_variable = building_trade_hq`、`num_relations_above_limit <= 0`。
   - `allow`：目标 Market 处于本国的 **trade range**；且本国 `trade_company_regions` 列表里**没有**该地区（`region`）。
   - 建造中会设 `building_trade_hq` 变量（防止同时造两个），完成/取消时清除。
3. **`on_built`（核心）**：
   ```
   create_building_country_in_location = { … }   # 在该地创建"建筑型国家"
     change_government_type = republic
     change_heir_selection = heir_selection:bank_selection
     make_subject_of = { target = overlord  type = subject_type:trade_company }
   change_building_owner = 该公司           # 总部归公司所有
   公司的 capital = 该地；home_region 变量 = 该地 region
   宗主：trade_company_regions += region（一个地区只能有一家公司）
         trade_company_subjects += 公司（宗主记录所有公司）
   ```
   —— 即：**总部建 = 自动生成一个共和国公司并成为你的附庸**，总部建筑转给公司。

## 三、六个公司建筑（`trade_company_buildings.txt`）
| 建筑 | 中文 | 说明 |
|---|---|---|
| `trade_company_headquarters` | 贸易公司总部 | 唯一；创建主体；**`capital_to_overlord_modifier = { number_of_satellite_trade_buildings = 3 }`（支持 3 个卫星建筑）** |
| `trade_company_garrison` | 公司驻防站 | 卫星 |
| `trade_company_harbor` | 公司物流港 | 卫星 |
| `trade_company_local_venture` | 公司业务处 | 卫星 |
| `trade_company_foreign_influence` | 公司拓殖区 | 卫星 |
| `trade_company_governance` | 公司军事处 | 卫星 |

共性：`is_foreign = yes`、`pop_type = burghers`、`own_or_overlord_relation_needed = trade_access`、`category = trade_category`、`max_levels = trade_company_building_max_level`；卫星只能由公司建（`can_build_satellite_trade_building`，并登记到 `trade_company_subjects` 变量）。

**总部修正**（`modifier` + `raw_modifier`）：
| 字段 | 值 |
|---|---|
| `merchant_capacity_from_building` | 0.2 |
| `building_owner_maritime_presence` | 0.02 |
| `building_owner_overlord_maritime_presence` | 0.05 |
| `scaled_gold_to_building_owner` | 0.2 |
| `scaled_gold_to_building_owner_overlord` | **0.5**（一半收入给宗主） |
| `local_crown_estate_power` | −0.2 |
| `raw_modifier`：`local_monthly_development` 0.01 / `local_monthly_prosperity` 0.001 / `local_build_buildings_efficiency` 0.2 | |

## 四、关键变量（改脚本要知道）
| 变量 | 作用 |
|---|---|
| `building_trade_hq`（国家） | 总部建造中标记，防重复 |
| `trade_company_regions`（国家，列表） | 已开公司的地区（**一地区一家**） |
| `trade_company_subjects`（国家，列表） | 该国所有贸易公司 |
| `trade_company_home_region`（公司） | 公司的母地区 |

## 五、改 vs 硬编码 / 坑
| 想改 | 动这里 | 注意 |
|---|---|---|
| 解锁时代 | `advances` 的 `unlock_subject_type = trade_company` | 在 `0_age_of_reformation.txt` |
| 公司条款 | `subject_types\trade_company.txt` | `can_be_annexed = no` / `subject_can_cancel = no` 是硬设计 |
| 卫星数量 | 总部 `capital_to_overlord_modifier` 的 `number_of_satellite_trade_buildings` | 默认 3 |
| 总部收入分成 | `scaled_gold_to_building_owner_overlord` | 0.5 |
| 区域限制 | `trade_company_regions` 变量列表逻辑 | 一地区一家，改时注意重复建 |

**坑**：
1. 公司是**建筑型主体**（`type = building`），不是普通附庸——外交界面里建不出来。
2. 总部**建成才创建公司**；`on_built` 里会改建筑归属，别在别的 `on_built` 里抢。
3. 公司**不能取消附庸关系、不能被吞并**；宗主可以取消。
4. 中文名：贸易公司总部 / 公司驻防站 / 公司物流港 / 公司业务处 / 公司拓殖区 / 公司军事处。

## 六、相关档
- 附庸类型总表：`vanilla\vanilla-diplomacy.md`、`fields\common-subject_types.md`
- 建筑字段：`fields\common-building_types.md`（含 `raw_modifier`、`on_built`）
- 市场/贸易：`vanilla\vanilla-trade-and-market.md`
- 革新：`vanilla\vanilla-tech-and-age.md`（`unlock_subject_type`）
