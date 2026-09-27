# 核心脚本体系（scripting-core）

四大基石：**script_values**（数值）、**scripted_effects**（效果宏）、**scripted_triggers**（条件宏）、**on_action**（触发钩子）。权威来源：`in_game\common\script_values\_script_values.info`、`in_game\common\scripted_effects\readme.txt`、`in_game\common\on_action\on_actions.info`。

## 一、Script Values（`in_game\common\script_values\`）

任意需要数字的地方都可引用脚本值。

**静态值**：`名字 = 数字`（如 `my_value = 10`），引用 `add_gold = my_value`。

**公式**（在 `{}` 内，按书写顺序执行，可嵌套 if/else_if/else）：

```
my_formula = {
    value = 0            # 设初值
    add = 5
    multiply = 2
    divide = 3           # 勿除以 0
    modulo = 2
    max = 10             # 超过 10 则钳到 10
    min = 1
    round = yes / ceiling = yes / floor = yes
    if = { limit = { <triggers> } add = 5 }
    else_if = { limit = { ... } ... }
    else = { ... }
    fixed_range = { min = 1 max = 10 }     # 随机定点数
    integer_range = { min = 1 max = 10 }   # 随机整数
}
```

**执行顺序 = 书写顺序**（官方示例：`value={add=5 multiply=4 max=10 add=5}` → 15）。

**内联**：公式可直接写在效果里，无需命名：

```
add_gold = {
    value = gold
    multiply = { value = 1 multiply = 0.5 }
}
```

**链式**：命名公式可跨作用域：`add_gold = { value = mother.example_age }`。

**范围**：`add_gold = { 1 5 }`（随机 1–5）；`add_gold = { 命名值A 命名值B }`；范围里不能内联公式。

**列表**：`add_gold = { every_child = { add = 1 } }`（按子女人数加钱）；有序列表 `ordered_child = { order_by = age max = 3 add = age }`。

**作用域**：公式内可切换作用域：`add_gold = { father = { any_child = { add = 1 } } }`。

性能警告（官方）：复杂公式每次求值都重算，勿滥用。

## 二、Scripted Effects（`in_game\common\scripted_effects\`）

- 定义：`my_effect = { <effects> }`；使用：`my_effect = yes`
- **参数**：`$参数名$` 文本替换。`my_effect = { add_prestige = $value$ }` → 使用 `my_effect = { value = 12 }`
- **作用域参数**：`$target$ = { add_prestige = $value$ }` → 使用 `my_effect = { target = scope:xxx value = 13 }`
- **拼接参数**：`my_arg_$type$ = yes` 用法 `my_arg = { type = hello }`（报错日志会大量刷 missing effect，慎用）
- **custom_description 坑（官方明示）**：效果键与 custom_description 的 text 键**不能同名**（否则 subject/object/value 取不到）——写 `my_effect = { custom_description = { text = my_effect_text ... } }`

## 三、Scripted Triggers（`in_game\common\scripted_triggers\`）

与 effects 同规则：`my_trigger = { <triggers> }`，支持 `$参数$`，使用 `my_trigger = { value = x }`。注意 EU5 的 trigger 和 effect **不能混写**（limit 里不得有效果）。

## 四、On Actions（`in_game\common\on_action\`）

**定义键全集**（`on_actions.info` 官方）：

```
my_on_action = {
    trigger = { ... }              # 触发时先查；false 则什么都不做
    weight_multiplier = { base = 1 modifier = { add = 1 <triggers> } }   # 作为 random_on_action 候选时用
    events = {                    # 列出的事件只要 trigger 满足就全发
        event_id_1
        delay = { days = 365 }    # 之后的条目延迟触发；延迟条目须在延迟前后都有效才发
        event_id_2
        delay = { months = { 6 12 } }   # 支持随机范围；新 delay 覆盖旧 delay
    }
    random_events = {             # 只挑一个发
        chance_to_happen = 25     # 百分比：是否进入评估
        chance_of_no_event = { value = 0 if = { limit = {...} add = 10 } }  # 脚本值形式
        100 = event_id_1          # 权重（乘以事件的 weight_multiplier）
        100 = 0                   # 0 条目 = 有机会不发（防止稀有事件必发）
    }
    first_valid = { event_id_1 event_id_2 fallback_without_trigger }   # 第一个 trigger 满足者
    on_actions = { on_action_1 }  # 触发其他 on_action（同事件规则）
    random_on_action = { 100 = on_action_1 100 = 0 }
    first_valid_on_action = { ... }
    effect = { <effects> }        # 直接跑效果；与 events 并发（不在 events 之前！）
                                  # effect 里设置的 scope/变量不会传到同 on_action 触发的事件
    fallback = another_on_action  # 本 on_action 什么都没跑时调用（防死循环！）
}
```

**硬编码 on_action**：`_hardcoded.txt`（5650 行）列出引擎内置钩子（on_battle_won / on_siege_won / on_new_age / on_storm_reached_location / on_capital_moved 等，每段头部注释标注 root 作用域与可用 scope）——**mod 直接写同名块追加内容即可**。常用脉冲文件：country_monthly.txt、country_yearly.txt、location_pulses.txt（天气/火山/地震）、character.txt。

## 五、作用域与变量（EU5 特色）

- 作用域词：`root` / `prev` / `this` / `scope:xxx`（**无 ROOT/PREV**）。
- 保存：`save_scope_as = xxx` → `scope:xxx` 引用；**具名 scope 不跨嵌套 effect 调用**（实测坑，见 `pitfalls.md`）。
- 变量：`set_variable = { name = x value = N }`、`change_variable = { name = x add/subtract/multiply/divide/modulo = N }`（**没有 value 键**，写 value=N 报错）、`has_variable = x`、`var:x` 引用、`remove_variable = x`。变量有作用域属性（国家变量/地点变量分开）。
- 数值范围：`effect = { 1 5 }` 随处可用（同 script value 范围）。
- tooltip 显示：`[Root.GetVariable('x').GetValue|V0]`。

## 六、触发器/效果词条来源

- 引擎触发词本地化：`in_game\localization\jomini\script_system\trigger_system_l_<lang>.yml`（可查词条存在性）
- 自定工具提示：`common\trigger_localization\`（trigger 显示名，如 `my_trigger = { <loc 键> }`）、`common\effect_localization\`（效果显示名；同名规则同 scripted_effects）
- 词条不确定 → grep 游戏本体 events/common 确认用法，别凭 EU4 记忆。
