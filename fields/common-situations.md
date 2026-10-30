# common/situations（局势）

> **一句话**：局势的字段：生成概率、关联 IO 与决议、起止条件、各类钩子效果，以及地图颜色与提示。
> **什么时候看**：写新局势、要挂国际组织决议或配置地图配色与 tooltip 时翻这篇。
> **体量**：100 行 · 约 5 分钟通读

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
| `hint_tag` | 22 | 22 | hint_red_turban_rebellions（1）、hint_nanbokuchou（1）、hint_little_ice_age（1）、hint_western_schism（1）、hint_golden_age_of_piracy（1） |
| `is_data_map` | 2 | 2 | yes（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`monthly_spawn_chance`**（7 种）：monthly_spawn_chance_unique（14）、monthly_spawn_chance_ultimate_high（2）、monthly_spawn_chance_ultimate（2）、0（1）、monthly_spawn_chance_low（1）、monthly_spawn_chance_high（1）、monthly_spawn_chance_very_low（1）
- **`voters`**（4 种）：fall_of_delhi_voters（1）、council_of_trent_voters（1）、nanbokuchou_voters（1）、guelphs_and_ghibellines_voters（1）
- **`resolution`**（3 种）："fall_of_delhi_resolution"（1）、"nanbokuchou_resolution"（1）、"western_schism_resolution"（1）
- **`international_organization_type`**（1 种）：catholic_church（2）
- **`is_data_map`**（1 种）：yes（2）
- **`custom_description`**（1 种）：GetTreatyOfToredesillasDesc（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `can_end` | 0 次（22 档） | **有**（出现在 2 个类目） |
| `change_format` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `format` | 0 次（22 档） | **有**（出现在 3 个类目） |
| `hidden` | 0 次（22 档） | **有**（出现在 2 个类目） |
| `max` | 0 次（22 档） | **有**（出现在 25 个类目） |
| `min` | 0 次（22 档） | **有**（出现在 19 个类目） |
| `monthly_change` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `monthly_change_hidden` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `start` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `variables` | 0 次（22 档） | **有**（出现在 1 个类目） |
| `warning_string_key` | 0 次（22 档） | 全库也没有 → 疑似废弃字段 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `legend_key` | 79 | 21 |
| `outcome` | 60 | 22 |
| `content_trigger` | 1 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 531 | ordered_international_organization_member、every_pop、if、every_market_in_world |
| `if` | 271 | on_ended、international_organization:japanese_shogunate.leader_country、if、location:wittenberg |
| `value` | 223 | add、if、supporters、order |
| `else_if` | 182 | if、the_revolution、secondary_map_color、outcome |
| `OR` | 162 | OR、NOT、hidden_trigger、any_current_war |
| `trigger_event_non_silently` | 155 | on_ended、c:ENG、if、immediate |
| `NOT` | 151 | any_neighbor_country、custom_tooltip、international_organization:hre.leader_country、any_owned_location |
| `custom_tooltip` | 150 | if、NOT、the_revolution、trigger |
| `name` | 145 | remove_list_global_variable、change_local_variable、set_local_variable、is_target_in_global_variable_list |
| `has_owner` | 116 | if、any_ownable_location_in_area、legend_key、else_if |
| `desc` | 114 | add_country_modifier、legend_key、outcome、? |
| `set_variable` | 105 | if、every_country_in_religion、c:DLH、situation:treaty_of_tordesillas.var:var_east_country |
| `trigger` | 87 | switch、international_organization:japanese_shogunate、?、outcome |
| `AND` | 80 | AND、any_foreign_building_countries_in_location、NOT、trigger_if |
| `color` | 79 | legend_key、? |
