# common/tutorial_lessons 与 tutorial_lesson_chains（教程系统）

> **一句话**：教程链、课与步骤三件套的字段，含存档总开关、两类转移的差别与特殊按钮 id 及配套界面文件。
> **什么时候看**：写教程课与步骤、要暂停游戏或跑效果，或要给步骤挂界面标签时翻这篇。
> **体量**：91 行 · 约 5 分钟通读

来源：`tutorial_lesson_chains\_tutorial_lesson_chains.info`、`tutorial_lessons\_tutorial_lesson.info`（**两份 info 合计约 6 KB，字段齐全**）

## 三件套结构

```
lesson_chain_name = {                    # ① 链（tutorial_lesson_chains\）
    trigger = { … }                      # 链启动条件
    delay = <秒>                          # 启动延迟
    save_progress_in_gamestate = yes/no  # ★ “railroaded”教学模式（见下）
}

lesson_name = {                          # ② 课（tutorial_lessons\）
    chain = <链名>                        # 归属哪条链（选课时优先当前链）
    start_automatically = yes/no          # trigger 满足时自动开始（默认 yes；手动开始用 start_tutorial_lesson 效果）
    trigger = { … }                      # 开始条件
    gui_tag = X                          # 任一步骤激活时设置的标签，GUI 里用 IsTutorialTagOpen('X') 检测（可多个）
    highlight_widget = X                 # 任一步骤激活时高亮该 GUI 控件（控件名须唯一）
    trigger_transition = { … }           # 任一步骤激活期间可发生的触发器转移（可多个）
    delay = <秒> / default_lesson_step_delay = <秒>
    finish_gamestate_tutorial = yes/no   # 本课结束时关闭 railroaded 模式（默认 no）
    shown_in_encyclopedia = yes/no       # 是否出现在百科（默认 yes）

    lesson_step_1 = {                    # ③ 步骤
        text = <loc 键>
        gui_transition = {               # ★ 等玩家点击
            button_id = X                # 转移绑定的按钮 id；"next" 这个 id 的按钮说明会显示为指引
            button_text = <loc 键>
            target = …                   # 转移目标（见下）
            enabled = { … }              # 可选：按钮禁用条件；button_id = next 的按钮会显示其 trigger 说明作为指引
        }
        trigger_transition = {           # ★ 条件满足自动推进
            trigger = { … }
            target = <步骤名> | lesson_finish | lesson_abort
            button_id = X                # 可选：显示一个禁用按钮（自动推进下玩家不会真点）
            button_text = <loc 键>
        }
        gui_tag = X / highlight_widget = X        # 与课的同类字段【相加】
        soundeffect / voice = X / repeat_sound_effect = yes/no   # 默认 yes
        delay = <秒> / animation = X
        pause_game = yes/no / force_pause_game = yes/no          # 见下方“依赖 gamestate”
        shown_in_encyclopedia = yes/no
        effect = { … }                   # 转移到该步骤时触发（同样依赖 gamestate）
    }
    lesson_step_2 = { … }
}
```

## ⚠️ `save_progress_in_gamestate` 是总开关

两份 info 对它的说明合起来是：

- 这是**"railroaded"（轨道式）教学模式**——玩家在书签界面选"教程"时启用；
- **多人游戏不可用**；
- 进度存进**存档（gamestate）**而不是全局 `tutorial.txt`；
- 关闭后（例如某课设了 `finish_gamestate_tutorial = yes`），带 `save_progress_in_gamestate = yes` 的链**再也无法触发**；
- **暂停游戏（`pause_game` / `force_pause_game`）与步骤 `effect` 都依赖它**。

## `gui_transition` vs `trigger_transition`（info 专门对比）

| | 行为 |
|---|---|
| `gui_transition` | 条件满足 → **按钮变为可用 → 等玩家点击**才推进 |
| `trigger_transition`（带 `button_id`） | 条件满足 → **自动推进**；那个按钮只是显示指引（玩家不会真的点到） |

## 原版实测

| 文件 | 体量 |
|---|---|
| `tutorial_lessons\00_tutorial_lesson_basics.txt` | 36 KB |
| `tutorial_lessons\00_tutorial_lesson_diplomacy.txt` | 30 KB |
| `tutorial_lessons\00_tutorial_lesson_military.txt` | 12 KB |
| `tutorial_lessons\00_tutorial_lesson_admin.txt` | 8 KB |
| `tutorial_lessons\_tutorial_lesson.info` | 4,627 B |
| `tutorial_lesson_chains\`（info + 数据） | 1 KB |

配套界面：`gui\tutorial_window.gui`（info 里点名要求参照其中的 `Tutorial.HasTransition` 用法）。

## 审查要点

- **教程步骤要暂停游戏或跑效果，必须链上有 `save_progress_in_gamestate = yes`**，否则字段写了也不生效。
- `highlight_widget` 的控件名**必须唯一**（info 原文："The widget's name should be unique for the functionality to work properly"）。
- `gui_tag` 是**课与步骤相加**的（不是覆盖）；GUI 侧用 `IsTutorialTagOpen('X')` 读。
- `button_id = next` 是特殊 id（defines `TUTORIAL_STEP_INSTRUCTION_BUTTON_ID`）：它的 `enabled` trigger 说明会作为**指引文字**显示。
- 未在 readme 中说明：本类目没有 readme，只有两份 `.info`；`target` 的完整取值除 info 列出的三种（步骤名 / `lesson_finish` / `lesson_abort`）外未文档化。
