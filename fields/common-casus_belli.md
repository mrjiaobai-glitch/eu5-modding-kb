# common/casus_belli（战争理由）

> **一句话**：战争理由字段表：创建与宣战的可见、启用条件，创建速度、战争目标与战争热情，以及和平条约与 AI 使用倾向。
> **什么时候看**：加新战争理由、调 AI 宣战倾向，或核对战争目标等引用时看。
> **体量**：132 行 · 约 6 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\casus_belli\` 全量 **68 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `allow_release_areas` | 6 | 6 | yes（6） |
| `blocks_unconditional_surrender` | 1 | 1 | yes（1） |
| `no_spy_network_cost` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`speed`**（6 种）：10（15）、8（2）、15（1）、6（1）、8.0（1）、25（1）
- **`years`**（2 种）：25（7）、15（6）
- **`additional_war_enthusiasm_attacker`**（3 种）：0.25（4）、0.75（2）、0.4（1）
- **`additional_war_enthusiasm`**（3 种）：0.1（4）、0.6（1）、0.5（1）
- **`allow_release_areas`**（1 种）：yes（6）
- **`ai_cede_location_desire`**（3 种）：-1000（2）、-100（2）、-10（1）
- **`cut_down_in_size_cb`**（1 种）：yes（5）
- **`ai_subjugation_desire`**（3 种）：-100（3）、-1000（1）、1000（1）
- **`can_expire`**（1 种）：no（4）
- **`max_warscore_from_battles`**（1 种）：100（4）
- **`allow_separate_peace`**（1 种）：no（3）
- **`no_cb`**（2 种）：no（2）、yes（1）
- **`trade`**（1 种）：yes（3）
- **`allow_wars_on_own_subjects`**（1 种）：yes（2）
- **`allow_white_peace`**（1 种）：no（2）
- **`allow_ports_for_reach_ai`**（1 种）：yes（1）
- **`blocks_unconditional_surrender`**（1 种）：yes（1）
- **`additional_war_enthusiasm_defender`**（1 种）：0.75（1）
- **`show_tags_in_ui`**（1 种）：yes（1）
- **`no_spy_network_cost`**（1 种）：yes（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `years` | 13 | 23 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `antagonism_reduction_per_warworth_defender` | 0 次（68 档） | 全库也没有 → 疑似废弃字段 |
| `Localization` | 0 次（68 档） | 全库也没有 → 疑似废弃字段 |
| `required_defender_peace_treaties` | 0 次（68 档） | 全库也没有 → 疑似废弃字段 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `scope:target` | 79 | cb_shed_shackles_of_the_ilkahante、declare_enabled、?、limit |
| `always` | 64 | create_enabled、create_visible、any_location_in_province、province |
| `not` | 41 | create_visible、OR、limit、? |
| `value` | 39 | additional_war_enthusiasm_attacker、if、add、scope:target |
| `OR` | 36 | create_visible、NOT、culture、AND |
| `is_member_of_international_organization` | 36 | OR、NOT、AND、? |
| `add` | 34 | ai_will_do、if、ai_cede_location_desire |
| `desc` | 30 | add、subtract |
| `government_type` | 23 | OR、scope:target、declare_enabled、NAND |
| `is_situation_active` | 22 | limit、OR、declare_enabled、create_enabled |
| `exists` | 19 | create_visible、limit、declare_enabled |
| `region` | 16 | province、OR |
| `is_subject` | 15 | create_enabled、create_visible、scope:target、declare_enabled |
| `limit` | 13 | trigger_if、if |
| `is_neighbor_of` | 12 | create_enabled、scope:target、any_location_in_province |

### 六、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `years` | 13 | 23 |
