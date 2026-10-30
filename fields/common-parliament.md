# common/parliament_agendas、parliament_issues、parliament_types（议会）

> **一句话**：议会三件套的字段表：议程、议题与议会类型，各字段作用域随 type 在国家与国际组织之间切换。
> **什么时候看**：写或改议会议程、议题、议会类型，要核对 type 决定的作用域时翻这篇。
> **体量**：114 行 · 约 6 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\parliament\` 全量 **20 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `show_message` | 1 | 1 | no（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`estate`**（8 种）：nobles_estate（96）、burghers_estate（93）、peasants_estate（90）、clergy_estate（75）、crown_estate（25）、tribes_estate（19）、dhimmi_estate（16）、cossacks_estate（16）
- **`chance`**（14 种）：10（121）、5（13）、1（8）、3（8）、50（5）、25（4）、100（4）、0（2）、15（2）、20（1）、1000（1）、8（1）、100000（1）、0.5（1）
- **`type`**（2 种）：country（44）、international_organization（19）
- **`importance`**（7 种）：1.5（15）、2（10）、3.0（3）、2.0（3）、1.0（3）、0.75（2）、3（1）
- **`special_status`**（6 种）：emperor（6）、elector（3）、free_city（3）、imperial_prelate（1）、imperial_prince（1）、senior_partner（1）
- **`show_message`**（1 种）：no（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `on_debate_start` | 0 次（20 档） | 全库也没有 → 疑似废弃字段 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_will_do` | 15 | 5 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 607 | else、set_individual_integration_level、change_institution_progress、if |
| `limit` | 403 | every_province、random_discriminated_culture、random_owned_location、ordered_market_center_in_country |
| `add` | 381 | max、dialect、ai_will_do、chance |
| `if` | 250 | ?、random_owned_location、multiply、if |
| `NOT` | 246 | any_country_in_religion、?、limit、any_location_in_province |
| `parliament_debate_failed_effect` | 143 | on_debate_failed |
| `multiply` | 129 | allow、max、value、add |
| `parliament_type` | 127 | NOT、OR |
| `type` | 124 | change_institution_progress、every_country_with_special_status_of_type、has_casus_belli_of_type_on、has_unlocked_parliament_issue_trigger |
| `max` | 116 | ordered_owned_rural_location、ordered_location_in_market、ordered_pop、ordered_province |
| `change_societal_value` | 115 | ?、else、on_accept、if |
| `modifier` | 106 | add_country_modifier、apply_modifier_on_all_members_excluding_leader、add_province_modifier、add_location_modifier |
| `OR` | 104 | potential、NOT、any_neighbor_country、any_neighbor_location |
| `years` | 102 | add_cooldown、add_country_modifier、apply_modifier_on_all_members_excluding_leader、add_location_modifier |
| `add_country_modifier` | 99 | ?、every_imperial_circle、ordered_international_organization_member、else |
