# 审查清单辅助：词条与 ID 存在性目录（ID 族总表）

> **一句话**：各类 ID 族的总表：分带前缀族与裸写族列出原版规模与实测引用写法，并给角色、开局数据族的特殊规则。
> **什么时候看**：审查引用类 ID 前先查这里的写法（前缀还是裸写），写反了不报错、只是永不命中。
> **体量**：83 行 · 约 4 分钟通读

> 用途：`SKILL.md` 第 5 节检查清单的按需加载表，审查引用类 ID **前**加载。**每族的原版规模与"脚本里到底怎么写"都来自原版实查**（2026-09，统计范围 `in_game\common` + `main_menu\common`（+ 注明处含 events/gui））。
>
> **为什么要有"引用写法"这一列**：同一族 ID 在原版里**有的必须带 `族:` 前缀、有的必须裸写**——写反了**不报错、只是永不命中**，是"改了没效果"的头号来源。

## 一、带前缀族：脚本里写 `<族>:<id>`

| 族 | 原版规模 | 前缀写法实测用量 | 权威档 |
|---|---|---|---|
| `culture` | 2,087 | **3,321** | `fields\common-culture-religion.md` |
| `international_organization` | 36 | **2,630**（`is_member_of_international_organization = international_organization:<类型名>`，仅 unique 类型有效） | `fields\common-international_organizations.md` |
| `language` | 53 档 | 1,351（`court_language ?= language:<id>`） | `fields\common-culture-religion.md` |
| `religion` | 293 | 1,293 | 同上 |
| **`modifier`** | **2,436** | **1,211**（`modifier:<键> = yes`）；出现在修正字段里时裸写 `country_modifier = { <键> = 1 }` | `fields\main_menu-modifier_type_definitions.md` |
| `societal_value` | 17 轴 | 1,055（**必须 `A_vs_B` 全串**） | `fields\common-societal_values.md` |
| `situation` | 22 | 932 | `fields\common-situations.md` |
| `estate_type` | 8 | 836（`country_has_estate = estate_type:<id>`） | `vanilla\vanilla-law-and-estate.md` |
| `region` / `area` / `province_definition` | 82 / 805 / 4,309 | 683 / 267 / 84（必须是 `map_data\definitions.txt` 的真实层级） | `vanilla\vanilla-map-and-geography.md` |
| `special_status` | 11 档 | 447 | IO 篇 |
| `estate_privilege` | 261 | 445 | `fields\common-estate_privileges.md` |
| `price` | — | 437（`implementation_price = price:<id>`） | `fields\common-prices.md` |
| `goods` | 74 | 294 | `fields\common-goods.md` |
| **`building_type`** | 465 | **1,369**（⚠️ **不是 `building:`**——`building:` 原版 **0** 处） | `fields\common-building_types.md` |
| **`sub_unit_category`** | 10 | **278**（⚠️ **不是 `unit_category:`**——0 处） | `fields\common-unit.md` |
| `country_rank` | 4 | 219（`rank_empire` 等） | `fields\common-country_ranks.md` |
| **`disaster_type`** | 36 | **201**（`disaster_type = disaster_type:<id>`） | `fields\common-disasters.md` |
| `casus_belli` | 102 | 195 | `fields\common-casus_belli.md` |
| `dynasty` | 开局数据 | 188 | `vanilla\vanilla-character-dynasty-cabinet.md` |
| `law` | 196 | 178 | `fields\common-laws.md` |
| `parliament_type` | 14 | 170 | `fields\common-parliament.md` |
| `resolution` | **21** | 154 | `fields\common-resolutions.md` |
| `heir_selection` | 44 | 79（`change_heir_selection = heir_selection:<id>`） | `fields\common-heir_selections.md` |
| `character` | 开局数据 | 76 | 角色篇 |
| `pop_type` | 8 | 61 | `vanilla\vanilla-pop.md`／POP 篇 |
| `unit_type` | 323 | 53 | `fields\common-unit_types.md` |
| `regency_type` | 15 | 29 | `fields\common-regencies.md` |
| `subject_type` | **20** | 21 | `fields\common-subject_types.md` |
| `trait` | 147 | 20（**仅 `add_trait = trait:<id>`**；`has_trait` 那侧裸写，见 §二） | `fields\common-traits.md` |
| `work_of_art` | 21 类型 | 16 | `fields\common-artist.md` |
| `cabinet_action` | 73 | 16 | `fields\common-cabinet_actions.md` |
| `mission_task` | 108 | 10（`mission_task_completed = mission_task:<id>`） | `fields\common-missions.md` |
| `generic_action` | 428 | 6 | `fields\common-generic_actions.md` |
| `holy_site_type` | 10 | 3 | `fields\common-holy_sites.md` |
| `peace_treaty` | **64** | **1** | `fields\common-peace_treaties.md` |
| `mission` | 11 | **1** | `fields\common-missions.md` |

## 二、裸写法族：加前缀反而错（原版 `<族>:` 实测 0 处）

| 族 | 原版正确写法（实测样例） | 权威档 |
|---|---|---|
| `advance` | `has_advance = <id>`（裸 id） | `fields\common-advances.md` |
| `road_type` | `unlock_road_type = gravel_road` | `fields\common-road_types.md` |
| `production_method` | `unlock_production_method = paper_guild_fiber_pulp_maintenance` | `fields\common-production_methods.md` |
| `trait`（查询侧） | `has_trait = <id>`（**38 处裸写 / `trait:` 0 处**）；新增才用 `add_trait = trait:<id>` | `fields\common-traits.md` |
| `gene` | `gene_<名> = { attribute = … value = { min max } }`（名字须在 `genes\` 定义） | `fields\common-genes-ethnicities.md` |
| `coa` | `coa = <COA 键>`（或 `list "<池名>"`）——**裸键**，见 4,566 个 COA 键 | `fields\main_menu-flag_definitions.md` |
| `game_concept` | 本地化里 `[<裸概念名>\|e]`——**只在 loc 中引用**，脚本不引用 | `fields\main_menu-game_concepts.md` |
| **开局模板**（205 档） | `include = "<模板名>"`（`10_countries.txt` 里 5,256 处 / 192 去重；**拼错不报错、只是没效果**） | `guides\new-country-tutorial.md` §2b |
| `unit_ability` / `area_preference` / `static_modifier` / `auto_modifier` / `topography` / `vegetation` / `climate` / `town_right` / `named_color` / `message_type` / `alert` | 各族前缀写法均为 **0 处**——引用方式见对应字段档（多数是按名/按枚举/由引擎侧读取） | 见 `fields\` 同名档 |

## 三、角色与开局数据族（特殊规则）

| 族 | 脚本里的写法 | 证据 |
|---|---|---|
| `death_reason` | **脚本里 0 引用**（只有本地化键）——不要在 mod 里发明 `death_reason = <id>` 效果 | 全库 grep |
| `regency` | 由继承法/法律逻辑内部引用，脚本直接写 id 的场合极少 | `regencies\*.txt` |
| `ethnicity` | 由角色生成侧读取（`template = "<族群 id>"`，**带引号**） | `fields\common-genes-ethnicities.md` |
| `artist_type` | 前缀写法 0 处（走 `artist_types` 定义侧与 `allow = { artist_type … }`） | `fields\common-artist.md` |
| 角色 / 王朝 | **本体不在 `common\`**——是开局数据（`main_menu\setup\start\05_characters.txt` / `04_dynasties.txt`） | 角色篇 |

## 四、核对规则（零幻觉）

- 引用类 ID：先查原版 `game\in_game\common\<类目>\`（或 `main_menu\common\` / `dlc\`）与 mod 自身对应目录，**存在才通过**；查不到标 `[存疑]`。
- **同时核写法**：按 §一/§二确认该族在原版到底是前缀还是裸写；写法不符**属于错误**（不是存疑）——因为原版同族有大量反例可证。
- 效果/触发器词条：引擎内置、无全表——明显笔误（重复字符/乱码/大小写错乱）→ 错误；拼写疑似但无法证实 → `[存疑]` + 最接近候选，不断言。
- 脚本化内容：`scripted_effects`（**476** 个）/ `scripted_triggers`（**501** 个）/ `on_action`（**216** 条）引用在 mod 或原版目录内可查；`$参数$` 未传会刷大量 missing 报错（见 `tools\error-log-decoder.md`）。
- **本地化键**存在性另有一套（键是全局的、含 DLC 与 `missions\` 子目录）→ `tools\loc-keys.md`。
