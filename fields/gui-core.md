# gui/*.gui（界面文件格式总纲）

> **一句话**：界面文件格式总纲：五个核心构造、属性词表、表达式位与数据函数，以及 mod 覆盖的四条规则。
> **什么时候看**：写或覆盖任何 .gui 文件、要查属性名与表达式该放哪里时翻这篇。
> **体量**：113 行 · 约 6 分钟通读

来源：**本体没有 GUI 格式 readme**——官方与 GUI 相关的文档只有 4 份（`gui\filters\readme.txt` 2,212 B、`gui\panels\{disaster,situation}\readme.txt` 463/540 B、`main_menu\gui\scripted_widgets\_scripted_widgets.info` 554 B）；本档由 **387 个 `.gui` 文件全量实查**归纳（统计口径见下）。

## 规模（实测）

| 域 | 文件 | 体量 |
|---|---|---|
| `in_game\gui\` | 408（**387 `.gui`**） | **9.4 MB** |
| `main_menu\gui\` | 89 | 1.28 MB |
| `loading_screen\gui\` | 13 | 98 KB |

`in_game\gui\` 子目录：`panels\`（123）/ `shared\`（51，过半是 tooltip 库）/ `attribute_columns\`（42）/ `filters\`（18）/ `select_interaction_cards\`（3）/ `sort_keys\`（1）。
单文件最大的：`ui_library.gui` **588 KB**（公共控件库，`types UiLibrary` 在第 18,341 行）、`location_window.gui` 338 KB、`outliner_entries.gui` 224 KB。

## 五个核心构造（全量计数）

```
types <集合名> {                       # 194 个类型集合
    type <新类型> = <基类型> {          # 859 个类型声明（继承）
        blockoverride "块名" { … }      # 22,506 次 ← 界面定制的主力
    }
    block "块名" { … }                 # 2,455 个可覆盖块定义
}

<控件名> = { using = <库控件> … }       # 实例化；using 共 18,331 次
```

| 构造 | 作用 |
|---|---|
| `types <集合名>` | 一组控件类型的命名空间；一个文件可有多个集合（`types CharacterColumns`、`types DisasterPanel`…） |
| `type 新 = 基` | **类型继承**：新类型派生自基类型（基类型可来自 `ui_library.gui` 或其他文件） |
| `block "名" { … }` | 在控件里声明**可被替换的块**（覆盖契约） |
| `blockoverride "名" { … }` | 替换某个块的内容；**块名是唯一契约** |
| `using = <名>` | 引用 `types UiLibrary` 里定义的库控件（复用的最小单位） |

## 实例化写法（真实片段，`ui_library.gui`）

```
button_main_tab_alt = {
    using = layoutpolicy_expanding
    blockoverride "tab_text" { raw_text = "Windows" }
    onclick = "[GetVariableSystem.Set( 'ui_library_tabs', 'windows' )]"
    down    = "[GetVariableSystem.HasValue( 'ui_library_tabs', 'windows' )]"
}
```

要点：**控件名 = 类型名**（`button_main_tab_alt` 是某个 `type`），块体里可混用 `using`（库控件）、`blockoverride`（改块）、普通属性、以及 `"[…]"` 表达式。`name = "xxx"` 给控件起可被脚本/其他界面引用的名字。

## 属性词表（出现最多的 35 个）

| 属性 | 次数 | 属性 | 次数 |
|---|---|---|---|
| `size` | 10,557 | `spacing` / `texture_density` / `blend_mode` | 2,436 / 2,294 / 2,231 |
| `text` / `text_single` / `raw_text` | 8,341 / 3,598 / 1,989 | `title` / `on_action` / `autoresize` | 2,210 / 2,172 / 2,155 |
| `texture` / `modify_texture` | 8,168 / 2,855 | `margin` / `action_tooltip` / `expand` | 2,110 / 2,034 / 1,937 |
| `layoutpolicy_horizontal` | 4,546 | `vbox` / `color` / `textcontext` | 1,857 / 1,835 / 1,466 |
| `icon` | 4,480 | `background` / `click_type` | 1,455 / 1,367 |
| `name` | 4,218 | `alwaystransparent` / `default_format` | 1,152 / 1,114 |
| `tooltipwidget` | 3,811 | `maximumsize` / `position` / `click_mode` | 1,099 / 1,087 / 1,084 |
| `alpha` / `datacontext` | 3,802 / 3,740 | `hbox` / `widget` | 3,267 / 2,926 |
| `parentanchor` / `align` | 2,837 / 2,834 | | |

读法：**尺寸 + 文本 + 贴图**三件套占绝对多数；`datacontext`（3,740）与 `tooltipwidget`（3,811）说明"数据上下文 + tooltip"是 EU5 界面的骨架。

## 表达式位（`"[…]"`）

| 位置 | 作用 |
|---|---|
| `visible` | 可见性（**不能用裸 `yes/no`**，在 scripted widget 里尤其；见审查要点） |
| `enabled` | 可用性 |
| `onclick` / `on_action` / `on_finish` | 点击/动作/完成回调（调效果函数或 ScriptedGui） |
| `datamodel` | 列表数据源（配合 `GetDataModelSize` 遍历） |
| `datacontext` | 控件的数据上下文（决定 `[…]` 里能取到什么） |
| `tooltip` / `action_tooltip` | tooltip 文本（多是 data function 或 loc 键） |

**GUI 变量系统**：界面态用 `GetVariableSystem`（`Set('键','值')` / `HasValue('键','值')` / `Exists('键')`），它**只在界面层存在**，脚本侧读不到；需要脚本也知道的状态要用 `set_variable`。

## 数据函数（实测 top 20）

`GetConceptTexture` **509** · `GetDataModelSize` 374 · `GetVariable` 205 · `GetMapMode` 175 · `GetGoodsIcon` 141 · `GetFixedPointCurrencyValue` 118 · `GetTarget` 106 · `GetUniqueInternationalOrganization` 99 · `GetEstateFromKey` 97 · `GetReligionIcon` 82 · `GetTargetObjectFromFlag` 74 · `GetSubUnitCategory` 70 · `GetCountryPopulation` 69 · `GetGraphicalCultureTexture` 69 · `GetReligionByKey` 67 · `GetAbility` 67 · `GetDescriptionFor` 63 · `GetEstateIcon` 62 · `GetStat` 61 · `GetCountry` 60

规律：**图标类函数占多数**（concept / goods / religion / estate / building / law），随后是列表（`GetDataModelSize`）与界面态（`GetVariable`）。

## ScriptedGui 调用（界面侧）

```
onclick   = "[GetScriptedGui('taxing_setup').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).End)]"
on_action = "[GetScriptedGui('subtract_one_percent').Execute(GuiScope.SetRoot(<scope>.MakeScope).End)]"
```

原版界面侧仅 **9 处**（全在 `economy_lateralview.gui`）。定义侧见 `fields\common-scripted_guis.md`。

## Mod 覆盖规则（四条）

1. **路径镜像**：mod 里放同名 `gui\…` 文件（`in_game\gui`、`main_menu\gui`、`loading_screen\gui` 各自镜像）。
2. **覆盖契约 = `block` 名**：改原版控件时覆盖它声明的 `block "名"`，不要复制整块改行号。
3. **优先类型继承**：`types <集合> { type 我的 = 原版基类 { blockoverride … } }`，避免整文件复制。
4. **表达式只在 `"[…]"` 里**。

## 审查要点

- ⚠️ **`visible = yes` 在 scripted widget 里失效**（`_scripted_widgets.info` 原文："'visible = yes' does not work, the boolean must be returned by a function"）；官方 workaround：`visible = "[EqualTo_CFixedPoint('(CFixedPoint)0', '(CFixedPoint)0')]"`。
- ⚠️ **类型名冲突**：`type` 名在同一集合内必须唯一；跨文件的同名类型会互相覆盖（原版的类型名是全局注册表的键）。
- ⚠️ **`using` 指向的库控件必须在** `ui_library.gui` 的 `types UiLibrary` 里存在（或 mod 自己声明）；拼错不报错，控件静默不显示。
- ⚠️ **面板类文件有类型要求**：灾难面板必须 `type = disaster_panel`、局势面板必须 `type = situation_panel`（`gui\panels\{disaster,situation}\readme.txt` 原文）。
- ⚠️ **按对象名一文件一占位**：`gui\panels\organization\` 下大量 27 B 存根（如 `foreign_league_hre.gui` 内容仅一行 `organization_war_theme = {}`）——新增 IO/局势面板要照这个约定补文件。
- ⚠️ **tooltip 文案不在 `.gui` 里**：改文案去 `common\trigger_localization\` / `effect_localization\`（见 `fields\common-trigger-effect-localization.md`）。
- 未在 readme 中说明：**本类目没有 readme**；字段语义与属性词表以本档实查统计为准。
