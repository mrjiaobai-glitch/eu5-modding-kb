# 原版解析：POP 与阶层机制（vanilla pop & estates）

> **一句话**：实测 8 种 POP 类型与阶层结构，给出增长、晋升、迁移、同化改宗与满意度叛乱阈值的原版常量，多在 `NPop`／`NCharacter` 段。
> **什么时候看**：要改 POP 类型、晋升／满意度／叛乱数值、阶层权力税收，或排查人口与就业问题时翻这篇。
> **体量**：164 行 · 约 8 分钟通读

版本基准：EU5 1.3.x。核心文件：`common\pop_types\00_default.txt`（240 行）、`common\estates\00_default.txt`（1275 行）、`common\employment_systems\`、`loading_screen\common\defines\00_defines.txt` 的 `NPop`（1591–1637）与 `NCharacter`（同化修正 1487–1506）。

## 一、POP 数据结构

每个 POP 拥有：**类型（pop_type）+ 规模（size）+ 文化 + 宗教 + 识字率 + 满意度 + 所属阶层（estate）**。
- `DISPLAY_SIZE = 1000`：1.0 规模 = 1000 人（界面以"千"为单位）
- 脚本操作全集（`common\effect_localization\pop_effects.txt`）：`add_pop_size`、`change_pop_religion`、`change_pop_culture`、`change_pop_type`、`split_pop`、`change_pop_owner`、`change_pop_allegiance`、`remove_pop_allegiance`、`add_pop_satisfaction`、`destroy_pop`

## 二、8 种 POP 类型（`pop_types\00_default.txt`）

| 类型 | 粮食消耗 | 晋升路径 `promote_to` | promotion_factor | 同化系数 | 关键 `literacy_impact` |
|---|---|---|---|---|---|
| `nobles` 贵族 | **20.0** | — | 0.1 | 0.05 | 文化传统 +0.2、月度控制 +0.01 |
| `clergy` 教士 | 5.0 | — | 0.1 | 0.05 | 改宗 +0.25、同化 +0.25 |
| `burghers` 市民 | 4.0 | — | 0.5 | 0.1 | 发展度 +0.25、贸易 +0.5、建筑速度 +0.25 |
| `laborers` 劳工 | 1.0 | soldiers, burghers | **1.5** | 0.5 | 生产效率、最大 RGO 规模 +0.1 |
| `soldiers` 士兵 | 5.0 | — | 1.0 | 0.5 | 防御 +0.1、人力 +0.1 |
| `peasants` 农民 | 1.0 | burghers, clergy, nobles, laborers, soldiers | 0.5 | 1.0 | 晋升速度 ×2.0、征召速度 +0.2 |
| `tribesmen` 部落民 | **0.0** | peasants | — | 0.0 | 部落晋升 0.5；距首都传播 −0.5、月度控制 −0.0005 |
| `slaves` 奴隶 | 1.0 | — | — | 1.0 | 奴隶满意度 −0.5 |

要点：
- `upper = yes` + `has_cap = yes`（贵族/教士/市民）→ 上层阶级**有比例上限**
- `grow = yes`（除奴隶外基本都可增长）；`tribal_rules = yes`（部落民特殊晋升）
- `city_graphics`：城市视觉权重（市民 1.0 → 农民 0.01）
- **部落民（`tribesmen`）单独立篇深挖**：POP / 阶层 / 文本 / 机制四层定义 + 18 项原版部落特权 + 杠杆清单（含"布尔键默认值先查 `auto_modifiers\country.txt`"这条硬规则）见 `vanilla\vanilla-tribes-and-tribal-estate.md`。
- 阶层映射：农民/劳工/士兵 → `peasants_estate`；贵族 → `nobles_estate`；部落民 → `tribes_estate`；另有 `dhimmi_estate`（齐米）、`cossacks_estate`（哥萨克）
- 市民 `counts_towards_market_language = yes`（决定市场语言，影响同化）

## 三、四大动态循环

### 1. 增长

**两个直接来源**（月增长，`NPop` 块；粮食盈余为主线）：

| 常量 | 值 | 含义 |
|---|---|---|
| `FOOD_SURPLUS_POP_GROWTH` | **0.0015** | 粮食有盈余时的基准月增长 |
| `GROWTH_FROM_FOOD_MULTIPLIER_MAX` | **2** | 盈余倍率上限（NEconomy；**1.3.x 旧档曾记 10，已订正**） |
| `GROWTH_FROM_FOOD_SURPLUS_THRESHOLD` | 1.25 | 盈余倍数达到该值才起效（NEconomy） |
| `FOOD_STORAGE_POP_GROWTH` | **0.001** | 有粮食储备时的额外月增长 |

**容量与阻尼**：`GROWTH_DAMPING_START = 0.6`——人口到容量 **60% 后增长递减**；容量由 **8 类 POP 各有** `local_<type>_desired_pop` / `_desired_pop_scaled`（location, percent）与 `global_<type>_desired_pop*`（country, percent）决定；市场侧 `REGIONAL_FOOD_BASE_CAPACITY = 100`、`FOOD_CAPACITY_FACTOR = 1.0`（NMarket）。

**修正键（mod 抓手，⚠️ 量纲不同）**：

| 键 | 作用域 | 量纲 |
|---|---|---|
| `local_population_growth` / `global_population_growth` | location / country | **percent** |
| `local_<type>_pop_growth` / `global_<type>_pop_growth`（8 类各有） | location / country | **addend** |
| `cap_maximum_population_growth_at_zero` | location | percent |

**需求侧（决定盈余真假）**：每 POP 食物消耗 **nobles 20 / clergy 5 / burghers 4 / soldiers 5 / laborers 1 / peasants 1 / slaves 1 / tribesmen 0**；`STARVING_ON_FOOD_DEMAND = 1.5`（饥饿时需求放大 1.5×）、`NEGATIVE_FOOD_ON_FOOD_DEMAND = 0.5`；`DEVELOPMENT_SCALE_ON_DEMAND = 0.05`、`PRODUCED_IN_MARKET_DEMAND = 0.1`。

**人口外流（"负增长"，`NPop`）**：`DISCRIMINATED_EMIGRATION_MONTHLY_RATE = 0.002`（受歧视文化每月外流）、`POST_CONQUEST_EMIGRATION_FRACTION = 0.0025` × `POST_CONQUEST_EMIGRATION_MONTHS = 12`（征服后 1 年外流）、`SETTLER_PULL_BASE = 2.0` / `SETTLER_PULL_RAMP_YEARS = 25`（边疆移民拉力）。

**死亡**：疾病（`NDisease` 块：`PANDEMIC_MIN_DEATHS = 50000` / `SEVERE_MIN_DEATHS = 1000` / `MODERATE_MIN_DEATHS = 50`）。⚠️ 战争/军队损耗是否直接扣 POP **未证**（见文末）。

> **与满意度解耦**：满意度常量（`BASE_SATISFACTION = 0.2`、文化档位 primary +0.25 / accepted +0.10 / tolerated +0.05 / discriminated −0.25）影响**移民与叛乱**，不直接进 growth；直接决定增员的只有粮食盈余/储备、容量阻尼、growth 修正键。
> **别混淆**：`promotion_factor` 是**类型间转移**（下一节），不改变总人口。

### 2. 晋升
- `MIN_SIZE_TO_BLOCK_PROMOTION = 0.5`（<500 人禁止晋升）
- 由**就业系统**（`common\employment_systems\00_default.txt`）间接控制：`equality`（一律平等）、`first_come_first_serve`（先建先填，`building_index × -1`）、`capitalism`（按 `building_potential_profit` 排序）及 4 个资本主义变体（基础设施/贸易/文化优先）
- 岗位长期空缺警报：`LONG_TIME_TO_FILL_EMPLOYEES_FROM_PROMOTES = 18` 个月（NLocation）
- 岗位盈余上限 `POP_JOB_SURPLUS_SCALE_CAP = 5`

### 3. 迁移
`migration_factor`：市民 0.5 > 农民/劳工 0.1 > 士兵 0.05 > 奴隶 0.01
驱动：`migration_attraction`（分 `rural_` / `non_rural_` 两个方向）；影响因素包括鼓励迁移（+1）、驱逐（−2）、瘟疫（−5）、建筑、城镇权利、宗教/文化修正等。

### 4. 同化与改宗
基础速率（`country_base_values`，`common\auto_modifiers\country.txt:89-90`）：`global_pop_assimilation_speed = 0.002`、`global_pop_conversion_speed = 0.002`

**同化修正表（`NCharacter` 1487–1506）**：同文化组 +0.3 / 同语言 +0.5 / 同语系 +0.1 / 市场语言 +0.4（本方或对方持有则 ±0.2）/ 主文化 **−1.0** / 被接纳文化 −0.75 / 容忍文化 −0.25 / 统一文化组 +1.0 / 目标对宗主好感 +0.2 / 宗主对目标好感 −0.1。

**改宗**：教士 POP 自身提供 `local_pop_conversion_speed_modifier = 0.25`；宗教建筑、改革、虔诚、宗教学校等 197 处修正可调（`global_pop_conversion_speed` / `_modifier`）。

## 四、满意度与叛乱（`NPop` 1591–1637）

```
BASE_SATISFACTION = 0.2
PRIMARY_CULTURE_SATISFACTION = 0.25 / ACCEPTED 0.10 / TOLERATED 0.05
CULTURE_OPINION_SATISFACTION = 0.05 / RELIGION_OPINION_SATISFACTION = 0.05
DIFFERENT_RELIGION_BASE_SATISFACTION = -0.05
GOODS_PRICE_SATISFACTION_SCALE = -0.05 / LACK_OF_GOODS_SATISFACTION_SCALE = -0.5
ESTATE_SATISFACTION_OFFSET = 0.5 / ESTATE_SATISFACTION_SCALE = 2.0
SCALE_ESTATE_NON_ACCEPTED = 0.5
```

叛乱阈值（默认值在 `auto_modifiers\country.txt:197-198`，可被修正覆盖）：
```
pop_join_rebel_threshold = 0.20    # 低于此值开始支持叛乱
pop_leave_rebels_threshold = 0.35  # 高于此值停止
REBEL_JOIN_MIN_THRESHOLD = 0.05    # 低于此值必定叛乱
REBEL_LEAVE_MIN_THRESHOLD = 0.1
MIN_REBEL_POP_SIZE_THRESHOLD = 5   # 至少 5000 人支持才成真叛乱
REBEL_DANGER_THRESHOLD = 0.95
monthly_rebel_growth = 0.001       # country_base_values
YEARLY_POP_SATISFACTION_FROM_EVENTS_DECAY = 0.01
```

满意度事件效果（`add_pop_satisfaction = pop_satisfaction_*`，1000+ 处引用）：五档 weak / mild / severe / extreme / ultimate，各有 bonus / penalty。叛乱诉求在 `common\rebel_demands\`；谈判成本 `script_values\rebel_values.txt`：`rebel_size × 10 + 25`。

**POP 碎片合并**（性能优化，原始声明在 `NPop`）：`POP_MINORITY_SIMILAR_THRESHOLD = 4`（同类型 ≥4 个按文化/宗教合并）、`POP_MINORITY_JUST_MERGE_THRESHOLD = 8`（≥8 个并入最大）。

## 五、阶层（`common\estates\00_default.txt`）

| 阶层 | `power_per_pop` | `tax_per_pop` | 特性 |
|---|---|---|---|
| `crown_estate` 王室 | 0 | 0 | `ruler = yes`、满意度恒满 |
| `nobles_estate` 贵族 | **25** | **100** | `bank = yes`（可放贷）、`characters_have_dynasty = always`、`can_generate_mercenary_leaders = yes` |
| `clergy_estate` 教士 | — | — | — |
| `burghers_estate` 市民 | — | — | — |
| `peasants_estate` 农民 | — | — | — |
| `tribes_estate` / `cossacks_estate` / `dhimmi_estate` | — | — | 部落/哥萨克/齐米 |

三套修正块（以贵族为例，第 61–77 行）：
- `satisfaction = { ... }` × (满意度 − 低满意度阈值)：贵族给月威望 +0.2、月正统性 +0.5
- `high_power = { ... }` × (相对权力 − 阈值)：`nobles_estate_max_tax = -1.0`、`levy_combat_efficiency_modifier = 1.0`、`fort_maintenance_efficiency = 1.0`
- `low_power = { ... }`：`nobles_estate_max_tax = 0.5`（税收惩罚）

`opinion` 块按来源逐项列出（基础 opinion、威望差、权力投射差、社会价值轴偏差如 `aristocracy_vs_plutocracy`、`serfdom_vs_free_subjects`）。

## 六、POP 的经济与军事作用

**经济**（`NPop` / `NMarket` / `NEconomy`）：
- 需求：`PRODUCED_IN_MARKET_DEMAND = 0.1`（自产打折）、`DEVELOPMENT_SCALE_ON_DEMAND = 0.05`、`POP_NEEDS_INCOME_SCALE = 1.33`
- 税收：阶层 `tax_per_pop` × 权力/满意度
- 识字率产出：见第 2 节 `literacy_impact`
- 就业：建筑/RGO 岗位由就业系统分配

**军事**：`ARMY_MANPOWER_LOSS_IMPACT = 1.0`、`NAVY_SAILOR_LOSS_IMPACT = 1.0`、`GARRISON_MANPOWER_LOSS_IMPACT = 0.5`、`ARMY_LEVY_SIZE_IMPACT_SUPPORT = 1`；异文化征召 `wrong_culture_levy_size = -0.75`。

**弃地警报**：`LOCATION_DEPOPULATION_ALERT_MONTHS = 60`。

> **人口容量与疫病死亡**：地点的 `local_population_capacity` 由气候（热带 +50%）、地形（山地 **−50%**）、植被（农田 100 / 沙漠 10）与地点等级共同决定；疫病死后的 POP 合并容差 `MAX_ACCEPTABLE_PERCENTAGE_DIFFERENCE_TO_AMALGAMATE_POPS = 0.05` 也在 `NDisease`。详见 `vanilla\vanilla-hazards-and-environment.md` §二与 §5.4。

## 七、界面与触发器

- 界面：`gui\pops_overview.gui`、`gui\pop_editor.gui`、`gui\shared\pop_tooltips.gui`（PopNeeds / PopLiteracy / PopLiteracyLocation）、`LocationView.GetPops`
- 属性列：`gui`/`common\attribute_columns\`（`location_population`、`province_definition_population`、`region_population`、`international_organization_population`）
- 触发器：`pop_type`、`pop_size`、`is_upper_class`、`is_linked_to_foreign_building`、`pop_knows_about_goods`（`common\trigger_localization\pop_triggers.txt`）；`location_population_percentage`、`religion_population_in_country`、`culture`/`religion` 人口统计
- 地图模式：文化/宗教按 POP 比例；`SECONDARY_CULTURE_THRESHOLD_PERCENT = 0.9`（主文化 <90% 才显示次文化）

## 八、Mod 改造建议

| 想改什么 | 动哪里 |
|---|---|
| 新增 POP 类型 | `common\pop_types\` 新文件（定义 promote_to/estate/grow/literacy_impact）；注意引擎对 pop_type ID 的引用点（建筑雇佣、征召、疾病） |
| 晋升/同化/改宗速率 | defines `NPop`/`NCharacter` 段；或用 `local_pop_conversion_speed(_modifier)`、`local_pop_promotion_speed_modifier`、`global_pop_assimilation_speed` |
| **人口增长** | `NPop`（`FOOD_SURPLUS_POP_GROWTH`/`FOOD_STORAGE_POP_GROWTH`/`GROWTH_DAMPING_START`）+ `NEconomy`（`GROWTH_FROM_FOOD_MULTIPLIER_MAX=2`/`_SURPLUS_THRESHOLD=1.25`）；修正键：`local/global_population_growth`（**percent**）与 `local/global_<type>_pop_growth`（**addend**，8 类各自），容量用 `*_desired_pop` 族 |
| 满意度与叛乱 | `auto_modifiers\country.txt` 的 `pop_join_rebel_threshold`（国家基础值）+ 各修正；`NPop` 阈值 defines |
| 阶层权力/税收 | `common\estates\`（`power_per_pop`/`tax_per_pop`/high_power/low_power） |
| 就业优先级 | `common\employment_systems\`（`priority` 脚本值） |
| 叛乱诉求 | `common\rebel_demands\` |

**硬编码**：POP 增长公式主体、需求/价格结算、满意度合成、叛乱爆发判定。
