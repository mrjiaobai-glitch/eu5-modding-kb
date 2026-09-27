# common/character_interactions（角色交互）

来源：`in_game\common\character_interactions\readme.txt`

## 顶层字段

```
<interaction_name> = {
    message = <yes/no>                     # 是否显示消息
    sound = <sound>
    on_other_nation = <yes/no>             # 可否对别国角色使用（默认 no）
    on_own_nation = <yes/no>               # 可否对本国角色使用（默认 no）
    is_consort_action = <yes/no>           # 是否是对配偶的交互
    potential = <trigger>; allow = <trigger>   # scope:actor = 执行国
    price = <price>（引用 \common\prices\）; price_modifier = <script value>
    payer = <script>（默认 actor）; payee = <script>（默认无人）
    select_trigger = { ... }               # 格式同 common-cabinet_actions.md 的 select_trigger；source 此处多 world
    ai_tick = <never/daily/monthly>; ai_tick_frequency = <scripted value>
    show_message = no / show_message_to_target = no / should_execute_price = no / show_in_gui_list = no
    ai_will_do = <effect script>; effect = <effect>
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
}
```

## select_trigger 要点（公共格式详见 common-cabinet_actions.md）

- `looking_for_a`：character/location/province/area/region/country/value/boolean 等
- `target_flag`：自定义 scope 名（默认 target, target_1...）；`source`：actor/recipient/target/.../target_4/**world**
- `source_flags` 性能选项：neighbor/possible_colonial_charters/include_dead/include_any_present/possible_exploration_areas/adjacent_locations/vacant_adjacent_locations/adjacent_provinces/border/border_or_recipients_capital_area/provinces_ai_wants_to_give_away/only_actual_locations
- `interaction_source_list = <effect>`：scope:actor = country，add_to_list = source 填表
- `allow_null` / `allow_null_trigger` / `allow_self`（仅国家）/ `name` / `visible` / `enabled` / `selected`（root = 被测对象）/ `map_color` 等

## 审查要点

- `potential`/`allow` 中 root 是角色，`scope:actor` 才是国家——勿把国家级 trigger 直接放 root。
- price 引用须在 common/prices 存在。
- 未在 readme 中说明：本地化键格式。
