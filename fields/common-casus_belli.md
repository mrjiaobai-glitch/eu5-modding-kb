# common/casus_belli（战争理由）

来源：`in_game\common\casus_belli\readme.txt`

## 字段

```
<casus belli ID> = {
    no_cb = <yes/no>                         # 仅用于 no-cb cb
    trade = <yes/no>                         # 是否贸易相关 cb
    create_visible = <trigger>               # 能否看到该 cb（root = country, scope:target = target）
    create_enabled = <trigger>               # 能否创建 cb（root = country, scope:target）
    declare_enabled = <trigger>              # 能否用它宣战（root = country, scope:target）
    province = <trigger>                     # 目标为省份时检查省份是否有效（root = province, scope:actor, scope:recipient）
    speed = <float>                          # 每月创建进度（%）；100 = 完成
    additional_war_enthusiasm = <script value>   # root = country, scope:war, scope:attacker, scope:defender, scope:target(角色,可选), scope:target_province(可选), scope:target_country(可选)
    additional_war_enthusiasm_attacker = <script value>  # 仅攻击方
    additional_war_enthusiasm_defender = <script value>  # 仅防御方
    war_goal_type = <war goal ID>            # 该 cb 的战争目标
    allow_separate_peace = <yes/no>          # 是否允许单独议和（默认 yes）
    cut_down_in_size_cb = <yes/no>           # 仅 AI；让 AI 更常选释放条约
    days / weeks / months / years = <int>    # 覆盖 NDiplomacy::CASUS_BELLI_MONTHS
    max_warscore_from_battles = <int>        # 覆盖 WARSCORE_MAX_FROM_BATTLES
    ai_subjugation_desire = <script value>   # root = country; scope:recipient = subject; scope:subject_type; scope:war
    ai_cede_location_desire = <script value> # root = country; scope:location; scope:war
    antagonism_reduction_per_warworth_defender = <script value>  # root = country; scope:recipient; scope:war
    can_expire = <yes/no>
    allow_wars_on_own_subjects = <yes/no>    # 能否对自己的附庸使用
    allow_ports_for_reach_ai = <yes/no>
    ai_will_do = <script value>              # AI 何时使用该 cb；覆盖基于战争目标征服成本的常规计算（root = country, scope:target）
    custom_tags = { <strings> }
    show_tags_in_ui = <yes/no>
    allow_white_peace = <yes/no>             # 默认 yes
    required_peace_treaties = { <scripted peace treaties> }           # 任一侧执行才能非白和平结束
    required_attacker_peace_treaties = { ... }   # 攻击方领袖须执行
    required_defender_peace_treaties = { ... }   # 防御方领袖须执行
    ai_wait_with_sending_peace = <trigger>   # root = sender, scope:recipient, scope:war
}
```

## 本地化（readme 声明）

- `<casus belli ID>`（名称）、`<casus belli ID>_PROV`、`<casus belli ID>_desc`
- 注：实测审查中还常见原版 loc 用 `cb_` 前缀键——审查时以原版 localization 中该 cb 的实际键为准，键缺失显示 raw key。

## 审查要点

- `war_goal_type` 引用须在 common/wargoals 存在；`required_*_peace_treaties` 引用须在 common/peace_treaties 存在。
- 各 trigger/script value 的作用域不同（create_* 是 country/target；province 是 province/actor/recipient）。
- 未在 readme 中说明：无。
