# 游戏本体结构地图（game-layout）

> **一句话**：游戏本体四个加载域的结构地图：逐目录列出内容、实查档数与可改点，含各区根部散文件与 DLC 包结构。
> **什么时候看**：找某个文件在哪、或想知道某一层能不能改时，先在这张结构图上定位。
> **体量**：119 行 · 约 6 分钟通读

根：`<game>\`（本机示例 `F:\SteamLibrary\steamapps\common\Europa Universalis V\game\`）。四个顶层目录对应四种加载域：**in_game**（游戏内）、**loading_screen**（启动/加载）、**main_menu**（主菜单/全局静态数据）、**dlc**（DLC 包，本身就是 mod 结构模板）。

## in_game\（游戏内主域）

| 目录 | 内容 | 制作要点 |
|---|---|---|
| `common\` | 全部游戏机制数据，~160 个类目（见 `systems-map.md`） | mod 主要战场 |
| `events\` | 事件文件：顶层 26 个文件 + 15 个子目录（character / colonization / culture / debug / DHE / diplomacy / disaster / economy / estates / exploration / government / missionevents / religion / situations / ai_area_conqest_events）；全库 **349 档 / 10.5 MB / 7,470 个事件 / 361 个 namespace**，`DHE\` 占 55.5% | 新事件放 `events\<前缀>_events.txt`；`readme.txt` 是权威格式；体系实测见 `vanilla\vanilla-events-and-missions.md` |
| `content_source\map_objects\` | **地图对象生成器（14 档 / 20 MB）**：`generators\vegetation_generators.txt`（`layer`/`max_density`/`mask`/加权 `meshes`；low/medium/high 三件套用 `@density_factor_*` 宏）+ `ambience_generators\`（`entity` + 条件字段 + `avoid_sea`）+ `masks\*.png` **12 张 / 20 MB**（掩码名与脚本同键）。详见 `vanilla\vanilla-dlc-and-assets.md` §六 | 地图美术层 |
| `fonts\` | 字体定义 | — |
| `gfx\` | 美术资源：animation_state_machines / city_materials / compound_nodes / graphical_culture_types / images / map / models / scenes / terrain2 / music_player_gui | mod 美术放同名子目录 |
| `gui\` | **387 个 `.gui` 文件**（408 文件 / 9.4 MB）+ 子目录 `shared\`（51，**过半是 tooltip 库**）/ `panels\`（123，9 个主题组）/ `attribute_columns\`（42）/ `filters\`（18）/ `select_interaction_cards\`（3）/ `sort_keys\`（1）；另有 **41 个 `*_lateralview.gui`** 侧栏主视图 | UI 层**文档最薄**（5 份 ~4.8 KB），机制详见 `vanilla\vanilla-gui.md` |
| `gui\`（另两个域） | **`main_menu\gui\`** 89 文件 / 1.28 MB（含 **`messagetypes.txt` 177 KB / 1,348 条消息类型**、`scripted_widgets\`、`notifications\jomini_message.gui` 默认通知窗口）；**`loading_screen\gui\`** 13 文件（字体模板 / 文本格式 / tooltip / sounds / defaults） | 主菜单与加载界面；mod 按同名路径镜像 |
| `localization\` | **只有** `jomini\script_system\trigger_system_l_<lang>.yml`（引擎触发词本地化，11 语言） | mod 语言文件**不**放这里（见 `localization.md`） |
| `map_data\` | default.map / definitions.txt / adjacencies.csv / ports.csv / nodes.dat / location_templates.txt / locations.png / rivers.png | 地图底层，mod 极少动 |
| `setup\countries\` | 国家定义：按地区分文件（anatolia.txt、british_isles.txt …）+ `00_readme.info`（TAG 格式权威） | 新国家/改国家在这 |

## loading_screen\（加载域）

| 路径 | 内容 |
|---|---|
| `common\defines\00_defines.txt` | **全部引擎常量**（2609 行）：NGame/NCombat/NPop/NUnit/NWeather/NCharacter/NLocation 等 ~30 个 N 块（索引见 `defines.md`） |
| `common\defines\graphic\00_graphics.txt` | 图形常量 |
| `common\defines\jomini\` | 引擎级：00_tooltips / adjacencies / fog_of_war / icons / mapeditor / rivers / roads |
| `localization\<lang>\` | 语言文件（languages.yml、load_tips_l_<lang>.yml） |
| `localization\jomini\` | savegame_gui_l_<lang>.yml、cw_ugc_dlc_l_<lang>.yml |
| `sound\` | **音频层**（1,267 档 / 2,292 MB）：`audio_settings.txt`（音效引擎 + 6 音频档案 + VCA 总线）、`banks\windows\`（**10 个语义化 `.bnk`** + `Media\*.wem` **1,225 个**（按 Wwise ID 命名）+ `Init.txt` 映射表）、`map\ambience\`（11 个环境音文本档 + sensor 掩码 + 官方 `sensorgen.py`）、`persistent_objects\`。**可改：替换曲目 / 调总线 / 加档案 / 按文化配乐**；硬边界：新增曲目要回 Wwise 重打包 |
| `fonts\` | **字体层**（289 档 / 541 MB）：**250 `.ttf` + 24 `.otf`**，13 个字族目录（NotoSansSC/JP/KR、NotoSerifSC/JP/KR、NotoSansMono、CormorantGaramond、MapNamesFonts…），每族带 `OFL.txt`；**6 个 `.font` 定义档**（`loading_screen_fonts.font` 31 KB / `cw_fonts.font` 15 KB / 两个 headers / `main_menu\fonts` / `in_game\fonts`）——`fontfiles` 声明"哪些语言用哪些字体文件"，`font` 声明"样式名 → 字体组" |
| `gui\` | 字体模板 `fonttemplates.gui`（1.5 KB，多数是注释示例）、文本样式 `textformatting.gui`（704 行）、tooltip、`shared\sounds.gui` |
| `dependency_graphs\` | **引擎表现层系统依赖图**（2 档 / 6 KB）：`game_ecs_systems.dep`（342 行）是**节点图**——`graph={nodes={<系统名>={id node inputs={link={pin_id="earlier" linked_node=M linked_pin="later"}}}}}`，**29 个节点全是引擎内部表现层系统名**（`city_audio_sync_point` / `city_graphics_start·end` / `unit_graphics_sync_point` / `weather_sync_point` / `siege_effects_sync_point` / `trade_graphics_sync_point` / **`prepare_unit_coa`** / `reset_unit_coa_pointers` / `populate_seen_by_any_camera` / `enqueue_particle_systems_with_zoom` / `visible_locations_gfx_manager` / `whale_rotator` …）；`pin_id="earlier"` ↔ `linked_pin="later"` 即依赖方向。`.editordata` 是节点编辑器的视图元数据（缩放与各节点坐标）。**mod 完全不可改**——价值只在于"知道引擎在哪些 sync point 上同步表现层" |

## main_menu\（主菜单域，含全局静态数据）

除 `common\` / `gui\` / `fonts\` / `localization\` / `setup\` / `music\` 外，还有两个小目录：

| 路径 | 内容 |
|---|---|
| `notifications\game.txt` | **通知/对话框定义**（2 KB / 原版 7 条）：`level = dialog\|alert` + `category` + `window_file`/`window_name` + 4 个 loc 键；文件头两个 `@default_window_*` 宏。详见 `vanilla\vanilla-gui.md` §6.5 |
| `gui_animations\test_asset.json` | **界面动画资产样例**（1,974 B，**全库唯一且无任何引用——测试资产**）：`animationSets[].{name,duration,paths[].{path:"父.子",keyframes:{size/position:[{time,value:{x,y}}]}}}`。**真实动画功能在 `.gui` 里**（56 个文件在用 `using = Animation_*` + `PdxGuiTriggerAllAnimations`），见 `vanilla\vanilla-gui.md` §3.5 |

`common\` 13 个类目（mod 常用前 5 个）：

| 类目 | 用途 |
|---|---|
| `static_modifiers\` | **静态修正定义**：country.txt（12937 行）、character.txt、difficulty.txt、capital_in_topography.txt 等；格式 = `键 = { game_data = { category = <country/location/...> } <modifier> = 值 }` |
| `modifier_type_definitions\00_modifier_types.txt` | **修正键注册表**（17616 行）：新增修正键必须在此声明 category/percent/boolean/format/min/max/cap_zero_to_one/scale_with_pop/ai/bias_type/should_show_in_modifiers_tab（文件头注释即权威） |
| `game_rules\00_game_rules.txt` + `_game_rules.info` | 游戏规则：default 设置、apply_modifier（player/ai/all 分类）、**defines 覆盖**（在规则里改 define）、flag（blocks_achievements 等） |
| `game_concepts\00_game_concepts.txt` | 概念词条（百科链接文本） |
| `achievements\` | 成就：standard_achievements.txt + 各 DLC；须注册进 achievement_groups.txt |
| `coat_of_arms\` | 纹章（coa_templates / 预置国家纹章） |
| `flag_definitions\00_flag_definitions.txt` | 旗帜定义 |
| `modifier_icons\` | 修正图标映射 |
| `named_colors\` | 颜色命名 |
| `scenarios\00_scenarios.txt` | 剧本 |
| `scripted_lists\` / `scripted_triggers\` / `script_values\` | 主菜单域脚本（加载较早，可被游戏内引用） |

`localization\<lang>\`：**语言文件主目录**（english 114 个 yml，按主题命名 `<主题>_l_english.yml`，如 actions_l_english.yml、advances_l_english.yml、buildings_l_english.yml）。

`setup\`（230 档 / 10.6 MB）——**开局数据层**，新手最容易漏掉的一层：

| 子目录/文件 | 规模 | 作用 |
|---|---|---|
| `setup\start\` | **25 档**（02–27，缺 01/17） | 逐地点/逐国的开局数据：`06_pops.txt` 5 MB 的 `define_pop`、`10_countries.txt` 1.6 MB 的领土与开局状态、`05_characters.txt` 2.4 MB 的角色库、`04_dynasties.txt` 王朝… |
| **`setup\templates\`** | **205 档 / 0.2 MB** | ★ **开局模板层**：`10_countries.txt` 里用 `include = "<模板名>"`（**5,256 次 / 192 个去重目标**）拼出开局。模板可带 `government`（168 档，含继承法/议会/**13 条社会价值观初值**）、`starting_technology_level`（123）、`discovered_regions/areas/provinces`（探索知识）、`court_language`、`country_rank`；**模板可嵌套**（57 档用 include）。命名 = `expl_<地区>`（探索）或 `<宗教/文化区>_<政体>[_no_coast/_not_present/_no_censor…]`。用法见 `guides\new-country-tutorial.md` §2b |

## 根部散文件（不在任何子目录里的配置——第八轮补齐）

**各区根目录下还有 21 个散文件**，分三类（都**不在 `common\` 里**，所以最容易漏）：

**① `loading_screen\`（17 个：设置层 + 构建标记 + 杂项）**

| 文件 | 大小 | 作用 |
|---|---|---|
| `compound_settings.txt` | 9,940 B / 303 行 | **设置项定义**：`setting = { name = "quality"  gui_name = "SETTING_QUALITY"  category = "Graphics"  enum_entry = { … } }` |
| `settings_layout.txt` | 5,702 B | 设置界面布局 |
| `gpu_score_db.json` | 21,115 B | **GPU 评分库**（→ 画质档推荐） |
| `recommended_settings_data.json` / `pdx_default_settings.json` / `settings_relations.json` | 3,592 / 2,204 / 1,551 B | 推荐画质 / 默认设置 / 设置间联动 |
| `paths.settings` + `paths_settings.json` | 2,504 + 50 B | 路径设置 |
| **`vfs_skip_files.config`** | **97 B / 4 行** | **告诉 VFS 跳过哪些文件**（原版列了 `gfx/FX/cw/particle2.fxh`、`gfx/FX/cw/particle2.shader`、`gui/multiplayer_lobby.gui`、`/settings_layout.txt`） |
| `caesar_branch.txt`（`develop`）/ `caesar_rev.txt` / `clausewitz_branch.txt` / `clausewitz_rev.txt` | 8–15 B | **引擎代号与构建分支/版本标记**（caesar / clausewitz 两个引擎名就来自这里） |
| `credits.txt` / `license-fi.txt` / `manifest.json` / `game_flow_graphs.anchor` | 37 KB / 17 KB / 19 B / 22 B | 制作名单 / 芬兰语许可 / 清单 / 流程图锚点 |

**② `in_game\` 与 `main_menu\` 根（各 2 个）**

| 文件 | 大小 | 作用 |
|---|---|---|
| **`in_game\gui_validation_settings.json`** | 4,853 B / 181 行 | **GUI 校验跳过清单**（`skip_types = [ "area_exploration", "area_integration", "area_name", … ]`）——GUI 报错的白名单，做界面时"为什么这个 datamodel 不报错"就看它 |
| `in_game\terrain_decals.json` | 2,805 B | **地形贴花**（`decal_name: "heightmap_1_16"` + `decals[]`）——与 `gfx\map\map_objects\decal_*.txt` 呼应 |
| `main_menu\checksum_manifest.txt` | 345 B / 27 行 | 校验清单（只列目录名 + `sub_directories`） |
| **`main_menu\create_mod_tags.json`** | 2,286 B / 104 行 | **Steam 创意工坊 mod 分类标签**（`{"tags":[{"LocKey":"ADVANCEMENTS_TAG","SteamRef":"Advancements"}]}`）——**发布 mod 时选标签用** |

**③ 三个从未在别处出现的资产类型**

| 类型 | 档数 | 位置 |
|---|---|---|
| **`.animsm`**（模型动画状态机） | **155** | `main_menu\gfx\animation_state_machines\` **149** + `in_game\gfx\animation_state_machines\` 4 + DLC 1 + loading_screen 1（每档配一个 `.editordata`） |
| **`.particle2`**（粒子） | **130** | `main_menu\gfx\particles\`（environment 43 / arms 21 / legacy 13 / particle_effects 12 …） |
| `.config` | 1 | 就是上面的 `vfs_skip_files.config` |

## dlc\（DLC = mod 结构模板）

- `D000_shared\`：main_menu 共享（common\dlc、gfx\interface、localization\dlc\<lang>）——"共享包"最小模板
- `D008_fate_of_the_phoenix\`：完整模板：in_game / loading_screen / main_menu 三分区（765 / 23 / 350 档，共 **1,141 档 / 539 MB**）+ 根文件 `D008_fate_of_the_phoenix.dlc.json`（dlc 元数据，形似 metadata.json）+ thumbnail.dds + checksum_manifest.txt
- **DLC 只新增、不覆盖基础包**（实测 1,141 档与基础包 **0 路径碰撞**）；官方只在**基础包文件**里用 `has_dlc = "<dlc 名>"` 开门（**44 个基础包文件**在用）。结构与解析规则详见 `vanilla\vanilla-dlc-and-assets.md`
- `D015_ancient_monuments_pack\`、`D017_sacred_sites_pack\`：内容包

## 关键数量事实（实查）

- `in_game\common\` 共 **73 个 readme.txt**（72 个类目 + production_methods\__readme.txt）+ `setup\countries\00_readme.info`
- `main_menu\localization\english\` **114 个 yml**（每语言同数；russian 131、german/spanish 118 略多）
- `main_menu\common\static_modifiers\country.txt` 12937 行、`00_modifier_types.txt` 17616 行——改原版修正前先 grep 这些文件
- 引擎触发词条在 `in_game\localization\jomini\script_system\trigger_system_l_<lang>.yml`（11 语言，各 1 文件）
