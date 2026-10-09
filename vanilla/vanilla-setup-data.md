# 原版解析：开局数据层（vanilla setup data）

> **一句话**：讲开局数据分三处共 276 档：国家定义、205 档开局模板与 `setup\start\` 的逐地点逐国状态，并给出"改开局该动哪一档"的对照表。
> **什么时候看**：加国家、改某国开局政体科技领土或人口，或要搞清 `setup\countries`／`templates`／`start` 三层的分工时翻这篇。
> **体量**：122 行 · 约 6 分钟通读

版本基准：EU5 1.3.x。**本篇为什么存在**：地图层级只给"地"，**开局的国、人、POP、市场、思潮、外交、军队全在这一层**——而这一层分三处、共 276 档，此前只有零散线索。本篇把 `main_menu\setup\start\` **全部 25 档**逐档对齐，并首次成档 **`setup\templates\`（205 档开局模板）**。

| 层 | 位置 | 规模 | 管什么 |
|---|---|---|---|
| ① 国家定义 | `in_game\setup\countries\` | 46 档 / 324 KB / **2,340 个国家块** | 地图色、主文化、国教、简介分类、难度 |
| ② 开局模板 | `main_menu\setup\templates\` | **205 档 / ~0.2 MB** | 政体+继承法+议会+社会价值初值、科技等级、已知海域、宫廷语言、国家等级 |
| ③ 开局数据 | `main_menu\setup\start\` | **25 档 / 10.6 MB** | 逐地点/逐国的开局状态（POP、建筑、思潮、道路、外交、战争、军队…） |

> **改开局的三条路**：改"这个国家是什么"→ ①；改"它开局带什么"→ ②（**首选**，`include = "<模板名>"`）；改"某个国家/地点的具体数值"→ ③。端到端流程见 **`guides\new-country-tutorial.md`**。

---

## 一、`setup\start\` 全 25 档（编号 02–27，缺 01 与 17）

每档都是**一个顶层 manager 块**包住全部内容：

| 档 | 顶层块 | 内容 | 规模 |
|---|---|---|---|
| `02_core.txt` | `institution_manager` + `religion_manager` | 初始思潮出生地（feudalism→aachen / legalism→rome / meritocracy→dadu）+ 学派关系 | 97 行 / 1.6 KB |
| `03_markets.txt` | `market_manager` | `add_market = lubeck …` 市场中心清单 | 194 行 / 3.7 KB |
| `04_dynasties.txt` | `dynasty_manager` | 王朝库（**开局数据**，改"有哪些家族"在这里） | 9,636 行 / 238 KB |
| `05_characters.txt` | `character_db` | 角色库（**2.4 MB / 104,547 行**；子女必须写在父母之后） | 104,547 行 / 2.47 MB |
| `06_pops.txt` | `locations` | **逐地点逐 POP**：`define_pop = { type size culture religion }`（**最大档 5 MB / 107,398 行**） | 107,398 行 / 5.08 MB |
| `07_cities_and_buildings.txt` | `locations` | 逐地点的城市等级与初始建筑 | 4,491 行 / 257 KB |
| `08_institutions.txt` | `locations` | 逐地点的思潮接纳状态（`feudalism = yes` …） | 53,959 行 / 662 KB |
| `09_roads.txt` | `road_network` | 初始道路 | 1,650 行 / 33 KB |
| `10_countries.txt` | `countries` | **逐国领土与开局状态**：`own_control_core = { … }` / `starting_technology_level` / `include = "<模板>"` / `government = { … }` | 62,966 行 / 1.58 MB |
| `11_art.txt` | `work_of_art_manager` | 初始艺术品（"文化影响"来源） | 197 行 / 21 KB |
| `12_diplomacy.txt` | `diplomacy_manager` | 初始外交关系（同盟/联姻/条约） | 829 行 / 50 KB |
| `13_religion.txt` | `building_manager` | 宗教建筑初始分布（⚠️ 顶层块名是 `building_manager`，不是 religion） | 105 行 / 5.6 KB |
| `14_development.txt` | `development` | 初始发展度 | 383 行 / 6.9 KB |
| `15_international_organizations.txt` | `international_organization_manager` | IO 初始成员（HRE/天朝/教会…） | 1,662 行 / 67 KB |
| `16_wars.txt` | `war_manager` | 开局进行中的战争 | 975 行 / 14 KB |
| **`18_opinions.txt`** | `diplomacy_manager` | **初始观感**：`opinion = { first = DAN second = SWE type = border_aggression }`（86 条；`first` = 持有该修正方，`second` = 被看待方；也可写 `trust = { … }`） | 122 行 / 6.6 KB |
| **`19_diseases.txt`** | `disease_outbreak_manager` | **初始疫病**：`add_disease_resistance = { type resistance regions areas locations }`（9 条）+ `add_disease_outbreaks`（1 条） | 463 行 / 12 KB |
| **`20_rivals.txt`** | `diplomacy_manager` | **初始宿敌**：`rival = { first = FRA second = ENG }`（45 条，**成对写**——英法互相各一条） | 80 行 / 2.1 KB |
| **`21_locations.txt`** | `locations` | **地点级开局修正**：`malmo = { timed_modifiers = { { modifier = "skane_herring_market" start_date = 1111.1.1 … } } }`（2 处，极少量特例） | 29 行 / 360 B |
| **`22_situations.txt`** | `situation_manager` | **开局即生效的局势**状态（如 `rise_of_the_ottomans` 的初始 status/variables；原版**几乎全是注释样例**） | 47 行 / 714 B |
| **`23_colonies.txt`** | `colony_manager` | **初始殖民地**：`<预设省份> = { tag = SWE category = exclusive reason = treaty_of_noteborg }` | 114 行 / 1.9 KB |
| **`24_town_rights.txt`** | `townrights_manager` | **初始城镇特权**：`magdeburg = magdeburg_rights_town_rights`（**地点 = 特权**，一行一条） | 188 行 / 5.5 KB |
| **`25_area_preferences.txt`** | `countries` | **初始区域偏好**（探索/征服取向）：`countries = { SWE = { … } }` | 1,627 行 / 21 KB |
| **`26_ai_personalities.txt`** | `countries` | **逐国 AI 性格**：`TUR = { ai_personality = ai_aggressive }` | 130 行 / 5 KB |
| **`27_armies.txt`** | `unit_manager` | **开局军队**：`army = { country = VEN location = chioggia sub_units = { a_footmen = { strength = 0.94 } … } }` | 113 行 / 1.5 KB |

**读法要点**：

- **按 location 索引 vs 按 TAG 索引**要看清楚：`06/07/08/21` 是 location 键（小写地点名），`10/25/26` 是 TAG 键，`18/20` 用 `first`/`second` 成对，`23` 用**预设省份名**（不是 location）。
- 四个 `locations = { … }` 檔（`06/07/08/21`）**只写有内容的条目**，没写到的地点用默认（所以"新国家必须有 POP"这件事落在 `06_pops.txt`）。

---

## 二、最常改的四档（细节）

**`06_pops.txt`（5 MB，逐地点逐 POP）**

```txt
locations = {
    ath = {                                                  # 地点 id（小写）
        define_pop = { type = clergy  size = 0.007  culture = picard  religion = catholic }
        define_pop = { type = peasants size = 17.617 culture = picard  religion = catholic }
    }
    …
}
```

**`10_countries.txt`（1.58 MB，逐国领土与开局）** —— 见 `guides\new-country-tutorial.md` §2 的 ATH 实例（`own_control_core = { … }` + `include = "<模板>"` + `government = { … }`）。

**`18_opinions.txt` / `20_rivals.txt`（初始外交的温度）** —— 都挂在 `diplomacy_manager` 下，但**语法完全不同**：观感是 `opinion = { first second type }`（`type` 是修正名，如 `border_aggression` / `recent_conflict_lost`），宿敌是 `rival = { first second }`（**无 type**，且原版成对写、双向各一条）。

**`23_colonies.txt`（初始殖民地）** —— 键是**预设省份**（`korela_province`…），字段 `tag`（宗主）/ `category = exclusive` / `reason`（历史条约名，用于 tooltip）。

---

## 三、开局模板层（`main_menu\setup\templates\`，205 档）

`10_countries.txt` 里 `include = "<模板名>"` 出现 **5,256 次 / 192 个去重目标**——**原版每个国家的开局状态都是模板拼出来的**：

| 模板字段 | 档数 | 内容 |
|---|---|---|
| `government` | 168 | 政体 + 继承法 + 议会 + **13 条社会价值观初值** |
| `starting_technology_level` | 123 | 开局科技等级 |
| `include` | 57 | **模板可嵌套** |
| `discovered_regions` / `discovered_areas` / `discovered_provinces` | 33 / 39 / 25 | **开局已发现海域/地区/省份**（探索知识） |
| `court_language` | 14 | 宫廷语言 |
| `country_rank` | 10 | 国家等级 |

命名：`expl_<地区>`（探索）与 `<宗教/文化区>_<政体>[_no_coast|_not_present|_no_censor|_tribesmen…]`。用法与选取见 `guides\new-country-tutorial.md` §2b。

---

## 四、改开局：动哪一档

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 加/改一个国家本身 | `in_game\setup\countries\<地区>.txt` | 字段分布见 `fields\setup-countries.md`（`culture_definition`/`religion_definition` 才是必填） |
| 改某国开局的政体/科技/社会价值/已知海域 | `setup\templates\` 或其 `include` 列表（`10_countries.txt`） | **优先改模板**，别逐国手写；模板名拼错**不报错** |
| 给某国加领土 | `10_countries.txt` 的 `own_control_* = { <location…> }` | location id 必须真实 |
| 加人口 | `06_pops.txt`（逐 location 的 `define_pop`） | 5 MB，改前备份 |
| 加建筑/城市 | `07_cities_and_buildings.txt` | 建筑 id 须在 `building_types` 存在 |
| 改开局思潮 | `08_institutions.txt`（逐 location） | 思潮 id 见 `common\institution\` |
| 加初始外交关系 / 观感 / 宿敌 | `12_diplomacy.txt` / `18_opinions.txt` / `20_rivals.txt` | 观感要 `type`；宿敌**双向各写一条** |
| 加初始城镇特权 | `24_town_rights.txt` | 特权 id 见 `common\town_rights\` |
| 加初始殖民地 | `23_colonies.txt` | 键是**预设省份**，不是 location |
| 改逐国 AI 性格 | `26_ai_personalities.txt` | 性格 id 见 `common\ai_personalities\`（8 种） |
| 加开局军队 | `27_armies.txt` | `sub_units` 用 `unit_type` id + `strength` |
| 加初始疫病/抗性 | `19_diseases.txt` | 疫病 id 见 `common\diseases\`（7 种） |
| 加初始角色/王朝 | `05_characters.txt` / `04_dynasties.txt` | ⚠️ **子女必须写在父母之后**，否则崩溃 |

**硬编码**：manager 块的**名称与语法**（`institution_manager` / `market_manager` / `countries` / `locations` …）由引擎识别；谁先加载、合并覆盖规则不可脚本控制；`21_locations.txt` 的 `timed_modifiers` 需要 `start_date`（历史修正的起始点）。

---

## 五、中文检索键

概念：`province` / `province_definition` / `location`（词条见地图篇）。
相关档：`fields\setup-countries.md`（① 字段权威）· `vanilla\vanilla-map-and-geography.md`（地图层级与 location id）· `guides\new-country-tutorial.md`（端到端加国家）· `vanilla\vanilla-character-dynasty-cabinet.md`（② 的角色/王朝部分）· `vanilla\vanilla-pop.md`（POP 数值）· `vanilla\vanilla-international-organizations.md`（IO 初始成员）
