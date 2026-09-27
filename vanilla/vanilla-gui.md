# 原版解析：界面层 GUI（vanilla GUI）

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| `in_game\gui\` | **408 文件 / 9.4 MB / 387 个 `.gui`** | **只有 3 份 readme（合计 3.2 KB）** |
| `main_menu\gui\` | 89 文件 / 1.28 MB（含 `messagetypes.txt` 177 KB） | `scripted_widgets\_scripted_widgets.info` 554 B |
| `loading_screen\gui\` | 13 文件 / 98 KB（字体模板、文本格式、tooltip、sounds、defaults） | — |
| `in_game\gui\filters\` | **18 文件 / 27 KB** | **`readme.txt` 2,212 B ← GUI 层唯一有实质内容的文档** |
| `in_game\gui\attribute_columns\` + `common\attribute_columns\` | 42 + 18 文件 | `common\attribute_columns\readme.txt` 2,142 B |
| `in_game\gui\sort_keys\` | 1 文件 / 1.6 KB | — |
| `in_game\common\scripted_guis\` | **2 文件**（`economy_satisfaction_target.txt` **17.8 KB**） | `scripted_guis.info` 1,004 B |
| `in_game\common\customizable_localization\` | **26 文件 / 3.8 MB** | `customizable_localization.info` 604 B |
| `main_menu\gui\messagetypes.txt` | **177 KB / 1,348 条消息类型** | — |
| `gui\shared\` | 51 文件 / 1.6 MB（**过半是 tooltip 库**） | — |

> ⚠️ **官方文档总量：5 份、约 4.8 KB**（filters readme 2,212 + attribute_columns readme 2,142 + scripted_guis.info 1,004 + customizable_localization.info 604 + scripted_widgets.info 554 + 两个面板 readme 463/540）。**10.7 MB 的界面层只配了 4.8 KB 说明**——GUI 是 EU5 文档最薄的一层，学它只能靠逆向 `.gui` 本体。

## 术语对照（中文译名与内部名）

| 内部名 | 含义 |
|---|---|
| `types <集合名>` | **类型集合**：一组可实例化的控件类型（原版 **194 个**） |
| `type 新 = 基` | **类型继承**：新类型从基类型派生，用 `blockoverride` 改差异（原版 **859 个**） |
| `block "名"` / `blockoverride "名"` | **覆盖契约**：前者定义可替换块（**2,455** 个），后者替换它（**22,506** 次） |
| `using = <名>` | **控件复用**：引用 `ui_library.gui` 中的库控件（**18,331** 次） |
| `datamodel` | 列表数据绑定（**1,215** 次） |
| `GetVariableSystem` | **GUI 变量系统**（界面态存储：`Set` / `HasValue` / `Exists`） |
| `GetScriptedGui('名').Execute(GuiScope…)` | **脚本化 GUI（SGUI）** 的界面侧调用 |
| `attribute_column` | 列表**列**定义（`gui\` 管外观、`common\` 管数据与排序） |
| `filter` | 列表**筛选器**（`scope`/`trigger`/`range`/`tag`/`group`…） |
| `sort_key` | 排序键（`icon` + `tooltip`） |
| `message type` | **消息类型**（日志/弹窗/地图提示/暂停弹窗的组合） |
| `scripted widget` | **声明式挂载的控件**（在 `.txt` 里把 `.gui` 文件映射成一个 widget 名） |
| `customizable localization` | **动态文本**：`[<scope>.Custom('<key>')]` |

## 一、总览：GUI 是"四层拼起来"的

```
① 语法层   .gui 文件本身（types / type 继承 / block+blockoverride / using / 裸控件实例）
② 数据层   "[...]" 表达式 + Get* 数据函数 + GetVariableSystem（界面态）
③ 脚本层   common\scripted_guis（玩家能点、AI 也能点的按钮）
④ 配置层   四个数据驱动目录：attribute_columns（列）/ filters（筛选）/ sort_keys（排序）/ messagetypes（消息）
           ＋ 支撑：trigger/effect_localization（tooltip 文本）、game_concepts（概念图标）、
              customizable_localization（动态文本）、alert_descriptions / scriptable_hints
```

**关键认知**：改 EU5 的界面，**大多数时候不该去改 `.gui`**——列表的列、筛选器、排序键、消息类型、tooltip 文本都是**数据驱动**的，改数据目录比改 588 KB 的控件库安全得多。

## 二、规模与组织

### 2.1 三个加载域

| 域 | 文件数 | 体量 | 内容 |
|---|---|---|---|
| `in_game\gui\` | 408（387 `.gui`） | **9.4 MB** | 游戏内全部界面 |
| `main_menu\gui\` | 89 | 1.28 MB | 主菜单/多人/设置/调试 + **`messagetypes.txt`** + `scripted_widgets\` |
| `loading_screen\gui\` | 13 | 98 KB | `fonttemplates` / `textformatting` / `tooltip.gui` / `shared\sounds.gui` / `shared\defaults.gui` |

> 附带更正：kb 的 `guides\game-layout.md` 原写"`gui\` ~180 个 `.gui`、mod 一般不改"。实测 **387 个 `.gui`**，且 `main_menu\gui` / `loading_screen\gui` 两层此前未被登记。

### 2.2 `in_game\gui\` 的内部结构（6 个子目录）

| 目录 | 文件 | 体量 | 内容 |
|---|---|---|---|
| `panels\` | 123 | **1.39 MB** | 按主题分 9 组：`organization` **603 KB** / `situation` 298 KB / `disaster` 187 KB / `religion` 125 KB / `trade` 79 KB / `market` 56 KB / `goods` 28 KB / `left_panel` 26 KB / `right_panel` 16 KB |
| `shared\` | 51 | **1.6 MB** | **过半是 tooltip 库**：`location_tooltips` 255 KB、`combat_tooltips` 155 KB、`cards` 120 KB、`market_tooltips` 104 KB、`buttons` 74 KB、`tab_tooltips` 72 KB、`government_tooltips` 72 KB…另有 `windows` / `standard_types` / `unit_cards` / `cabinet_cards` |
| `attribute_columns\` | 42 | 171 KB | 列表列的外观定义（配 `common\attribute_columns\`） |
| `filters\` | 18 | 27 KB | 筛选器（**唯一有 readme 的目录**） |
| `select_interaction_cards\` | 3 | 23 KB | 选择交互卡片 |
| `sort_keys\` | 1 | 2 KB | 排序键 |

### 2.2b 前端/启动界面 GUI（8 个子目录，第八轮补齐）

游戏内界面之外的"壳层"界面（**都不是 gameplay UI，但 mod 会碰到其中的 mod 管理界面**）：

| 目录 | 文件 | 内容 |
|---|---|---|
| **`main_menu\gui\mods_gui\`** | 2 档 / **106 KB** | **mod 管理界面**（`mods_gui.gui` **101,518 B** + `mods_gui_templates.gui` 4,651 B）——playset 排序、依赖提示都在这里 |
| `main_menu\gui\pdx_account\` | 6 档 / 21 KB | Paradox 账号层：登录窗、建号窗、社交资料、法律文档 + `pdx_custom_types.gui` / `social_custom_types.gui` |
| `main_menu\gui\object_explorer\` | 1 档 | `modifier_debug_inspector.gui`（**调试用修正检查器**） |
| `main_menu\gui\applicationutils\` | 1 档 | `screenshot.gui` |
| `loading_screen\gui\applicationutils\` | 1 档 | `tools_gui_dialogs.gui` |
| `loading_screen\gui\preload\` | 1 档 | **`fonts.gui`（字体预加载）**——字体相关的加载顺序看它 |
| `loading_screen\gui\guitest\` | 1 档 | `shortcuts.shortcuts`（按键别名） |
| `loading_screen\localization\jomini\pdx_mod_dlc_manager\` | 1 档 / 865 B | **mod 上传/playset 的界面文案**（`cw_ugc_dlc_l_english.yml`：Workshop 不可用、mod 名长度 3–60、路径非法、playset 自动排序…）——**发布 mod 时的报错文案就是这些** |

### 2.3 根目录：41 个 `*lateralview.gui` + 巨型单文件

- **41 个 `*lateralview.gui`** 是 EU5 的侧栏主视图模式：`production_lateralview` 179 KB、`government_lateralview` 153 KB、`foreign_country_lateralview` 145 KB（外交篇里提到过）、`diplomacy_lateralview` 127 KB…
- 单文件最大的：**`ui_library.gui` 588 KB（公共控件库）**、`location_window.gui` 338 KB、`outliner_entries.gui` 224 KB、`multiplayer_lobby.gui` 214 KB、`alertmanager.gui` 192 KB、`single_unit_window.gui` 140 KB、`map_markers.gui` 121 KB、`cooltip.gui` 111 KB

### 2.4 27 B 存根契约（一个值得注意的原版模式）

`panels\organization\` 下大量文件只有一行，**文件名与内容不对应**：

```
gui/panels/organization/foreign_league_hre.gui   →   organization_war_theme = {}
gui/panels/organization/jurchen_confederation.gui →   organization_geodiplomatic_theme = {}
```

即：**按对象名占位、内容指向一个主题块**。写 mod 加 IO/局势时要照这个"一对象一文件"的约定补齐，否则面板没有主题可挂。

## 三、`.gui` 语法模型（实测计数）

```
types <集合名> {                          # 194 个类型集合
    type <新类型> = <基类型> {             # 859 个类型声明
        blockoverride "块名" { … }         # 22,506 次 ← 界面定制的主力手段
    }
    block "块名" { … }                    # 2,455 个可覆盖块定义
}

<控件名> = { using = <库控件> … }          # 实例化；using 共 18,331 次
```

### 3.0 校验白名单：`in_game\gui_validation_settings.json`（第八轮补齐）

GUI 报错的"豁免清单"在**区根目录**（不在 `gui\` 里）：`in_game\gui_validation_settings.json`（4,853 B / 181 行）

```json
{ "skip_types": [ "area_exploration", "area_integration", "area_name", "area_population", … ] }
```

**用途**：列出**不参与校验的 datamodel 类型**——即"这些类型取不到数据也不会报错"。**排查"我的控件为何没有报错提示/为何静默取不到值"时先看它**；mod 若想沿用同样的豁免，需在自己包里提供同名文件（合并语义同其它 JSON 配置）。

### 3.1 控件库与复用

`ui_library.gui` 里的 **`types UiLibrary`**（第 **18,341** 行起）定义了全部基础控件；实例化时用 `using = <名>` 引用。原版最常被复用的：

| 库控件 | 次数 | 用途 |
|---|---|---|
| `layoutpolicy_expanding` | **2,101** | 布局策略：占满剩余空间 |
| `tooltip_title_icon_size` / `tooltip_concept_title_icon_size` | 1,411 / 109 | tooltip 标题图标尺寸 |
| `piechart_angles` / `bg_circle_piechart` / `bg_circle` | 386 / 283 / 151 | 饼图 |
| `color_black_texture` / `color_new_gold_texture` / `color_white_texture` | 368 / 315 / 141 | 纯色底 |
| `bg_cabinet_card_frame` / `bg_paper_card` / `bg_card_header_01` | 299 / 245 / 120 | 卡片 |
| `overlay_window_texture` / `overlay_stone_texture` / `overlay_cloth_texture` | 142 / 130 / 111 | 窗口叠层 |

### 3.2 真实片段（`ui_library.gui` 的标签页按钮）

```
button_main_tab_alt = {
    using = layoutpolicy_expanding
    blockoverride "tab_text" { raw_text = "Windows" }
    onclick = "[GetVariableSystem.Set( 'ui_library_tabs', 'windows' )]"
    down    = "[GetVariableSystem.HasValue( 'ui_library_tabs', 'windows' )]"
}
```

一个 5 行块里出现了全部四种要素：**库控件复用**（`using`）、**块覆盖**（`blockoverride`）、**GUI 变量**（`GetVariableSystem`）、**表达式绑定**（`"[…]"`）。

### 3.3 交互与状态字段计数

| 构造 | 次数 | 说明 |
|---|---|---|
| `visible` | **8,362** | 可见性（同样要写表达式） |
| `enabled` | 1,148 | 可用性 |
| `onclick` | 495 | 点击行为（表达式，通常是效果函数或 SGUI 调用） |
| `datamodel` | 1,215 | 列表的数据来源（`datamodel = "[…]"`） |

### 3.4 `GetVariableSystem`：GUI 自己的变量存储

界面态（当前标签页、筛选状态、临时选择）不占用游戏变量系统，而用 GUI 专属的 `GetVariableSystem`：`Set( '键', '值' )` / `HasValue( '键', '值' )` / `Exists( '键' )`。**它只在界面层存在，脚本侧读不到**——需要脚本也知道的界面态才走 `set_variable`。

### 3.5 界面动画（timeline animation，**原版 56 个 `.gui` 在用**）

界面里的"滑入/淡出/轮播"不是硬编码，而是**库里的一套动画模板 + 一个触发函数**：

```txt
# 用法：using 一个动画模板，再用 blockoverride 定义两个状态的属性
using = Animation_Show_Hide_UI
blockoverride "on_hide_ui" { position_y = -125   alpha = 0 }
blockoverride "on_show_ui" { position_y =  125   alpha = 1 }
```

```txt
# 触发：组名由函数拉起（原版实例：点开 CB 提示时重置轮播）
on_action = "[PdxGuiTriggerAllAnimations('reset_carousel')]"
```

**实测关键字频次**（`in_game\gui` + `main_menu\gui`）：

| 关键字 | 次数 | 说明 |
|---|---|---|
| `Animation_Curve_Default` | 53 | 缓动曲线常量 |
| `top_text_animation` / `rows_top_animation_left` / `rows_bg_animation_left` | 40 / 34 / 23 | 原版动画模板族（`ui_library.gui` 侧） |
| **`PdxGuiTriggerAllAnimations`** | 31 | 触发函数（参数 = 动画组名） |
| `background_animation` | 23 | 背景动画槽位（`blockoverride "background_animation"`） |
| `Animation_Show_Hide_UI` / `Animation_Show_Hide_Properties` | 19 / 13 | 显隐类动画模板 |
| `Animation_Curve_EaseOut` | 13 | 缓动曲线 |
| `outliner_show/hide_animation_template` | 各 11 | 大纲栏显隐 |

**动画资产的格式样例**在 `main_menu\gui_animations\test_asset.json`（**1,974 B，全库唯一一个，且没有任何文件引用它——是测试资产**）：

```json
{ "animationSets": [ { "name": "MyTimelineAnimation1", "duration": 20.0,
    "paths": [ { "path": "topWidget.childWidget",          // ← 控件路径（点号表示父子）
        "keyframes": { "size":     [ {"time":0.0,"value":{"x":1,"y":1}}, {"time":1.0,"value":{"x":20,"y":20}} ],
                       "position": [ {"time":0.0,"value":{"x":20,"y":20}}, {"time":0.3,"value":{"x":100,"y":-20}} ] } } ] } ] }
```

**审查要点**：`using = Animation_*` 的模板必须在 `ui_library.gui` 的 `types UiLibrary` 里存在（拼错静默失效，见 §8.1）；`blockoverride` 里的槽名（`on_hide_ui` / `on_show_ui` / `background_animation`）是**模板契约**，写错就是"动画不跑"；`PdxGuiTriggerAllAnimations` 的参数是**动画组名**，不是控件名。

## 四、数据函数层（GUI 能拿到什么）

实测出现频率最高的取数函数（前 20）：

| 函数 | 次数 | 函数 | 次数 |
|---|---|---|---|
| `GetConceptTexture` | **509** | `GetEstateIcon` | 62 |
| `GetDataModelSize` | 374 | `GetStat` | 61 |
| `GetVariable` | 205 | `GetCountry` | 60 |
| `GetMapMode` | 175 | `GetSubUnitCategoryIcon` | 57 |
| `GetGoodsIcon` | 141 | `GetTotalMerchantCapacity` | 55 |
| `GetFixedPointCurrencyValue` | 118 | `GetMarketEntry` | 54 |
| `GetTarget` | 106 | `GetModifierValueNoFormat` | 47 |
| `GetUniqueInternationalOrganization` | 99 | `GetPlayStyleItem` | 46 |
| `GetEstateFromKey` | 97 | `GetBuildingIcon` | 43 |
| `GetReligionIcon` | 82 | `GetPolicyForLaw` | 40 |

读法：**图标类函数占绝对多数**（concept/goods/religion/estate/building/law icon）——EU5 界面是"数据 + 图标 + tooltip"三件套；`GetDataModelSize` 与 `GetVariable` 说明列表与界面态是第二大用途。

## 五、Scripted GUI：玩家能点、AI 也能点

### 5.1 定义（`common\scripted_guis\`，权威 = `scripted_guis.info` 1,004 B）

```
<scripted_gui_key> = {
    scope = <scope 类型>              # 该 SGUI 对哪种作用域可用
    is_shown = { trigger }            # 是否显示
    is_valid = { trigger }            # 是否可用
    effect = { effect }               # 激活时发生什么
    saved_scopes = { <字符串…> }       # 需要事件目标的 scope 列表
    notification_key = <键>            # 激活时的通知（默认 jomini_scripted_gui_confirm）
    confirm_title = {} / confirm_text = {}   # 确认窗标题与正文
    ai_is_valid = { trigger }         # AI 是否可用（默认 false）
    ai_chance = {}                    # AI 激活概率（1–100 的脚本值）
    ai_frequency = {}                 # AI 评估频率（月）
}
```

**`ai_is_valid` / `ai_chance` / `ai_frequency` 是重点**：SGUI 不只是玩家的按钮，**AI 会按频率自己评估并点它**——所以一个 SGUI 等于"给 AI 也开了一条行动路径"。

### 5.2 界面侧调用

```
onclick  = "[GetScriptedGui('taxing_setup').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).End)]"
on_action = "[GetScriptedGui('subtract_one_percent').Execute(
                 GuiScope.SetRoot(TaxRateSetting.GetEstate.GetCountry.MakeScope).End)]"
```

`GetScriptedGui('<名>').Execute(GuiScope.SetRoot(<scope>.MakeScope).End)` —— **`.End` 不能漏**，`SetRoot` 的参数必须是 `MakeScope` 过的对象。

### 5.3 原版用量（少而深）

界面侧只出现 **9 次**，全部集中在 `economy_lateralview.gui`（税率与经济满意度面板）；定义侧 `economy_satisfaction_target.txt` **17.8 KB**，定义了 `taxing_setup`（把各阶层税收目标初始化为 0.5）、`subtract_one_percent`、`subtract_five_percent`、`subtract_ten_percent`、`set_to_zero_percent`、`add_one_percent` 等一组按钮。

> 结论：**SGUI 是"原版几乎没用、但能力完整"的机制**——它是把复杂脚本操作暴露成界面按钮（并让 AI 也能用）的正规途径。

## 六、四个数据驱动目录

### 6.1 `attribute_columns`（列）——`gui\` 管外观、`common\` 管数据

**`common\attribute_columns\readme.txt`（2,142 B）是权威**，它的定位写得很清楚：这些列用于 **generic_action / 国家交互 / 角色交互的 `select_trigger`**（即"选目标"列表）。

```
<column_tag> = {
    widget = <gui 里的控件类型>     # 会拿到 InteractionTarget 以取数据（InteractionTarget.GetCountry 等）
    width = <默认列宽>
    fixed_height = <固定高度>       # 固定高度可让大量条目渲染更快
    is_constant_width = yes/no     # no 时该列吸收剩余宽度（默认 yes）
    contains_select_target_button = yes/no   # 该控件已含选择按钮，避免重复生成
    single_widget_for_row = yes/no           # 整行用一个控件（多写列会报错）
    sort = {                       # 0..n 个排序头
        sort_key = <唯一键>
        sort_text = <脚本文本>      # 文本列用；可用 root 与 scope:actor/recipient/target
        sort_value = <脚本值>       # 数值列用
        sort_by_tooltip_key = <tooltip 文本>
    }
}
```

实例（`common\attribute_columns\20_province.txt`）：`create_colonial_charter_column` 用 `widget = province_create_colonial_charter`、`width = 150`、`fixed_height = 32`、`is_constant_width = no`，排序用 `sort_key = "colony"` + `sort_text` 里 `SORT_TEXT_PROVINCE_NAME` / `MODIFIER_NONE` 双分支 + `sort_by_tooltip_key = "PROVINCE_SORT_BY_NAME"`。

### 6.2 `filters`（筛选器）——GUI 层唯一有实质文档的目录

`gui\filters\readme.txt`（2,212 B）字段全表：

| 字段 | 作用 |
|---|---|
| `scope` | 可被该筛选器操作的对象类型 |
| `trigger` | 该对象是否通过筛选 |
| `range = { min / max / step / format }` | 让筛选器提供**数值区间**选择 |
| **`tag`** | 管道分隔的标签列表（`"a\|b\|c"`）；视图的 `.gui` 用 `WithFilterTags('x\|y\|z')` 让标签**有交集**的筛选器出现。**省略 tag = 对所有视图可见**（readme 原文：只用于"确实到处都该有"的筛选器，**大多数筛选器至少该声明一个 tag**） |
| `group` / `exclusive_group` | 同组显示 / 单选框互斥（**组内每个筛选器都必须写**） |
| `invert` / `enabled_at_start` | 默认排除、勾选才包含 / 初始是否勾选 |
| `hidden_in_searchbar` | 仍生效、仍在侧栏与自动补全里，但**搜索栏不显示 chip**——面板已提供专用按钮时避免同一功能出现两次 |
| `group_sorting` | 组内排序（目前仅字母序） |

配套本地化键（readme 明确要求）：`search_filter_<tag>_name`（名称）、`search_filter_<tag>_desc`（描述）、`search_filter_<tag>_format`（区间数字的显示格式）。

实例（`filters\06_country.txt`）：

```
country_only_within_diplomatic_range = {
    scope = country
    tag = diplomacy
    enabled_at_start = yes
    group = 1
    trigger = { exists = scope:target  scope:target = { within_diplomatic_range = root } }
}
```

### 6.3 `sort_keys`（排序键）

`gui\sort_keys\00_sort_keys.txt`（1.6 KB）：`<键> = { icon = <?>  tooltip = <?> }`，如 `province_location_wealth = { icon = wealth }`、`present_pops = { icon = "market"  tooltip = MARKET_ITEM_SORT_BY_PRESENT_POPS }`。

### 6.4 `messagetypes.txt`（消息/通知系统，`main_menu\gui\`）

**1,348 个消息类型**，每个都必须写全 7 个字段（实测 7 字段各出现 1348 次）：

| 字段 | 含义 |
|---|---|
| `log` | 是否进消息日志 |
| `onmap` | 是否在地图上提示 |
| `popup` | 是否弹窗 |
| `idle` | 空闲/汇总提示 |
| `option` | 是否带选项 |
| `pausepopup` | 是否暂停并弹窗 |
| `message_category` | 分类（进设置里的消息过滤面板） |
| `sound`（可选，53 条） | 提示音 |

分类分布：**diplomacy 599 / society 304 / government 188 / geopolitics 98 / wars 72 / military 50 / economy 37**（合计 1,348）。

### 6.5 `main_menu\notifications\game.txt`（通知/对话框，1 档 / 2 KB）

**与 `messagetypes` 是两码事**：`messagetypes`（1,348 条）管"**消息**怎么提示（日志/地图/弹窗/暂停）"，`notifications` 管"**这个对话框长什么样、按哪个窗口弹**"。原版 **7 条**，全部是对话框/系统提示：

```txt
@default_window_file = "gui/notifications/jomini_message.gui"     # 文件头两个宏 = 默认窗口
@default_window_name = "jomini_message"

mission_unlock_missions = {          # ← 通知键（引擎按名调用）
    level = dialog                   # dialog / alert
    category = tutorial              # 分类（social / system / multiplayer / mods / tutorial）
    window_file = @default_window_file
    window_name = @default_window_name
    title_text  = MISSION_UNLOCK_MISSIONS          # 4 个 loc 键
    text        = MISSION_UNLOCK_MISSIONS_WARNING
    accept_text = OK
    decline_text = CANCEL            # 可选：纯提示类只有 accept_text
}
```

- 原版 7 条：`mission_unlock_missions` / `mod_tool_confirm_delete` / `toggle_observe_blocking_country` / `failed_loading_savegame_errors` / `failed_saving_savegame_errors` / `confirm_continue_playset_issues` / `dlc_ingame_purchase_notification`。
- 默认窗口在 `main_menu\gui\notifications\jomini_message.gui`（3,820 B）：`basic_dialog`，`name = "jomini_message"`、`modal = yes`、`layer = confirmation`，标题走数据模型 `[JominiNotification.GetTitle]`；同档还定义第二个窗口 `playset_warning_message`（被 `confirm_continue_playset_issues` 用 `window_name` 指定）。
- **mod 侧的入口是 Scripted GUI**：`common\scripted_guis\` 的 **`notification_key`**（`scripted_guis.info:7`，默认 `jomini_scripted_gui_confirm`）——**该字段原版零使用**（所有原版 SGUI 都用默认确认窗）。要让自己的按钮弹自定义对话框，就得写这个字段 + 在 `notifications\` 里定义同名键。
- 其余通知由**引擎按名调用**（存档失败、DLC 购买、多人阻塞等），脚本侧没有同形引用（GUI 里出现的是**同名的 loc 键**，不是通知键）。

## 七、相关支撑层

| 层 | 位置 | 说明 |
|---|---|---|
| **tooltip 库** | `gui\shared\*_tooltips.gui`（51 文件里过半） | `location_tooltips` 255 KB 最大；tooltip 的**文本**来自 `common\trigger_localization\`（44 文件 261 KB）与 `effect_localization\`（36 文件 233 KB）——改 tooltip 文案多是改这两个目录，而非 `.gui` |
| 概念图标 | `main_menu\common\game_concepts\00_game_concepts.txt`（61 KB） | 撑起 `GetConceptTexture` 的 509 次调用；新增概念要在此注册 |
| **动态文本** | `common\customizable_localization\`（26 文件 **3.8 MB**） | `text = { trigger  localization_key  fallback }` + `random_valid` + `parent`/`suffix` 继承；调用写法 `[<scope>.Custom('<key>')]`（info 原文） |
| **declarative widget** | `main_menu\gui\scripted_widgets\` | 在 `.txt` 里写 `gui/<文件>.gui = <widget 名>` 即可挂载，**不必塞进已有 .gui**；⚠️ 见 §八 的坑 |
| 警报 | `common\alert_descriptions\`（20 KB）+ `alertmanager.gui`（192 KB） | 警报定义（title/texture/priority） |
| 提示 | `common\scriptable_hints\`（20 KB） | 教学/提示条 |
| 地图模式 | `gui\map_markers.gui`（121 KB）+ `attribute_columns` | 地图标记与上色（配 `defines` 的 `NMapColors`） |

## 八、Mod 改造建议（可改 vs 硬编码）＋ 三个坑

### 8.1 四条硬规则

1. **路径镜像**：mod 里放同名 `gui\…` 文件（三域各自镜像：`in_game\gui`、`main_menu\gui`、`loading_screen\gui`）。
2. **覆盖契约是 `block` 名**：想改原版控件，覆盖它声明的 `block "名"`，而不是复制整块改行号。
3. **类型继承优于复制**：`types <集合> { type 我的 = 原版基类 { blockoverride … } }`。
4. **表达式只写在 `"[…]"` 里**；`visible` / `enabled` / `onclick` / `datamodel` 都是表达式位。

### 8.2 三个坑（都有官方原文）

1. ⚠️ **scripted widget 里 `visible = yes` 不生效**。`_scripted_widgets.info` 原文：「The scripted widget inside the .gui file is required to have a 'visible' attribute. **'visible = yes' does not work**, the boolean must be returned by a function.」官方 workaround：`visible = "[EqualTo_CFixedPoint('(CFixedPoint)0', '(CFixedPoint)0')]"`。
2. ⚠️ **筛选器省略 `tag` = 全视图可见**（readme 警告"most filters should declare at least one tag"）；`exclusive_group` 必须写在组内**每一个**筛选器上，漏一个就退化成普通勾选。
3. ⚠️ **SGUI 的 `.End`**：`Execute(GuiScope.SetRoot(<scope>.MakeScope).End)` 漏写 `.End` 会静默失效。

### 8.3 该改哪一层（决策表）

| 想改的东西 | 改哪里 |
|---|---|
| 列表里显示哪些列 / 怎么排序 | `common\attribute_columns\`（数据）+ `gui\attribute_columns\`（外观） |
| 列表的筛选器 | `gui\filters\` + `search_filter_*` 三个 loc 键 |
| 排序下拉的图标与提示 | `gui\sort_keys\` |
| 消息是弹窗还是只进日志 | `main_menu\gui\messagetypes.txt` |
| tooltip 文案 | `common\trigger_localization\` / `effect_localization\`（**不是** `.gui`） |
| 一段文本随国家/宗教/语言变化 | `common\customizable_localization\` + `[<scope>.Custom('key')]` |
| 给玩家做一个"按钮执行复杂脚本"，并让 AI 也会用 | `common\scripted_guis\` + 界面里 `GetScriptedGui(...).Execute(...)` |
| 新增一个面板/视图 | `gui\panels\<主题>\<对象名>.gui`（照原版存根契约）+ 必要时在 `types` 集合里加类型 |
| 换贴图/图标 | `gfx\interface\...`（GUI 里以路径引用；`GetConceptTexture` 类走概念注册表） |

**硬编码边界**：控件渲染与布局引擎、`.gui` 文件的加载与合并时机、`"[…]"`
 表达式的求值、GUI 变量系统的生命周期、脚本化 GUI 的通知/确认窗流程、消息系统的调度（哪条消息在何时弹）。

## 九、中文检索键

**界面层没有独立的概念词条**——界面文案散在各主题的 loc 文件里（`main_menu\localization\<lang>\<主题>_l_<lang>.yml`，english 114 个文件）。定位方法：按 GUI **控件名/类型名**反查（如 `button_main_tab_alt`、`organization_war_theme`），或按约定前缀查（`search_filter_*`、`SORT_TEXT_*`、`*_SORT_BY_*`）。

**关键文件速查**（改界面时按此顺序找）：

| 目的 | 文件 |
|---|---|
| 基础控件与库控件 | `gui\ui_library.gui`（588 KB，`types UiLibrary`） |
| 侧栏主视图 | `gui\*_lateralview.gui`（**41 个**） |
| 面板 | `gui\panels\<organization|situation|disaster|religion|trade|market|goods|left_panel|right_panel>\` |
| tooltip | `gui\shared\*_tooltips.gui` |
| 警报/提示 | `gui\alertmanager.gui`、`common\alert_descriptions\`、`common\scriptable_hints\` |
| 消息类型 | `main_menu\gui\messagetypes.txt` |
| 字体与文本格式 | `loading_screen\gui\fonttemplates.gui`、`textformatting.gui` |

**关联字段档（界面层已全档覆盖，共 12 档）**：`fields\gui-core.md`（`.gui` 格式总纲）、`gui-attribute_columns.md`、`gui-filters.md`、`gui-panels.md`、`gui-sort_keys.md`、`gui-messagetypes.md`、`gui-scripted_widgets.md`、`common-scripted_guis.md`、`common-customizable_localization.md`、`common-alert_descriptions.md`、`common-scriptable_hints.md`、`common-trigger-effect-localization.md`；另有 `fields\common-attribute_columns.md`（列的数据/排序侧）与 `fields\gfx-map_modes.md`（地图模式）。
