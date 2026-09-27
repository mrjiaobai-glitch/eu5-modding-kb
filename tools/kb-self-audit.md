# 本库数字体检（kb-self-audit）

> **为什么需要它**：本库到处引用"N 档 / N KB / N 行 / 第 X 行 / N 个定义"。版本一更新、或当初数错一次，这些数字就会**安静地骗人**。本档给**可复跑的核对清单**，并记录各轮体检结果。**每一轮改动后请追加一行"轮次"记录。**

## 一、复跑方法（三条铁律）

1. **顶层定义计数一律用花括号深度扫描**，不要用缩进锚点（`pitfalls.md` §十三：缩进锚点会把必填字段判成可选、把总数算错）。
2. **取值必须按白名单过滤**：`^\s*type\s*=` 会捞到事件体内的 `estate_type` / `casus_belli`；`^\s*category\s*=` 会捞到叛军的 `estate` / `pretender`。
3. **总数对账**：分项相加必须等于总数（例：事件分目录相加 = 7,470；类型分布相加 = 7,470）。**对不上就是方法错了，不是数据错了。**

## 二、第二轮（2026-09）修掉的漂移

| # | 位置 | 旧值 | 实测 | 根因 |
|---|---|---|---|---|
| 1 | `common\resolutions\` | 18 个决议 | **21 个定义** / 24 档 | `enact_policy` / `policy_vote` / `repeal_law` 用「名字一行、`{` 另起一行」写法，同行正则漏数 |
| 2 | `common\subject_types\` | 17 种附属国 | **20 个定义** / 17 数据档 | 把**数据档数**当成了类型数（`samanta.txt` 含 3 个、`hre.txt` 含 2 个、`pronoia` 在 DLC 档） |
| 3 | `common\peace_treaties\` | 53 条款 | **64 个条款定义** / 53 数据档 | "条款"与"文件"混用 |
| 4 | `gui\sort_keys\` | 39 个键 | **37 个键** | `in_range_*` 手数成 11 个，实为 **9** 个 |
| 5 | `common\on_action\` 顶层 | 217 条 | **216 条** | 逐档相加即 216（各档表本身无误） |
| 6 | `main_menu\setup\start\` | 16 文件 | **25 档**（编号 02–27，缺 01/17） | 旧档只解析到 `16_wars.txt` 就当成全量 |
| 7 | `common\casus_belli\` | （仅"67 文件"） | 补：**102 个 CB 定义** / 67 数据档 | 精度补足 |
| 8 | `map_data\ports.csv` / `adjacencies.csv` | 4421 行 / 184 行 | 4421 / 184 **数据行**（文件 4,422 / 185 行 = 含表头） | 口径未说明 |

## 二·bis、第三轮（2026-09 下半月）——本轮新增/修正

| # | 位置 | 旧值 | 实测 | 根因 |
|---|---|---|---|---|
| 9 | `modifier_type_definitions`（AI 篇与 README 都引用） | "注册表 2393 个修正" | **目录合计 2,436**（主档 `00_modifier_types.txt` 2,393 + `01_byz` 29 + `02_generic_bureaucracies` 14） | 只算了主档、未说明口径（**`ai = yes` 48 与 category 分布均只统计主档**，结论不变） |

本轮同时把两个"被引用很多次却无档"的类目补齐，并把纹章与旗帜整层首次成档（**新档 5 个：fields ×4 + vanilla ×1**）：

| 新档 | 覆盖 | 关键实测数字 |
|---|---|---|
| `fields\main_menu-modifier_type_definitions.md` | 修正键注册表 | 2,436 键；`percent` 1,317 / `color` 817 / `boolean` 722 / `decimals` 126 / `already_percent` 30 / `no_difference_sign` 3 + **必填 `game_data{category, ai}`**；category 10 值（country 1,870 / location 345 / IO 94 / unit 70 / character 43…）；loc `MODIFIER_TYPE_NAME_` 2,507 + `_DESC_` 2,477 |
| `fields\main_menu-game_concepts.md` | 概念词条 | 696 条；`texture` 691 / `alias` 416 / `hidden` 1；loc `game_concept_*` 3,058 |
| `fields\main_menu-flag_definitions.md` | 旗帜规则 | 259 列表 / 1,133 定义；`coa`·`priority` 各 100%、`trigger` 875、`subject_canton` 105、`allow_overlord_canton` 108；`includes`·`allow_revolutionary_indicator`·`revolutionary_canton` 0 |
| `fields\main_menu-coat_of_arms.md` | 纹章本体 + 随机池 + 图集 | 9 档 / 4,566 个 COA 键；`instance` 17,297 / `color1` 13,375 / `colored_emblem` 8,135；贴图 4,138 档 ≈519 MB |
| `vanilla\vanilla-heraldry-and-flags.md` | 五层模型 + 可改点 | 图集 4 档（192×128×672 / 144×96×1,176 / 72×48×1,176 / 盾 360×240×1） |

> **本轮口径教训**：**"目录合计"与"主档"必须写明**——`modifier_type_definitions` 的 2,393 与 `resolutions` 的 18 都属此类（前者只算主档、后者漏了异形写法）。凡引用"某目录有 N 个"，一律先确认**统计范围**（主档 / 全目录 / 含 DLC）。

**上一轮（2026-09 上半月）已修**：`.txt.txt` 加载问题（用补丁记录证伪"死文件"）、on_action 22 文件 → 21 数据档 + 1 info、`_hardcoded.txt` 137 → 134 KB、events 348 → 349 档、`outcome` 26 档样本 → 全量 7,470。

## 三、第二轮复核通过（抽样，全部 ✔）

| 类目 | 声称 | 类目 | 声称 |
|---|---|---|---|
| religions | 293 | cultures | 2087 |
| traits | 147 | government_reforms | 328 |
| bureaucracies | 25 | cabinet_actions | 73 |
| country_interactions | 138 | scripted_relations | 34 |
| international_organizations | 36 | disasters / situations | 36 / 22 |
| diseases / movements | 7 / 4 | religious_schools | 43 |
| holy_sites / gods | 241 / 127 | heir_selections / regencies | 44 / 15 |
| artist_types / artist_work | 13 / 21 | pop_types / estates / government_types | 8 / 8 / 5 |
| area_preferences | 83 | societal_values | 17 |
| advances | 3178 | town_rights / town_setups | 50 / 117 |
| goods | 74 | alert_descriptions | 133 |
| messagetypes | 1348 | scriptable_hints | 93 |
| static_modifiers | 20 | ai_personalities | 8 |
| ai_diplochance | 16 表 / **91 去重键**（使用 168 处） | country_ranks | 4 |
| location_templates | 28573 行 | default.map | 1629 行 |
| `in_game\gui` | 408 档 / 387 `.gui` | events | 349 档 / 7,470 事件 |
| missions | 11 链 / 108 节点 | `_hardcoded.txt` | 132 钩子 |
| defines `NAI` | 746 常量（515 起） | country_interactions readme | 238 行 |

## 四、两条口径说明（下次别误判）

1. **行号区间口径**：本库写的 `NXxx = { }` 区间是 `起始行 – 结束行`，**结束行 = 该段 `}` 所在行**（个别处多算一行空行）。精确定位一律 `grep -n '^NXxx = {'`，**别信区间**。
2. **GUI 语法计数口径**：`vanilla\vanilla-gui.md` 的计数是 **`in_game\gui` 全部 `.gui`**、模式为**不锚定**的 `key\s*=`（锚定 `^\s*key\s*=` 会少算：`onclick` 得 396 而非 495；三区合计则完全不同：`visible` 9,060 而非 8,362）。

## 五、第二轮新发现（已写进对应档）

- **`00_defines.txt` 里 `NInternationalOrganization` 出现两次**：`2552–2554`（`MONTHS_TO_DISBAND_FROM_BIAS`）与 `2604–2609`（议会四常量）——引用该段必须带行号（`guides\defines.md`）。
- 顶层 defines 段落共 **30 个**；文件总行数 **2,609**。
- 全库仅 **2 个 `.txt.txt`**（missions 1 + `gfx\map\map_objects` 1），内容照常加载（`vanilla\vanilla-dlc-and-assets.md`）。
- 事件 `image` 路径实测规则：170 条唯一路径 → 167 落 `main_menu\gfx`、3 落 `dlc\D008\main_menu\gfx`（**区根相对路径，跨基础包 + DLC**）。
- **第四轮新增**：`loading_screen\setup\templates\`（**205 档开局模板层**，`include` 引用 5,256 次 / 192 去重）与 **音频层／字体层**首次成档——`vanilla\vanilla-audio-and-fonts.md`（音频：10 个语义化 `.bnk` + 1,225 个 `.wem`（名=Wwise ID）+ `Init.txt` 映射表 + 82 曲 + 8 个文化配乐原型；字体：289 档 ttf/otf + **6 个 `.font` 定义跨三区** + `fontfiles`/`font` 两层语法）；`guides\new-country-tutorial.md`（含"2,340 国真实字段分布"：readme 模板里的 `male_regnal_names` 原版 **0 国**在用）。
- `illustration_tags` 是**加权块** `{ 10 = happy }`（原版权重恒 10），不是标签集合——`fields\common-events.md` 原写法已修正。

- **第五轮新增（本轮）**：**开局数据层全部解析** → 独立篇 `vanilla\vanilla-setup-data.md`（`setup\start` **25/25 档** + `setup\templates` 205 档 + `setup\countries` 46 档）；**地图对象生成层**成档（`content_source\map_objects`，写进 `vanilla\vanilla-dlc-and-assets.md` §六：`@density_factor_*` 三件套 + 掩码 + 加权 meshes）；**通知系统**成档（`main_menu\notifications\game.txt` 7 条 + `jomini_message.gui`，写进 `vanilla\vanilla-gui.md` §6.5，并确认 SGUI 的 `notification_key` 原版**零使用**）。
- **README 的空章节占位已兑现并撤除**（开局数据层从"待写"变成正式篇）。

## 六、下一轮待办

- ~~`flag_definitions` / `coat_of_arms` 无档~~ → **第三轮已完成**（`vanilla\vanilla-heraldry-and-flags.md` + 两个字段档）。
- ~~`setup\start` 的 10 档未解析~~ → **第五轮已完成**（全 25 档 + 模板层）。
- ~~`content_source\map_objects`~~ → **第五轮已完成**（`vanilla-dlc-and-assets.md` §六）。
- ~~`sound` / `fonts`"建议不做"~~ → **第四轮已做**（`vanilla\vanilla-audio-and-fonts.md`）。
- ~~**仍未成档的小类目**~~ → **第六轮已全部成档**（7 篇新字段档，fields 94 → **101**）：`main_menu-modifier_icons`（2,427 条）· `main_menu-named_colors`（**4,103 色**，与 `setup\countries` 的 1,004 处 `color = map_*` 交叉验证）· `main_menu-scenarios`（10 个）· `common-biases`（1,140 条）· `common-goods_demand`（355 条 + 13 分组）· `common-persistent_dna`（105 条）· `common-small-registries`（**六合一**：insults 73 / scripted_country_names 12 / building_categories 15 / location_ranks 4 / historical_scores 21 / hegemons 5）。唯一未单独成档的是 `main_menu\scripted_lists`（已并入 `fields\main_menu-game_rules.md`）。
- ~~**仍未涉及的层**：`loading_screen\dependency_graphs`（ECS 依赖图 2 档）、`main_menu\gui_animations`（1 档测试资产）~~ → **第七轮已登记**（写进 `guides\game-layout.md`：`game_ecs_systems.dep` 的 **29 个引擎表现层系统节点** + `.editordata` 编辑器元数据；`test_asset.json` 的动画 `keyframes` 格式）。
- **第八轮（穷尽审计）补齐三类此前完全没写的**：①**根部散文件 21 个**——`loading_screen\` 的 17 个（`compound_settings.txt` 设置项定义 / `settings_layout.txt` / `gpu_score_db.json` / `pdx_default_settings.json` / **`vfs_skip_files.config`（VFS 跳过清单）** / `caesar_·clausewitz_branch·rev.txt` 构建标记 / `credits.txt` …）+ `in_game\gui_validation_settings.json`（**GUI 校验白名单 `skip_types`**）与 `terrain_decals.json` + `main_menu\create_mod_tags.json`（**创意工坊标签表**）与 `checksum_manifest.txt`；②**8 个前端 GUI 子目录**（`mods_gui` **101 KB**、`pdx_account` 6 档、`object_explorer`、`applicationutils` ×2、`preload\fonts.gui`、`guitest`、`pdx_mod_dlc_manager` 上传报错文案）；③**三个资产类型** `.animsm` **155 档**（模型动画状态机，`main_menu\gfx\animation_state_machines` 149）、`.particle2` **130 档**（粒子）、`.config` 1 档。
- 若游戏版本更新：优先复跑 §三 的"目录级计数"与 `guides\defines.md` 的段落行号。

## 七、穷尽审计方法（可复跑——第八轮就是靠它抓出遗漏）

**覆盖度自查跑这三步**：

1. **目录级**：遍历全树目录（排除 `\gfx\ \fonts\ \sound\banks\ \models\ \terrain2\ .vscode` 这些纯资产大目录），对每个**含文件**的目录，检查 kb 全文是否提及它的名字。
2. **扩展名级**：统计所有非 `.dds/.txt/.gui` 的资产扩展名（`.animsm` / `.particle2` / `.config` / `.editordata` / `.asset` / `.mesh` …），逐个查 kb 是否提及。
3. **根部散文件级**：逐区列出**区根目录下的散文件**（不在任何子目录里）——**最容易漏的一类**，第八轮的 21 个就是这么冒出来的。

**第八轮结果**：目录级 **0 个未提及** ✔ · 扩展名级 **0 个未提及** ✔ · 根部散文件**已全部登记** ✔ · 有 readme/info 的类目**无一处无档** ✔。
