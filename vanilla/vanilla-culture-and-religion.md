# 原版解析：文化与宗教（vanilla culture & religion）

> **一句话**：实查 2087 个文化、528 种语言、293 个宗教及其组／信条／学派／圣地／神祇／运动，给出地位容量、统一度、好感与同化改宗速率修正链。
> **什么时候看**：加文化或宗教、改地位与容量、做运动传播或改宗内容，或需要宗教字段表（无官方 readme）时翻这篇。
> **体量**：433 行 · 约 20 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、总览：两层可分离的系统](#一总览两层可分离的系统)
- [二、文化（culture）](#二文化culture)
  - [2.1 定义层](#21-定义层)
  - [2.2 三层地位 + 容量](#22-三层地位--容量)
  - [2.3 文化统一度](#23-文化统一度)
  - [2.4 文化好感（五档）](#24-文化好感五档)
  - [2.5 文化战争：传统 = 防御，影响 = 攻击](#25-文化战争传统--防御影响--攻击)
  - [2.6 同化速率修正链（`defines:1487–1506`）](#26-同化速率修正链defines14871506)
  - [2.7 动态文化](#27-动态文化)
  - [2.8 游戏规则：文化变更四档（`main_menu\common\game_rules\00_game_rules.txt`）](#28-游戏规则文化变更四档main_menucommongame_rules00_game_rulestxt)
- [三、宗教（religion）](#三宗教religion)
  - [3.1 定义层：实查 39 个字段](#31-定义层实查-39-个字段)
  - [3.2 宗教统一度与容忍度三档](#32-宗教统一度与容忍度三档)
  - [3.3 宗教好感与学派好感](#33-宗教好感与学派好感)
  - [3.4 改宗国教（`generic_actions\general_religion.txt:1–336`）](#34-改宗国教generic_actionsgeneral_religiontxt1336)
  - [3.5 POP 皈依](#35-pop-皈依)
  - [3.6 宗教影响力：一种货币](#36-宗教影响力一种货币)
  - [3.7 十个子系统](#37-十个子系统)
  - [3.8 宗教启用机制（⚠️ 最容易踩）](#38-宗教启用机制️-最容易踩)
  - [3.9 运动（movements）—— 宗教/文化的传播引擎](#39-运动movements-宗教文化的传播引擎)
  - [3.10 宗教 IO / 局势 / 灾难](#310-宗教-io--局势--灾难)
- [四、语言（文化的硬依赖）](#四语言文化的硬依赖)
- [五、耦合点（写 mod 时的接口总表）](#五耦合点写-mod-时的接口总表)
- [六、Mod 改造建议（可改 vs 硬编码）](#六mod-改造建议可改-vs-硬编码)
- [七、⚠️ 统计方法学警告（2026-09 本轮实测）](#七️-统计方法学警告2026-09-本轮实测)
- [八、中文检索键](#八中文检索键)

版本基准：EU5 1.3.x。核心文件：`in_game\common\cultures\`（53 文件 / **2087 文化**）、`culture_groups\`（**209 组**）、`languages\`（53 文件 / **528 语言与方言**）、`language_families\`（**56 语族**）、`religions\`（29 文件 / **293 宗教**）、`religion_groups\`（**29 组**）、`religious_aspects\`（**177 信条**）、`religious_schools\`（**43 学派**）、`holy_sites\`（**241 圣地** / 10 类型）、`gods\`（**127 神祇**）、`movements\`（**4 运动**）、`chivalric_orders\`（**15 骑士团**）；常量在各 **`NCulture`（`defines:1447–1510`）/ `NReligion`（2575–2581）/ `NSpreadable`（2583–2590）** 块与 `NPop`/`NEstate` 的相关键。

> ⚠️ **全库唯一没有字段权威文档的大系统是宗教**：`religions\` 29 个文件里既无 `readme.txt` 也无 `.info`（文化有 `00_cultures.info`、文化组有 `00_culture_groups.info`）。宗教字段只能**实查 + `game_concepts` 反推**，本文的宗教字段表即为实查结果。

| 目录 | 文件数 | 定义数 | 权威 |
|---|---|---|---|
| `cultures\` | 53 | **2087** | `00_cultures.info`（778B） |
| `culture_groups\` | 1 | **209** | `00_culture_groups.info`（306B） |
| `languages\` | 53 | **528** | 无（字段实查） |
| `language_families\` | 1 | **56** | 无（只有 `color`） |
| `ethnicities\` / `genes\` | 54 / 11 | 60 / 12 | `genes\_genes.info` |
| `religions\` | 29 | **293** | **无** |
| `religion_groups\` | 1 | **29** | 无 |
| `religious_aspects\` | 19 | **177** | `readme.txt`（498B） |
| `religious_schools\` | 6 | **43** | 无 |
| `religious_figures\` | 2 | **2 类**（`muslim_scholar` / `guru`） | 无 |
| `holy_sites\` / `holy_site_types\` | 17 / 1 | **241** / **10** | 各有 readme |
| `gods\` | 12 | **127** | `readme.txt`（1828B，本域最详细） |
| `movements\` | 5 | **4** | `readme.txt`（6295B，本域最详细） |
| `chivalric_orders\` | 3 | **15** | 无 |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `culture` / `culture_group` | 文化 / 文化组 | 一个文化可属**多个**组；**文化组可选，语言必填** |
| `primary_culture` | **主流文化** | 代表国家身份认同 |
| `accepted_culture` / `tolerated_culture` | **已接纳文化** / **相容文化** | 三层地位的中上两层 |
| `dominant_culture` | 优势文化 | 地点/国家内规模最大的文化（≠ 主导） |
| `cultural_unity` | **文化统一度** | 主流文化人口占比 |
| `cultural_view`（`opinions`） | 文化好感 | 敌视/厌恶/中立/友好/亲密 |
| `cultural_tradition` / `cultural_influence` | **文化传统** / **文化影响** | 文化战争的**防御力** / **攻击力** |
| `culture_war_power` | 文化战争力量 | 攻/防比，影响同化·整合·间谍网·外交·围城 |
| `assimilation` | 同化 | 其他文化 POP 转为主流文化 |
| `religion` / `religion_group` | 宗教 / 宗教组 | 293 / 29 |
| `religious_unity` | **宗教统一度** | 官方宗教人口占比 |
| `tolerance_own` / `_heretic` / `_heathen` | 容忍度（国教/异端/异教） | 三个独立数值 |
| `religious_view` / `school_opinion` | 宗教好感 / 学派好感 | 同五档 |
| `religious_influence` | **宗教影响力** | 货币资源（仅 17 个宗教有） |
| `religious_aspect` | **宗教信条** | 可增删换的教义槽位 |
| `religious_school` / `sect` | 宗教学派 / 宗派 | 43 学派；宗派仅佛教系 |
| `religious_figure` | 宗教人士 | 2 类：伊斯兰学者、古鲁 |
| `god` / `omen` | 神 / 神谕 | 127 神祇，神谕有实装期 |
| `holy_site` | 圣地 | 241 座，`importance` 1–5 |
| `canonization` / `saint` | 封圣 / 圣人 | 75 宗教影响力 |
| `cardinal` / `curia` | 枢机 / 教廷 | 仅天主教；curia 是 IO 特殊地位 |
| `patriarch` | 牧首 | 东正教系，含自治牧首区 |
| `reform_desire` | **改革呼声** | 天主教内部冲突值 |
| `tithe` | 什一税 | catholic `= 0.02` |
| `movement` | 运动 | 宗教/文化在人口中的传播引擎 |
| `establishment`（对照） | 投产度 | 见生产建筑篇 |

## 一、总览：两层可分离的系统

```
国家身份
 ├─ 文化层（culture）
 │    主流文化 → 文化统一度；接纳/相容（吃 cultures_capacity 预算）
 │    文化好感（五档）+ 文化战争（传统=防 / 影响=攻）→ 同化速率
 │    语言必填 → 名字库 / 通用·宫廷·市场·礼仪语言 / 同化加成
 └─ 宗教层（religion）
      官方宗教 → 宗教统一度；容忍度三档（国教/异端/异教）
      宗教好感（五档）+ 学派好感 → 外交观感 / POP 满意度
      宗教影响力（货币）→ 教会权力行动 · 信条 · 封圣 · 牧首 · 教廷决议
```

**两层是独立轴的**：文化好感与宗教好感各自影响 POP 满意度（各 0.05），同化（文化）与皈依（宗教）走各自的速率修正链，互不替代。

## 二、文化（culture）

### 2.1 定义层

权威 `cultures\00_cultures.info`（778 行内的示例块）：

```
my_culture = {
    language = my_language              # language or dialect —— 必填（2087/2087）
    color = map_ENG
    tags = { catalan_gfx swedish_gfx european_gfx }
    country_modifier / location_modifier / character_modifier = { ... }
    opinions = { danish_gfx = enemy }   # 键可为文化名，也可为 gfx 标签
    culture_groups = { polish_group slavic_group }   # 可多个、可省略（47 个省略）
    suppress_no_pops_error = yes        # 历史/未来文化允许启动无 POP
}
```

**字段实测（n = 2087）**：

| 字段 | 数量 | 占比 |
|---|---|---|
| `language` | **2087** | **100%（必填）** |
| `color` / `tags` | 2087 | 100% |
| `opinions` | 2074 | 99.4% |
| `culture_groups` | 2040 | **97.7%（47 个无组）** |
| `use_patronym` | 49 | 2.3% |
| `goods_demand_modifier` | 7 | 0.3% |
| `dynasty_name_type`（`descendant` / `patronym`） | 5 | 0.2% |
| `country_modifier` / `location_modifier` / `character_modifier` | 各 4 | 0.2% |
| `active = no`（休眠文化，如 `roman_culture`） | 1 | — |
| `noun_keys` / `adjective_keys`（文化名构词，`british.txt:30/36`） | 各 1 | — |

**47 个"无文化组"文化**（留空 = 标成没有亲族，全是孤立/边地民族）：阿伊努、琉球、楚克奇、科里亚克、伊捷尔缅、楚凡、鄂莫克、阿瑙尔、瓦杜尔、奥留本、延丁、延德尔、肖龙巴、科雷姆、奥诺伊德、霍龙博伊、尤皮格特、萨克雷尔、埃文塞尔、尼姆昌、克列克、阿柳托尔、萨米、涅涅茨、埃涅茨、恩加纳桑、凯特、汉特、曼西、塞尔库普、塔夫吉、梅里亚、马里、科普特、努比亚、丰吉、通朱尔、安达曼、**阿尔巴尼亚**、雅兹、阿兰、博托斯、卡塔帕斯、蒂塞斯、雅罗斯、昆科、马内肯克。

> 语言必填的机制原因：`languages\` 的每个条目就是**名字库 + 构名规则**（`male_names`/`female_names`/`dynasty_names`/`lowborn`、patronym、regnal number、地名前后缀），且 define `MINIMUM_NAMES_PER_LANGUAGE = 10`（"名字少于 10 个就报错"）是硬检查；主流文化的 `language` 还直接决定国家 `common_language`。
> 无文化组的机制后果：`has_culture_group` 为假 → 拿不到接纳折扣 `ACCEPTANCE_SHARED_CULTURE_GROUP_FACTOR = −0.30`、同化加成 `ASSIMILATION_SHARED_CULTURE_GROUPS_MODIFIER = 0.3`；内阁 `merge_culture_group` 的 `potential` 要求 `has_any_culture_group = yes` → **这些文化永远无法合并文化组**。

文化组（`00_culture_groups.info`，306B）只有三个修正块：`country_modifier` / `location_modifier` / `character_modifier`——**组本身不列成员**，成员关系只写在文化侧。

### 2.2 三层地位 + 容量

| 层级 | 中文 | POP 满意度 | 征召 | 同化自身 |
|---|---|---|---|---|
| `primary_culture` | 主流文化 | **+0.25**（`PRIMARY_CULTURE_SATISFACTION`） | 可 | −1.0（最慢） |
| accepted | 已接纳文化 | **+0.10**（`ACCEPTED_CULTURE_SATISFACTION`） | **无需特定法律**即可当 levy | −0.75 |
| tolerated | 相容文化 | **+0.05**（`TOLERATED_CULTURE_SATISFACTION`） | 同上 | −0.25 |
| 其余 | 受歧视文化 | — | 需法律 | 基线 |

- **容量共享**：`cultures_capacity` / `cultures_capacity_modifier`，接纳与相容**同吃一份预算**；AI 移除前提写作 `used_cultures_capacity > modifier:cultures_capacity`。
- 基础成本：`ACCEPTED_CULTURE_BASE_COST = 3`、`TOLERATED_CULTURE_BASE_COST = 1.0`；另有月度维护费（`accepted_culture_maintenance_cost_modifier` / `tolerated_culture_maintenance_cost_modifier`、`has_cultural_maintenance`）。
- **5 个通用行动**（`generic_actions\culture.txt`，555 行）：

| 行动 | 价格（`prices\00_hardcoded.txt:354–374`） | 门槛 |
|---|---|---|
| `add_accepted_culture` | 威望 5 | 目标**须已是相容**、非本国主流、且 ≥2.5% 人口 或 ≥1000 人 或为某阶层优势文化 |
| `remove_accepted_culture` | 稳定 2 | 移除时该文化全部 POP 掉 `pop_satisfaction_ultimate_penalty`；AI 在 >7.5% 时权重归零 |
| `add_tolerated_culture` | 威望 5 | ≥1% 人口 或 ≥1000 人 或阶层优势文化 |
| `remove_tolerated_culture` | 稳定 1 | 同上 |
| `change_primary_culture` | 政府力 50 + 稳定 50 | **10 年冷却**；旧主流自动转接纳；统治者与家族联动 |

- 相关效果：`add_accepted_culture` / `demote_accepted_culture` / `add_tolerated_culture` / `remove_tolerated_culture` / `change_culture` / `change_language` / `add_cultural_tradition` / `add_cultural_influence` / `set_cultural_view` / `change_cultural_view`（`effect_localization\culture_effects.txt`）。
- 触发器（`trigger_localization\culture_triggers.txt`，20 条）：`cultural_tradition`、`cultural_tradition_power`、`cultural_influence`、`cultural_influence_power`、`cultural_view` / `reverse_cultural_view`、`is_accepted_in`、`is_primary_or_accepted_in`、`is_tolerated_in`、`has_culture_group`、`any_culture_group`、`has_shared_culture_group`、`has_graphical_culture`、`culture_opinion_impact`、`is_active`、`is_already_merged`、`is_merged_culture_group(_of)`、`merged_culture_group_contains_culture`、`has_any_culture_group`。

### 2.3 文化统一度

`cultural_unity` = **主流文化人口占全国比例**（词条 `game_concept_cultural_unity_desc`）；地点有 `local_cultural_unity`。内阁 `promote_culture` / `assimilate_area` 均把 `cultural_unity < 1.0` 写进 `allow`。

### 2.4 文化好感（五档）

`enemy 敌视 / negative 厌恶 / neutral 中立 / positive 友好 / kindred 亲密`（`general_tooltips_l_simp_chinese.yml:145–149`）。

| 作用点 | 常量 |
|---|---|
| 国家间观感 | `NEGATIVE_CULTURE_OPINION_ON_COUNTRY = 10`、`POSITIVE_CULTURE_OPINION_ON_COUNTRY = 5` |
| POP 满意度 | `CULTURE_OPINION_SATISFACTION = 0.05` |
| 同化 | `ASSIMILATION_TARGET_CULTURE_OPINION_OF_OWNER_MODIFER = 0.2`、`ASSIMILATION_OWNER_CULTURE_OPINION_OF_TARGET_MODIFER = −0.1`（负号：越喜欢越没动力同化） |
| 角色抽取 | `RANDOM_CHARACTER_CHANCE_CULTURE_OPINION_MULTIPLIER` |
| 雇佣兵要价 | `MERCENARY_LEADER_CULTURE_OPINION_MULTIPLIER = −0.07` |

修改途径：通用行动 `improve_our_cultural_view` / `reduce_our_cultural_view`（价格 `improve_our_cultural_view_price`，`generic_actions\improve_our_cultural_view.txt`）+ 国家交互 `improve_cultural_view`（要求对方改善）。

### 2.5 文化战争：传统 = 防御，影响 = 攻击

| 概念 | 中文 | 角色 | 来源（词条原文） |
|---|---|---|---|
| `cultural_tradition` | 文化传统 | 文化战争**防御力** | 相关建筑、社会价值、革新及大量其他因素 |
| `cultural_influence` | 文化影响 | 文化战争**攻击力** | **艺术品完工**给月度增长，另有威望、特定建筑、革新、法律；**有自然月度衰减** |
| `culture_war_power` | 文化战争力量 | 攻/防比 | 影响同化、整合、间谍网、外交甚至围城 |

常量（`NCulture`）：`CULTURE_WAR_RANGE = 0.5`、`CULTURE_WAR_IMPACT = 10`、`CULTURE_WAR_IMPACT_ON_SPY_NETWORK = 0.5`、`TRADITION_DECAY = −0.05`、`INFLUENCE_DECAY = −0.05`、`TRADITION_POWER_SCALE = 1000`、`INFLUENCE_POWER_SCALE = 1000`、`ARTIST_INFLUENCE_GAIN_FACTOR = 2`、`ARTIST_MONTHLY_PROGRESS = 0.01`、`ART_DESTROY_CHANCE_ON_CONQUEST = 10`、`ART_CAPTURE_CHANCE_ON_CONQUEST = 40`。

游戏内四处消费它：`SIEGE_CULTURE_WAR`（围城）、`ASSIMILATE_CULTURE_WAR`（同化）、`INTEGRATE_CULTURE_WAR`（整合）、`REBEL_NATIONALISM_CULTURE_WAR`（民族主义叛军）。

### 2.6 同化速率修正链（`defines:1487–1506`）

| 条件 | 修正 |
|---|---|
| 共享文化组 | `ASSIMILATION_SHARED_CULTURE_GROUPS_MODIFIER = 0.3` |
| 主导方文化 | `ASSIMILATION_DOMINANT_ACTOR_CULTURE_MODIFIER = 0.5` |
| 同语言 / 同语族 | `0.5` / `0.1` |
| 双方都是市场语言 / 仅本国 / 仅对方 | `0.4` / `0.2` / `−0.2` |
| 市民且语言 = 市场语言 | 按 `(1 − market_access)` 处理，下限 `ASSIMILATION_BURGHER_MARKET_LANGUAGE_MIN_MULTIPLIER = 0.01`（**不再完全阻断**） |
| 目标为主流 / 接纳 / 相容 | `−1.0` / `−0.75` / `−0.25` |
| 已合并文化组 | `ASSIMILATION_IS_MERGED_CULTURE_GROUP_MODIFIER = 1.0` |
| 文化好感双向 | `+0.2` / `−0.1` |

POP 层：`local/global_pop_assimilation_speed(_modifier)`；8 个 pop 类型各有 `global/local_<poptype>_assimilation_blocked`（`modifier_types:10607–10727`）。
推动渠道：内阁 `promote_culture`（地点 `local_pop_assimilation_speed = 0.04`、`local_migration_attraction = −0.15`）、`assimilate_area`（需 `modifier:allow_cabinet_assimilate_area = yes`）、法律/特权/革新/社会价值。

### 2.7 动态文化

| 内阁行动 | 能力 | 时长 | `potential` / `allow` | 效果 |
|---|---|---|---|---|
| `form_new_culture` | adm | 10 年 | `NOT = { is_dominant_country_of = culture }`；王国或帝国级 | `form_new_culture = yes` |
| `merge_culture_group` | adm | 10 年 | `culture = { has_any_culture_group = yes is_merged_culture_group = no }`；`is_dominant_country_of = culture` | `merge_culture_group = scope:target` |

- 合并后 `ASSIMILATION_IS_MERGED_CULTURE_GROUP_MODIFIER = 1.0`（同组同化直接翻倍）。
- `AGE_CULTURE_CAP = 2.5`（文化相关的年龄上限，与姓名生成/角色相关）。

### 2.8 游戏规则：文化变更四档（`main_menu\common\game_rules\00_game_rules.txt`）

`culture_conversions_rule`：`dynamic_culture`（默认，可换任意文化）/ `culture_group_only`（仅同组）/ `culture_present_in_country`（仅国内存在）/ `any_culture`（**带 `flag = blocks_achievements`，禁成就**）。
`change_primary_culture` 的 `visible`/`enabled` 分支就是按 `has_game_rule` 挑这三档写的。
> 规则系统的机制本身见 `main_menu\common\game_rules\_game_rules.info`（可带 `defines = { ... }` 覆盖常量、`apply_modifier`、`flag = disable_/force_<production_method>` 等）。

## 三、宗教（religion）

### 3.1 定义层：实查 39 个字段

| 分组 | 字段（数量 / 293） |
|---|---|
| **基础（100%）** | `group` 293、`color` 293、`definition_modifier` **293** |
| 主体 | `religious_aspects`（信条**槽位数**）268（91.5%）、`opinions` 248（84.6%）、`tags` 135（46.1%）、`custom_tags` 9 |
| 语言/名字 | `language`（礼仪语言，如 `church_dialect`）26、`unique_names` 16 |
| 影响力 | `has_religious_influence` 17（5.8%） |
| 教会结构（`christian.txt` 为主） | `has_canonization` 7、`has_autocephalous_patriarchates` 4、`has_patriarchs` 2、`has_religious_head` 1、`has_cardinals` 1、`important_country` 1（PAP）、`use_icons` 1、`tithe` 1（0.02） |
| 改革 | `needs_reform` 1（catholic）、`religious_focuses` 1（tonal）、`num_religious_focuses_needed_for_reform` 1 |
| 学派/宗派 | `religious_school` **3 个宗教**（`ibadi` 18 条 / `shia` 16 条 / `sunni` 21 条 → 43 学派）、`max_sects` 5、`factions` 1 |
| 人物 | `max_religious_figures_for_religion` 5 |
| 专有子系统 | `has_karma` 6、`has_honor` / `has_purity` / `has_avatars` / `has_omens` / `has_yanantin` / `saints_concept` 各 1 |
| 启用/锁定 | `enable = <日期>` 12、`culture_locked` 2（以色列系）、`ai_wants_convert` 4 |
| 需求 | `goods_demand_modifier` 5、`clergy_goods_demand_modifier` 4 |

**catholic 实块**（`christian.txt:188`）示范了主结构：`needs_reform = yes`、`has_religious_influence = yes`、`has_religious_head = yes`、`has_cardinals = yes`、`has_canonization = yes`、`important_country = PAP`、`tithe = 0.02`（注释："2% is 20% of the tenth"）、`definition_modifier` 里写 `clergy_estate_max_tax = −1.0`、`monthly_religious_influence = 0.1`、`maximum_religious_influence = 900`、`can_have_monasteries = yes`、`country_allow_canonization = yes`、`cannot_declare_no_cb_wars_on_religion_head = yes`。

**宗教组（`religion_groups\00_default.txt`，29 组）字段**：`color` 29、`convert_slaves_at_start` 28、`allow_rgo_slave_demand` 20、`modifier` 20、`allow_slaves_of_same_group` 3、`clergy_goods_demand_modifier` 3、`goods_demand_modifier` 1（穆斯林组把 wine/beer/liquor 需求设为 0）。

**宗教分布**（`group` 字段）：folk_asian 63、folk_african 50、folk_se_asian 40、christian 15、folk_european 14、folk_north_american 12、folk_melanesian 11、folk_south_american 9、buddhist 7、dharmic/muslim/tonal/folk_micronesian 各 3、其余 1–2。

### 3.2 宗教统一度与容忍度三档

- `religious_unity` = **官方宗教人口占比**；地点有 `local_religious_unity`。内阁 `promote_religion` 的 `allow` 是 `religious_unity < 1.0`。
- **容忍度是三个独立数值**：`tolerance_own` / `tolerance_heretic` / `tolerance_heathen`（`modifier_types:6976/6983/6990`）。
  - 词条原文：正值**提高 POP 满意度并改善信仰同一宗教国家的外交观感**；负值**增加 unrest 并恶化观感**。
  - 标度：`TOLERANCE_ON_SATISFACTION_SCALE = 0.05`（1 点容忍 = 5% 满意度）、`DIFFERENT_RELIGION_BASE_SATISFACTION = −0.05`、`RELIGION_OPINION_SATISFACTION = 0.05`。
  - 原版来源：革新（`4_choices_adm.txt:229/269/285`、`country_*.txt` 大量 ±0.5~2）、灾难修正（`religious_turmoil` 给 `tolerance_heretic = −2`）、阶层修正。

### 3.3 宗教好感与学派好感

同五档（`general_tooltips_l_simp_chinese.yml:214–218`）。效果：`set_religious_view` / `change_religious_view` / `set_school_opinion`；触发器 `religious_view` / `reverse_religious_view` / `school_opinion` / `reverse_school_opinion`。原版用脚本效果改写整族观感，如 `CHANGE_PROTESTANT_VIEW_KINDRED_EFFECT`（让某宗教视宗教改革系为"亲密"）。

### 3.4 改宗国教（`generic_actions\general_religion.txt:1–336`）

| 项 | 值 |
|---|---|
| 价格 | **稳定 50**（`price:convert_religion`） |
| 冷却 | **25 年** |
| `allow` 硬门 | `modifier:blocked_from_conversion = no`、非殖民附庸、非"神权附庸于神权宗主" |
| 效果 | **解除所有旧国教盟友的同盟** → `change_religion` → `change_religion_for_ruler_and_family`（统治者全家跟着改）→ 移除 `enacted_placitum_regium` |
| 可见性 | 按 `dynamic_religion` / `religion_group_only` / `religion_present_in_country` 三档；**皇帝只能换同组** |
| `enabled` | 非 dynamic 档要求国内有该宗教 POP，且"同组 或 该宗教占比 > 0.4" |
| 更换阈值 | `CHANGE_RELIGION_THRESHOLD = 1.25`（新宗教须大 25%） |

AI 权重里有大量历史钩子：BOH 专门被推向胡斯派（+500）、TUR 在 `historical_ai_choices` 下 −10000、附庸与宗主不同教 −1000。

### 3.5 POP 皈依

- 修正：`local/global_pop_conversion_speed(_modifier)`、异端专用 `global/local_heretic_pop_conversion_speed_modifier`、异教专用 `..._heathen_...`；8 个 pop 类型各有 `*_conversion_blocked`。
- 内阁 `promote_religion`（`ability = mil`，`allow` 要求至少一个阶层未被 `global_<estate>_conversion_blocked` 挡），地点给 `local_pop_conversion_speed = 0.04`、`local_migration_attraction = −0.15`。
- 词条明说"**内阁是推动皈依最有效的途径**"，法律/特权/革新/社会价值次之。
- POP 效果：`change_pop_religion` / `change_pop_culture`（见 `pop_effects.txt`）。

### 3.6 宗教影响力：一种货币

- 获取：`monthly_religious_influence`（catholic 0.1、hussite 0.1…）；上限 `maximum_religious_influence`（catholic **900**、hussite **400**）；门 `has_religious_influence = yes`（仅 17 宗教）。
- 支出（`prices\00_hardcoded.txt` 与 `01_buildings.txt`）：

| 用途 | 价格 |
|---|---|
| 教廷提案 `propose_curia_action` / 改枢机票 `change_curia_vote` | 50 / 20 |
| **封圣 `canonize`** | **75** |
| 东正教会议 `orthodox_synod` / 教区 `periphora` / 牧首代表团 | 75 / 10 / 20（+scaled_gold 2） |
| **创建自治牧首区 / 加入 / 迁座** | **80** / 20 / 20 |
| 邀请外国教士 `invite_foreign_cleric` | 100 |
| 选神谕 `select_omen_god` | 50 |
| 增删信条（基督教系）`add/remove_religious_aspect_christian` | 50（inti/hellenism 另有分组价） |
| 教会税 `demand_church_tax_price` | 50 |
| 枢机 `cardinal_price`（建筑价） | 33 |
| 宗教动荡行动 `religious_turmoil_actions_price` | 50 |
| 希腊多神教 `sponsor_troop_feast` / `grant_a_triumph` | 10（+金） / 50（+金） |

- 价格表把它作为**独立资源列**（`prices\readme.txt:18`）。

### 3.7 十个子系统

| 子系统 | 规模 | 机制要点 |
|---|---|---|
| **信条 aspects** | 177 条 / 268 个宗教有槽位 | `religious_aspects = N` 是**槽位数**（catholic 根本没这字段 → `max_religious_aspects = 0` → 三个行动 `potential` 不通过）；`add/change/remove_religious_aspect` 三行动，价格按 `christian` / `inti` / `hellenism` 分三组；readme：`religion`（可多个）、`visible`/`enabled`（**country 作用域**）、`modifier` |
| **学派 schools** | 43 个 / 3 个宗教挂载 | `enabled_for_country` / `enabled_for_character` + `modifier`；宗教用**重复行** `religious_school = X` 列出可用学派；触发器 `has_religious_schools` / `school_opinion` |
| **宗派 sects** | `max_sects` 5 个佛教系 | 触发器 `has_sects` / `max_sects`；行动 `increase_literacy_from_religious_sects` 等（价格带 `religious_sects_cost_modifier`） |
| **宗教人士 figures** | 2 类：`muslim_scholar`、`guru` | `enabled_for_religion = { group = ... }`；`max_religious_figures_for_religion`、`number_of_allowed_religious_figures`；`RELIGIOUS_FIGURE_CHANCE_OF_MOVING = 0.01`；邀请同学派 `scaled_gold 0.2 max 250` / 异学派 `0.4 max 500`，解雇稳定 10；AI 每 6 个月换一次学者（`AI_PERFORMANCE_SCHOLARS_MONTHS_BETWEEN_UPDATES`） |
| **神祇 gods / 神谕 omens** | 127 神 / 12 文件 | readme 最完整：`religion`/`group` 两种挂载写法（可带 `name_key`）、`potential`/`allow`、**`years/months/weeks/days` 渐进实装期（修正按完成度缩放）**、`on_activate`/`on_fully_activated`/`on_deactivate`、三作用域 scaled+triggered modifier、`is_female`（脚本 `is_god_female`）；效果 `add_god`/`remove_god`/`add_omen`/`add_omen_god`/`remove_omen`；`OMEN_LENGTH_MONTHS = 120`、`select_omen_god = 50` |
| **圣地 holy sites** | 241 座 / 10 类型 | `location` / `type` / `importance`(1–5) / `religions = {...}` / 可选 `god`、`avatar`；类型定义给 `country_modifier` + `location_modifier`（**按 importance 缩放**）+ `religion_modifier`（**50% 主导宗教 + 50% 所有者宗教**）；触发器 `has_holy_sites` / `all_holy_sites_owned_by_or_below_of` |
| **封圣 canonization** | 7 个宗教 | `has_canonization`、`country_allow_canonization`、价格 75；概念 `saint` / `canonization` |
| **枢机 cardinals / 教廷 curia** | 仅 catholic | `has_cardinals`；`num_cardinals` / `total_cardinals` 触发器；**`curia` 是 IO 特殊地位**（`international_organization_special_statuses\catholic_church.txt`）：`auto_bestowal_trigger = { num_cardinals > 0 }`、`auto_rescind_trigger = { num_cardinals = 0 }`、`priority = 10`，地图色按 `is_pope` 切换 |
| **牧首 patriarchs** | 东正教系 | `has_patriarchs` / `has_autocephalous_patriarchates`；行动 `create_autocephalous_patriarchate`(80) / `join_autocephalous_patriarchate`(20) / `relocate_ecumenical_patriarchate`(20) / `invite_patriarch_delegation`；`PERIPHORA_DAYS_PER_LOCATION = 30`；内阁 `support_patriarchate` |
| **改革呼声 reform_desire** | catholic / tonal | `needs_reform`、效果 `add_reform_desire` / `set_needs_reform`；触发器 `need_reforms` / `reform_desire`；`RELIGIOUS_FOCUS_COST = 100`；**直接扣教皇权威**（见 3.10） |

**专有子系统按宗教绑定**：`has_karma`（佛教 5 + 达摩系，`KARMA_POP_SIZE_CONVERSION_MULTIPLY = 1`）、`has_honor` / `has_purity` / `factions`（佛教）、`has_avatars`（达摩系）、`has_yanantin`（`folk_peruvian`）、`has_omens` + `saints_concept`（`folk_european`）、`culture_locked`（以色列系 2 个）。

### 3.8 宗教启用机制（⚠️ 最容易踩）

`enable = <日期>` 只出现 12 次，且**宗教改革系的全部写 `9999.1.1`**（路德/加尔文/胡斯/罗拉德/瓦勒度等，真日期只写在注释里）：

```
lutheran:  enable = 9999.1.1   #historically 1534.11.3
hussite:   enable = 9999.1.1   #historically 1412.10.18
```

真正启用它们的是**局势**：`situations\reformation.txt` 的 `on_start` 里 `religion:lutheran = { enable_religion = yes }`（并要求 `current_year >= 1510`）。
**改这些 `enable` 日期会直接破坏宗教改革链**；要"提前/延后新教"应改局势或 `enable_religion` 调用点。
触发器 `is_religion_enabled`；脚本触发器 `reformation_is_enabled`（= 曾发生过改革局势 或 路德/加尔文已启用）。

### 3.9 运动（movements）—— 宗教/文化的传播引擎

4 个运动：`lutheranism_movement`、`calvinism_movement`、`hellenism_religion_movement`、`roman_culture_movement`。readme（6295B）明写这是**传染病模型**：

- 核心参数：`r0`（一人每周期传染几人）、`environmental_infection`、`calc_interval_days`、`location_spread_threshold`、`monthly_spawn_chance`、`spawn`。
- **扩散路径**（readme 原文）：邻接地（一般人口流动）+ 市场中心（赶集）+ 与之贸易的地点（商人往来）+ 所有者的首都（进城）。
- **不扩散条件**：目标无人、两国禁运、目标已有 ≥50% 存在度、目标已停滞。
- **人群门槛**：`required_languages` / `required_language_families` / `required_religions` / `required_religion_groups` / `required_cultures` / `required_pop_types` / `required_tags`；`specific_pop_type_effect` 可按 pop 类型/宗教/文化/语言乘倍率。
- **四轴语境乘数**：`development` / `literacy` / `local_control` / `pop_satisfaction` 各可设 `neutral`/`positive`/`negative`。
- **阻力与增长**：每个运动三作用域各两条（`local_/national_/global_<tag>_resistance_modifier` 与 `..._growth_modifier`）。
- 常量 `NSpreadable`：`ESTIMATED_TRAVELLERS_TO_NEIGHBOURING_LOCATIONS = 0.06`、`..._TO_CAPITAL = 0.06`、`..._TO_MARKET_CENTRE = 0.06`、`ESTIMATED_PEOPLE_PER_UNIT_MERCHANT_CAPACITY = 0.012`、`ESTIMATED_MIXING_WITH_UNITS = 0.06`、`ESTIMATED_MIXING_WITHIN_UNITS = 0.35`。
- 路德宗运动内建**时代分期**（`lutheranism_movement.txt:21–56`）：改革后 5–40 年且特伦特未开 → r0 `{0.0 0.030}`；特伦特进行中 → `0.012`；特伦特结束后 <60 年 → `0.004`；之后 `0.002`；另有城市等级（town ×1.05 / city ×1.15 / megalopolis ×1.20）与**印刷机制度 ×1.25** 等逐项乘数。

### 3.10 宗教 IO / 局势 / 灾难

**IO（宗教相关）**

| IO | 要点 |
|---|---|
| `catholic_church`（教廷） | `unique = yes`、`has_target = no`、`max_active_resolutions = 1`、`has_leader_country = yes`、`leader_type = character`、`leader_title_key = "CATHOLIC_CHURCH_LEADER"`、`gold = yes`、`unlock_withdraw_from_organization_treasury = yes`；变量 **`papal_authority` 0–100 / start 60**，月变化 = **−改革呼声×0.2** + 内部和平 0.03 + 政策（`ultramontanism` +0.02 / `gallicanism` −0.01 / `conciliarism` −0.025 / invisible church…）；另有变量 `religion`(start = catholic)；`can_initiate_policy_votes` 需 `council_of_trent` 激活 |
| 教会决议 | `resolutions\` 25 个文件里 **13 个绑定 `catholic_church`**（开除教籍、十字军、各教皇诏书…），另加 `western_schism`；决议用 `international_organization_type = catholic_church` 绑定，`potential` 用脚本触发器 `papacy_active`（=`modifier:papacy_blocked = no`）；`no_papal_bull_active` 检查未生效的诏书修正 |
| `religious_leagues`（宗教联盟） | `protestant_union` / `catholic_league`：`unique = yes`、领袖为国家级、互斥；创建/邀请需 `war_of_religions` 局势激活**且为 HRE 成员**、宗教为 catholic 或 `is_protestant`；加入冷却 5 年（`war_of_religions_join_variable`） |
| `jihad` / `shinto` / `sikhism` | 各自专属机制（`religious_factions.txt` 的 20 个行动覆盖幕府朝廷、一向一揆、切支丹、宗派等） |

**局势**：`reformation`（1510 起，`monthly_spawn_chance_high` = 0.04；`on_start` 启用路德宗 + 在**有大学**的天主教地点把 50% POP 分裂出来，权重北德 +12 / 斯堪的纳维亚 +6 / 南德 +4 / HRE +4 / 伊比利亚·意大利 −0.5，排除罗马与教宗国）、`council_of_trent`、`hussite_wars`、`war_of_religions`（23KB，最大）、`western_schism`、`guelphs_and_ghibellines`。

**灾难 `religious_turmoil`**（`disasters\religious_turmoil.txt`）：

```
can_start = {
    has_any_active_disaster = no
    religion.group = religion_group:christian
    current_age = age_4_reformation
    is_subject = no
    NOT = { religion = religion:orthodox / miaphysite }
    reformation_is_enabled = yes
    situation:council_of_trent = { situation_has_ended = no }
    religious_unity < 0.80
    stability < 20
    societal_value:spiritualist_vs_humanist < 70
    NOT = { has_reform = government_reform:religious_tolerance }
    "estate_power(estate_type:clergy_estate)" < 0.20
    NOT = { tag = FRA }
}
modifier = { tolerance_heretic = -2 }
```

结束条件 `religious_turmoil_end_trigger`：宗教统一度 > 0.90 **或** 精神/人文轴 > 90 **或**（轴 < −60 且教士满意度 > 0.6 / 教士力量 > 0.20）。灾难期间解锁 3 个专属行动（`religious_turmoil_actions.txt`：推精神派、推人文派、支持皈依努力），全部以 `disaster_type = disaster_type:religious_turmoil` 为前置。

## 四、语言（文化的硬依赖）

- `languages\` 528 个条目 = **名字库 + 构名规则**：`male_names`（446 条有）、`female_names`（439）、`dynasty_names`（436）、`lowborn`（436）、`family`（295）、`dialects`（31，**子块**，方言可继承/覆盖父语言）、`character_name_order`（23）、`character_name_short_regnal_number`（23）、`patronym_suffix_son`(15)/`_daughter`(13)、`patronym_prefix_son`(10)/`_daughter`(10)、`location_prefix`(18)/`_suffix`(6)/`_vowel`(4)/`_elision`(2)/`_ancient(_vowel)`、`descendant_prefix`/`_suffix`/`_male`/`_female`、`fallback`(4)、`require_genitive_location_names`、`ship_names`(3)、`dynasty_template_keys`(3)。
- `language_families\` 56 个语族，只存 `color`。
- **四种语言身份**：通用语言（`common_language`，由主流文化的 `language` 决定）、宫廷语言、市场语言、礼仪语言。常量：`STATE_CLERGY_LITURGICAL_POWER = 20`、`COUNTRY_POWER_COURT_LANGUAGE = 1`、`MERCHANT_POWER_COMMON_LANGUAGE = 1`、`CULTURAL_INFLUENCE_ON_LANGUAGE_POWER = 1`、`MARKET_TOTAL_MERCHANT_POWER_ON_LANGUAGE_POWER = 1`、`NOBLE_LIKE_LANGUAGE_THRESHOLD = 0.5`、`MINIMUM_NAMES_PER_LANGUAGE = 10`、`COURT_LANGUAGE_NEGATIVE_CULTURE_OPINION_FACTOR = 0.0`（"不采纳我们不喜欢文化的语言"）。
- 相关修正：`change_court_language_cost_modifier`、`change_liturgical_language_cost_modifier`、`prevented_from_changing_court_language_by_overlord`、`ruler_name_in_court_language`、`language_change_threshold_modifier`、`court_language_is_{liturgical,common,market}_language_importance_modifier`、`allow_diplomacy_force_change_court_language`。
- 触发器 `has_fixed_liturgical_language`；效果 `change_language`。
- 修正 `antagonism_language_influence`——语言也进"文化战争"的观感账。

## 五、耦合点（写 mod 时的接口总表）

| 文化/宗教 → | 作用 | 关键键 |
|---|---|---|
| POP 满意度 | 主流 +0.25 / 接纳 +0.10 / 相容 +0.05；异教基准 −0.05；容忍度 ×0.05；文化·宗教好感 ×0.05 | `NPop`（1615–1622） |
| 阶层满意度与力量 | `ESTATE_CULTURE_ACCEPTED/TOLERATED/DISCRIMINATED_SATISFACTION = −0.05/−0.10/−0.20`（力量同值）；主导权权重 主流 1.20 / 接纳 1.00 / 相容 0.50 / 受歧视 0.25；阈值 1.10 | `NEstate`（1657–1684） |
| 征召 | 接纳/相容 POP 无需法律即可当 levy；`wrong_culture_levy_size` | — |
| 外交/侵略 | 文化·宗教好感 → 国家观感；`ANTAGONISM_ADDITIONAL_MODIFIER_SAME_CULTURE 0.4` / `SAME_RELIGION 0.35` / `DIFFERENT_RELIGION −0.35`；`ANTAGONISM_STATIC_DIFFERENT_RELIGION 10`、`..._DIFFERENT_CULTURE_GROUP 4`；`FRIENDLY_CULTURE_SCALE_ON_SHARED_BORDER 0.1`；`antagonism_culture_influence` / `antagonism_religion_influence` / `antagonism_language_influence`。**外交侧直接读本层**：AI 接受度键 `culture_view` / `religion_view` / `same_culture` / `same_court_language` / `same_common_language`；王室联姻与共主邦联见 `vanilla\vanilla-diplomacy.md` | — |
| 征服/和平 | `PEACE_COST_EFFICIENCY_FOR_SAME_CULTURE/RELIGION = 0.10`；AI `CONQUER_DESIRE_HERETIC_BONUS 10` / `HEATHEN_BONUS 20`、`CONQUER_DESIRE_SAME/ACCEPTED_CULTURE_BONUS 1/5`；`PEACE_MAKE_SUBJECT_OTHER_DOMINANT_CULTURE/RELIGION = 1.2` | — |
| 附庸 | `SUBJECT_UPKEEP_COMMON_RELIGION = −0.1`；附庸类型里的 `culture_war` 键 | — |
| 叛乱 | `REBEL_FORT_LOYALTY_CULTURE/RELIGION_BONUS = 10`；`monthly_religious_rebel_growth`、`monthly_clergy_estate_rebel_growth` | — |
| 地图模式 | `SECONDARY_CULTURE_THRESHOLD_PERCENT = 0.9`、`SECONDARY_RELIGION_THRESHOLD_PERCENT = 0.9` | — |
| 奴隶制 | 宗教组 `allow_slaves_of_same_group` / `convert_slaves_at_start` / `allow_rgo_slave_demand`；`allow_slave_conversion`、`auto_slave_raid_different_religion` | — |
| 生产 | `goods_demand_modifier` / `clergy_goods_demand_modifier`（宗教组与宗教都能改商品需求，如穆斯林组把酒类需求清零） | — |
| 科技（对照） | 礼仪语言力量、识字率、教士满意度、思潮数 → 研究进度（见科技篇） | — |

## 六、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 新增文化 | `common\cultures\<新文件>.txt` + `localization\...\cultures_l_<lang>.yml` | **必须写 `language`**（否则无名字库可生成角色/王朝名）；`culture_groups` 可省 |
| 新增文化组 | `common\culture_groups\00_culture_groups.txt` | 组不列成员，成员写在文化侧 |
| 新增语言 | `common\languages\` | 每语言至少 10 个男/女名（`MINIMUM_NAMES_PER_LANGUAGE`），否则报错 |
| 新增宗教 | `common\religions\<新文件>.txt` | 15 个字段必写（`group`/`color`/`opinions`/`tags`/`definition_modifier`…）；漏 `group` 会进不了组统计 |
| 改信条槽位 | 宗教的 `religious_aspects = <N>` | **是槽位数不是开关**；catholic 没有该字段 → 信条行动不可用 |
| 新增信条 | `common\religious_aspects\` + 挂到宗教 | 只在 `religious_aspects` 列表里的宗教可用；`visible`/`enabled` 是 **country 作用域** |
| 新增学派 / 宗派 | `religious_schools\` / 宗教的 `max_sects` | 学派要在宗教里用重复行 `religious_school = X` 挂载 |
| 新增神祇/神谕 | `common\gods\` + 效果 `add_god` / `add_omen` | 渐进实装期（`years/months/weeks/days`）会缩放修正 |
| 新增圣地 | `holy_sites\` + `holy_site_types\` | `importance` 缩放 location 修正；religion 修正按 50/50 分给主导宗教与所有者宗教 |
| 改接受/相容成本与容量 | define `NCulture` 的 `ACCEPTED/TOLERATED_CULTURE_BASE_COST` + 修正 `cultures_capacity` | 容量是**接纳+相容共享预算** |
| 改同化节奏 | `NCulture` 的 `ASSIMILATION_*` 全族 + 修正 `global_pop_assimilation_speed` | 逐 pop 类型的 `*_assimilation_blocked` 共 16 个 |
| 改文化战争 | `NCulture` 的 `TRADITION_/INFLUENCE_*`、`CULTURE_WAR_*`、`ARTIST_*` | 传统/影响有**自然月度衰减**（−0.05） |
| 改容忍度 | `modifier_types` 已有 `tolerance_own/heretic/heathen`，用在革新/法律/特权/灾难 | 标度 0.05/点 |
| 改宗教影响力经济 | `monthly_religious_influence` / `maximum_religious_influence`（写宗教 `definition_modifier`）+ `prices\00_hardcoded.txt` 的教廷价格 | 只有 `has_religious_influence = yes` 的宗教有这套 |
| 改宗教启用时机 | ⚠️ **不要改 `enable` 日期**；改 `situations\reformation.txt` 或 `enable_religion` 调用点 | 新教全部 `enable = 9999.1.1` |
| 改运动传播 | `common\movements\` + `NSpreadable` | 每个运动要自建 6 条修正（3 作用域 × 阻力/增长） |
| 改教廷 | `international_organizations\catholic_church.txt`（变量、政策、决议绑定） | `max_active_resolutions = 1`；`curia` 特殊地位在 `international_organization_special_statuses\` |
| 改文化/宗教变更规则 | `main_menu\common\game_rules\00_game_rules.txt`（4 档 + `any_culture`/`any_religion` 带 `blocks_achievements`） | 规则可覆盖 define |
| 名字生成 | `languages\` 的名字库与 `patronym_*` / `location_prefix` / `descendant_*`；文化 `use_patronym`、`dynasty_name_type` | `noun_keys`/`adjective_keys` 见 `british.txt:30/36` |

**硬编码边界**：语言**必填**（2087/2087，无语言则无法生成人物/王朝/地名，`MINIMUM_NAMES_PER_LANGUAGE` 检查）；宗教的 `group`/`color`/`definition_modifier` 三个字段 100% 出现（可视为结构必需）；同化/皈依的**基础速率**不在 define 里（只有修正键）；`culture_war_power` 的最终合成、POP 满意度里文化与宗教各项的加权、教廷决议投票结算、运动的扩散寻路（邻接+市场+首都）都是引擎流程。

## 七、⚠️ 统计方法学警告（2026-09 本轮实测）

**统计本体字段的出现率，不能用缩进锚点（`^\t`）。** 同一目录内 tab / 空格 / 行首空格混用——例如 `cultures\argentinian.txt` 的 `cunco_culture`、`east_asia.txt` 的 `she_culture` 是"**一个空格 + 名字**"写在顶格的——用 `^\t` 或 `^name` 统计会系统性低估，得出**假缺失率**，甚至把必填字段判成可选。

本轮实测：用缩进锚点得出"`language` 1882 / `culture_groups` 2042 / 宗教 `group` 243（50 个没有组）"——**全错**。改用花括号深度解析（扫到 `{` 就跳整个块体）后：

| 字段 | 缩进锚点（错） | 深度解析（对） |
|---|---|---|
| 文化 `language` | 1882 / 2083 | **2087 / 2087 = 100%** |
| 文化 `culture_groups` | 2042 | 2040（**缺 47**） |
| 宗教 `group` | 243（"50 个没有"） | **293 / 293 = 100%** |
| 宗教 `definition_modifier` | 243 | **293 / 293 = 100%** |

**正确做法**：`(?m)^[ \t]*key[ \t]*=` 容错正则，或按 `{`/`}` 计深度只取第 0 层块内的第 1 层字段。**附带结论**：文化总数是 **2087**（不是顶格统计的 2083）。

## 八、中文检索键

概念（`game_concepts_l_simp_chinese.yml`）：`game_concept_culture`（文化）、`primary_culture`（主流文化）、`accepted_culture`（已接纳文化）、`tolerated_culture`（相容文化）、`dominant_culture`（优势文化）、`cultural_unity`（文化统一度）、`cultural_tradition`（文化传统）、`cultural_influence`（文化影响）、`culture_war` / `culture_war_power`（文化战争 / 文化战争力量）、`assimilation`（同化）、`religion`（宗教）、`religious_unity`（宗教统一度）、`tolerance`（容忍度）、`heretic` / `heresy`（异端）、`heathen`（异教）、`conversion`（皈依）、`reform_desire`（改革呼声）、`religious_influence`（宗教影响力）、`religious_head`（宗教领袖）、`cardinal`（枢机）、`patriarch`（牧首）、`canonization` / `saint`（封圣 / 圣人）、`religious_aspect`（宗教信条）、`religious_school`（宗教学派）、`sect`（宗派）、`religious_figure`（宗教人士）、`god` / `omen`（神 / 神谕）、`holy_site`（圣地）、`tithe`（什一税）、`movement`（运动）。

好感档位（`general_tooltips_l_simp_chinese.yml:144–149 / 213–218`）：`culture_opinion_enemy/negative/neutral/positive/kindred` = 敌视 / 厌恶 / 中立 / 友好 / 亲密（`same` = 主流）；宗教同五档，另有 `CULTURE_OPINION_TOOLTIP` / `RELIGION_OPINION_TOOLTIP` 详解。

行动与界面：`actions_l_simp_chinese.yml`（`improve_our_cultural_view` 提高我们的文化好感 / `reduce_our_cultural_view`）、`country_interactions_l_simp_chinese.yml:483–486`（`improve_cultural_view` 要求提高文化好感）、`government_l_simp_chinese.yml:463/466`（`INTEGRATE_CULTURE_WAR` / `ASSIMILATE_CULTURE_WAR` 逐项说明）、`units_l_simp_chinese.yml:1596–1597`（`SIEGE_CULTURE_WAR`）、`rebel_l_simp_chinese.yml:22`（`REBEL_NATIONALISM_CULTURE_WAR`）、`scripted_effects_l_simp_chinese.yml:236–239`（`CHANGE_PROTESTANT_VIEW_KINDRED_EFFECT`）。

界面文件：`gui\culture_*.gui` / `religion_*.gui`（文化、宗教、教廷面板）；地图模式由 `main_menu` 侧 `gfx-map_modes.md` 所述定义驱动。
