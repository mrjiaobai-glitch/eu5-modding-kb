# 本库数字体检（kb-self-audit）

> **一句话**：本库数字体检：可复跑的核对方法三条铁律、各轮修掉的数字漂移表、复核通过的类目计数与穷尽审计方法。
> **什么时候看**：怀疑本库某个引用数字有问题时，按本文的方法回原版复跑核对，别照着旧数字用。
> **体量**：276 行 · 约 13 分钟通读

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

## 八、人类可读性外壳（2026-09 新增；内容未变，只加入口与路标）

本库原本是"agent 优先"的高密度参考（大量表格与速记式表达）。为了让人类读者也能用，加了一层外壳——**正文内容一行未改**：

| 外壳 | 位置 | 作用 |
| --- | --- | --- |
| 仓库首页 | `README.md` | 定位 + 常见任务速查（原先那 6 张索引表已移出） |
| 全量索引 | `INDEX.md` | 原先 README 的 6 张表**原样搬来**（agent 与检索用） |
| 新手路线 | `START-HERE.md` | 零基础 7 步路线 + 按任务／按角色导览 + 新手最易翻车的 5 件事 |
| 术语总表 | `glossary.md` | 中英对照 + 常见误译；**每条标出处**，只汇总库内既有内容 |
| 每篇摘要块 | 全部文档 H1 之后 | `> **一句话** / **什么时候看** / **体量**` 三行，严格取自该篇自身内容 |

**维护口径（下次别漏）**：

1. **新增文档必须同时**：① 在 `INDEX.md` 对应表加一行；② 在 H1 之后加 3 行摘要块，其中「体量」一行先留占位符（由脚本填入行数与阅读时长）。
2. 跑编写副本里的 `kb\scripts\kb-refresh-shell.ps1`：填体量数字、为超过 300 行的文档生成／刷新「## 目录」。加 `-Verify` 会用 **`github-slugger`**（GitHub 锚点算法的权威实现）离线逐个核对目录锚点。⚠ **别用 GitHub 的 markdown API 验锚点**——`POST /markdown` 与 `contents?html+json` **都不返回标题 id**（见 §九 第 4 条）。**该脚本不随库发布**——克隆者只需读文档。
3. **换行符统一 LF**、**编码 UTF-8 无 BOM**（全库 148 篇一致；本地化数据文件的 BOM 要求是游戏侧规则，与本条无关）。
4. 发布走 `kb\scripts\publish-eu5-repo.ps1`（增量上传 + 断链／目录计数／规模声明校验）；新增根文件要同步该脚本的"根文件齐全"清单。

## 九、第四轮（2026-09）——知识错误订正

本轮订正的全是**"文档断言与实测相反"**这一类（不是笔误，是会直接误导创作的错）：

| # | 原写法 | 实测证据 | 订正后 | 涉及文档 |
| --- | --- | --- | --- | --- |
| 1 | 本地化"**两处镜像的键必须完全一致**" | 本体 `in_game\localization\` 只有 **11 个** yml（全是 `jomini\script_system\trigger_system_l_<lang>.yml` 引擎词条），`main_menu\localization\` 有 **5,835 个**；原版事件键 `ages_of_eu.1.title` 落在 `main_menu\localization\english\advances_l_english.yml`。创意工坊 **36 个**带 yml 的 mod：**22 个只放 main_menu / 7 个只放 in_game / 7 个两者都用**，且"两者都用"的 7 个里**没有一对同路径同名文件** | 两处**都会被加载、但不互为镜像**；同键以加载顺序后者为准（**2026-10-30 更正**：同键实为**先处理者胜、后到者丢弃**；但**本地化例外——后加载覆盖生效**，见本轮 §十四） | `localization.md`、`mod-skeleton.md`、`pitfalls.md`、`START-HERE.md`、`merging.md` |
| 2 | yml 键"**顶格（不缩进）**在语言头下" | 本体 `main_menu\localization\simp_chinese\` 全部 yml：键行 **152,165 行全部缩进**、**顶格 0 例**（顶格会被解析成新顶层块 → 该文件键全部失效） | 键**必须缩进一个空格**；语言头放**第 1 行**（本体 5,829/5,835；创意工坊有 4 个 mod、约 40 个文件在头前放注释，引擎是否容忍**未验证**） | `localization.md`、`new-mission-pack-tutorial.md` |
| 3 | 9 个术语译名与本体概念键不符（`merchant_power`="商人力量" 等） | 本体 `game_concepts_l_simp_chinese.yml` 逐键核对 | 见 `glossary.md`「**已订正**」节（9 词 / 87 处） | 贸易篇 / 法律阶层篇 / 战斗篇 / 外交篇等 |
| 4 | §八 第 2 条称"`-Verify` 用 GitHub markdown API 核对锚点" | 实测 `POST /markdown` 与 `contents?html+json` **都不返回标题 id**（id 由网页端生成） | 改用 **`github-slugger`** 离线核对：353/353 命中 | 本节 §八 |

**新增工具（编写副本 `kb\scripts\`，不随库发布）**：

- `kb-field-census.ps1` —— 类目**字段普查**：扫 `in_game\common\<类目>\` 全量文件，产出「readme 声明 vs 原版实际使用」（含**原版在用但 readme 未声明**的字段清单）、枚举取值白名单、块内键 top 榜。解析按**净花括号数**记账（单行平衡块不能触发深度递减），并把深度 1 里"值为块"的键分流为**子条目**（如 `laws` 的政策名），避免虚增字段数。
- `mod-lint.ps1` —— mod **静态自检**：骨架 metadata、花括号配对、本地化（BOM／语言头／顶格／重复键）、事件（namespace／ID 唯一／loc 键存在性），输出按错误/警告/建议/存疑分级。

**教训（写在这里防止再犯）**：本库出的知识错误几乎都源于**"凭印象写规则、没有量本体"**。凡是要写成铁律的断言，必须先给出本体或大规模实测的**计数**；给不出计数的，一律降级为"待核实"。

**发布后核对的口径（2026-09 踩过）**：`raw.githubusercontent.com` 有 CDN 缓存，**刚推完拉 raw 可能仍是旧内容**（实测：推完立即拉 `fields/common-unit_types.md` 得 45 行旧版，而 API/网页已是 142 行新版；加 `?v=` 参数也绕不过）。**核对一律走 API**：`GET /repos/{owner}/{repo}/contents/<path>?ref=main`（返回 base64 正文 + sha）或 `GET /repos/{owner}/{repo}/git/trees/main?recursive=1` 比对 blob sha；两者与本地发布副本的 git-blob 哈希一致才算推成功。

## 十、第五轮（2026-10）——本体进入测试版后的行号漂移修复

**背景**：本体于 **2026-10-01** 整体更新为测试版（当日重排 **4,942 个文件**），知识库基于旧正式版。本轮做的是**行号引用回校**，不是内容重写。

**扫描口径**：全库 154 篇中，带 `文件:行号` 形式引用共 **263 处**（分布在 37 篇）。逐条回本体核对，信号取两种：① **越界**（引用的行号 > 文件当前行数）；② **落在空行/孤立括号**（行号在范围内但原内容已位移）。

**关键教训（差点误判）**：初版脚本按 **leaf 文件名**索引本体，导致同名文件错配——例如 155 行的 `subject_types\readme.txt` 被匹配到 28 行的 `casus_belli\readme.txt`，于是"越界 338 处"这种**全崩的假信号**差点触发批量替换。**改为按文档给出的完整相对路径匹配 + 逐条语义核对后**，真失效面是 **~40 处**（16 越界 + 24 空行错位），**且其中还要扣除同名错配的假阳性**。

**已修（均为高置信、命中数=1 才替换，共 30 处）**：`fields\common-subject_types.md`(13)、`common-missions.md`(3)、`vanilla-hazards-and-environment.md`(3)、`vanilla-ai.md`(3)、`vanilla-dlc-and-assets.md`(2)、`new-mission-pack-tutorial.md`/`common-cabinet_actions.md`/`common-regencies.md`/`common-traits.md`/`pitfalls.md`/`vanilla-diplomacy.md`/`vanilla-events-and-missions.md`/`vanilla-map-and-geography.md`(各 1)。

**`guides\defines.md` 单独重算**：所有 N 块起始行位移，按 grep 全量重测——`NCountry 125 / NCombat 445 / NAI 562 / NCharacter 1661 / NDiplomacy 2208 / NWar 2640 / NColony 2746 / NWeather 2788`；文件总行数 **2609 → 2836**；`NAI` 常量 **746 → 约 839**；去重块数 **30 → 31**（测试版新增 `NPortraitAssetShowcase` L1742，并把 `NInternationalOrganization` 分成 L2776 / L2830 两处）。

**`NColony` 专例（殖民篇 + defines 篇共 6 处）**：块从 `2524–2550` 整段下移到 **`2746–2774`**（行号仍在旧文件长度范围内，属于"范围内但指错块"，光靠越界/空行两种信号扫不出来，靠逐条语义核对才发现）。

**仍存疑、未擅改（需人工带原文复核，宁缺勿错）**：巨型重排文件（`00_modifier_types.txt`、静态修正 `country.txt`、本地化大 yml）里的行号，多数仍在范围内但无法从行号本身证明语义仍对；本轮只改了能靠**字段名精确回查**锁定的。**结论沿用 §一 铁律：定稿前一律以 grep 定位为准，别信行号。**

## 十一、第六轮（2026-10）——jomini 解析器预检工具（本机，不发布）

**动机**：手搓 tokenizer（`kb\scripts\mod-lint.ps1` 的括号/引号扫描）在 EU5 方言上会误报——字符串里的 `{`、`?=`、BOM、`$宏$`、日期。改用**引擎同款解析器**判"能不能解析"。

**先破一个前提**：GitHub 上的 `jomini` 开源库（`rakaly/jomini` Rust ★96、`nickbabcock/jomini` TS ★96）**不是帕社引擎**，是第三方 Clausewitz/Jomini **文本格式解析器**（nickbabcock README 自陈与引擎"重名纯属巧合"）。**引擎本身没开源**；故"整合升阶阈值/移速公式/`dominant_culture` 取法"等规则数值，公开渠道**查不到**，前述"引擎侧未证"标签维持。

**选型**：本机无 `cargo`、`static.crates.io` 不通 → Rust 版装不了；有 `node`、`npm registry` 通 → 用 **JS 版 `jomini@0.10.0`**（同一套格式语义）。

**工具**：`<本机>\tools\jomini-validator\`（**独立于 mod-lint，不发布**）
- `validate.mjs`：**按扩展名分派**——`.txt` 用 `Jomini.parseText`（语法/引号/方言权威）**＋ 自研花括号配平**（因 **jomini 对未闭合括号宽容**，不补就漏报）；`.gui`/`.yml` **跳过**（GUI 是另一套 DSL，jomini 全量误报；yml 交给 mod-lint 的 BOM/键层）。`walk` 防 junction/符号链接成环 + 无权限目录容错。
- `lint-jomini.ps1`：入口。退出码≠0=有未闭合硬伤；stdout JSON（`txtBrace` 错误 / `txtJomini` 需复核）；**空扫描（0 文件）退 2**，杜绝"没扫到还判通过"。

**实测假阳性率**：对 1.4 本体 `in_game\common` **1998 个 `.txt`** → 硬伤 0、jomini 仅 1 处需复核（`script_values\eu4_conversions.txt` 顶层裸 `key=数字`，jomini 判 unexpected EOF，属已知边缘）≈ **0.05%**。

**并入（本机）**：两份活技能 `SKILL.md`（`<本机>\skills\…` + `<用户目录>\.dsh\skills\…`，两份字节一致）审查流加"第 4 步 机械预检"（有 node+工具就跑、缺则跳过退回人工），原第 4 顺延为第 5。**因写死本机私有路径，发布到 GitHub 前须加 `LOCAL-ONLY` 哨兵 + 发布脚本剥离规则**（已于第七轮落地：publish 2.3b 整段剥离）。

**沙箱坑（记防重犯）**：① 本沙箱 **PowerShell 用管道捕获子进程 stdout 会死锁** → 改 `Start-Process -RedirectStandardOutput 文件`。② **绝不可 `Get-Process node | Stop-Process`**——DSH 宿主自身即 `node.exe …dsh\lib\bin.js`，误杀 = 会话中断（本轮踩过，两次"结果未落"即此因）。

**并发遗留**：`kb\eu5-modding\tools\` 被其他会话加了 `save-decoding.md`（tools 6 档 vs 发布期望 5），**发布前需登记 INDEX + 计数 5→6**；本机自用不受影响。

## 十二、第七轮（2026-10）——批量替换污染事故与修复流程

**事故**：用 PowerShell 批量改计数时，替换规则写成 `@{ 文件 = @(@('旧串','新串')) }`——**单元素嵌套数组被展平成字符串**，`foreach($r in $rule)` 里的 `$r` 变成字符串，`$r[0]`/`$r[1]` 退化为**单个字符**，于是"整串替换"变成"单字符全局替换"。

**污染面（3 个文件）**：
| 文件 | 发生的替换 | 表现 |
| --- | --- | --- |
| `eu5-modding\README.md` | `法 → 律` | "法律"变"律律"（4 处） |
| `skills\eu5-mod-review\references\README.md`（两份） | `1 → 2` | **`103 档`→`203 档`**、`12 档`→`22 档`、`publish-eu5-repo.ps1`→`.ps2` |
| `guides\defines.md` | `- → 空格` | 列表 `- `、表格 `|---|`、负号 `-2/-1`、`vanilla-ai.md` 全坏 |

**发现**：`git -C <本机 kb 父目录> diff` 逐行看 diff（KB 有 `kb\.git` 仓库）+ 全库签名扫描（`律律` / `203 档` / `.ps2` / 文件里 `-` 数量为 0）。

**修复**：
1. `git checkout -- 'eu5-modding/README.md' 'eu5-modding/guides/defines.md'` 恢复到干净 HEAD（**只丢被污染的未提交改动**）；
2. 用 **`edit` 工具逐条重做**（不再走脚本），核对 8 处 defines 改动全部落位；
3. skills `references\README.md` 无本地 git → 从**已发布的 GitHub 副本**取回结构，按当前真实计数重建（103/14/27/4/7），两份哈希一致；
4. 复跑 `kb-refresh-shell.ps1` + `-Verify`（体量一致、501 锚点 0 失效）+ 签名扫描（仅 `fields\` 3 处 `22 档` 为**真实普查计数**，属误报）。

**铁律（下次照做）**：
- 批量替换**禁止**把 `@(@(a,b))` 塞进哈希表当规则；一律用 `edit` 工具，或 `@( [pscustomobject]@{Old='…';New='…'} )` 并对 `$r.Old`/`$r.New`。
- 批量替换后**必须**跑 `git diff` 看真实差异 + 签名扫描；只看脚本"✔"输出会漏掉单字符污染。
- 计数改动先 `Get-ChildItem` 数实际档数再写声明，**别手推**。
- 恢复用 `git checkout` 前，先确认该文件的未提交改动**只含污染**（看 diff）。
- 教训：**"脚本报告成功"≠"内容正确"**——本轮两次都打印 ✔，实际已损坏。

## 十三、第八轮：2026-10-29 正式版基线复测（第八轮）

**触发**：游戏升级正式版——`binaries\eu5.exe` **2026-10-29 11:19**、checksum **`de9dbde3…`**；相对 1.4.1（2026-10-13）变更 **5,433 档**，并新增内容包 **`D009_across_the_pillars`**。**游戏内版本 = 1.4.5（Rio Salado）**，界面校验码 `b88a`，build 哈希 `06472e2a`、clausewitz `2026.43.0`、branch `ud009/release/next`、build 时间 2026-10-23 15:39 UTC。

**复测结论（当前事实类文档已同步；历史台账不改写）**：

| 项 | 旧 | 新（2026-10-29） |
|---|---|---|
| `00_defines.txt` 行数 / N 块 | 2,852；NCountry 125–334 · NUnit 363–444 · NCombat 447–562 · NAI 564–1600 · NLocation 1905–1968 · NDiplomacy 2223–2654 · NWar 2656–2717 | **2,861**；**125–335 / 364–445 / 448–563 / 565–1607 / 1914–1977 / 2232–2663 / 2665–2726**（**数值常量全部未变**） |
| 修正键注册表（目录合计） | 2,436（00: 2,393 / byz 29 / generic 14） | **2,713**（00: **2,659** / byz **30** / generic **24**；去重 2,711）。子统计：percent 1,595 / color 872 / boolean 754 / decimals 139 / already_percent 31 / no_difference_sign 6；`ai=yes` 仍 48；category：country 2,049 / location 428 / IO 100 / unit 70 / character 47 / all 7 / movement 4 / religion 3 / province·rebel 各 1 |
| 事件 | 349 档 / 7,470 / 361 ns / 10.52 MB | **373 档 / 7,963 / 387 ns / 11.38 MB**；类型 country 7,905 / exploration 36 / location 20 / omens 1 / age 1；outcome neutral 7,500 / negative 272 / positive 191；DHE **160 档 / 4,128 事件（51.8%）** |
| setup | `main_menu\setup\start\`（25 档） | **`setup\1337\`（26 档，新增 `04_religious_orders.txt`）+ 新增 `setup\1658\`**；国家定义 45 档 / 2,353 块；模板 **205→207**；include **5,256/192 → 5,361/220** |
| 类目档数 | advances 226 / building_types 45 / laws 30 / country_interactions 93 / generic_actions 115 | **227 / 46 / 31 / 94 / 116**；IO **35 文件**（36 块）；resolutions **25 文件**（19 块） |
| DLC | 4 包（D000/D008/D015/D017） | **5 包**：新增 **D009 3,273 档 / 1,200.9 MB**（伊比利亚/马格里布）；D008 1,200 档 / 523.5 MB；DLC 定义档 **4 个**（新 category **`dlc_chronicle_pack`**）；全库 `has_dlc` **775 处**（基础包 **368/66 文件**：d008 275/44 + d009 93/25；DLC 目录内 407） |

\\* 含 **27 个未写 `type` 的事件**（默认国策级）与 `debug\qa_debug.txt` 里 1 个未闭合事件——所以"逐类型相加 = 7,963"成立。

**方法论铁律（本轮新增）**：
- **历史台账不改写**：本档前七轮的旧数字是"当时实测"，保留原样；只改**当前事实类**文档（INDEX / 布局 / 事件 / DLC / defines / 字段档）。
- 事件计数用**逐事件块解析 + 总数对账**；简单行匹配会把嵌套的 `x.y = {` 也算上（本次多 51 个）。
- 修正注册表口径固定为**三档相加**（2,713），另注去重值（2,711）。
- 大范围替换一律 `[pscustomobject]@{F;O;N;C}` + 期望命中数，替换后 `git diff --stat` + 旧值残留扫描。

**遗留（下一轮）**：
1. `fields\` 普查数字仍标 `（EU5 1.3.x）`，未按 1.4.x 重跑 census（约 40 档需重生成）；
2. 资产级子计数（事件 `image` 路径、`illustration_tags` 权重）仍是 1.3.x 期实测；
3. D009 机制未深挖：`battle_for_the_strait` / `golden_century` 局势、`d009_grand_alliance` IO、`convivencia`/`mudejar`/`morisco` 少数群体事件链、11 条美洲远征（Cibola 等）。

## 十四、第九轮：wiki 对照 + exe 静态取证（2026-10-30）

**触发**：用户提供 [EU5 Wiki: Modding](https://eu5.paradoxwikis.com/Modding)，其链出的 [Mod compatibility](https://eu5.paradoxwikis.com/Mod_compatibility)（`Mod_files_load_order` 重定向）与库内 `merging.md` / `localization.md` **至少 5 处冲突**。

**已用 exe 字符串 + 本体 readme 定死（可引用）**：
| 结论 | 证据 |
|---|---|
| 六个前缀存在：INJECT / REPLACE / TRY_INJECT / TRY_REPLACE / INJECT_OR_CREATE / REPLACE_OR_CREATE | `binaries\eu5.exe` 全部命中 |
| **`EDIT:` 不存在**（库里旧表写成 jomini 支持，已删） | exe `EDIT:` **0 命中** |
| INJECT/REPLACE 目标必须存在 | 引擎串 `You are trying to inject/replace to a non-existing entry: {}:{} in the file {}` |
| **同键重复 = 先加载者胜**（后者不创建） | 引擎串 `Duplicated key {} will not be created from file: {}`（`gamedatabase.h`） |
| 文件名序 = **ASCII 序**，子目录晚于父目录 | 本体 `in_game\common\levies\readme.txt:27`「file name order first, then declaration order」 |
| 本地化重复会写日志且报两个文件 | 引擎串 `Duplicate localization key. Key '{}' is defined in both '{}' and '{}'.` |
| wiki 的官方工具属实 | exe 含 `script_docs` / `dump_data_types` / `-debug_mode` / `--mod_stubber` / `-handsoff_save_at_dates` / `automated_test` |

**实跑结果（2026-10-30 11:52 / 11:57 两轮，EU5 **1.4.5**，测试 mod `kb_load_order_test`，播放集 `KB测试`）**：
| 项 | 结果 | 证据 |
|---|---|---|
| 同名文件 | **整文件覆盖**（非块合并） | 同路径同名 `traits/04_explorer.txt` → `error.log` **无**重复键日志；对照：换文件名重复 `navigator` → `Duplicated key navigator will not be created from file: common/traits/zz_kb_dup_test.txt:1` |
| 本地化同键 | **后加载覆盖生效** | 简中 `EXIT`（原「退出」）界面显示为 `ZZZ_MOD_OVERRIDE_WORKS`；`debug.log`：`Key 'EXIT' defined in multiple files: ...core_l_simp_chinese.yml, ...kb_test_l_simp_chinese.yml` |
| 事件同 ID | 重复会被记录，**后到者不创建** | `Duplicated event ID 'kb_test.1' found. New Location: 'events/kb_aaa_test.txt:3', Previous Location: 'events/kb_mmm_test.txt:3' / '...kb_zzz_test.txt:3'`；游戏数据库同型日志明确 `will not be created` |
| 文件处理顺序 | **不是 ASCII 升序** | 两轮中 `kb_aaa_test.txt` 始终是 "New Location"（最后处理）；BOM 警告却按 aaa→zzz（预扫描序≠解析序） |

**仍未知**：事件文件解析序的确切排序规则（既非升序，3 文件轮的 mmm/zzz 相对顺序未定）；`replace/` 目录在 EU5 是否仍有独立语义（wiki 说法，未测，但不影响"普通目录覆盖"结论）。

**为什么这轮没跑成**：本体从未启动过（`Documents\...\Europa Universalis V\` 只有空 `mod\`，无 logs）；直跑 `eu5.exe -debug_mode` 只进 **Checksum Builder** 模式（打印各 DLC 文件 Name/Contents checksum 后 exit 0，未初始化游戏会话）；`--mod_stubber <name>` 恒报 `Invalid mod name`；exe 内没有 `-mod=` 加载开关。**EU5 没有游戏启动器**（与 EU4/CK3/VIC3 不同）：连 Steam 直接进游戏，mod 在**游戏内 Mod 管理器**（playset / `OpenModTools` / `ToggleAllMods`）里启用，启用状态由游戏自身保存；引擎里的 `pdx-launcher-session-token` 只是账号/会话 token，**不是**启动器依赖。实跑需进游戏后在 Mod 管理器启用测试 mod，属 GUI 交互，未做。 **后续（同日晚）：已跑通**——用户 Steam 启动 → 游戏内 Mod 管理器选播放集 `KB测试`（`playsets.json` 预置）→ 启动即加载；日志在 `D:\Documents\Paradox Interactive\Europa Universalis V\logs\`（error.log / debug.log），两轮读日志完成验证，无需进入战役。

> **更正（2026-10-30，用户指出）**：本轮初稿误以为"mod 经外部启动器启用"，并据此写了 5 处 launcher 提法。已全部订正为"**无启动器、游戏内 Mod 管理器**"：`START-HERE.md:12`、`guides\mod-skeleton.md`（一句话 + 目录注释 + 启用说明）、`vanilla\vanilla-dlc-and-assets.md:330`、本档此段。

> **同日再订正（用户指出）**：本机 Documents 在 **D 盘**——mod 目录 D:\Documents\Paradox Interactive\Europa Universalis V\mod\、日志 …\logs\；guides\mod-skeleton.md 旧写的 %USERPROFILE%\Documents\… 已改（guides\testing.md 的日志路径同步改为 D 盘）。**mod 启用状态存在 playsets.json**（每播放集 orderedListMods：path + isEnabled，路径以 / 结尾），已据此为测试 mod 新建播放集 KB测试；此前把测试 mod 放到 C 盘导致游戏内看不到（已删）。


**铁律（本轮新增）**：
- wiki 页自身标注 `last verified for 1.1` + `requires verification`，**不可直接照抄**；一律用**引擎字符串 + 本体 readme** 交叉验证，冲突项标 ⚠️待实跑。
- 待实跑项在文档里**必须标注验证状态**，且给出"验证前的安全做法"（改原版文件先整文件复制；改事件用新 namespace+新 ID；覆盖本地化键同时放 `replace/`）。

**本轮改动**：`guides\merging.md`（六前缀表重写 + 🧪 实跑结论 + **新增 §八 与 Wiki 差异对照表**）、`guides\localization.md`（同键覆盖：wiki 说法 v1.4.5 被实测推翻）、`guides\mod-skeleton.md` / `pitfalls.md` / `guides\defines.md` / `fields\main_menu-named_colors.md` / `fields\main_menu-game_concepts.md`（旧"zzz_ 后加载可覆盖"类说法全部改正并标 **⚠️ 与 wiki 不同**）、`guides\defines.md`+本档（版本 1.4.5 身份）。

> **约定（长期）**：凡与 [EU5 Wiki](https://eu5.paradoxwikis.com/Mod_compatibility) 不一致的结论，正文对应处一律标 **⚠️ 与 wiki 不同**，并在 `guides\merging.md` §八 登记；未实测的 wiki 说法标 **⏳ 待测**，不得当成结论。


