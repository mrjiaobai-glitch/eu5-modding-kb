# common/disasters（灾难）

> **一句话**：灾难字段：每月生成概率、开始与结束条件、进行中的国家修正与按月效果，并附防重复标记的注意事项。
> **什么时候看**：写灾难，或排查灾难反复触发与概率写法时看。
> **体量**：78 行 · 约 4 分钟通读

来源：`in_game\common\disasters\readme.txt`

## 字段

```
<disaster_id> = {
    custom_description = <string>      # customizable_localization 中的自定义描述键
    monthly_spawn_chance = <script value>  # 每月生成概率（0..1）；root = country, scope:disaster = 灾难
    modifier = <modifier>              # 灾难进行中施加给国家的修正
    can_start = <trigger>              # 能否开始（root = country, scope:disaster = 灾难类型）
    can_end = <trigger>                # 是否应结束（root = country, scope:disaster）
    on_start = <effect>                # 开始时（root = country, scope:disaster）
    on_monthly = <effect>              # 进行中每月（root = country, scope:disaster）
    on_end = <effect>                  # 结束时（root = country, scope:disaster）
    map_mode = <map mode tag>          # 可选；查看该灾难时显示的地图模式
    fire_only_once = <yes/no>          # 同一国家是否只能触发一次
}
```

## 审查要点

- 防重复标记：`fire_only_once = no` 的灾难若用变量/flag 防重复（如 `can_start` 里 `NOT { xxx_resolved = yes }`），**不得在 `on_end` 里移除该标记**——否则灾难无限重复（实测教训，见 SKILL.md 第 8 节）。
- `monthly_spawn_chance` 是 script value（0..1），不是字面整数概率。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\disasters\` 全量 **36 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `image` | 36 | 36 | "gfx/interface/illustrations/disaster/decline_of_mali_disaster.dds"（2）、"gfx/interface/illustrations/disaster/time_of_troubles.dds"（2）、"gfx/interface/illustrations/disaster/death_of_hayan_wuruk.dds"（2）、"gfx/interface/illustrations/disaster/succession_crisis.dds"（1）、"gfx/interface/illustrations/disaster/struggle_for_royal_power.dds"（1） |
| `content_priority` | 2 | 2 | 700（1）、800（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`monthly_spawn_chance`**（7 种）：monthly_spawn_chance_very_high（13）、monthly_spawn_chance_unique（8）、monthly_spawn_chance_low（7）、monthly_spawn_chance_very_low（4）、monthly_spawn_chance_medium（1）、monthly_spawn_chance_ultimate（1）、monthly_spawn_chance_ultimate_high（1）
- **`fire_only_once`**（1 种）：yes（14）
- **`content_priority`**（2 种）：700（1）、800（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `custom_description` | 0 次（36 档） | **有**（写在别的类目） |
| `map_mode` | 0 次（36 档） | **有**（写在别的类目） |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 100 | random_enemy、every_character、random_rebel、every_rebel |
| `random_list` | 78 | on_monthly、hidden_effect、if、show_as_tooltip |
| `trigger_event_non_silently` | 75 | hidden_effect、if、on_start、on_end |
| `trigger_event_silently` | 75 | time_of_troubles、?、hidden_effect、custom_tooltip |
| `set_variable` | 63 | on_monthly、random、hidden_effect、if |
| `NOT` | 63 | hidden_trigger、any_enemy、can_start、any_neighbor_country |
| `if` | 57 | dynasty:lancaster_dynasty、?、c:MAJ、dynasty:york_dynasty |
| `OR` | 53 | any_character、?、castilian_civil_war、custom_tooltip |
| `remove_variable` | 48 | ?、every_character、on_end、if |
| `value` | 47 | divide、can_start、set_variable、add |
| `name` | 39 | set_variable、change_variable、create_rebel |
| `hidden_effect` | 37 | on_monthly、?、on_start、on_end |
| `has_any_active_disaster` | 35 | OR、can_start |
| `add` | 30 | divide、can_start、if、value |
| `custom_tooltip` | 30 | ?、random_list、can_start、court_and_country |
