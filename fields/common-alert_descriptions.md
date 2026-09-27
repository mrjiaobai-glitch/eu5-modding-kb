# common/alert_descriptions（警报）

来源：**无 readme**——`in_game\common\alert_descriptions\00_default.txt`（**20 KB / 133 条警报**）实查

## 字段

```
<alert_key> = {
    title = <loc 键>          # 133/133 —— 警报标题
    texture = <图标路径>       # 133/133 —— 相对 gfx/interface 的图标（如 "alerts_icons/disease_outbreak"）
    priority = <颜色>          # 133/133 —— red / orange / yellow / green…（决定警报条上的排序与配色）
    hint = <提示键>            # 38/133（可选）—— 关联到 common\scriptable_hints\ 里的 hint 键
    game_concept = <概念键>    # 15/133（可选）—— 标题里挂百科概念链接
}
```

原版实例：

```
has_imminent_revolt = {
    title = "ALERT_POSSIBLE_REVOLTS_TITLE"
    hint = "hint_rebels_growing"          # ← 触发逻辑在 scriptable_hints 里
    texture = "alerts_icons/imminent_revolt"
    priority = orange
}

has_disease = {
    title = "ALERT_DISEASE_OUTBREAK_TITLE"
    texture = "alerts_icons/disease_outbreak"
    priority = red
}
```

## 关键机制：**警报的外观在这里，"何时出现"在 `scriptable_hints`**

- `common\scriptable_hints\scripted_hints.txt`（**93 条提示**）里的条目带 `needed = { <trigger> }`——info 原文说明：它由全局界面函数 `IsHintNeeded('hint_key')` 调用，**传入玩家国家**。
- 警报通过 `hint = "<提示键>"` 与提示挂钩（原版 38/133 有 `hint`）。
- 没有 `hint` 的警报（如 `has_disease`）说明其触发由**引擎侧**判定，数据只提供外观与优先级。

> 所以"改警报什么时候亮"要改 `scriptable_hints`，"改警报长什么样/排多前"才改这里。

## 原版实测（133 条）

- **必填 3 项**：`title` / `texture` / `priority`（133/133 全有）
- **可选**：`hint` 38 条、`game_concept` 15 条
- 命名多为 `has_*` / `can_*` / `is_*` / `lack_*` / `about_to_*`（如 `has_high_inflation`、`can_embrace_institutions`、`is_over_fort_limit`、`lack_pop_for_rgo`、`about_to_lose_great_power_status`）——**名字即警报语义**
- 配套界面：`gui\alertmanager.gui`（**192 KB**）

## 审查要点

- `title` / `texture` / `priority` 缺一不可（原版 133 条无一遗漏）。
- `priority` 决定警报在列表里的位置与配色；写非原版值（如 `purple`）行为未定义。
- `texture` 是**相对路径**（`alerts_icons/…`、`diplomatic_actions/…`），实际文件在 `gfx\interface\` 下；路径错 → 警报图标空白。
- 想让 mod 自己的警报有触发条件：**在 `common\scriptable_hints\` 里写带 `needed` 的提示**，再用 `hint =` 挂上（见 `fields\common-scriptable_hints.md`）。
- 未在 readme 中说明：本类目**没有 readme**；`priority` 的合法取值清单、无 `hint` 警报的引擎判定条件均未文档化。
