# gui/scripted_widgets（声明式挂载控件）

> **一句话**：声明式挂载控件：用一行 txt 映射即可挂上控件，且 visible 必须写成函数返回的布尔值。
> **什么时候看**：想让 mod 控件出现在原版界面上而不覆盖原版文件时翻这篇。
> **体量**：45 行 · 约 3 分钟通读

来源：`main_menu\gui\scripted_widgets\_scripted_widgets.info`（**554 B，本类目唯一权威**）

## 机制

**scripted widget = 不必写进已有 `.gui` 文件就能挂上的控件**。info 原文：

> Scripted widgets are a way of creating widgets without needing to define them inside an existing .gui file.
> A scripted widget is declared following this format within a .txt file inside this folder:
> `gui/scripted_widget_file_name.gui = scripted_widget_name`

即：在 `main_menu\gui\scripted_widgets\` 下放一个 `.txt`，每行写

```
gui/<你的控件文件>.gui = <widget 名>
```

引擎就会去那个 `.gui` 文件里找同名控件并挂载。

## ⚠️ 唯一但致命的约束（info 原文）

> Note: The scripted widget inside the .gui file is required to have a **`visible`** attribute. **`visible = yes` does not work**, the boolean must be returned by a function.
> As a workaround, `visible = "[EqualTo_CFixedPoint('(CFixedPoint)0', '(CFixedPoint)0')]"` works as an equivalent to `visible = yes`.

翻译成操作要点：

1. scripted widget **必须**有 `visible` 属性；
2. 写 `visible = yes` **无效**——必须是**函数返回的布尔**；
3. 恒真 workaround（官方给的）：`visible = "[EqualTo_CFixedPoint('(CFixedPoint)0', '(CFixedPoint)0')]"`。

## 原版用量

**原版只有文档、没有实例**——`main_menu\gui\scripted_widgets\` 目录下只有 `_scripted_widgets.info` 一个文件。也就是说这是**一条"有能力、无样例"的扩展路径**：想让 mod 的控件出现在原版界面上（而不覆盖原版文件），这是正规入口，但得自己从 info 的 8 行里推。

## 审查要点

- 目录位置固定：**`main_menu\gui\scripted_widgets\`**（不是 `in_game\gui\`）。
- `.txt` 里写的是 **`gui/…` 相对路径**（正斜杠），指向的 `.gui` 文件放在常规 gui 目录里。
- `visible` 必须写成表达式（见上）；这条不满足时控件不会出现，且**没有明显报错**。
- 未在 readme 中说明：能否挂载到任意原生界面、能否带参数、加载顺序——info 只有 8 行，**其余为未文档化行为，改动前先实测**。
