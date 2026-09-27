# common/parliament_agendas、parliament_issues、parliament_types（议会）

覆盖 readme：`in_game\common\parliament_agendas\readme.txt`、`in_game\common\parliament_issues\readme.txt`、`in_game\common\parliament_types\readme.txt`

## parliament_agendas

```
<agenda key> = {
    type = <country/international_organization>   # 默认 country
    estate = <estate type>                        # 仅 type = country 时有效；可多个
    special_status = <special status key>         # 仅 type = international_organization 时有效；可多个
    potential = {}     # 作用域随 type（root = country/IO；country 时另 target = estate_type / special_status）；只显示满足的议程
    allow = {}         # IO 时对 actor 检查（仅特定成员可过议程）；country 时经 is_available_for / is_allowed_for 在 select_triggers 中使用
    on_accept = {}     # 作用域随 type
    on_bribe = {}      # 仅 country；bribe_estate 时施加于行贿国（root = briber, target = estate_type, recipient = bribee）
    can_bribe = {}     # 仅 country；同上作用域
    chance = {}        # script value；作用域随 type
    importance = <script value>  # 越高对议会议题影响越大；未定义恒为 1
}
```

## parliament_issues

```
<parliament_issue> = {
    type = <country/international_organization>   # 默认 country
    estate = <estate type>                        # 仅 type = country
    special_status = <special status>             # 仅 type = international_organization
    modifier_when_in_debate = { <modifiers> }     # country 或 IO 修正
    allow = { <triggers> }                        # root = country/IO
    potential = { <triggers> }                    # root = country/IO
    selectable_for = { <triggers> }               # root = 尝试选择的国家, scope:recipient = IO
    chance = { <chance parameters> }              # root = country/IO
    on_debate_passed / on_debate_failed / on_debate_start = { <effects> }  # root = country/IO
    wants_this_parliament_issue_bias = { <scripted maths> }  # 政府: root = country；IO: root = country, scope:actor, scope:recipient = IO, scope:target
}
```

## parliament_types

```
<parliament type key> = {
    type = <country/international_organization>
    potential = { <triggers> }    # root = country 或 io（随 type）；决定是否可见
    allow = { <triggers> }        # root 随 type
    locked = { <triggers> }       # root 随 type
    modifier = { <country modifiers> }
}
```

## 审查要点

- 三者的 `type` 字段决定 root 作用域（country 或 international_organization）——写错作用域读不到。
- agendas 的 `estate`/`special_status` 引用须存在。
- IO 的 parliament_type 只利用**为 IO 定义**的 parliament_types。
- 未在 readme 中说明：本地化键格式。
