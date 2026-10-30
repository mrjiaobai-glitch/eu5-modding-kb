# common/scriptable_hints（可脚本化提示）

> **一句话**：可脚本化提示的 needed 字段与 93 条原版提示实测，含每帧求值的性能警告与警报联动方式。
> **什么时候看**：给 mod 的警报加触发条件、或要查提示的优先级与分组等未文档化字段时翻这篇。
> **体量**：48 行 · 约 3 分钟通读

来源：`in_game\common\scriptable_hints\____info.txt`（**552 B，权威**——虽然极短）+ `scripted_hints.txt`（**19.7 KB / 93 条提示**）实查

## 字段（info 原文只有 `needed`）

```
<some_hint> = {
    needed = {                     # 该提示是否需要显示
        <trigger>
        # 由全局界面函数 IsHintNeeded('hint_key') 调用，传入玩家国家
    }
}
```

⚠️ info 里带**性能警告原文**：「It is possible to cause some performance issues if you're running expensive triggers since **this is called per frame**. It is likely fine to run this in tooltips etc; but consider carefully if you want to use it for anything on permanent display. If you decide you want to do that, consider asking a programmer to make you a trigger with caching.」

即：**`needed` 每帧都会被调用**——不要在里面写昂贵触发器；长期常驻的提示更要用缓存触发器。

## 原版实测（93 条提示）

字段出现率（含 `needed` 块内部的条件）：`needed`（内联条件）、`sort_priority` **82**、`priority` **80**、`hint_tag` **69**、`style` **60**、`player_playstyle` **60**、`hide` **48**、`is_alert_shown` **29**、`can_see_situation` 18、`tag` 8、`any_active_disaster` 6、`is_situation_active` 5 …

| 字段 | 说明（按原版用法推断，info 未文档化） |
|---|---|
| `priority` / `sort_priority` | 提示的显示顺序（出现率最高的两个字段） |
| `hint_tag` | 提示分组标签（对应界面上的提示类别） |
| `style` / `player_playstyle` | 提示样式与"面向哪种玩法"（60 条，成对出现） |
| `hide` | 在什么条件下隐藏 |
| `is_alert_shown` | 与警报联动的可见性（29 条） |
| `can_see_situation` / `is_situation_active` / `any_active_disaster` | 局势/灾难相关提示的门控 |

**命名规律**：`hint_<主题>`（`hint_disease`、`hint_low_stability`、`hint_rebels_growing`、`hint_rise_of_the_ottomans`、`hint_black_death`…），其中不少与**局势/灾难同名**（局势一激活就出提示）。

## 与警报系统的联动

`common\alert_descriptions\` 里的警报用 `hint = "<提示键>"` 引用本类目的条目（原版 38/133）——**提示负责"何时亮"，警报负责"长什么样"**。详见 `fields\common-alert_descriptions.md`。

## 审查要点

- `needed` **每帧求值**：避免复杂触发器（info 明确警告）；能缓存的判据尽量简化。
- 想给 mod 的警报加触发条件，正规做法就是在这里加一条 `hint_*` + 在警报里挂 `hint =`。
- 除 `needed` 外的字段（`priority` / `hint_tag` / `style` / `playstyle` / `hide` …）**info 完全没写**——它们是从 93 条原版数据里归纳的；改动前先照抄相似提示的写法。
- 未在 readme 中说明：本类目**没有 readme**，info 只有 9 行且只覆盖 `needed`。
