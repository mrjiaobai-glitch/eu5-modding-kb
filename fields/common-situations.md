# common/situations（局势）

> **一句话**：局势的字段：生成概率、关联 IO 与决议、起止条件、各类钩子效果，以及地图颜色与提示。
> **什么时候看**：写新局势、要挂国际组织决议或配置地图配色与 tooltip 时翻这篇。
> **体量**：82 行 · 约 4 分钟通读

来源：`in_game\common\situations\readme.txt`

## 字段

```
<situation_id> = {
    custom_description = <string>       # customizable_localization 中的自定义描述键
    monthly_spawn_chance = <script value>  # 每月生成概率（0..1）（scope:situation）
    international_organization_type = <IO type tag>  # 关联的 IO 类型
    resolution = <resolution tag>       # 关联的决议
    voters = <global_list_tag>          # 在上述决议中有投票资格的人列表
    can_start = <trigger>               # 能否开始（root = situation）
    can_end = <trigger>                 # 能否结束（root = situation）
    visible = <trigger>                 # 玩家国能否看到并参与（root = country, scope:target = situation）
    on_start = <effect>                 # 开始时，一般性设置（root = situation）
    on_monthly = <effect>               # 每月（root = situation）
    on_ending = <effect>                # 结束前、状态改变前（root = situation）
    on_ended = <effect>                 # 结束后、状态改变后（root = situation）
    tooltip = <effect>                  # 生成地图 tooltip，不实际执行（root = location, scope:target = situation）
    map_color / secondary_map_color = <script color>  # root = location, scope:target = situation
}
```

## 审查要点

- `international_organization_type`/`resolution`/`voters` 引用须存在。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\situations\` 全量 **22 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `hint_tag` | 22 | 22 | hint_war_of_religions（1）、hint_red_turban_rebellions（1）、hint_treaty_of_tordesillas（1）、hint_black_death（1）、hint_colonial_revolution（1） |
| `is_data_map` | 2 | 2 | yes（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`monthly_spawn_chance`**（7 种）：monthly_spawn_chance_unique（14）、monthly_spawn_chance_ultimate_high（2）、monthly_spawn_chance_ultimate（2）、monthly_spawn_chance_low（1）、monthly_spawn_chance_high（1）、monthly_spawn_chance_very_low（1）、0（1）
- **`voters`**（4 种）：nanbokuchou_voters（1）、guelphs_and_ghibellines_voters（1）、fall_of_delhi_voters（1）、council_of_trent_voters（1）
- **`resolution`**（3 种）："fall_of_delhi_resolution"（1）、"western_schism_resolution"（1）、"nanbokuchou_resolution"（1）
- **`international_organization_type`**（1 种）：catholic_church（2）
- **`is_data_map`**（1 种）：yes（2）
- **`custom_description`**（1 种）：GetTreatyOfToredesillasDesc（1）

### 三、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `legend_key` | 79 | 21 |
| `content_trigger` | 1 | 1 |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 530 | every_owned_non_rural_location、random_in_global_list、legend_key、random_location_in_continent |
| `if` | 269 | map_color、?、every_war_participant、order_by |
| `value` | 223 | map_color、else_if、if、else |
| `else_if` | 185 | on_ended、every_current_policy_in_international_organization、?、every_international_organization_member |
| `OR` | 157 | ?、c:CHI、any_location_in_region、NOT |
| `trigger_event_non_silently` | 152 | random_in_global_list、?、c:FRA、random_in_list |
| `NOT` | 129 | can_start、custom_tooltip、var:war_of_religion_current_war、can_end |
| `custom_tooltip` | 122 | any_country_with_capital_in_geography、?、can_end、if |
| `has_owner` | 116 | if、any_ownable_location_in_area、limit、else_if |
| `set_variable` | 97 | every_country_in_religion、c:PAP、situation:rise_of_the_ottomans、situation:treaty_of_tordesillas.var:var_east_country |
| `name` | 85 | sort_global_variable_list、change_local_variable、remove_list_global_variable、change_variable |
| `desc` | 80 | add_country_modifier、legend_key、? |
| `color` | 79 | legend_key、? |
| `AND` | 72 | custom_tooltip、OR、any_foreign_building_countries_in_location、AND |
| `random_list` | 67 | every_country_in_religion、custom_tooltip、every_present_country、? |
