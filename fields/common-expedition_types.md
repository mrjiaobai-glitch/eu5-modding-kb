# common/expedition_types（远征类型）

> **一句话**：远征类型（国家拥有、角色率领、沿航点路径航行并跑生命周期钩子的航程）的 31 个顶层字段全景 + 12 种原版远征的起止条件、奖励与失败条件实测表。
> **什么时候看**：新增一条远洋/朝圣/测绘/使团航线，要调 AI 派发权重或奖励，或核对 `origin`/`travel_mode`/`should_end_at_last_waypoint` 这类"默认值会咬人"的字段时翻这篇。
> **体量**：460 行 · 约 21 分钟通读

## 目录

- [术语对照](#术语对照)
  - [12 种远征的文件与本地化名](#12-种远征的文件与本地化名)
- [字段全景：31 个顶层字段](#字段全景31-个顶层字段)
  - [枚举取值白名单（本体实测）](#枚举取值白名单本体实测)
  - [readme 声明、13 个定义文件从未使用](#readme-声明13-个定义文件从未使用)
  - [readme 未声明、本体在用的字段](#readme-未声明本体在用的字段)
- [`variables` 变量块（8 文件 · 26 个变量）](#variables-变量块8-文件--26-个变量)
- [作用域与生命周期钩子](#作用域与生命周期钩子)
- [12 种远征对照表](#12-种远征对照表)
  - [总表](#总表)
  - [逐条要点](#逐条要点)
- [一个完整例子：高地勘探测绘 `mining_survey_expedition`](#一个完整例子高地勘探测绘-mining_survey_expedition)
  - [1. 类型定义（L1-6）](#1-类型定义l1-6)
  - [2. `potential` 与 `can_start`（L8-60）](#2-potential-与-can_startl8-60)
  - [3. `on_start`（L68-117）](#3-on_startl68-117)
  - [4. 开场事件：二选一（`in_game\events\expeditions\mining_survey_events.txt:4-48`）](#4-开场事件二选一in_gameeventsexpeditionsmining_survey_eventstxt4-48)
  - [5. `on_arrive_to_waypoint`：60% 好 / 40% 坏（L125-149）](#5-on_arrive_to_waypoint60-好--40-坏l125-149)
  - [6. `on_end`（L156-171）](#6-on_endl156-171)
  - [7. 为什么实际节奏是「每 20 年一轮」](#7-为什么实际节奏是每-20-年一轮)
- [审查要点](#审查要点)
- [中文检索键](#中文检索键)

来源：`in_game\common\expedition_types\readme.txt`（475 行，字段权威）+ 同目录 13 个定义文件（12 种远征）+ `in_game\common\scripted_effects\expedition_effects.txt` + `in_game\events\expeditions\*.txt`。
实测口径：`kb\scripts\kb-field-census.ps1 -Category expedition_types`（下称"普查报告 §X"），数字全部可在上列文件中复核。

## 术语对照

| 词 | 含义 | 出处 |
| --- | --- | --- |
| 远征类型 / type | `expedition_types\*.txt` 里的顶层块，如 `mining_survey_expedition = { }`，引擎类 `CExpeditionType` | readme.txt:3-6 |
| 远征实例 / expedition | 某国实际派出的那一次航程，`scope:expedition` 指向它 | readme.txt:378-379 |
| 航点 / waypoint | 路径上的一个站点；`waypoints = { }` 静态声明，或 `add_new_waypoint` 运行时追加 | readme.txt:61-70 |
| root | **国家**作用域（除 `leader` 块是角色、`variables.monthly_change` 是远征自身） | readme.txt:97-98, 370-373 |
| `scope:location` | 抵达钩子提供的当前地点（只在两个抵达钩子里有效） | readme.txt:378-379 |
| `scope:expedition` | 当前远征实例；`var:x` 的读写都挂在它身上 | readme.txt:361-363 |
| 无空闲态不变式 | 远征要么有下一个航点、要么已结束——没有"停着"这种状态；`should_end_at_last_waypoint = no` 时脚本必须在 `on_arrive_to_waypoint` 里 `add_new_waypoint`/`end_expedition`/`expedition_return_home`/`pause_expedition` 四选一，否则引擎**直接判失败并 ERRORLOG** | readme.txt:163-169 |
| 12 种远征 / 13 个类型 id | 本目录 13 个非 readme 的 `.txt` **各定义一个类型**，合计 13 个 id；按主题归并成 12 种远征（其中"朝圣"一门有 `pilgrimage_expedition` 与 `holy_site_pilgrimage_expedition` 两个类型） | 目录实查 + glob `**/expedition_types/*.txt` |

> ⚠ 有 11 个 `*_outcome_overview` 本地化键在 `main_menu\localization\english\expedition_types_l_english.yml` 里存在，但**全库没有任何脚本定义对应的远征类型**：`bermuda_test_expedition`、`pining_expedition`、`danish_india_expedition`、`el_dorado_expedition`、`fountain_of_youth_expedition`、`city_of_caesars_expedition`、`steppe_embassy`、`eastern_alliance_embassy`、`arctic_passage_nw`、`gold_fleet_tierra_firme`、`gold_fleet_nueva_espana`（`Get-ChildItem game -Recurse -Include *.txt | Select-String '^\s*<id>\s*=\s*\{'` 零命中）。它们是**未实装的本地化残留**，不是可抄的模板。

### 12 种远征的文件与本地化名

| # | 定义文件 | 类型 id | 玩家见到的中文名 | 中文名来源键 |
| --- | --- | --- | --- | --- |
| 1 | `cape_route_to_india.txt` | `cape_route_to_india_expedition` | 通往印度的好望角航线 | `cape_route_to_india_expedition`（L161） |
| 2 | `cartographic_survey.txt` | `cartographic_survey_expedition` | 沿海测绘 | `cartographic_survey_expedition`（L169） |
| 3 | `circumnavigation_expedition.txt` | `circumnavigation_expedition` | 环球航行 | 同名键（L38） |
| 4 | `grand_embassy.txt` | `grand_embassy_expedition` | 大使团出访 | `grand_embassy_expedition`（L186） |
| 5 | `grand_tour.txt` | `grand_tour_expedition` | 壮游之地（**无 `<id>` 同名键**，只有 `grand_tour`／`grand_tour_expedition_desc`） | L46 / L190 |
| 6 | `hajj_caravan.txt` | `hajj_caravan_expedition` | 王室朝觐 | `hajj_caravan_expedition`（L197） |
| 7 | `holy_site_pilgrimage.txt` | `holy_site_pilgrimage_expedition` | 朝圣圣地 | `holy_site_pilgrimage_expedition`（L201） |
| 8 | `mining_survey.txt` | `mining_survey_expedition` | 高地勘探测绘 | `mining_survey_expedition`（L209） |
| 9 | `pacific_crossing.txt` | `pacific_crossing_expedition` | 寻找回航航线 | `pacific_crossing_expedition`（L218） |
| 10 | `pilgrimage_expedition.txt` | `pilgrimage_expedition` | 大朝圣 | 同名键（L9） |
| 11 | `relic_expedition.txt` | `relic_expedition` | 圣物使团 | 同名键（L85） |
| 12 | `treasure_fleet.txt` | `treasure_fleet_expedition` | 宝船船队 | `treasure_fleet_expedition`（L231） |
| 13 | `western_ocean_voyage.txt` | `western_ocean_voyage_expedition` | 西洋远航 | `western_ocean_voyage_expedition`（L234） |

**两套中文名不是笔误**：`<id>` 是**发起动作**的名字（如 `holy_site_pilgrimage` = "前往圣地朝圣"，L13），`<id>_expedition` 是**远征面板**里的名字（"朝圣圣地"）。两者都用 `_desc` / `_historical_info` / `_outcome_overview` 后缀。另有第三种例外键：`pilgrimage_expedition`、`circumnavigation_expedition`、`relic_expedition`、`cape_route_to_india`、`cartographic_survey`、`mining_survey`、`treasure_fleet`、`western_ocean_voyage`、`hajj_caravan`、`grand_embassy`、`pacific_crossing`、`grand_tour` 同时存在**不带 `_expedition` 的同名键**（L9/L38/L85/L42/L77/L81/L20/L30/L34/L58/L65/L46），是发起动作的另一套文案——mod 抄名字时**两套都要查**，缺一个就显示 raw key。

## 字段全景：31 个顶层字段

「共享」= 给出该字段的类型数 / 13。「readme 行」给 readme 里的声明位置；标「未在 readme 中说明」的字段在 readme 里**完全没有条目**。

| 字段 | 值类型 | 作用域 / 语义 | readme 行 | 原版用量 |
| --- | --- | --- | --- | --- |
| `unique` | yes/no（默认 no） | 该国同时只能有一个本类型实例 | 9-15 | **13/13**（12 yes、1 no） |
| `potential` | trigger | **国家**；只控制 UI 里显不显示本类型，与能否发起无关 | 95-103 | **13/13** |
| `can_start` | trigger | **国家**；点"发起"时的门槛 | 106-113 | **13/13** |
| `leader` | trigger | **角色**；可选哪些角色带队。常用词：`is_ruler`、`is_explorer`、`is_admiral`、`is_adult`、`is_heir_of_court_country` | 116-124 | **13/13** |
| `on_start` | effect | **国家**（`scope:expedition` 可用）；发起瞬间。设变量、注入动态航点、`pause_expedition`、排开场事件都在这 | 381-382 | **13/13** |
| `on_end` | effect | **国家**；成功完成——发奖励、交货、返乡事件 | 394 | **13/13** |
| `utility` | script value | **国家**；AI 自动派发的评分，**≤ 0 不会被选** | 251-263 | **12/13**（`relic_expedition` 没有） |
| `travel_speed` | 正数（默认 1） | 每段基础航行时间的标量乘数，<1 更慢 | 86-92 | **13/13**（0.5 ~ 1） |
| `on_arrive_to_new_location` | effect | **国家** + `scope:location`；每次进入新地点都触发（含返程、含全动态路径的每一步） | 388-390 | 12/13（`treasure_fleet` 缺） |
| `on_arrive_to_waypoint` | effect | **国家** + `scope:location`；每个航点（静态或动态）触发。`returns_home` 的引擎自建返程段**不触发**，除非类型另有静态 `waypoints` | 391-393 | 12/13（`treasure_fleet` 缺） |
| `origin` | 枚举 | 出发地：`capital`/`closest_owned`/`closest_port`/`closest_non_rural_port`/`none`。**对第一个静态航点求解**；没有静态航点时除 `none` 外全部回落到首都 | 73-83 | 11/13 |
| `dynamic_first_waypoint` | yes/no（默认 no） | 第一个航点由 `on_start` 的第一次 `add_new_waypoint` 注入。**完全没有静态航点时必填**，且该调用成了"出发地"而非路线一站；配 `origin = none` | 46-58 | 11/13（全 yes） |
| `on_fail` | effect | **国家**；`fail_expedition` 被脚本调用时触发；**只有** `fail_if_no_leader = yes` 的类型才在失去领队时触发。`on_start` 设的变量会留到失败时，这里也要清 | 395-399 | 10/13 |
| `on_monthly` | effect | **国家**；每月。只想在"真在航行"时触发要加 `scope:expedition = { expedition_is_traveling = yes }`；暂停中的远征 Traveling 状态仍为真，还需再排 `NOT = { scope:expedition = { expedition_is_paused = yes } }` | 383-387 | 9/13 |
| `variables` | 块 | 见下节。声明即"远征状态"面板里的一行 | 340-374 | 8/13 |
| `show_start_message` | yes/no（默认 yes） | 是否弹通用的"远征已出发"通知。**对走自家专用动作发起的类型无效**（如朝圣动作、中国宝船） | 223-234 | 8/13（全 no） |
| `travel_mode` | 枚举 | `both`（默认）/`land`/`sea`。`sea` 放行每段**终点**（港口算陆地）；某段终点在该域内不可达 → 寻路失败并 ERRORLOG | 278-294 | 7/13 |
| `repeatable` | yes/no（默认 **yes**） | 成功后能否再来一次。设 no 时**失败仍可重启**（失败不置完成标记） | 18-26 | 7/13 |
| `category` | 枚举 | `exploration`（默认）/`commercial`/`military`/`religious`/`diplomatic`。**纯表现**，无游戏效果；显示名走 loc 键 `expedition_category_<值>` | 266-275 | 6/13 |
| `icon` | 字符串 | ⚠ **未在 readme 中说明**。原版 6 次取值恒等于 `category_religious`（4）/`category_diplomatic`（2），与同块 `category` 一一对应；`main_menu\localization` 里查不到同名 loc 键，推断是 UI 图集标识而非 loc 键 | — | 6/13 |
| `fail_if_no_leader` | yes/no（默认 no） | 领队死亡/被清空即自动失败，跑 `on_fail` 并发 EXPEDITION_FAILED 消息，**不置完成标记**。默认 no 时引擎会补一个新探险家并触发 `on_expedition_leader_replaced` | 127-150 | 5/13（全 yes） |
| `returns_home` | yes/no（默认 no） | 最后一个航点后自动加返程段。**只有 `should_end_at_last_waypoint = yes`（默认）时引擎才自动加**；否则脚本自己调 `expedition_return_home` | 28-43 | 5/13（全 yes） |
| `ai_leader_source_list` | 块 | **国家**；告诉 AI 去查哪些角色（`ruler = { add_to_list = source }`）。留空 AI 只查一部分角色，但最终会查完 | 297-306 | 5/13 |
| `should_end_at_last_waypoint` | yes/no（默认 **yes**） | 到达路径末项是否立即完成并跑 `on_end` | 153-169 | 4/13（全 no） |
| `leader_utility` | script value | **角色**；给角色打分，≤ 0 不会被选 | 308-319 | 4/13 |
| `triggers_regency` | yes/no（默认 no） | 统治者带队时国内进入摄政 | 214-221 | 3/13（全 yes） |
| `show_end_message` | yes/no（默认 yes） | 成功后是否弹通用"远征归来"。只压弹窗，不影响 `on_fail` 的 EXPEDITION_FAILED 消息 | 237-248 | 2/13（全 no） |
| `waypoints` | 列表 | 有序静态航点，写具名地点键 | 61-70 | 2/13 |
| `on_construction_finished` | effect | **国家**；`start_expedition_wait_construction` 的施工在当前地点完成时触发，同样受无空闲态不变式约束 | 400-404 | 1/13（仅 `mining_survey`） |
| `ai_chance_to_check` | script value | AI 每月检查本类型的概率，**钳在 0~1**；不写则用 define `EXPEDITION_DEFAULT_CHANCE_TO_CHECK` | 321-332 | **13/13** |
| `stalled_status_text` | loc 键 | ⚠ **未在 readme 中说明**。`stall_expedition` 停滞期间 UI 状态文字，原版 2 处：`grand_tour_expedition_stalled_status`、`grand_embassy_expedition_stalled_status` | — | 2/13 |
| `on_fail` 之外的钩子 | — | readme 还提到 `on_expedition_leader_replaced` on_action（引擎换将时触发），它**不是**本类目字段 | 143 | — |

### 枚举取值白名单（本体实测）

| 字段 | 出现过的值（次数） |
| --- | --- |
| `unique` | `yes`(12)、`no`(1，grand_tour) |
| `travel_speed` | `0.75`(6)、`1`(4)、`0.8`(1)、`0.5`(1)、`0.9`(1) |
| `origin` | `capital`(5)、`closest_non_rural_port`(4)、`none`(2) |
| `travel_mode` | `sea`(5)、`both`(2) |
| `category` / `icon` | `religious`/`category_religious`(4)、`diplomatic`/`category_diplomatic`(2) |
| `dynamic_first_waypoint` | `yes`(11) |
| `repeatable` | `no`(4)、`yes`(3) |
| `fail_if_no_leader`、`returns_home`、`triggers_regency` | 各自只用过 `yes`（5/5/3）——**省略即否** |
| `should_end_at_last_waypoint`、`show_start_message`、`show_end_message` | 各自只用过 `no`（4/8/2）——**省略即默认 yes** |

### readme 声明、13 个定义文件从未使用

口径：**「13 类型文件」= 本目录 13 个非 readme 的 `.txt` 内命中次数**（这一列为 0 才算"没被原版当类型字段用过"）；「全库其它位置」把 `trigger_localization`／`effect_localization`／`events\expeditions\`／其余类目一并算上。数字由 `Get-ChildItem game -Recurse -Include *.txt,*.info | Select-String '\b<词条>\b'` 按目录分类统计得出，可复跑。

| 名字 | 13 类型文件 | 全库其它位置 | 结论 |
| --- | --- | --- | --- |
| `expedition_return_home` | 0 | `effect_localization\expedition_effects.txt` 1 + `relic_expedition_events.txt:973,1249,1501` | 只由事件调用（`should_end_at_last_waypoint = no` 的 `returns_home` 类型自己发起返程） |
| `resume_expedition` | 0 | effect_loc 1 + 168 处事件（`mining_survey_events.txt:46` 等） | 只由事件调用（解除停滞） |
| `expedition_is_in_port` | 0 | **仅 readme 自身 1 处** | ⚠ readme 声明但**全库无任何脚本使用** |
| `clear_expedition_waypoints` | 0 | effect_loc 1 + `circumnavigation_expedition_events.txt` 等 3 处 | 有 effect_loc 词条，原版 3 处调用 |
| `set_expedition_leader` | 0 | effect_loc 1 + **3 处事件**（在役换将） | 有 effect_loc 词条，原版确有调用——**不是**"只靠引擎自动补人" |
| `stall_expedition` | **1**（`grand_embassy.txt:146`） | effect_loc 1 + 171 处事件 | 类型文件里也用过 1 次 |
| `expedition_is_stalled` | **3**（`grand_embassy.txt:158,180,220`） | trigger_loc 1 + 10 处事件 | readme 说的"可从脚本查状态"属实 |
| `expedition_is_waiting_on_construction` | 0 | **仅 `trigger_localization\expedition_triggers.txt` 1 处** | 有触发器词条，原版脚本 0 处调用 |
| `end_expedition` | **4**（`grand_tour.txt:297`、`relic_expedition.txt:181`、`pacific_crossing.txt:312` 等） | effect_loc 1 + 3 处事件 | 无空闲态不变式的兜底手段，原版在用 |
| `add_new_waypoint = { location = ... hidden = yes }`（隐藏航点块形式） | 0 | **全库 0 处** | 只在 readme:424-442 声明；隐藏航点机制原版未使用 |
| `<type>_waypoint_<location>` 航点消息 loc 约定 | — | **全库 0 处**（`^\s*[a-z_0-9]+_waypoint_[a-z_0-9]+\s*:` 零命中） | 免脚本消息机制已就位（`messages_l_english.yml:15446-15454` 的 `EXPEDITION_WAYPOINT_*` 通用键）但**原版一个都没写** → mod 空白区 |
| `ai`（是否让 AI 考虑本类型，默认 yes） | **0** | **全库 0 处**（只有 readme:339 自己） | ⚠ 唯一真正"声明了但原版从没用过"的类型级字段；普查报告 §2.3 判"疑似废弃" |
| `add_to_list`、`limit`、`if`、`set_variable`、`NOT`、`min`、`max`、`add`、`value`、`name`、`format`、`change_format` 等 | 大量（见下 `variables` 节） | 全库普遍 | 普查报告把它们算进"readme 字段"口径才得出 39 个 0 使用；**它们是通用脚本词条／块内键**，本类目块内确实在用，不要误判为无效 |

> 结论：**类型级字段里只有 `ai` 一个原版零使用**（`expedition_is_in_port` 是第二个，性质上属触发器）。其余 readme 词条都只是"不写在类型文件里，而由事件/效果调用"。
>
> 另有 3 个**块内键** readme 声明但全库零使用：`hidden`（`add_new_waypoint` 的块参数）、`monthly_change_hidden`（`variables` 的条目键）、`format`（`variables` 的条目键——13 个文件只用了 `change_format`）。

### readme 未声明、本体在用的字段

只有两个，都是**顶层**字段（普查报告 §2.2 只抓到一个 `stalled_status_text`，因为它的"字段"识别只认 `key = <标量>` 形式，`icon = category_religious` 被当成裸值跳过了——这两个都要在别处交叉验证过才敢写）：

- `icon`（6 次；取值恒等于 `category_religious`／`category_diplomatic`，见上表）
- `stalled_status_text`（2 次；`grand_tour.txt:9`、`grand_embassy.txt:14`）

## `variables` 变量块（8 文件 · 26 个变量）

变量名就是 `scope:expedition` 上 `set_variable`/`change_variable`/`clamp_variable`/`var:` 用的同名键——**在这里声明是纯增量的**，已有脚本继续有效；`start` 只是替换 `on_start` 里的 `set_variable` 初值。

```
variables = {
    <变量名> = {
        start = 100                    # 初值；远征创建时写入，早于 on_start，on_start 仍可覆盖
        min = 0                        # 显示范围下限
        max = 100                      # 显示范围上限
        icon = "gfx/interface/..."     # 可选 DDS，值旁的小图标
        display = piechart             # 可选；piechart = 环形量表，fraction = "X/max" 条，不写 = 纯数值
        format = "SOME_FORMAT"         # 可选 loc 键，控制数值怎么显示
        change_format = "SOME_FORMAT"  # 可选 loc 键，控制月度变化怎么显示
        monthly_change = { ... }       # 可选脚本值，每月应用并按 min/max 钳制
        hidden = no                    # 可选；yes 则整行不显示
        monthly_change_hidden = no     # 可选；yes 则只藏月度变化那行
    }
}
```

块内键实测（普查报告 §四 + 本次深度 3 实测）：

| 块内键 | 次数 / 文件数 | 说明 |
| --- | --- | --- |
| `min` | 26 / 8 | 每个变量都写了 |
| `icon` | 26 / 8 | 每个变量都写了 |
| `start` | 25 / 8 | 仅 `grand_tour` 的 `gt_diligence` 省略（它由 `on_start` 按继承人三维均值算） |
| `max` | 20 / 7 | `cargo`、`clues`、`hunt_retinue`、`retinue` 这几个"消耗型"变量故意无上限 |
| `display` | 15 / 7 | `piechart` 11 次、`fraction` 4 次 |
| `monthly_change` | 8 / 4 | 出现在 `cape_route_to_india`、`circumnavigation_expedition`、`pacific_crossing`、`pilgrimage_expedition` |
| `change_format` | 8 / 4 | 同上 4 个文件，取值恒为 `"VARIABLE_CHANGE_FORMAT"` |
| `subtract` | 15 / — | `monthly_change` 与 `random` 块里唯一用过的运算符 |
| `desc` | — | `subtract` 的说明 loc 键，如 `EXPEDITION_PROVISIONS_TRAVEL_DRAIN` |

**`monthly_change` 在远征自己的作用域求值**（可直接读 `var:x` 与航行状态），这与 situations / disasters 的 `monthly_change` 跑在**类型作用域**不同（readme:370-373）。原版标准写法（`cape_route_to_india.txt:19-31`）：

```
monthly_change = {
    subtract = {
        desc = "EXPEDITION_PROVISIONS_TRAVEL_DRAIN"
        value = 0                      # 基线 0
        if = {
            limit = {
                expedition_is_traveling = yes
                NOT = { expedition_is_paused = yes }   # 停靠/暂停时不消耗
            }
            add = 2                    # 航行中每月扣 2
        }
    }
}
```

**26 个原版变量名**（深度 2 实测，按声明次数排）：`provisions`(4 文件)、`cargo`(3)、`morale`(3)、`num_ships`(3)、`retinue`(2)、`clues`(1)、`connections`(1)、`discretion`(1)、`experience`(1)、`gt_diligence`(1)、`guards`(1)、`hunt_retinue`(1)、`knowledge`(1)、`modernization`(1)、`opportunities`(1)、`piety`(1)、`splendor`(1)。
（`provisions`/`cargo`/`morale`/`num_ships` 四个变量在 `cape_route_to_india`、`circumnavigation_expedition`、`pacific_crossing` 三个远洋类型里逐字重复声明，`provisions` 另有 `pilgrimage_expedition` 一版——**远洋类型的"船员状态四件套"是照抄的**。）
每个声明变量需要 **`<name>` 与 `<name>_desc` 两条 loc**（readme:364-365）；`main_menu\localization\simp_chinese\expedition_types_l_simp_chinese.yml` 里对得上，如 `morale`(L213)/`morale_desc`(L214)、`provisions`(L224)/`provisions_desc`(L225)。

## 作用域与生命周期钩子

```
<type_id> = {
  # ── 身份 / 重复性（国家）
  unique / repeatable / category / icon            # 标量
  potential / can_start                            # trigger，国家
  leader                                           # trigger，角色
  # ── 起点 / 路线（国家）
  origin / travel_mode / travel_speed / waypoints / dynamic_first_waypoint / returns_home
  # ── 时序 / 界面（国家）
  should_end_at_last_waypoint / show_start_message / show_end_message
  triggers_regency / fail_if_no_leader / stalled_status_text
  # ── 数据（远征实例）
  variables = { <名> = { start/min/max/icon/display/format/change_format/monthly_change/hidden/… } }
  # ── AI（国家）
  utility / ai_chance_to_check / ai_leader_source_list / leader_utility
  # ── 生命周期钩子（全部跑在国家作用域）
  on_start                          #            + scope:expedition
  on_monthly                        #            + scope:expedition
  on_arrive_to_new_location         #            + scope:expedition + scope:location
  on_arrive_to_waypoint             #            + scope:expedition + scope:location
  on_construction_finished          #            + scope:expedition（施工完成）
  on_end                            #            + scope:expedition
  on_fail                           #            + scope:expedition
}
```

- **抵达钩子提供 `scope:location`**，其它钩子没有；`scope:expedition.expedition_current_location` 才是任何钩子都能拿的当前位置（readme:378-379, 418-419）。
- `on_fail` 触发路径只有两条：脚本显式 `fail_expedition = scope:expedition`，或 `fail_if_no_leader = yes` 且领队没了。**没有 `fail_if_no_leader` 的类型引擎永远不会自己判失败**（readme:395-399）。
- `pause_expedition` 写成 `scope:expedition = { pause_expedition = 30 }`（**块内**）；`stall_expedition`/`resume_expedition`/`end_expedition`/`fail_expedition`/`expedition_return_home` 是**当值传**：`stall_expedition = scope:expedition`（readme:204-207）。混用是高频错误。

## 12 种远征对照表

### 总表

| 类型 id | 类别 | 目标 | 关键门槛（`potential` / `can_start`） | 奖励类型概要 | 失败条件 | 文件 |
| --- | --- | --- | --- | --- | --- | --- |
| `cape_route_to_india_expedition` | exploration | 沿非洲南下绕好望角到科泽科德 | `potential`：`tag = POR`；`can_start`：`has_advance = por_pursuing_waves` + `por_feitorias` + 有非农村港口 | 国家修正（`cape_route_pioneer`/`portuguese_african_network`/`east_africa_trade_dominance`）+ 威望 + 正统 + 胡椒售金 | 无 `on_fail` 门槛；`on_fail` 只发失败事件并清变量 | `cape_route_to_india.txt` |
| `cartographic_survey_expedition` | exploration | 测绘本国海岸港口 | `potential`：`has_advance = explorer_commisions_advance` + 有港口；`can_start`：非农村港口 + `at_war = no` + 无 `accurate_charts` + 无冷却 + 至少一处非首都港口 | 逐港地块修正（**10/15/18/20 年**）+ 国家修正 `accurate_charts`（5475 天 = 15 年）+ 威望 | `on_fail` 只清 `coasts_surveyed` | `cartographic_survey.txt` |
| `circumnavigation_expedition` | exploration | 环球航行（13 个静态航点） | `potential`：首都所在 `region:iberia_region`；`can_start`：`open_sea_exploration` + 发现美洲 + 发现摩鹿加 + 有殖民领附属国 + 核心港口 | **全球首次**：威望 +100 + 正统 +10 + 国家修正 `first_circumnavigation`；任意完成 +10 与额外威望；丁香售金 | `num_ships <= 0` 则由 `on_monthly` 判失败 | `circumnavigation_expedition.txt` |
| `grand_embassy_expedition` | diplomatic | 走访西欧宫廷、偷师制度与军队 | `potential`：`has_variable = grand_embassy_setup`；`can_start`：`has_ruler = yes` + 统治者 `owner = root` | 制度进度（`5 × (1 + modernization/100)`，首都 ×1.5）+ 陆军传统 +（访问过港口时）海军传统 | `fail_if_no_leader = yes`；`on_fail` 清 `on_grand_embassy` 与旗手变量 | `grand_embassy.txt` |
| `grand_tour_expedition` | exploration（无 `category`） | 继承人游历意大利名城 | `potential`：有继承人 + 首都属欧洲且非意大利；`can_start`：`gold >= 250` + 继承人成年男性 <35 岁且不在内阁 + 无冷却 + 已接纳科学/军事革命 | 继承人三项属性成长 + 特质（`free_thinker`/`well_connected`/`worldly`）+ 角色修正 `returned_from_grand_tour` + 威望 | `fail_if_no_leader = yes`；`on_fail` 清 20+ 个 `gt_visited_*` 变量 | `grand_tour.txt` |
| `hajj_caravan_expedition` | religious | 统治者赴麦加朝觐 | `potential`：`religion_group:muslim` + 有统治者；`can_start`：`at_war = no` + 统治者无 `hajji_pilgrim` + 首都非麦加 + 到麦加有路 + 无冷却 | 威望 +20 + 宗教影响力 +20 + 返乡 +10 正统 + 国家修正 `hajj_pious_return`（5 年）+ 首次永久 `hajji_pilgrim` | **出发当年未抵麦加即彻底失败**（`hajj_caravan.txt:282-288`） | `hajj_caravan.txt` |
| `holy_site_pilgrimage_expedition` | religious | 统治者赴选定圣地祈祷 | `potential`：有统治者 + 宗教有圣地 + `has_variable = pilgrimage_holy_site`；`can_start`：无冷却 + 统治者属于本国 | 宗教货币（`religious_influence`/`purity`/`karma`，按宗教有无该货币逐条发放）+ 亚南廷 ±20 + 秘教/法学倾向 + 教团虔诚标记 | 无 `on_fail` 门槛，只清变量与角色修正 | `holy_site_pilgrimage.txt` |
| `mining_survey_expedition` | exploration（无 `category`） | 勘查本国高地矿脉 | `potential`/`can_start`：见下节完整拆解 | 地块矿脉修正（15~20 年）+ 威望或金 + 国家修正 `thorough_survey`（7300 天） | **无 `on_fail` 块**——不会被判失败（`fail_if_no_leader` 也没设） | `mining_survey.txt` |
| `pacific_crossing_expedition` | exploration（无 `category`） | 找出太平洋回航航线（马尼拉大帆船） | `potential`：发现美洲 + 无全局变量 `tornaviaje_discovered` + 在美洲太平洋海岸有存在；`can_start`：`open_sea_exploration` + 该地理区域内有自家港口 | 国家修正 `manila_galleon_route` 或 `southern_return_route`（各 10950 天）+ 全球首次 +100 威望 +15 正统 + 丁香售金 | 无 `on_fail`（`on_end` 整体带 `has_variable = tornaviaje_returning` 守卫，没找到回航线则什么都不给） | `pacific_crossing.txt` |
| `pilgrimage_expedition` | religious | 天主教统治者大朝圣（罗马/耶路撒冷等） | `potential`：`religion:catholic` + 统治者也是天主教 + 首都非新月地区；`can_start`：`at_war = no` + 统治者成年且无 `character_on_pilgrimage`/`pilgrim_ruler` + 无摄政 + 无冷却 | 正统 `10 × piety/50` + 教士阶层满意度同比例 + 教宗好感 + 角色修正 `pilgrim_ruler` | `fail_if_no_leader = yes`；`on_fail` 按领队是否存活分走两个事件 | `pilgrimage_expedition.txt` |
| `relic_expedition` | religious | 天主教政权访求圣物 | `potential`：`religion:catholic` + 非 4/5/6 时代 + 神权政体或灵性主义 < -75；`can_start`：`gold >= 80` + `at_war = no` + 无冷却 | 威望 +10 + 教士满意度 + 宗教影响力 + 灵性主义微移 + 领队 +10 adm/+10 dip + 圣物（`work_of_art_type:holy_relic`） | 无 `on_fail`；`clues` 或 `hunt_retinue` 耗尽由事件判失败 | `relic_expedition.txt` |
| `treasure_fleet_expedition` | diplomatic | 宝船队巡访选定区域 | `potential`：`enabled_treasure_voyages` + `chinese_treasure_voyage_setup`；`can_start`：非农村港口 + `gold >= 200` + `at_war = no` + 无冷却 | 外交覆盖 + 朝贡 + 珍稀货物与使节（具体在 `treasure_fleet_events` 链里结算） | 无 `on_fail` 门槛；`on_fail` 清宝船状态 | `treasure_fleet.txt` |
| `western_ocean_voyage_expedition` | exploration | 向西找出通往印度的海路（哥伦布） | `potential`：首都 `sub_continent:western_europe` + 有港口 + 未发现美洲；`can_start`：`has_advance = open_sea_exploration` + 有非农村港口 + 无冷却 | **发现美洲**：威望 +20 + 事件 `western_ocean_voyage_events.7`；沿途逐段 `discover_area` | 无 `on_fail` 门槛，只清 `landfall_set` | `western_ocean_voyage.txt` |

> **奖励口径换算**（`main_menu\common\script_values\default_values.txt`）：`prestige_weak_bonus = 5`、`prestige_mild_bonus = 10`、`prestige_severe_bonus = 15`、`prestige_extreme_bonus = 20`、`prestige_radical_bonus = 100`；`legitimacy_mild_bonus = 10`、`legitimacy_severe_bonus = 15`。中文 `_outcome_overview` 文案里的 +5/+10/+20/+100 与之一一对应。

### 逐条要点

**1. `cape_route_to_india_expedition`**（葡萄牙专属）
- `potential` 直接写 `tag = POR`（`cape_route_to_india.txt:70`）——把整条航线锁给一个国家的最简写法。
- 21 个动态航点全在 `on_start` 里 `add_new_waypoint` 注入（L111-131），抵达科泽科德后再追加返程 4 站（L348-353）；`dynamic_first_waypoint = yes` + `origin = closest_non_rural_port`。
- 奖励是全球首次判定：`NOT = { has_global_variable = cape_route_discovered }` → 设全局变量 + `prestige_extreme_bonus` + `cape_route_pioneer`（10950 天）；否则只给 `prestige_radical_bonus`（L357-369）。商站数（`cape_feitorias_established`）3/5 档再换更强的贸易修正（L403-423）。
- 货物变现范式：`add_gold = { value = scope:expedition.var:cargo multiply = "capital.market.market_price(goods:pepper)" }` + `capital.market.add_goods_supply`（L392-401）。

**2. `cartographic_survey_expedition`**
- `can_start` 用 `NOT = { has_country_modifier = accurate_charts }` 反制自己的奖励（L16）——于是"冷却"实际由 `on_end` 给的 `accurate_charts`（`days = 5475` = 15 年，L156-159）决定，`add_cooldown = 5 年`（L31-34）只是短期锁。
- `on_start` 全动态生成航线：1 个最发达自有港口 + 最多 5 个港口的相邻海域与沿岸地点（L44-77），每条已访问地点打 `cartographic_survey_visited` 变量去重。
- `on_end` 三档结算（L153-171）：`coasts_surveyed >= 3` → `accurate_charts` + `prestige_mild_bonus`；`>= 1` → `prestige_weak_bonus`；再统一清 `coasts_surveyed` 与所有 `cartographic_survey_visited`（**变量建在 `every_owned_location` 上，清理也必须遍历地点**，这是容易漏的一步）。
- 10 种地块修正的年限不统一（`cartographic_survey_events.txt`）：`surveyed_safe_harbour`/`surveyed_reef_passage`/`surveyed_smugglers_cove` 10 年、`surveyed_fishing_grounds`/`surveyed_lumber_coast`/`surveyed_salt_pans` 15 年、`surveyed_dye_coast`/`surveyed_vineyard_coast` 18 年、`surveyed_amber_coast`/`surveyed_pearl_beds` 20 年。

**3. `circumnavigation_expedition`**
- 唯一带**静态 13 站 `waypoints`** 且同时 `origin = closest_non_rural_port` 的长航线（L10-24）；`on_start` 仍用 `add_new_waypoint` 把出发港插到最前（L123-132）。
- `on_end` 全球首次判定 + `first_circumnavigation` 修正（`exploration_preparation_time_modifier = -0.5`、`exploration_maintenance_efficiency = 0.25`，10950 天），并按 `num_ships >= 3`、`morale` 高低加减威望（L359-372）。

**4. `grand_embassy_expedition`**
- `should_end_at_last_waypoint = no` + `stalled_status_text`：航线在途中不断发现、由事件决定下一站，"学习"期间用 `stall_expedition` 停住（L146）——**有 `stalled_status_text` 的两个类型之一**。
- `on_start` 用 `ordered_known_country` + `order_by = "capital.distance_to(root.capital)"` 选最近的合格东道国（L83-88），选不到就硬编码 `location:amsterdam` 兜底（L97）。
- `on_end` 的制度传播量：`5 × (1 + var:modernization × 0.01)`，首都再 ×1.5（L272-290）；陆军传统恒给，海军传统需 `has_variable = grand_embassy_visited_port`（L321-330）。

**5. `grand_tour_expedition`**（唯一 `unique = no`）
- 门槛最"人物化"：`can_start` 里直接写 `heir = { is_adult = yes is_female = no character_age < 35 NOT = { in_cabinet = yes } }`（L49-58），`leader` 块用 `is_heir_of_court_country = yes` + `is_ruler = no` 双保险。
- `on_end` 按 `knowledge`/`connections`/`experience` 三者相对高低给三个不同特质（L307-343），并把三者折算成继承人 mil/adm/dip（L349-359）。
- `on_arrive_to_waypoint` 用一长串 `else_if scope:location = location:<城市>` 派发事件，最后 `else = { end_expedition = scope:expedition }` 兜底（L296-298）——**这是"无空闲态不变式"的标准兜底写法**。

**6. `hajj_caravan_expedition`**
- 唯一把"**当年抵达**"写成硬失败条件的类型：`on_arrive_to_waypoint` 在麦加比较 `current_year > var:hajj_departure_year`，超年走 `hajj_caravan_events.16`（失败）（L276-289）。
- 路线按 5 个 `hajj_caravan_routes_via_*` 触发器分支选途经点（L75-107），`ai_leader_source_list` 指向 `ruler`。

**7. `holy_site_pilgrimage_expedition`**
- 目标写在**国家变量**里：`potential` 要求 `has_variable = pilgrimage_holy_site`，`on_start` 读 `root.var:pilgrimage_holy_site.location` 生成航点（L13, L44）——由"朝圣动作"预先设好，所以 `holy_site_pilgrimage_desc` 明说"需使用朝圣行动启程，而非探险界面"。
- 奖励按宗教能力分三路：`add_religious_currency_pilgrimage`（`religious_influence`/`purity`/`karma`，L65-67）、`add_yanantin` ±20（按圣地神明性别，L70-85）、`mysticism_vs_jurisprudence` 微调（L88-97）。

**8. `mining_survey_expedition`** —— 见下节完整拆解。

**9. `pacific_crossing_expedition`**
- **`on_fail` 块根本不存在**：没找到回航航线时 `tornaviaje_returning` 变量不置位，`on_end` 整个被守卫挡住（L317-318），玩家一无所获但不判定失败。
- `origin = none` + `dynamic_first_waypoint = yes` 的**全动态范式**：出发港在 `on_start` 里由 `is_in_scripted_geography = scripted_geography:pacific_coast_americas_geography` 筛出并作为第一个航点（L95-117）。

**10. `pilgrimage_expedition`**（唯一 `travel_speed = 0.5`，最慢）
- `can_start` 里用 `custom_tooltip` 包住 `owner = root` 校验（L86-89）——让"统治者必须在国内"这条能显示成人话。
- `on_end` 的奖励随 `var:piety` 线性缩放：`legitimacy_mild_bonus × piety / 50`、`estate_satisfaction_mild_bonus × piety / 50`，教宗好感同样缩放（L456-481）；`on_fail` 按领队是否存活分走 `pilgrimage_expedition_events.17`（活着）或 `.9`（死了）（L496-506）。

**11. `relic_expedition`**
- `repeatable = yes` + 无 `on_fail` 块，失败交给事件链（`clues <= 0` 或 `hunt_retinue <= 0`）。
- `on_end` 只发 `custom_tooltip` 并按 `var:relic_pick`（1~11）逐个分支——真正发圣物的是 `scripted_effects\expedition_effects.txt` 的 `relic_expedition_grant_relic_effect`（L220-230，`create_art = { type = work_of_art_type:holy_relic }` + `move_art_and_owner` 到首都）。

**12. `treasure_fleet_expedition`**
- `origin = none` + `dynamic_first_waypoint = yes`，目标区域来自国家变量 `var:chinese_treasure_target_area`，`random_location_in_area` 里挑一个沿海地点（L62-67）。
- `on_start` 里 `pause_expedition = 14` 起手停顿（L45-47），`on_monthly` 分"航行中"（30% 触发 7 个事件之一）与"停靠中"（35% 触发 5 个之一）两套随机表（L184-219）。
- `on_end` 不直接发奖励，而是 `set_chinese_expedition_scopes = yes` + 派发 `treasure_fleet_events.9`（L228-229）——奖励全在事件链里。

**13. `western_ocean_voyage_expedition`**
- 唯一在 `on_arrive_to_waypoint` 里用 `random_list` 四选一（各 25%）决定登陆点，再置 `landfall_set = 1` 防重复（L74-81）——**"第一次抵达触发一次"的标准写法**。
- 三个静态航点全是海流/海岸名（`northern_fuerteventura_coast`、`atlantic_north_equatorial_current10/25`，L10-14），`returns_home = yes` + `show_start_message = no`。

## 一个完整例子：高地勘探测绘 `mining_survey_expedition`

`in_game\common\expedition_types\mining_survey.txt`（212 行）。这是**最短但覆盖全部机制**的原版类型：动态航点、开场决策事件、逐站随机好坏、施工等待、收尾国家修正，一个不缺。

### 1. 类型定义（L1-6）

```
mining_survey_expedition = {
	unique = yes
	travel_speed = 0.75          # 比基准慢 25%
	dynamic_first_waypoint = yes
	origin = capital             # 出发地 = 首都
	# 没有 category / icon → 落在默认 exploration 类，无专属图标
	# 没有 travel_mode → 默认 both
	# 没有 fail_if_no_leader / on_fail → 这个类型永远不判失败
}
```

### 2. `potential` 与 `can_start`（L8-60）

两块的筛选条件**逐字相同**（L9-30 与 L38-59）：至少一处自有地点同时满足

- 地形 `topography = mountains` 或 `hills`（L11-12），且
- 原产品属于 14 种矿产（L14-29）：`iron`、`copper`、`tin`、`lead`、`silver`、`goods_gold`、`coal`、`stone`、`marble`、`gems`、`mercury`、`salt`、`alum`、`saltpeter`。

`can_start` 在此之上再加 4 条（L34-37）：

| 条件 | 行 | 作用 |
| --- | --- | --- |
| `at_war = no` | 34 | 战时不能派 |
| `gold >= 40` | 35 | 现金门槛（对照开场事件的实际扣款见下） |
| `NOT = { has_country_modifier = thorough_survey }` | 36 | **反制自己的 `on_end` 奖励** —— 这是节奏的真正来源 |
| `NOT = { has_cooldown = mining_survey_cooldown }` | 37 | 短期锁（`on_start` 里上 5 年） |

`leader`（L62-66）：`is_adult = yes`、`is_expedition_leader = no`（已在带队的不行）、`has_exploration = no`。

### 3. `on_start`（L68-117）

1. `add_cooldown = { type = mining_survey_cooldown years = 5 }`（L69-72）——5 年短冷却。
2. 把领队从军队指挥位上摘下来（L73-78）——13 个类型里 13 个都写了这段同款 `scope:expedition = { if = { limit = { exists = expedition_leader } remove_commander = expedition_leader } }`。
3. 初始化两个国家变量：`prospects_struck = 0`、`highlands_surveyed = 0`（L79-80）。
4. `ordered_owned_location` 选途经点（L82-112）：limit 同 `potential` 的高地+矿产条件，再排首都（L104），`order_by = relative_raw_material_price`（L106，矿价越高越优先），`max = 4`（L107，**最多 4 站**），每选一处 `scope:expedition = { add_new_waypoint = prev }`（L109-111）。
5. `scope:expedition = { add_new_waypoint = root.capital }`（L114）——**首都作为终点**，这也是 `origin = capital` 之外的显式回程站。
6. `trigger_event_non_silently = { id = mining_survey_events.1 days = 5 }`（L116）——第 5 天弹开场事件。

### 4. 开场事件：二选一（`in_game\events\expeditions\mining_survey_events.txt:4-48`）

事件 `immediate` 里 `stall_expedition = scope:expedition`（L26）把远征无限期停住，`after` 里 `resume_expedition = scope:expedition`（L46）放行——**这是"等玩家决策"的正确姿势**（`pause_expedition` 只能猜时长）。

| 选项 | 键 | 成本 | 附带 |
| --- | --- | --- | --- |
| a 配齐化验师与工匠 | `mining_survey_events.1.a` | `change_gold_effect = { scale = -2 }` | `add_prestige = prestige_weak_bonus`（+5）+ `pause_expedition = 10`（10 天） |
| b 让地方头人供给 | `mining_survey_events.1.b` | `change_gold_effect = { scale = -1 }` | — |

`change_gold_effect`（`in_game\common\scripted_effects\country_gold_effects.txt:1-9`）= `add_gold = { value = scaled_gold_for_effect }`，`scale` 存成临时脚本值 `scope:scale`；`scaled_gold_for_effect`（`in_game\common\script_values\scaled_gold.txt:1-23`）为

```
1 × scale  +  首都财富 × 0.2 × scale  +  国家经济基础 × 0.05 × scale
（scale 缺省为 1；随后按 |scale| 与 5000×|scale| 钳制）
```

所以「2× 基准金」= 上面整式的 2 倍，`can_start` 的 `gold >= 40` 只是保底，实付随首都财富与经济基础水涨船高。

### 5. `on_arrive_to_waypoint`：60% 好 / 40% 坏（L125-149）

进门条件（L127-136）：当前地点是山地/丘陵、`scope:location.owner ?= root`、且不是首都。命中则 `highlands_surveyed +1`（L137），然后

```
random_list = {
	60 = { dispatch_good_mining_event = yes }   # L139
	40 = { dispatch_bad_mining_event = yes }    # L140
}
```

- 派发在 `scripted_effects\expedition_effects.txt`：`dispatch_good_mining_event`（L100-157）按 `raw_material` 分 14 路到 `mining_survey_events.10~23`；`dispatch_bad_mining_event`（L161-218）分 14 路到 `mining_survey_events.30~43`。
- **好结局**（如 `mining_survey_events.10`，铁矿）：两个选项都挂 18 年地块修正 `rich_iron_seam` 并 `prospects_struck +1`；a 给 `prestige_weak_bonus`（+5），b 给 `change_gold_effect = { scale = 1 }`（L76-92）。
- **坏结局**（`.30~.43`）无收益。
- 14 种地块修正与年限（`main_menu\common\static_modifiers\location.txt`）：`rich_silver_vein`/`rich_gold_strike`/`productive_gem_pit` 给 **+75%** 本地该矿产出（20 年）；其余 11 种给 **+50%**（铁矿 18 年、铜/铅/锡 18 年、大理石/水银/铝 18 年、煤/石/盐/硝石 15 年）。**年限区间 15~20 年**。
- 抵达后有施工等待：`start_expedition_wait_construction = { duration = 30 goods_demand = demand:upgrade_rgo_demand_mining }`（L143-146），施工完成时跑 `on_construction_finished` → `add_prestige = prestige_weak_bonus`（L151-153，+5）。

### 6. `on_end`（L156-171）

```
add_country_modifier = { modifier = thorough_survey days = 7300 }   # L157-160
if   { limit = { var:prospects_struck >= 2 }   add_prestige = prestige_mild_bonus }   # +10
else_if { limit = { var:highlands_surveyed >= 1 } add_prestige = prestige_weak_bonus } # +5
remove_variable = prospects_struck
remove_variable = highlands_surveyed
```

`thorough_survey`（`main_menu\common\static_modifiers\country.txt`）只有一条 `global_raw_material_output = 0.05`（+5% 全国原材料产出），**7300 天 = 20 年**。

### 7. 为什么实际节奏是「每 20 年一轮」

`can_start` 第 36 行 `NOT = { has_country_modifier = thorough_survey }` 与 `on_end` 的 7300 天修正直接对冲：**上一轮刚结束，下一轮就被自己的奖励挡在门外，且这个封锁（20 年）比 `on_start` 上的 `mining_survey_cooldown`（5 年）长得多**。所以 5 年冷却形同虚设，真实周期由 20 年的 `thorough_survey` 决定。想改节奏的 mod 作者只需动 7300 或删掉第 36 行 `NOT`。

## 审查要点

1. **无空闲态不变式**：`should_end_at_last_waypoint = no` 的类型，每个 `on_arrive_to_waypoint` 分支（**包括最后一个航点、包括动态加的回程段**）都必须落到 `add_new_waypoint`/`end_expedition`/`expedition_return_home`/`pause_expedition` 之一。漏一个分支 → 引擎判失败 + ERRORLOG。`grand_tour.txt:296-298` 的 `else = { end_expedition = ... }` 就是标准兜底。
2. **`stall_expedition` 没有兜底**：`pause_expedition` 到期会走 FailWithExhaustedPath，**停滞不会**——决策事件的每个选项都必须 `resume_expedition` 并配 `add_new_waypoint`/`end_expedition`/`fail_expedition`，否则该远征永久卡死（readme:204-212）。
3. **`fail_if_no_leader` 默认 no**：不写它，领队死了引擎会自动补一个探险家并触发 `on_expedition_leader_replaced`，`on_fail` 永不触发。想让"领队死 = 全剧终"必须显式 `= yes`，并且**写 `on_fail` 块**——否则玩家只被告知"远征失联"，不知道损失了什么（readme:127-144）。
4. **`origin` 的静默回落**：没有任何静态 `waypoints` 时，除 `none` 外的所有 `origin` 值都会回落到首都，**可能根本不在 `travel_mode` 允许的域内**，于是第一个航点就寻路失败。全动态类型请照 `pacific_crossing.txt:6`（`origin = none`）与 `treasure_fleet.txt:8` 写。
5. **`travel_mode = sea` 只放行"每段终点"**：港口被当作陆地所以能靠港，但**港口之间的陆路中转是禁止的**——内陆航点配 `sea` 必 ERRORLOG。默认 `both` 是最安全的。
6. **`repeatable = no` 不等于"不能再发起"**：`fail_if_no_leader` 触发的失败**不置完成标记**，所以 no 的类型失败后仍可重开；而成功后才真正锁死。
7. **开局变量与收尾清理要成对**：`on_start` 设的变量在失败时仍留在远征上，`on_fail` 与 `on_end` **都要清**（readme:397-399）。`cape_route_to_india.txt` 在 `on_end`(L433-448) 与 `on_fail`(L469-484) 里各清 14 个变量，可以当模板。
8. **变量名要配两条 loc**：`<name>` + `<name>_desc`，否则"远征状态"面板显示 raw key（readme:364-365）。
9. **`variables.min/max` 只是显示范围**：只有配了 `monthly_change` 的变量才会每月被钳制；纯脚本读写的变量会跑出这个区间（如 `min`/`max` 写 0/100 但脚本加到 150）。
10. **`switch` 内部值不校验**：动辄上百行的 `variables` 与 `on_monthly` 极难人工核对，改动后用 debug 事件或 `common\tests\` 跑一遍（见 `guides\testing.md`）。
11. **`icon` 与 `stalled_status_text` 都不在 readme 里**：`icon` 原版恒等于 `category` 对应的图集名（`category_religious`/`category_diplomatic`），照抄这两个值最稳；`stalled_status_text` 需自建 loc 键。
12. **本地化键要成对**：`<id>`（发起动作名）与 `<id>_expedition`（远征面板名）两套都要写；`grand_tour_expedition` 就是缺了 `<id>` 同名键的例子。另有 `_desc`、`_historical_info`、`_outcome_overview` 三类后缀，`_outcome_overview` 是**只在该键存在时**才显示在 Start 提示里的手写激励语（readme:466-475）。
13. **`<id>_waypoint_<地点键>` 是免脚本消息约定**：原版 0 处使用，写一个就能在该静态航点自动发非弹窗 EXPEDITION_WAYPOINT 消息，**只对静态 `waypoints` 生效**（动态航点没有脚本键），且只给人类玩家看（readme:445-464）。
14. **AI 侧三个门槛别一起压死**：`utility` ≤ 0 直接不选、`leader_utility` ≤ 0 不选该角色、`ai_chance_to_check` 被钳在 0~1。要观察者模式实测 AI 是否真会派（`guides\testing.md`）。注意 readme 声明的 `ai = no` **全库零使用**——假设它不生效，要关 AI 就把 `potential` 写成 `is_human = yes` 或把 `utility` 写成 0。
15. **普查报告的"字段"口径会骗人**：它只认 `key = <标量>`，于是 `icon`（裸值）被漏记、`if`/`limit`/`add`（通用脚本词条）被误记成"readme 字段零使用"。判断一个字段到底有没有用过，**必须再按目录 grep 一遍**（本节 §"readme 声明、13 个定义文件从未使用"的表就是这么来的）。

## 中文检索键

| 你会搜的中文 | 内部 id / 键 |
| --- | --- |
| 远征 / 探险 / 探险队 | `expedition_types`、`scope:expedition`、loc `custom_search_filter_expedition_category_name`（"远征类型"） |
| 远征类型分组 / 分类 | `category` 枚举 + loc `expedition_category_exploration`("探索扩张") / `_commercial`("商业") / `_military`("军事") / `_religious`("宗教") / `_diplomatic`("外交") |
| 勘测 / 测绘 / 沿海测绘 / 海图 | `cartographic_survey_expedition`、`cartographic_survey_cooldown`、国家修正 `accurate_charts` |
| 高地勘探 / 探矿 / 矿脉 / 高地勘探测绘 | `mining_survey_expedition`、`mining_survey_cooldown`、国家修正 `thorough_survey`、地块修正 `rich_*`／`productive_*` |
| 好望角 / 印度航路 / 香料 / 商站 | `cape_route_to_india_expedition`、`cape_route_pioneer`、`cape_route_established`、`portuguese_african_network`、`east_africa_trade_dominance`、建筑 `por_feitoria`／`por_trading_post` |
| 环球航行 / 麦哲伦 | `circumnavigation_expedition`、国家修正 `first_circumnavigation` |
| 宝船 / 宝船队 / 郑和 / 朝贡 | `treasure_fleet_expedition`、`treasure_fleet_cooldown`、事件 `treasure_fleet_events.*` |
| 朝觐 / 麦加 / 哈吉 | `hajj_caravan_expedition`、`hajj_caravan_cooldown`、`hajji_pilgrim`、`hajj_pious_return` |
| 朝圣 / 圣地 / 大朝圣 | `pilgrimage_expedition`、`holy_site_pilgrimage_expedition`、`pilgrim_ruler`、`character_on_pilgrimage` |
| 壮游 / 大游学 / 继承人游学 | `grand_tour_expedition`、`grand_tour_cooldown`、`returned_from_grand_tour`、变量 `knowledge`／`connections`／`experience`／`gt_diligence` |
| 大使团 / 使团 / 出访 / 制度考察 | `grand_embassy_expedition`、`on_grand_embassy`、变量 `discretion`／`modernization`／`opportunities` |
| 圣物 / 真十字架 / 荆冠 / 圣枪 | `relic_expedition`、`relic_expedition_cooldown`、`relic_expedition_grant_relic_effect`、`work_of_art_type:holy_relic` |
| 向西航行 / 哥伦布 / 发现美洲 | `western_ocean_voyage_expedition`、`western_ocean_voyage_cooldown` |
| 回航航线 / 马尼拉大帆船 / 复航 | `pacific_crossing_expedition`、`manila_galleon_route`、`southern_return_route`、变量 `tornaviaje_*` |
| 船员士气 / 补给 / 船数 / 货物 | 变量 `morale`、`provisions`、`num_ships`、`cargo`（+ 各自 `_desc`） |
| 领队 / 探险家 / 队长 | `leader`、`is_expedition_leader`、`expedition_leader`、`set_expedition_leader`、`make_into_expedition_leader` |
| 航点 / 途经点 / 路线 | `waypoints`、`add_new_waypoint`、`clear_expedition_waypoints`、`expedition_current_location` |
| 停滞 / 卡住 / 暂停 / 返航 | `stall_expedition`、`resume_expedition`、`stalled_status_text`、`pause_expedition`、`expedition_is_stalled`、`expedition_return_home`、`expedition_is_returning` |
| 摄政 / 统治者出访期间 | `triggers_regency` |
| AI 派发 / AI 会不会派 | `utility`、`leader_utility`、`ai_chance_to_check`、`ai_leader_source_list`、define `NAI|EXPEDITION_DEFAULT_CHANCE_TO_CHECK` |
