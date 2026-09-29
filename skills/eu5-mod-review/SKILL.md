---
name: eu5-mod-review
description: EU5（jomini 引擎）模组全流程：制作与审查一体。制作侧：搭建 mod 骨架（metadata.json 注册、三区 common、localization 放置与 BOM）、按游戏本体文件约定编写事件/效果/触发器/修正/单位/建筑等脚本、正确使用 INJECT/REPLACE 合并与本地化镜像、用游戏自带测试与 error.log 排错；审查侧：检查语法结构、作用域、具名 scope、var 变量体系、本地化键前缀规则、BOM 双镜像、合并前缀、括号配对，并对照原版 game 目录验证词条/ID 存在性，输出分级结构化自检报告。另含原版机制解析（vanilla-*.md **23 篇**：天气/战斗/POP/天命/科技时代/贸易市场/生产建筑/文化宗教/法律阶层议会/灾难局势/国际组织IO/角色王朝内阁/AI/政体改革官僚/殖民与探索/事件与任务/DLC 与美术资源/纹章与旗帜/界面GUI…，标注可改点与硬编码边界）。全部知识基于游戏本体目录实际文件（**101 篇字段库**（74 份 readme 提炼 + 实查补缺）+ 制作层知识库 + **23 篇原版机制解析** + 5 篇查表工具），索引见 <KB>\README.md 与 <KB>\INDEX.md
whenToUse: 用户要求新建/制作/编写/扩展/审查/检查/自检/验证 EU5 模组内容（事件、科技、建筑、单位、修正、国家、本地化、游戏规则、define 调整等），或询问"怎么做 X"需要按原版规范落地时
disable-model-invocation: false
---

# EU5 Mod Development（制作 + 审查）

> **知识库根 `<KB>` 解析**（按序探测，命中即用）：
> 1. **同仓形态**：`<本 SKILL.md 所在目录>\..\..\`——存在 `README.md` + `fields\` + `vanilla\` 即命中（`git clone` 本仓库后就是这种形态）
> 2. **本机形态**：`D:\dsh-plugins\kb\eu5-modding\`（作者本机；他人克隆仓库时命中第 1 条）
>
> 下文所有 `<KB>\...` 均指命中的那个根；知识库总索引为 `<KB>\README.md`。

你是 EU5（jomini 引擎）模组制作者与审查器。所有规范**只以游戏本体为准**：`F:\SteamLibrary\steamapps\common\Europa Universalis V\game\`（in_game / loading_screen / main_menu / dlc 四区）。动笔前先看 `<KB>\README.md`（人类入口 + 任务速查）与 `<KB>\INDEX.md`（全量文档逐档映射表）加载对应文档：**字段权威库**（fields\*.md，**101 篇**：74 份 readme 提炼 + 实查补缺，只含 readme 实际声明的内容）+ **制作层知识库**（`guides\`：game-layout/mod-skeleton/merging/event-making/scripting-core/systems-map/law-design/defines/localization/testing + 2 篇 E2E 教程，游戏本体实查）+ **查表层**（`tools\review-checklist.md` 审查清单 / `tools\loc-keys.md` 键表 / `tools\audit-ids.md` / `tools\kb-self-audit.md` 数字体检）+ 实测经验（`<KB>\cases\blades-and-thrones-2026-08.md`，EU5 1.3.x，冲突以知识库文档为准并标注）。**EU5 与 EU4 脚本体系不通用**：无 event_target/global_event_target、无 `KEY:0`、无 province_event、事件选项无 EU4 式 weight、作用域词是 root/prev/this 而非 ROOT/PREV。

---

# 第一部分：制作工作流

### 0. 定位原版目录（必做，所有对照与模板来源）
1. 探测 `SteamLibrary\steamapps\common\Europa Universalis V\game\`（本机为 `F:\SteamLibrary\steamapps\common\Europa Universalis V\game\`）；缺失则先定位再动手。
2. 三区 common 分工：**in_game\common**（主体：事件、科技、建筑、单位、修正器等）／**loading_screen\common\defines**（define 常量、graphic 图形常量、jomini 引擎常量）／**main_menu\common**（achievements、coat_of_arms、flag_definitions、game_concepts、game_rules、modifier_icons、modifier_type_definitions、named_colors、scenarios、scripted_lists、scripted_triggers、script_values、static_modifiers）。
3. localization 不在 in_game：**main_menu\localization\<lang>\**（主语言文件，english 114 个）与 **loading_screen\localization\<lang>\**（languages.yml、load_tips）与 **loading_screen\localization\jomini\**（savegame_gui、cw_ugc_dlc）；引擎触发词条在 **in_game\localization\jomini\script_system\trigger_system_l_<lang>.yml**。细节见 `<KB>\guides\localization.md`。

### 1. 建 mod 骨架（先搭目录，再写内容）
- 目录：`Documents\Paradox Interactive\Europa Universalis V\mod\<mod>\`，launcher 注册需 `.metadata\metadata.json`（name/id/version/supported_game_version/short_description/tags，真实样例见 `<KB>\guides\mod-skeleton.md`）。
- 结构模板照抄游戏本体 DLC：`game\dlc\D008_fate_of_the_phoenix\` 与 `game\dlc\D000_shared\` 展示 mod 包分区；本机已装 mod `刀锋与王座` 是现成参照。
- 大改小抄：**只放要改的文件**；同路径同名文件 = 覆盖合并（见合并体系）。

### 2. 选合并策略（每次动手前先决定）
- **新增独立内容**：直接新建文件，文件名数字前缀决定加载顺序（`00_` 最先；mod 用大数字或 `zzz_` 前缀保证后加载覆盖）。
- **改原版条目**：INJECT:（追加到同名块末尾；**顺序敏感块如 levies 特化单位、country_name_construction 不可用**）／REPLACE:（整块替换，**必须逐字段保留原版内容**）／REPLACE_OR_CREATE: ／INJECT_OR_CREATE:。
- **on_action**：同名块整块定义 = 合并语义（追加列表），直接写同名块即可。
- **事件不能 REPLACE/INJECT**：改原版事件 = 新文件 + 原版 namespace + 复制修改（无害 "Duplicated event ID"）。
- 全部规则与坑见 `<KB>\guides\merging.md`。

### 3. 写脚本（每个系统先加载对应文档）
- **事件**：`<KB>\guides\event-making.md`（权威格式 = `in_game\events\readme.txt` 逐条 + `volcano_events.txt` 真实样例）。要点：`namespace` 必定义、ID 1–9999 全局唯一、type 五选一（country/location/unit/exploration/age_event）、title/desc 键 = `<ns>.<id>.title/.desc`、选项 name = `<ns>.<id>.a`、ai_chance 用 base/add。
- **效果/触发器/脚本值/on_action**：`<KB>\guides\scripting-core.md`（`_script_values.info` 全文要点、scripted_effects/triggers 的 `$参数$` 约定、on_actions.info 全部键）。
- **各 common 类目**：先看 `<KB>\guides\systems-map.md`（每类目：用途、入口文件、readme 位置、对应字段文档），再加载字段文档核对，照原版样例复制改。
- **改机制数值/结构前**：先读对应 `<KB>\vanilla\vanilla-*.md` 原版解析（**23 篇**：weather / combat / pop / mandate-of-heaven / tech-and-age / **events-and-missions** / **dlc-and-assets** / **heraldry-and-flags** / gui …），里面写清了该系统由哪些文件组成、关键常量在哪、**哪些脚本可改、哪些引擎硬编码**、以及挂钩点；避免改错层（例如天气的风暴实际效果是硬编码，只能改生成与 `on_storm_reached_location`；时代按年份全局推进，`unique` 与 `modifier` 都是全世界生效的时代修正）。
- **修正（modifier）**：新增修正键必须在 `main_menu\common\modifier_type_definitions\00_modifier_types.txt` 注册（category/percent/boolean/format 等），再在 `main_menu\common\static_modifiers\` 定义或直接用于效果。
- **define 调整**：`loading_screen\common\defines\00_defines.txt`（N 块索引见 `<KB>\guides\defines.md`）或 `graphic\00_graphics.txt`；也可在 game_rules 里按设置覆盖（`_game_rules.info` 的 defines 段）。

### 4. 本地化（EU5 特有：BOM）
- 文件：`main_menu\localization\<lang>\<主题>_l_<lang>.yml`（真实 mod 也放 `in_game\localization\<lang>\`），首行 `l_english:`（或 `l_simp_chinese:` 等），**必须 UTF-8 BOM**（无 BOM 整文件被忽略/乱码）。
- 键规则：名称即键（事件 `<ns>.<id>.title`、修正 `STATIC_MODIFIER_NAME_` 等，全表见 `<KB>\tools\loc-keys.md`）；占位符 `$KEY$` 配对、`[Root.GetName]` 等 GUI 表达式。
- 详见 `<KB>\guides\localization.md`。

### 5. 测试与排错
- **游戏自带测试**：`in_game\common\tests\` 写 test（year/success/failure/end_year/success_child），格式与日志见 `<KB>\guides\testing.md`；error.log 每行含 "Script location: <文件:行>" 定位报错文件。
- 用 debug 事件（`orphan = yes` + `is_human = yes` 触发）验证效果；`events\debug\qa_debug.txt` 是现成模板。
- 观察者模式跑 50–100 年验证 AI 生态（事件是否真能被 AI 触发、灾难是否不重复触发）。
- 常见坑速查：`<KB>\pitfalls.md`。

### 6. 收尾自查
- 括号配对、`#` 注释是否吞代码、yml BOM 是否被编辑工具剥掉（剥掉要补回）。
- 引用的每个 ID（事件、修正、科技、建筑、单位、宗教、文化、地点）对照原版目录存在。
- 触发/效果词条不确定时**先查原版在用**（grep 游戏本体），不要凭 EU4 记忆补。
- 完成后执行下方"第二部分：审查流程"自检一遍。

---

# 第二部分：审查流程

## 前置流程
1. mod 目录：`Documents\Paradox Interactive\Europa Universalis V\mod\<mod>\`（metadata.json 注册）或 `SteamLibrary\steamapps\workshop\content\3450310\<id>\`。
2. 原版目录（词表对照）：优先探测 `SteamLibrary\steamapps\common\Europa Universalis V\game\`；缺失则降级"存疑"。common 分三处：`in_game\common\`（主体）、`loading_screen\common\`（defines）、`main_menu\common\`（modifier_type_definitions/static_modifiers 等）。
3. glob 发现：`**/*.txt`、`**/*.gui`、`**/*.yml`、`**/*.json`、`**/*.info`；排除 `.metadata`、备份、工具脚本。
4. 审查某类目前**先加载**知识库 `fields\` 对应文档，再逐个 read，按文末格式输出报告。

## 字段权威表（必读）
审任何类目文件前，查 `<KB>\README.md` 映射并加载 `<KB>\fields\*.md`（字段名/类型/取值/作用域/坑；readme 未写者标"未在 readme 中说明"，勿凭 EU4 补）。高价值：`common-events.md`（type/category/ai_chance 权威格式）、`common-subject_types.md`、`common-international_organizations.md`、`common-country_interactions.md`（diplo_chance 全键）、`common-missions.md`（任务链 11 条/108 节点）、`main_menu-modifier_type_definitions.md`（修正键注册表 2,436 键）、`main_menu-flag_definitions.md` + `main_menu-coat_of_arms.md`（纹章旗帜）、`main_menu-game_concepts.md`（概念词条）。

## 语法速记（细节以知识库 `fields\` / `guides\` 为准）
- `key = value` / `key = { }`；`#` 注释；值：数字/yes-no/字符串/脚本值。
- 事件 `namespace` + **type 五选一**（country/location/unit/exploration/age_event）；`category` 三值只定图标；`outcome` 三值 **`positive`/`neutral`/`negative`**（good/bad 无效，`negative` **合法**勿误报）；选项 `ai_chance` 用 base/add；触发 `trigger_event_silently` / `trigger_event_non_silently`。
- 作用域 root/prev/this/scope:xxx（save_scope_as）；无 ROOT/PREV、无 event_target。变量 set_variable/change_variable（仅 add/subtract/multiply/divide/modulo，无 value 键）/var:x/has_variable。本地化无 `:0`。合并前缀 INJECT:/REPLACE:/REPLACE_OR_CREATE:/INJECT_OR_CREATE:（INJECT 追加到末尾，顺序敏感块不可用）。

## 检查清单

### 1 结构与格式
括号配对/嵌套；`#` 吞代码；引号闭合。`.yml` 须 **UTF-8 BOM**（无 BOM 整文件被忽略/乱码）；`.txt` 建议 BOM。**BOM 双镜像（必查）**：`main_menu\localization\<lang>\` 与 `in_game\localization\<lang>\` 键必须完全一致（加载 main_menu 侧为主），脚本对比键数。重复键/空值；yml 键须在语言头（`l_simp_chinese:`/`l_english:`）下缩进（顶格键毁文件）。

### 2 事件结构（权威格式见 <KB>\fields\common-events.md）
事件 ID 必须 `namespace.integer`（0<integer<10000）且 mod 内唯一。type 五选一；category 只定图标；outcome = positive/neutral/negative（三值，默认 neutral）。title/desc/选项 name（`<id>.a/.b`，不写裸中文）/custom_tooltip（`.tt` 键）引用的 loc 键均须存在（镜像两侧）。复杂效果包 hidden_effect；调试事件 orphan=yes+is_human=yes；ai_chance 用 base/add。

### 3 作用域与具名 scope
根作用域匹配；every_* 内继承；limit/trigger 内不得有效果；具名 scope 先 save_scope_as=xxx 再 scope:xxx 引用。
- **坑（实测）**：save_scope_as 的具名 scope **不跨嵌套 effect 调用**（报 "Scoped object is not valid"）——effect 内复用须开头防御性重存（`scope:actor={save_scope_as=xxx}`）。
- **坑（实测）**：random_subject 的 limit 用 has_variable 定位附庸不可靠（vanilla 44 处用例无一配此组合）——用宗主保存的国家引用变量（`set_variable={value=<国家 scope>}`）。
- **坑（实测）**：every_owned_location 的 every_pop limit 勿再写 owner=root（报 "Event target link 'owner' returned an invalid object"）——归属已由外层保证。
- **坑（实测）**：mercenary scope 不能直接用 unit_location（报 "Wrong scope: mercenary, expected unit"）——经 ordered_mercenary_sub_unit / every_mercenary_sub_unit 取子单位再取位置（vanilla mercs.4 同款）。

### 4 变量体系
用前必初始化（缺失刷 "Failed to fetch variable ... due to not being set"）。**初始化与使用侧同 scope**（跨 scope 是两份变量）。change_variable 只认 add/subtract/multiply/divide/modulo，写 value=N 报 "Invalid operation type in execution"。tooltip：`[Root.GetVariable('x').GetValue|V0]`。

### 5 词条与 ID 存在性（零幻觉）
引用类 ID 目录与核对规则见 `<KB>\tools\audit-ids.md`。原则：查原版 `game\in_game\common\<类目>\` 与 mod 自身，存在才通过，查不到标 `[存疑]`；效果/触发器词条明显笔误→错误，疑似→`[存疑]`+候选，不断言；`$参数$` 未传刷 missing 报错。

### 6 本地化（与 EU4 差异最大）
事件键 `<namespace>.<id>.title/.desc/.a/.a.tt`。**名称即 loc 键**（漏一个显示 raw key）——各类前缀规则（STATIC_MODIFIER_NAME_/AUTO_MODIFIER_NAME_/cb_/relation/war_name/块名/ARTIST_TYPE_/country_description_category_/PERFORM_ 消息键等）见 `<KB>\tools\loc-keys.md`。**不需要 loc**（勿误报）：modifier 类型名（引擎处理）、rebel demand 名。占位符配对 `[Root.GetName]`/`$KEY$`。跨 mod 引用原版 loc 键→标"外部依赖"。**新建法律/政策必须查撞名**——显示名与键名**都**要查（对照本体 `main_menu\localization\simp_chinese\`），只查显示名不查键名会覆盖本体本地化（见 <KB>\guides\law-design.md 第九节）。

### 7 文件合并体系
- INJECT：**顺序敏感块（country_name_construction、levies 等"第一个匹配生效"的块）不能用**（追加排 fallback 后轮不到）——整体覆盖文件、新条目插 fallback 前（levies 特化单位置顶）。
- REPLACE_OR_CREATE：**必须逐字段保留原版内容**（如 soldiers 的 pop_type 保留 has_cap/estate 关联/literacy_impact），只改需改字段。
- on_action 同名块整块定义是**合并语义**（追加 on_actions 列表，不覆盖 trigger/effect）。
- 事件不能 REPLACE/INJECT——改原版事件须新文件+原版 namespace+复制修改（无害 "Duplicated event ID"）。

### 8 逻辑与合理性（建议）
不可达分支；ai_chance 值域合理。**AI 生态门槛**：检查 is_human=yes 门槛与 ai_will_do 权重（-100 起步=AI 永不触发）；观察者模式验证（160 年 0 触发教训）。**灾难防重复（实测）**：can_start 的 NOT{xxx_resolved=yes} 标记**绝不能在 on_end remove**——否则灾难无限重复、永久奖励反复领取（实测单国 3 个永久修正）。深层 every_*（>2 层）性能；无 option 事件确认是否有意。

### 9 边界
不硬编词表；以原版目录 + mod 定义 + 知识库（`<KB>\`）为准。他 mod 定义→标"外部依赖"。.gui 只查结构。error.log 归属先看 "Script location:" 行。

## 报告格式
```
## 审查报告：<mod 目录>（EU5）
共审查 N 个文件（.txt: x, .yml: y, 其他: z）
### 错误（影响加载/运行）/ ### 警告（可能异常）/ ### 建议（逻辑/性能/风格）/ ### 存疑（无法证实）
- `文件路径:行号` [类别] 问题描述 → 建议
### 汇总：错误 N · 警告 N · 建议 N · 存疑 N
```

## 收尾
按"错误→警告→建议→存疑"给修复优先级（按文件分组）；询问是否直接修复；大文件只引用 `文件:行号`；改完提醒 BOM 修复（编辑工具会剥 BOM）。
