# 原版解析：科技与时代（vanilla tech & age）

版本基准：EU5 1.3.x。核心文件：`common\age\00_default.txt`、`common\advances\`（190+ 文件）、`common\institution\`（6 个时代文件 + readme）、`events\ages.txt`、`common\script_values\institution_spread.txt`、`loading_screen\common\defines\00_defines.txt:190-193`。

## 术语对照（中文译名与内部名不同，易错）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `age` | **时代** | 6 个，全局推进 |
| `institution` | **思潮** | 每时代 3 种（**不是"制度"**） |
| `advance` | **革新** | 3178 条（**不是"科技"**） |
| `research` | 研究 | 国家同时只能研究一项革新 |
| `embrace` | 接纳 | 接纳思潮后解锁对应革新 |

## 一、三层链条

```
时代按年份全局推进（6 个）
   ↓ 每时代 3 种思潮在地点诞生并地图传播
国家「接纳」思潮（需足够人口住在思潮已完全传播的地点）
   ↓ 解锁该时代革新研究资格（+ requires / potential / for 门控）
研究：同时只能 1 项；进度 = f(礼仪语言力量, POP 平均识字率, 教士满意度, 已接纳思潮数)
   ↓ 进度 ≥ research_cost
获得革新：unlock_*（建筑/法律/单位/生产方法/改革/CB/附庸类型/能力）+ 常驻修正
```

## 二、时代（`common\age\00_default.txt`，373 行）

### 6 个时代

| ID | 中文名 | `year` | 时代区间 | price_stability | max_price | known_goods_demand_threshold | burgher_max_trade_range |
|---|---|---|---|---|---|---|---|
| `age_1_traditions` | 传统 | 1（开局） | 1337–1342 | 0.10 | 3 | 100 | 600 |
| `age_2_renaissance` | 文艺复兴 | **1342** | 1342–1437 | 0.10 | 3 | 80 | 700 |
| `age_3_discovery` | 地理大发现 | 1437 | 1437–1537 | 0.08 | 4 | 60 | 800 |
| `age_4_reformation` | 宗教改革 | 1537 | 1537–1637 | 0.06 | 5 | 40 | 900 |
| `age_5_absolutism` | 专制主义 | 1637 | 1637–1737 | 0.04 | 5 | 20 | 1000 |
| `age_6_revolutions` | 大革命 | 1737 | 1737–1837 | 0.02 | 5 | 10 | 1100 |

区间依据 `advances\_advances_template.txt` 注释（`# age_2_renaissance (1337-1437)` 等）。`age_1` 的 `year = 1` 是特殊值（表示开局即生效）。

### 推进机制

- **全局按年份自动推进**（所有国家共享同一时代，非玩家触发、非科技门槛）。`NGame`：`START_DATE = "1337.4.1"`、`END_DATE = "1836.12.31"`。
- 推进触发两个硬编码 on_action（`on_action\_hardcoded.txt:493-522`）：
  - `on_new_age_global`（全局一次）：`setup_area_preferences` 重设 AI 区域偏好
  - `on_new_age`（每国一次）：触发 `ages_of_eu.1` + （若开 `ai_personalities_random_per_age` 规则）重掷 AI 人格
- **`ages_of_eu.1`（`events\ages.txt`）= 时代开局专精三选一**：`type = age_event`，三个选项各只有一句效果 `set_age_preference = adm/dip/mil`，决定该时代可研究哪些"分叉革新"（对应 advance 的 `for` 字段）。原版给奥斯曼加强制权重（`has_or_had_tag = TUR` → `ai_will_select` 1000）以拿 Deus Vult CB。

### 两个修正块：`modifier` 与 `unique`

⚠️ **区分经实测确认，非 readme 声明**（`common\age\` 无 readme；`Age_tooltip` 是引擎内置类型，GUI 中只有 `using =`，无脚本定义）。

| 块 | 语义 | 特征 |
|---|---|---|
| `modifier` | **每代递进的标准时代修正** | 一套骨架键每代重复出现、只调数值 |
| `unique` | **该时代独有的特色修正**（unique to this age） | 每代各不相同、只出现一次；因时代全局推进，**一进入该时代即对全世界生效**（与"谁先到"无关） |

证据（结构层面）：
1. **同一键会跨块"搬家"**：`global_war_score_efficiency` 在 age_5 属 `modifier`（0.2）、在 age_6 属 `unique`（0.5）；`subject_loyalty` 在 age_5 属 `modifier`（−10）、在 age_6 属 `unique`（−30）——排除"首发奖励"解读。
2. **`age_1_traditions` 完全没有 `unique` 块**（只有 `modifier`），`unique` 自 age_2（第 59 行）起才有——符合"该时代独有增益"而非"首个进入者奖励"。

**`unique` 内容**（每代两条）：

| 时代 | unique 修正 |
|---|---|
| 文艺复兴 | `diplomatic_capacity_modifier = 0.25`、`cultural_tradition_modifier = 0.25` |
| 地理大发现 | `exploration_mission_speed = 0.25`、`colonial_maintenance_efficiency = 0.50` |
| 宗教改革 | `global_pop_conversion_speed_modifier = 0.33`、`merchant_maintenance_efficiency = 0.50` |
| 专制主义 | `revoke_privilege_cost_modifier = -0.33`、`global_integration_speed_modifier = 0.25` |
| 大革命 | `global_war_score_efficiency = 0.5`、`subject_loyalty = -30` |

**`modifier` 骨架键的数值趋势**：

| 键 | 传统 | 文艺复兴 | 大发现 | 宗教改革 | 专制主义 | 大革命 |
|---|---|---|---|---|---|---|
| `expected_army_size_modifier` | — | 1.0 | 2.0 | 4.0 | 8.0 | **16.0** |
| `expected_navy_size_modifier` | −0.8 | −0.64 | −0.48 | −0.32 | −0.16 | — |
| `levy_maintenance_modifier` | — | 0.1 | 0.25 | 0.5 | 0.75 | 1.0 |
| `lack_of_control_impact_on_warscore` | 0.1 | 0.2 | 0.4 | 0.6 | 0.8 | 1.0 |
| `global_navy_levy_size_modifier` | — | −0.2 | −0.75 | −1.0 | −2.0 | −2.0 |
| `antagonism_religion_influence` | — | 0.1 | 0.15 | 0.2 | 0.1 | — |

### 其他字段

| 字段 | 说明 | 趋势 |
|---|---|---|
| `efficiency` | 全局效率（抗膨胀核心） | **1.0 → 0.1**（逐代递减） |
| `mercenaries` | 雇佣兵修正 | 文艺复兴 +0.25 → 专制主义 −0.40 → 大革命 −0.9（雇佣兵衰落） |
| `war_score_from_battles` | 战斗提供战争分数的修正 | 大发现 −0.05 → 大革命 −0.2 |
| `hegemons_allowed` | 是否允许霸权 | 自 age_3 起 yes |
| `months_for_exploration_spread` | 探索传播月数 | 1800 → 1800 → 1800 → 1200 → 600 → 300 |
| `construction_center_max_level` | 建筑中心等级上限（在 modifier 内） | 专制主义 2 → 大革命 5 |
| `goods_demand = {}` | 商品需求曲线（酒/烟草/咖啡/糖/茶） | 从负到正，模拟商品普及史 |
| `max_ai_privilege_per_estate` / `min_ai_privilege_per_estate` | AI 各阶层特权数量上下限 | 逐代收紧（大革命全 0） |

`goods_demand` 实值（原版注释点明历史意图）：

| 商品 | 传统 | 文艺复兴 | 大发现 | 宗教改革 | 专制主义 | 大革命 |
|---|---|---|---|---|---|---|
| liquor | −0.75 | −0.5 | 0 | +0.5 | +1.0 | +1.5 |
| tobacco | −0.75 | −0.75 | −0.25 | +0.5 | +1.0 | +1.5 |
| coffee | −0.5 | −0.5 | −0.25 | 0 | +0.5 | +1.25 |
| sugar | −0.25 | −0.1 | +0.25 | +0.75 | +1.25 | +1.75 |
| tea | −0.25 | −0.25 | −0.1 | 0 | +0.5 | +1.5 |

## 三、思潮（`common\institution\`）

### 结构（权威：`institution\readme.txt`）

```
<institution_id> = {
    age = <age_id>
    location = <fallback 地点>          # 无有效诞生地时的兜底
    can_spawn = { <location 作用域触发器> }
    promote_chance = <script value>
    # 9 条传播通道（引擎内置，值用 script value 定义速率）：
    spread_from_friendly_coast_border_location / spread_from_any_coast_border_location /
    spread_from_any_import / spread_from_any_export / spread_from_was_possible_spawn /
    spread_scale_on_control_if_owner_embraced / spread_embraced_to_capital /
    spread_to_market_member / spread_to_market_center
    spread = <scripted value>
}
```

### 每时代 3 种（age_2 实例，`age_2_renaissance_institutions.txt`）

| 思潮 | fallback 地点 | `promote_chance` |
|---|---|---|
| `renaissance` 文艺复兴 | florence | 平均识字率 + 市民数×10 |
| `banking` 银行系统 | genoa | 平均识字率 + 市民数×10 |
| `professional_armies` 职业军队 | paris | 平均识字率 + 人力修正×2 |

诞生受**游戏规则三档**控制：`institution_spawn_fixed_historical`（固定历史地点）／`institution_spawn_random_city`（任意城市/大都市）／`institution_spawn_plausible`（合理地点）。原版含 DLC 分支：拜占庭/罗马赢得"凤凰之命"灾难后，文艺复兴可在**君士坦丁堡**诞生。

### 传播速率（`script_values\institution_spread.txt`，early/mid/late 三档递减）

| 通道 | early | mid | late |
|---|---|---|---|
| 友好海岸边境 | 识字率×0.040 | ×0.030 | ×0.020 |
| 任意海岸边境 | 识字率×0.005 | ×0.0025 | ×0.001 |
| 进出口贸易 | 开发度×0.03 + 识字率×0.1 | ×0.01 + ×0.025 | ×0.004 + ×0.01 |
| 曾可能诞生地 | ×0.2 | ×0.05 | ×0.025 |
| 已接纳者→首都 | ×0.05 | ×0.025 | ×0.012 |
| 市场成员之间 | ×0.03 | ×0.012 | ×0.006 |
| 市场中心 | `value = 1.0`（固定，不分档） | | |

`spread_scale_on_control_if_owner_embraced = 2`：所有者已接纳时，控制度对传播的加权 ×2。

### 接纳（embrace）

官方词条（`game_concepts_l_simp_chinese.yml:834`）：**"当一个国家有足够多的（比例）人口居住于某个思潮已完全传播的地点时，该国家就能接纳这一思潮。接纳思潮意味着国家已经完全理解这一思潮，并且能够研究该思潮解锁的革新。"**

- 加速：内阁行动 **`study_institutions`**（`ability = adm`；`potential = num_locations > 0`；`allow = NOT 部落政体`；选择省份需 `has_possible_institution_spawn` + 核心整合 + 不重复）
- 相关修正键：`promote_institution_chance`、`local_/global_institution_growth_modifier`、`institution_growth`、`embrace_institution_cost_modifier`、`absorb_institutions_cost_modifier`
- 触发器：`num_embraced_institutions`

## 四、革新（`common\advances\`）

### 规模（实查计数）

| 时代 | 条目数 |
|---|---|
| 传统 | 613 |
| 文艺复兴 | **653** |
| 地理大发现 | 530 |
| 宗教改革 | 523 |
| 专制主义 | 459 |
| 大革命 | 407 |
| **合计** | **3178** |

组织方式：6 个时代主文件（`0_age_of_*.txt`）+ 按用途拆分（`1_building_unlocks`、`2_army_unlocks`、`2_ship_unlocks`、`3_*_unlocks`）+ **140 余个国家专属**（`country_ENG.txt`…）+ 文化/宗教/地区专属（`culture_japan.txt`、`religion_catholic.txt`、`region_asia.txt`）。

### 字段（权威：`advances\readme.txt` 全 28 行）

```
<advance ID> = {
    age = <age>                    # 所属时代
    icon = <icon>
    depth = 0                      # 科技树层级
    research_cost = 2.0            # 研究花费
    requires = <advance>           # 前置（可多条）
    potential = { <triggers> }     # 是否出现（NOTE: 不回溯生效）
    allow = { <triggers> }         # 是否可研究
    for = <adm/dip/mil>            # 时代专精分叉
    government = <government_type>
    country_type = <location/pop/building/army>
    starting_technology_level = 2  # 仅传统时代：高于国家起始等级者开局未研究
    unlock_unit / unlock_ability / unlock_interaction / unlock_country_interaction /
    unlock_relation_type / unlock_building / unlock_law / unlock_levy /
    unlock_government_reform / unlock_casus_belli / unlock_subject_type /
    unlock_production_method = <ID>
    allow_children = <yes/no>
    <modifiers>                    # 完成后的常驻修正
    modifier_while_progressing = { potential_trigger = {...} scale = <maths> <modifiers> }
                                   # 研究过程中的条件性缩放修正
}
```

真实样例（`0_age_of_traditions.txt`）：`written_alphabet`（`global_max_literacy = 5`、`global_monthly_literacy = 0.01`）→ `mapmaking`（`may_explore = yes`、`unlock_law = immigration_law`）→ `colonies`（`can_colonize = yes`）；`codified_laws`（`has_codified_laws`、`allow_council_parliament`）→ `subjects_advance`（`unlock_subject_type = vassal`）、`taxation_advance`（`enable_taxation`）。

### 研究机制

官方词条（`game_concepts_l_simp_chinese.yml:837/841`）：
- **一个国家同时只能研究一项革新**
- **`research_progress` 达到该革新的 `research_cost` 时即获得**
- 研究进度**主要取决于四项**：①**礼仪语言（liturgical_language）的语言力量**；②**POP 的平均识字率**；③**教士阶层（clergy_estate）的满意度**；④**已接纳的思潮数量**

礼仪语言（`game_concept_liturgical_language_desc`）：国家官方宗教的正式语言；天主教等固定不可改，其他宗教可改。语言力量（`game_concept_language_power_desc`）是"相对世界最强语言的百分比"，由"以该语言为宫廷语言的国家的主文化 POP 政治权力"+"以该语言为礼仪语言的国家的国教教士数量"计算。

### 核心 defines（`00_defines.txt:190-193`）

| 常量 | 值 | 含义 |
|---|---|---|
| `BASE_RESEARCH_COST` | 25 | 基础研究花费（advance db 加载时设定） |
| `BASE_UNIQUE_RESEARCH_COST` | 5 | 独特革新基础花费 |
| `AGE_RESEARCH_MODIFIER` | 0.15 | **每时代乘一次** |
| `PREVIOUS_AGE_REDUCTION` | −8 | 研究**前一个时代**革新的减免 |

修正键：`research_speed`（数值，country）、`research_speed_modifier`（百分比，country）。

**研究速度基础值受国家类型影响**（`auto_modifiers\country.txt:281-319`）：`location` 型 1、`building` 型 1、`army`/`navy` 型 1.0，而 **`pop` 型仅 0.1**（无地/部落式政权研究极慢）。建筑常提供 `research_speed_modifier`（多为 +0.05，个别 +0.1）。

### 额外获取途径

- 触发器 `num_of_advances_researched`（已研究数量）
- 通用行动 **偷科技**（`common\generic_actions\steal_technology.txt`，条件含 `num_of_advances_researched > scope:actor.num_of_advances_researched`）
- 脚本值 `research_delta` / `institutions_delta`（`script_values\research_values.txt`，用于"借鉴先进国"类行动）

## 五、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 时代起始年/时长 | `common\age\00_default.txt` 的 `year` | 全局推进，改年即改节奏；`age_1` 用特殊值 1 |
| 时代修正 | 同文件 `modifier`（递进骨架）/ `unique`（时代独有） | 需保持"全世界同时获得"的语义 |
| 商品需求曲线 | 同文件 `goods_demand` | 键为 `global_<商品>_pop_demand` |
| 专精分叉 | `events\ages.txt`（`set_age_preference`） | 改分叉须同步改 advance 的 `for` |
| 思潮种类/诞生地 | `common\institution\age_N_*_institutions.txt` | 每时代 3 种是设计惯例；`can_spawn` 须覆盖三档 game_rule |
| 思潮传播速率 | `common\script_values\institution_spread.txt` | early/mid/late 三档全局生效 |
| 新革新 | `common\advances\` 新文件 | `potential` 不回溯；`unlock_*` 目标 ID 必须存在 |
| 研究快慢 | defines 四常量 + `research_speed(_modifier)` | `AGE_RESEARCH_MODIFIER`/`PREVIOUS_AGE_REDUCTION` 是全局杠杆 |
| 国家专属树 | `country_XXX.txt` + `potential = { has_or_had_tag = XXX }` | 140+ 现成模板可抄 |

**硬编码**：时代按日期推进、`for` 专精分叉生效逻辑、研究进度累积公式主体、思潮 9 条传播通道（只能调速率）、`unlock_*` 生效方式、`Age_tooltip` 展示。

## 六、中文检索键

时代名与描述：`advances_l_simp_chinese.yml` 的 `age_1_traditions` … `age_6_revolutions`（+`_desc`）、`age_format_*`、`AGE_NUMBER`（"第$NUM$时代"）、`ages_of_eu.adm/dip/mil`（"注重行政/外交/军事"）。
概念：`game_concept_age`（时代）、`game_concept_institution`（**思潮**）、`game_concept_embrace`（接纳）、`game_concept_advance`（**革新**）、`game_concept_research(_progress)`（研究/研究进度）、`game_concept_language_power`（语言力量）、`game_concept_liturgical_language`（礼仪语言）。
