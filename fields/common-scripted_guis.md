# common/scripted_guis（脚本化 GUI）

来源：`in_game\common\scripted_guis\scripted_guis.info`（**1,004 B，本类目唯一权威**）+ `economy_satisfaction_target.txt`（**17.8 KB**，原版唯一数据文件）实查

## 字段（info 全表）

```
<scripted_gui_key> = {
    scope = <scope 类型>              # 该 SGUI 对哪种作用域可用
    is_shown = { <trigger> }          # 是否显示 / 对玩家是否可用
    is_valid = { <trigger> }          # 是否可以使用
    effect = { <effect> }             # 被激活时执行什么
    saved_scopes = { <字符串…> }       # 需要在 trigger/effect 里当事件目标用的 scope 列表
    notification_key = <loc 键>        # 激活时的通知（默认 jomini_scripted_gui_confirm）
    confirm_title = { … } / confirm_text = { … }   # 确认窗标题与正文
    ai_is_valid = { <trigger> }       # AI 是否可用（默认 false）
    ai_chance = { … }                 # AI 激活概率（1–100 的脚本值）
    ai_frequency = { … }              # AI 评估频率（月）
}
```

## 核心认知：SGUI 是"玩家能点、AI 也会点"的按钮

`ai_is_valid` / `ai_chance` / `ai_frequency` 三个字段决定了 SGUI **不只是界面按钮**：

- `ai_is_valid` 默认 **false** —— 不写就纯玩家功能；
- 写了之后，AI 会按 `ai_frequency`（月）评估、以 `ai_chance` 的概率**自己激活它**；
- 也就是说：**一个 SGUI = 给 AI 也开了一条行动路径**（把复杂脚本操作包装成双方都能用的动作）。

## 界面侧调用

```
onclick   = "[GetScriptedGui('taxing_setup').Execute(GuiScope.SetRoot(GetPlayer.MakeScope).End)]"
on_action = "[GetScriptedGui('subtract_one_percent').Execute(GuiScope.SetRoot(TaxRateSetting.GetEstate.GetCountry.MakeScope).End)]"
```

要点：`GetScriptedGui('<名>').Execute(GuiScope.SetRoot(<scope>.MakeScope).End)`——**`.End` 不能漏**，`SetRoot` 的参数必须是 `MakeScope` 过的对象。

## 原版实测

| 项 | 值 |
|---|---|
| 数据文件 | `economy_satisfaction_target.txt` **17.8 KB**（唯一） |
| 界面侧引用 | **9 处**，全在 `in_game\gui\economy_lateralview.gui` |
| 定义的 SGUI | `taxing_setup`（把 7 个阶层的税收目标变量初始化为 0.5）、`subtract_one_percent`、`subtract_five_percent`、`subtract_ten_percent`、`set_to_zero_percent`、`add_one_percent` … |
| 典型 `effect` 内容 | 对 `root` 做 `set_variable` / `change_variable`（因为 SGUI 的 root 由 `GuiScope.SetRoot` 决定） |

## 审查要点

- **`.End` 漏写会静默失效**（表达式语法错误不弹窗）。
- `SetRoot` 只接受 **`MakeScope` 过的对象**；直接传对象会取不到数据。
- `saved_scopes` 里声明的 scope 名才能在 `is_shown` / `is_valid` / `effect` 里当事件目标用——漏声明就引用不到。
- `ai_is_valid` 默认 **false**：想让 AI 用必须显式打开，并同时给 `ai_chance` / `ai_frequency`，否则等于没接。
- `confirm_title` / `confirm_text` 是**确认窗**（`notification_key` 决定通知样式，默认 `jomini_scripted_gui_confirm`）——危险操作应配确认窗。
- 未在 readme 中说明：本类目**没有 readme**，只有 13 行 info；`scope` 的合法取值清单、`is_shown` 与 `is_valid` 的界面表现差异均为未文档化行为。
