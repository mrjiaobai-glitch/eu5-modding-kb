# in_game/events（事件）

来源：`in_game\events\readme.txt`

## 文件结构

- 文件开头必须定义 `namespace = <event id name>`（该文件所有事件共用）。
- 事件 ID：`<event id name>.<id number>`，id 数字 **1–9999**，整个 ID 全局唯一。

## 事件字段

```
<namespace>.<id> = {
    type = <country_event/location_event/unit_event/exploration_event/age_event>  # 必填，五选一
    title = <loc 键>            # <namespace>.<id>.title；可用 immediate 中的作用域做本地化
    title = {                   # 备选：按序显示第一个满足 trigger 的标题（依赖 immediate 中保存的事件目标时用）
        first_valid = {
            triggered_desc = { trigger = { <triggers> } desc = <...>.title.a }   # root = type 定义的作用域
            triggered_desc = { desc = <...>.title.b }                            # 无 trigger 项兜底
        }
        # 备选 random_valid = {}
    }
    desc = <loc 键>             # 同 title，可用 first_valid 块形式（.desc.a/.desc.b）
    historical_info = <loc 键>  # 历史背景文本；也可用 first_valid 块形式
    trigger = { <triggers> }    # 触发条件；root = type 定义的作用域
    major = <yes/no>            # yes 则其他国家收到通知；默认 no
    major_trigger = { <triggers> }  # 谁能看到该事件通知；root = 观看国, scope:from = 原触发实体
    hidden = <yes>              # 事件对玩家隐藏，无需 title/desc
    immediate = { <effects> }   # 触发瞬间；root = type 作用域
    after = { <effects> }       # 任一选项被选**之后**；root = type 作用域
    on_trigger_fail = { <effects> }  # 排队事件/脚本直接触发的事件不满足 trigger 时运行；on-action 触发的失败**不会**运行
    fire_only_once = <yes>      # 每局只触发一次
    interface_lock = <no>       # 单人暂停游戏；默认 yes
    dynamic_historical_event = { tag = <country tag> from = <date> to = <date> monthly_chance = <int> }  # 时间窗内按月度几率触发；tag 可多个
    orphan = <yes>              # 不记录"未引用"错误；调试事件用
    hide_portraits = <yes>      # immediate 保存了角色目标时事件不显示角色肖像
    outcome = <positive/neutral/negative>  # 音频方向；默认 neutral
    category = <disaster_event/situation_event/international_organization_event>  # 事件图标；默认 generic_event 无图标
    illustration_tags = { 10 = happy  10 = exterior }   # 事件图片标签：**权重 = 标签**（原版 5,177 个事件在用；权重实测恒为 10，20 只出现 3 次）；标签走固定词汇表，见 vanilla\vanilla-dlc-and-assets.md §三
    weight_multiplier = { base = 1 modifier = { add = 1 <triggers> } }  # 该事件作为 random_on_action 列表候选时操纵权重；仅对 random_list/random_events 调用的事件有效
    image = "<event image path>"     # 事件图片
    option = { ... }                 # 可多个
}
```

## 选项字段

```
option = {
    name = <namespace>.<id>.a        # loc 键
    historical_option = <yes>        # 高亮为历史选项
    trigger = { <triggers> }         # 满足才显示；root = type 作用域；不想显示"可用原因"用 hidden_trigger = {}
    <effects>                        # root = type 作用域
    fallback = <yes>                 # 其他选项都不可用时本选项也可用（即使 trigger 未满足）
    exclusive = <yes>                # 本选项可用时隐藏所有非 exclusive 选项
    original_recipient_only = <yes>  # 仅对触发事件的国可用
    moral_option / evil_option / high_risk_option / high_reward_option = <yes>  # 语义 readme 标注 ???
    ai_will_select = { <script math> }  # AI 选此选项的脚本数学；覆盖 ai_chance
    ai_chance = {                   # AI 选择概率（old school mtth 风格）
        base = 1
        modifier = { add = 1 <triggers> }
    }
    show_as_unavailable = {}        # 未实现（NOT IMPLEMENTED）
}
```

## 审查要点

- `type` 五选一（country/location/unit/exploration/age）；**disaster_event/situation_event/international_organization_event 是 `category` 的值**（图标），不是 type。
- ID 数字 1–9999 且全局唯一。
- `ai_chance` 结构为 `base` + `modifier { add }`（勿写 value 键）。
- 未在 readme 中说明：character_event（旧技能曾列）不在 type 枚举中——以本 readme 为准。
- `outcome` 只取 `positive/neutral/negative`（默认 neutral）。**全量实测（349 档 / 7,470 事件）：neutral 7,331 / negative 126 / positive 50**——97.6% 的事件不写或写 neutral；`negative` / `positive` 合法但罕见，审查时勿判成错误（旧技能文案"good/bad 无效"曾被误读为"只能 neutral"；旧档里的"26 档样本 neutral 337 / negative 94"已按全量修正）。
- **文档有、原版零使用**（勿当非法字段，也别当惯例）：`interface_lock`（默认 yes 足够）、`exclusive`、`original_recipient_only`、`random_valid`（原版一律 `first_valid`）、`show_as_unavailable`（readme 自标 NOT IMPLEMENTED）；`hidden` 仅 7 处、`major` 174 处（2.3%）。目录分布与触发方式用量见 `vanilla\vanilla-events-and-missions.md`。
- 统计本目录字段时必须**按取值白名单过滤**：`^\s*type\s*=` 会捞到事件体内的 `estate_type` / `work_of_art_type` / `casus_belli`，`^\s*category\s*=` 会捞到叛军的 `estate` / `pretender`（见 `pitfalls.md` §十三）。
