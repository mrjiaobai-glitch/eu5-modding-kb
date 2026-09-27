# 事件制作（events）

权威来源：`in_game\events\readme.txt`（官方事件格式文档，115 行，逐条提炼如下）。真实样例：`in_game\events\volcano_events.txt`（火山，含 scripted_effect 嵌套）、`in_game\events\debug\qa_debug.txt`（调试事件模板）。

## 文件级规则

- 文件首行必须定义 `namespace = <名字>`（文件内所有事件的 ID 前缀）。
- 事件 ID = `<namespace>.<数字>`，**数字 1–9999**，整个事件 ID 全局唯一。
- 事件文件可以同时包含 scripted_effect / scripted_trigger 定义（volcano_events.txt 顶部就定义了 4 个 damage scripted_effect），但命名空间不与事件冲突。

## 事件结构（全字段）

```txt
namespace = my_mod

my_mod.1 = {
    type = country_event        # 五选一：country_event / location_event / unit_event / exploration_event / age_event（必填，决定 root 作用域）
    title = my_mod.1.title      # 或 title = { first_valid = { triggered_desc = { trigger = {...} desc = my_mod.1.title.a } triggered_desc = { desc = my_mod.1.title.b } } }
    desc = my_mod.1.desc        # 同上支持 first_valid 变体；可用 immediate 保存的 scope 做本地化
    historical_info = my_mod.1.historical_info   # 可选，历史背景文本；也支持 first_valid
    trigger = { ... }           # 事件触发条件。root = type 定义的作用域
    major = yes                 # 默认 no；yes 时向其他国家弹通知
    major_trigger = { ... }     # 谁能看到通知。root = 观看国，scope:from = 触发实体
    hidden = yes                # 隐藏事件（无标题/描述，如脚本内部事件）
    immediate = { ... }         # 事件触发瞬间执行的效果
    after = { ... }             # 玩家选择任意选项之后执行
    on_trigger_fail = { ... }   # 队列事件/脚本直接触发的事件不满足 trigger 时执行；on_action 触发的失败不执行
    fire_only_once = yes        # 每局只触发一次
    interface_lock = no         # 默认 yes（暂停游戏等玩家）；no = 不暂停
    dynamic_historical_event = { tag = FRA from = 1337.1.1 to = 1837.1.1 monthly_chance = 5 }  # 指定国家的随机历史事件窗口
    orphan = yes                # 不报"未引用"错误（调试事件用）
    hide_portraits = yes
    outcome = neutral           # positive / neutral / negative（音频方向，默认 neutral）
    category = situation_event  # disaster_event / situation_event / international_organization_event（只定图标；generic 默认无图标）
    illustration_tags = { ... } # 事件图标签（照抄现有事件的用法）
    weight_multiplier = { base = 1 modifier = { add = 1 <triggers> } }  # 在 random_events/random_list 中的权重修正
    image = "gfx/..."           # 事件图路径
    option = {                  # 可多个
        name = my_mod.1.a       # 本地化键
        historical_option = yes # 高亮为历史选项
        trigger = { ... }       # 选项可见条件（隐藏原因用 hidden_trigger = {} 包起来）
        <effects>               # 选项效果
        fallback = yes          # 其他选项都不可用时必显（即使 trigger 不满足）
        exclusive = yes         # 本选项可用时隐藏所有非 exclusive 选项
        original_recipient_only = yes
        ai_will_select = { <script math> }   # AI 选择意愿（脚本数学，覆盖 ai_chance）
        ai_chance = { base = 1 modifier = { add = 1 <triggers> } }  # AI 概率（mtth 式）
    }
}
```

## 触发方式

1. **on_action 挂接**（最常见）：`in_game\common\on_action\` 里的脉冲（country_monthly、location_pulses、country_yearly 等）或自定义 on_action 的 `events = { ... }`（必发，只要 trigger 满足）／`random_events = { 权重 = 事件ID chance_to_happen = N }`（随机选一）／`first_valid = { ... }`（首个满足者）。on_action 全部键见 `scripting-core.md`。
2. **脚本直接触发**：`trigger_event_silently = 事件ID` / `trigger_event_non_silently = 事件ID`（在任意效果里）。
3. **dynamic_historical_event**：给指定 tag 在时间窗口内每月按 chance 随机。

## 选项本地化键

- `my_mod.1.title` / `my_mod.1.desc` / `my_mod.1.historical_info`
- 选项：`my_mod.1.a`、`my_mod.1.b` …（**不写裸中文**）
- 自定义提示：`custom_tooltip = my_mod.1.tt`（键以 .tt 结尾）
- 所有键都须存在于本地化文件（见 `localization.md`），否则显示 raw key。

## 真实样例要点（volcano_events.txt）

```txt
scripted_effect weak_volcano_damage = {
    change_development = development_weak_penalty
    change_prosperity = prosperity_severe_penalty
    every_pop = {
        add_pop_size = { value = pop_size multiply = -0.1 }   # 人口 -10%
    }
    change_max_raw_material_workers = -1
    add_location_modifier = { modifier = volcanic_soil years = 20 mode = add_and_extend }
    every_unit_in_location = { damage_unit_percent = { 0 0.33 } }   # 范围内单位受损 0–33%
}
```

可见：效果可以嵌套 scripted_effect（weak_volcano_damage_cost = { owner = { change_gold_effect = { scale = -2 } } }）、对 POP/单位做范围操作、加带期限的地点修正（mode = add_and_extend）。

## 调试事件模板（qa_debug.txt）

`orphan = yes` + `trigger = { always = no }`（手动触发），immediate 里 `capital = { save_scope_as = start_location }` 保存作用域——这是验证硬编码效果（如 start_weather_system）的现成写法。

## 原版惯例实测（349 档 / 7,470 事件全量）

| 字段 | 原版用量 | 结论 |
|---|---|---|
| `option` 14,603 / `immediate` 6,911 / `illustration_tags` 5,176 / `fire_only_once` 3,802 | 高 | 常规事件 = option + immediate + 图 + 只触发一次 |
| `dynamic_historical_event` 3,232 | 高 | 历史事件的默认载体（`DHE\flavor_<TAG>.txt`，占全部事件 55.5%） |
| `historical_info` 1,727 / `image` 1,601 | 中 | 带历史背景与插图才算"编年史感" |
| `major` 174（2.3%）/ `hidden` **7** | 低 | 大事件通知与隐藏事件都是少数派，别当惯例 |
| `interface_lock` / `exclusive` / `original_recipient_only` / `random_valid` / `show_as_unavailable` | **0** | 文档有、原版从不写；`interface_lock` 默认 yes 就够，`show_as_unavailable` 自标 NOT IMPLEMENTED |
| `outcome` | neutral 7,331 / negative 126 / positive 50 | **不写 outcome 是绝对主流**；取值本身合法 |

触发方式用量：on_action 挂接（`events = { }` 53 处 / `random_events = { }` 38 处 / `first_valid` 0 处）、脚本直触（`trigger_event_silently` 1,299 处、`trigger_event_non_silently` 1,249 处，含块式 `{ id = … years = … }` 433 处）、DHE 3,232 处。→ 体系解析见 `vanilla\vanilla-events-and-missions.md`。

## 任务（missions）入口

- 字段权威 + 原版实测（11 条链 / 108 个节点）：`fields\common-missions.md`；体系解析：`vanilla\vanilla-events-and-missions.md` §五。
- 一句话骨架：链 = 候选池（`visible` + `chance` + defines 的 `POTENTIAL_MISSION_COUNT = 10`）；节点 = 限时任务（`icon` + `requires` + `duration` + `enabled` + `on_completion`，`final = yes` 标记链终点）。
- 三条硬规则：链的 `visible` 首行写 `game_has_missions_enabled = yes`（该游戏规则**默认关闭**）；**默认规则下任务级 `on_start` / `on_completion` 不执行**（`mission_rewards` 默认带 `task_rewards_disabled` 旗标），状态维护放 `on_persistent_*` 或链级；`select_trigger` 的 `target_flag` 用完要 `remove_variable`。

