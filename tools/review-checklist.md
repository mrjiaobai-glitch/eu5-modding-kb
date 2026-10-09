# 审查清单（review-checklist）：可机械判定的项

> **一句话**：把全库「审查要点」按检查方式重排的统一检查表：文件结构、引用完整性、作用域、语义陷阱、本地化、合并覆盖与防误报白名单。
> **什么时候看**：逐文件审查 mod 时当主检查表用；判不准某条是不是错，就翻第七节白名单。
> **体量**：88 行 · 约 4 分钟通读

> **用途**：`eu5-mod-review` 逐文件审查时的**统一检查表**。内容是把本库 24 篇 vanilla + 103 篇 fields 里的「审查要点」按**检查方式**重新归类——**能脚本判的给判据，判不了的给对照表**。每条末尾括号里是权威档，细节回原档看。

## 一、文件与结构（脚本可判）

| # | 检查项 | 判据 / 常见错法 |
|---|---|---|
| 1 | 事件文件首行是 `namespace =` | ID 形式 `<ns>.<数字>`，数字 **1–9999**，跨全 mod 唯一（`fields\common-events.md`） |
| 2 | 事件 `type` 属白名单 | `country_event/location_event/unit_event/exploration_event/age_event`；**统计时必须按取值白名单过滤**，否则 `estate_type`/`casus_belli` 等同名键会污染（`pitfalls.md` §十三） |
| 3 | 文件名与定义名一致 | 任务包 `generic_x_mission_pack.txt` → 链 `generic_x`；面板文件名 = 对象 id；成就须进 `achievement_groups`（`fields\common-missions.md`、`gui-panels.md`） |
| 4 | 单 `.txt` 扩展名 | `x.txt.txt` **仍会被加载**（别判成死文件）；反过来**不能靠加后缀停用**原版档，要用 `.bak`（`vanilla\vanilla-dlc-and-assets.md`） |
| 5 | 花括号配对 + 顶层计数 | 用**花括号深度扫描**；`key =` 与 `{` 分两行的写法（`child_educations` / `resolutions` 3 个 / `biases` 1 个）会让缩进锚点漏算（`pitfalls.md` §十三） |
| 6 | 本地化文件带 **BOM** | 读前 3 字节须为 `EF BB BF`（mod 目录要求 BOM；**本库自身 `.md` 不带 BOM**）（`pitfalls.md` §五） |

## 二、引用完整性（脚本可判：抽引用 → 回原版对应类目验存在）

`price:<id>` · `goods:<id>` · `unit_type/unit_category:<id>` · `estate_type:<id>` · `law:<id>` · `advance` · `production_method:<id>` · `subject_type` · `parliament_type` · `special_status` · `payment` · `international_organization_type` · `resolution` · `casus_belli` · `wargoals` · `peace_treaties` · `god` / `avatar` · `holy_site_type` · `building_types` id · `trait` id（`upgrades_to`/`recovery_trait`/`juvenile_form`） · `societal_value`（**必须 `A_vs_B` 全串**） · geography id（`scripted_geography` 集合与 `area/region/continent` 必须是 `map_data\definitions.txt` 的真实层级） · **`modifier:<键>` 必须在 `modifier_type_definitions` 注册**。

> 通例：**引用写错一律静默失效**（不报错、永不命中）。类目字段权威见 `fields\` 对应档；loc 键存在性规则见 `tools\loc-keys.md`。
>
> **两类高频引用另有专档**：修正键 → `fields\main_menu-modifier_type_definitions.md`（3 个档 / 2,436 键 / `category` 10 值 / 必填 `game_data`）；旗帜 COA 键 → `fields\main_menu-flag_definitions.md` 与 `fields\main_menu-coat_of_arms.md`（`coa = <键>` 必须能在 4,566 个 COA 键里找到，或写成 `list "<池名>"`）。

## 三、作用域（人工对照；kb 已记录的高频错误）

| 对象 | 容易写错的地方 |
|---|---|
| 角色 | `character_interactions` / `child_educations` 的 `potential`/`allow` root = **角色**（国家是 `scope:actor`）；`death_reason` 的 `trigger` 是角色作用域 |
| 地点/省份 | `building_types` 的 `allow` root = **location**（`remove_if` 是 building）；任务 `highlight` 里是 **`scope:province`** 不是 location；`town_rights` 的 `allow` 是 `scope:target` = location |
| 国家 | `production_methods` / `road_types` 的 `potential`/`enabled` 是国家；`subject_types` 的 `visible` 是 overlord、`can_attack` 是 subject type |
| 国际组织 | `religious_factions` 的 trigger 是 **IO 作用域**；`parliament_*` 的 `type` 决定 root（country 还是 IO） |
| 灾难/局势 | 灾难 `can_start`/`on_*` root = disaster；局势的 `visible`/`tooltip`/`map_color` 是 country/location（**与 `can_start` 不同**）（`vanilla\vanilla-disaster-and-situation.md`） |
| 战争/和约 | `casus_belli` 三段门作用域不同；`peace_treaties` 固定四作用域 winner/loser/war/target |
| 任务 | 链级与节点级脚本 root = 国家；`select_trigger` 用 `scope:actor` |

## 四、语义陷阱（静态查不出，须按表核对）

- **默认游戏规则会关掉任务包**（`mission_packs_enabled_rule` 默认 disabled），且**任务级 `on_start`/`on_completion` 默认不执行**（`mission_rewards` 默认带 `task_rewards_disabled`）→ 状态维护必须放 `on_persistent_*` 或链级（`fields\common-missions.md`）。
- **`country_has_estate` 恒真**（不是版本差异）；**mod 自定义 subject type 的 `is_subject_type` 疑似恒真**——门槛改用变量（`pitfalls.md` §七）。
- **灾难防重复标记不得在 `on_end` 移除**（会无限重复、永久奖励反复领）（`pitfalls.md` §六）。
- **`chance` 结果被 `GetCeiling()` 向上取整** → `factor < 1` 不降概率，排除条件要写 `allow`（`fields\common-traits.md`）。
- **`ai_is_valid` 默认 false**：AI 要用必须显式开，并配 `ai_chance`/`ai_frequency`；**`scripted_guis` 表达式漏 `.End` 静默失效**（`fields\common-scripted_guis.md`）。
- **`visible = yes` 在 scripted widget 里失效**，必须返回布尔函数（官方 workaround 见档）（`fields\gui-core.md`）。
- **`custom_description` 的 `text` 键不能与 effect/trigger 同名**（否则取不到 subject/object/value）（`fields\common-scripted_effects.md`）。
- **顺序敏感块**：`levies` 特化单位必须在文件顶部（第一个匹配生效）；`country_name_construction` 同类——**都不能用 INJECT**（`pitfalls.md` §四）。
- **`rebel_demands` 的 `concession_effect` 必须含 `pacify_rebel_pops`**（原版 12/13），否则让步后叛军不平息（`fields\common-rebel_demands.md`）。
- **`defines` 覆盖原版零使用**：改常量牵动多人哈希与存档兼容（`fields\main_menu-game_rules.md`）。
- **`law` 与 `policy` 是两个概念**（law 只锁/解锁，policy 才可切换）（`vanilla\vanilla-law-and-estate.md`）。
- **`law`／`reform` 的 `potential` 会被引擎周期复查，失效即整条撤销**——potential 里出现**动态值**（兵力、阶层力量、金币、战争状态）即是错误；只允许政体／宗教／改革／tag 这类几乎不变的条件。要求"已生效后 potential 恒真"（`guides\law-design.md` §三、`cases\laws-events-and-estates-2026-09.md` §1）。

## 五、本地化（脚本可判）

1. 键存在性：**键是全局的、不按目录归属**——核键范围必须包含 `dlc\*\main_menu\localization\<lang>\` 与 `main_menu\localization\<lang>\missions\`（只搜顶层会误报"任务全缺键"）（`tools\loc-keys.md` 判缺键三原则）。
2. **肯定/否定成对**：只写 `<键>` 不写 `NOT_<键>`，在 `NOT = { … }` 里就是 raw key；人称回退是**最后一个可用**而非第一个（`fields\common-trigger-effect-localization.md`）。
3. 占位符 `$…$` 必须配对（`$MONTHS|0$` 取整）；`[Root.GetName]` 方括号表达式照抄原版（`guides\localization.md`）。
4. 事件/任务/静态修正/特质/死因/继承法的键前缀各不相同：**逐类对照 `tools\loc-keys.md`**，别按印象写。
5. **新建法律／政策／建筑／改革必须查撞名**：**显示名与键名都**要对照本体 `main_menu\localization\simp_chinese\` 全目录查一遍——同名政策出现在两条不同法律里，UI 上就是两个同名条目（两条法律都合法加载、都能选，只是分不清）；**键名撞了会直接覆盖本体本地化**。只查显示名不查键名是不够的（`guides\law-design.md` §九）。

## 六、合并与覆盖（脚本可判 + 人工确认）

- **事件不能 REPLACE/INJECT**（复制修改只会产生重复事件 ID）（`pitfalls.md` §四）。
- **REPLACE 必须逐字段保留原版内容**（只改一个字段会丢掉同块其它字段）。
- on_action 同名块是**合并**语义（直接写同名块追加）；`INJECT:` 追加会排在 fallback 之后 —— 顺序敏感处别用（`guides\merging.md`）。

## 七、防误报白名单（**这些不是错误，别"顺手修正"**）

| 现象 | 为什么不是错 |
|---|---|
| 字段有 readme 却原版零使用（`interface_lock` / `exclusive` / `random_valid` / `show_as_unavailable`） | 文档字段 ≠ 原版惯例（`fields\common-events.md`） |
| "零引用 = 死代码" | 引擎按名识别 static modifier：`capital_in_*`、`ruler_*`、`difficulty_*`、`<estate>_tax_impact` 等**零脚本引用但生效**（`pitfalls.md` §七） |
| `enable = 9999.1.1` | 新教系的正常写法（由局势启用，不是假数据） |
| `religious_school = X` 重复多行 | 重复行不是列表块（只在 sunni/shia/ibadi） |
| `law_country_goup` 拼写 | 原版就这么拼 |
| 27 B 的 `xxx = {}` 存根 | **面板契约**，删了界面挂 |
| `sort_keys` 里的 `their`/`our`/`will`/`works` | 原版真实键名（缩写），非笔误 |
| `.txt.txt` 双扩展名 | 引擎按 `.txt` 后缀匹配，照常加载 |
| 文档写"17 种附属国"这类数字 | ⚠️ **这条相反**：kb 自己错过（实为 20 个定义）——数字一律回原版数，见 `tools\kb-self-audit.md` |
| 阶层私兵相关：`*_estate_allowed_private_army` 写在特权里 | 正常——**开关是国家修饰符，特权只是打开它**；给非贵族/哥萨克阶层开私兵必须同时补 `estates\00_default.txt` 的 `private_army_per_pop` + `private_army_unit_categories`（`fields\common-estates.md`） |
| 远征：`can_start` 禁止某修正、`on_end` 却给同名修正 | **不是笔误，是节奏设计**（`thorough_survey` 7300 天挡住重开，`add_cooldown` 形同虚设）；改节奏要动奖励天数或删 `NOT`（`fields\common-expedition_types.md`） |
| 远征：`stall_expedition = scope:expedition` 与 `pause_expedition = { }` 写法不同 | 前者**当值传**、后者**块内调用**，混用静默无效；停滞**没有引擎兜底**，每个选项必须自己 `resume_expedition` |

## 八、相关档

`pitfalls.md`（实战坑全表） · `tools\loc-keys.md`（loc 键全表） · `tools\audit-ids.md`（角色体系 ID） · `tools\kb-self-audit.md`（本库数字体检与复跑清单） · `guides\testing.md`（游戏内测试流程） · `cases\`（真实 mod 复盘） · `fields\common-estates.md`（阶层数据与私兵） · `fields\common-expedition_types.md`（远征系统）
