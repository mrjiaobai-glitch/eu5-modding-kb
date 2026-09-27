# common/generic_actions（通用行动）

来源：`in_game\common\generic_actions\readme.txt`

## 顶层字段

```
<action_tag> = {
    type = <owncountry/religious/religiousfaction/diplomacy/subject/character/location/internationalorganization/situation/internationalorganizationparliament>
    sound = <sound>
    message = <message key>
    potential = <trigger>            # scope:actor = 执行国
    allow = <trigger>                # scope:actor = 执行国
    ai_prerequisite = <trigger>      # 早期阶段无任何 scope/target 可用；root = 国家
    price = <price>                  # 引用 \common\prices\（price:<price_id> 或脚本结果）
    price_modifier = <script value>  # scope:actor/recipient/target...
    payer = <script>                 # 默认 actor
    payee = <script>                 # 默认无人
    select_trigger = { ... }         # 格式同 character_interactions；source_flags 另有 only_defending_sieges/only_attacking_sieges/include_subjects；另有 ai_override_value = <script value>（AI 唯一测试值，性能用）、tooltip_msg_key
    ai_tick = <never/daily/monthly>          # 禁止 AI 用或指定频率
    ai_tick_frequency = <scripted value>
    automation_tick = <never/daily/monthly>  # 自动化进程用
    automation_tick_frequency = <scripted value>
    show_message = no / show_message_to_target = no / should_execute_price = no / show_in_gui_list = no
    ai_will_do = <effect script>
    player_automated_category = <system>  # 玩家开启该系统自动化后按 ai_will_do 执行；取值：finances/research/trade/productionmethods/laws/cabinet/parliament/estates/exploration/colonies/cultureacceptance/religiousdoctrines/buildings/rgo/armybuilder/navybuilder
    effect = <effect>                # scope:actor/recipient/target... 外加 scope:price/scope:price_modifier/scope:payer/scope:payee
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
    maximum_targets_in_one_tick = <int>  # 一次检查执行多次；-1 = 无限；默认 1
    disallowed_duplicates_of_targets_for_ai = {}  # 目标 flags 列表，防止同 tick 重复目标（如 target_location）
    force_click_and_confirm_or_hold = yes  # 总是显示确认对话框（危险的 IO 行动如宣战用）
}
```

## 本地化（readme 声明）

- 行动本身：名称 tag = `<action_tag>`、描述 tag = `<action_tag>_desc`（注意：不是带前缀的形式）。
- 消息类型键（GUI 消息）：
  - 通用兜底：`PERFORM_<Key>_ACTION`
  - 玩家执行：`WE_PERFORM_<Key>_ACTION`
  - 他国执行：`OTHER_PERFORMS_<Key>_ACTION`
  - 他国对我们执行：`ACTION_<Key>_PERFORMED_ON_US`
- 消息字符串可用预设：`$ACTION$`（行动名）、`$EFFECT$`（效果）、`$DESC$`（行动描述）。

## GUI 按钮（action_button_default）

- 字段：`title`/`description`/`effects`/`conditions`（可覆盖默认 tooltip 文案）、`actor`/`recipient`/`target`（GUI 脚本，target 可多个）、`left_action`/`left_click_and_hold_action`/`right_action`/`right_click_and_hold_action`。

## 审查要点

- `type` 是枚举，拼错加载即错。
- `player_automated_category` 取值是固定枚举列表。
- 消息键四形式 + `PERFORM_` 兜底，缺键显示 raw key。
- 未在 readme 中说明：无。
