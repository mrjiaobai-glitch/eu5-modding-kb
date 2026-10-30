# 参考资料：界面修改指南（Interface modding guide）

> **来源**：[EU5 Wiki: Interface modding guide](https://eu5.paradoxwikis.com/Interface_modding_guide)（`action=raw` 抓取，2026-10-30；页面标记 **Pre-release**，可能落后于 1.4.5）。
> **性质**：外部资料（wiki 指南，示例来自 mod "The Idea Variation 2"），**未逐条实测**。
> **体量**：约 10 分钟

## 一、调试必备
- 启动项加 `-debug_mode`，改 gui 文件**立即生效**（热重载），不用反复重启。
- 调试控制台里用 **UI Bounds**（2D Tools 下）：hover 任意 UI 元素会显示黑框+**该元素所在文件**；`Alt+左键` 逐层循环 widget；`Alt+右键` 直接打开文件（需先在 **Settings → Game → System → Editor** 填编辑器路径，**反斜杠要写双份**：`C:\\Program Files\\Notepad++\\notepad++.exe`）。
- gui 文件在 **`in_game/gui`** 与 **`main_menu/gui`** 两处；UI Bounds 不告诉你该搜哪个，两个都要找。

## 二、文件结构（模块化）
| 概念 | 作用 | 写法 |
|---|---|---|
| **template** | 属性容器；`using = 模板名` 把模板里的属性（尺寸等）套到当前 widget | `template 名 { container = {} size = {100 100} }` |
| **types** | 可复用、可覆盖属性的 widget/container；被引用时直接写类型名 | `types 组 { type 基础 = widget { … } }` → `widget = { 基础 = {} }` |
| **window** | UI 的顶层实体；**新界面必须建 window** | `window = { name = … parentanchor = center size = {100% 100%} alwaystransparent = yes }` |
| **widget** | 成组的小 UI 元素；可交互的东西必须放在 widget 里 | — |

**更新怪癖**：脚本变量改了之后，**文本显示**有时不自动刷新（进度条按月度 tick 刷新）；用玩家点击的事件算出来才会更新，on_action/隐形事件算的值可能不刷新界面。

## 三、从零建一个自定义窗口
1. `mod/in_game/gui/mymod_interface.gui` 里写 `window = { name = "mymod_window" parentanchor = center size = { 100% 100% } alwaystransparent = yes }`。
2. **注册窗口**（变量开关方式，推荐）：建 `mod/in_game/gui/scripted_widgets/`（原版没有这目录），里面一个 **UTF-8 BOM** 的 .txt，写：
   ```
   gui/mymod_interface.gui = mymod_window
   ```
3. **可见性绑变量**：`visible = "[GetVariableSystem.Exists('mymod_window_open')]"`；开关：`onclick = "[GetVariableSystem.Toggle('mymod_window_open')]"`。
4. 控制台方式（不推荐但可用）：`GUI.CreateWidget gui/mymod_interface.gui mymod_window` / `GUI.ClearWidgets mymod_window` / 全关 `gui.clearwidgets`。

**加按钮（兼容性做法）**：先定义 type（如 `iv2_main_idea_button { type button_panel_tab_ideas = button_panel_tab_alt { tooltipwidget… onclick… input_action = "top_left_9" use_global_input_instance = yes icon { … } } }`），再在原版 `left_panel.gui` 里只写一行 `button_panel_tab_ideas = {}` —— 比整段改原版更好维护。
**关闭按钮**：`button_close_alt { blockoverride "close_onclick" { onclick = "[GetVariableSystem.Toggle('mymod_window_open')]" } }`。

## 四、高级元素（示例要点）
| 元素 | 关键写法 |
|---|---|
| **角色肖像** | `character_frame_small { blockoverride "portrait_datacontext" { datacontext = "[Country.MakeScope.GetVariable('iv2_researcher_adm').GetCharacter]" } }`（变量需别处先 set） |
| **进度条** | 外层 `icon` 画框 + `progressbar { min = 0 max = "[FixedPointToFloat(Country.MakeScope.Var('上限变量').GetValue)]" value = "[FixedPointToFloat(Country.MakeScope.Var('当前变量').GetValue)]" tooltipwidget = { … } }` |
| **通用行动按钮** | `card_header_action_button_04 { actor = "[Player]" parameter = { parameter_name = "target" parameter_value = "[Country]" } left_click_and_hold_action = { action_name = "你的动作" } }` |
| **纯展示（可悬停）** | 用 `button_default` 包 `hbox`(icon)+`hbox`(text_single)，整块可悬停显示 tooltip |
| **动态贴图** | ①`main_menu/common/game_concepts` 建概念带 `texture`（图放 `in_game/gfx/interface/icons`）②变量存 `flag:概念名` ③`icon = { texture = "[GetConceptTexture(GetPlayer.MakeScope.Var('dynamic_icon_var').GetFlagName)]" }`；注意：**别本地化 flag**、变量变更即时生效、未设置时回落默认概念图、贴图尺寸不同会改变元素大小（用 size 固定） |
| **tooltip** | `tooltipwidget = { using = 模板 }` 可挂在图标/按钮/文本/滚动条上 |

## 五、和本库的关系
- 字段权威：`fields\gui-*.md`（gui 结构/面板/脚本化控件/过滤器）与 `vanilla\vanilla-gui.md`。
- 合并/覆盖：`guides\merging.md`（同名 `.gui` 是整文件覆盖；`types` 同名冲突按加载序，见 §八）。
- 本档只保留"新建窗口/加按钮/动态贴图"的**操作路径**，具体属性表以 `fields\` 为准。
