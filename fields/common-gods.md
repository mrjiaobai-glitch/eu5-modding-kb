# common/gods（神祇）

> **一句话**：神祇字段：两种宗教挂载写法、可用门槛、生效时长与三类缩放修正，以及性别显示与增删神祇的效果。
> **什么时候看**：加神祇、配宗教挂载与名字键，或排查神没有名字时翻这篇。
> **体量**：99 行 · 约 5 分钟通读

来源：`in_game\common\gods\readme.txt`

## 字段

```
<god_id> = {
    religion = <religion key>        # 指定神适用的多个宗教/宗教组；两种格式：
    group = <group key>              #   格式1: religion = { religion = <键> name_key = <该宗教下神的名字 loc 键> }
                                     #   格式2: religion = <键>（名字就是神 id tag 本身时）
    potential = <trigger>            # root = country
    allow = <trigger>                # root = country
    years / months / weeks / days = <int>  # 完全生效时间；修正按比例缩放
    on_activate = <effect>           # root = country
    on_fully_activated = <effect>    # 100% 时
    on_deactivate = <effect>         # 移除时（root = country）
    country_modifier / province_modifier / location_modifier = <scaled & triggered modifier>
    is_female = <yes/no>             # UI 中显示 God 还是 Goddess；脚本用 is_god_female trigger 判断
}
```

## 效果

- `add_god = <god id>`、`remove_god = <god id>`

## 审查要点

- `religion`/`group` 两种写法（块形式带 name_key；裸键形式名字即 id）——块形式漏 name_key 则无名字。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\gods\` 全量 **11 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `icon` | 95 | 7 | god_folk_blue（17）、god_folk_green（17）、god_folk_brown（9）、god_folk_dark_blue（9）、god_folk_black（7） |
| `ability` | 6 | 1 | ADM（2）、DIP（2）、MIL（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`is_female`**（1 种）：yes（25）
- **`ability`**（3 种）：ADM（2）、DIP（2）、MIL（2）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `add_god` | 0 次（11 档） | **有**（写在别的类目） |
| `allow` | 0 次（11 档） | **有**（写在别的类目） |
| `days` | 0 次（11 档） | **有**（写在别的类目） |
| `location_modifier` | 0 次（11 档） | **有**（写在别的类目） |
| `months` | 0 次（11 档） | **有**（写在别的类目） |
| `name_key` | 0 次（11 档） | **有**（写在别的类目） |
| `on_activate` | 0 次（11 档） | **有**（写在别的类目） |
| `on_deactivate` | 0 次（11 档） | **有**（写在别的类目） |
| `on_fully_activated` | 0 次（11 档） | **有**（写在别的类目） |
| `OR` | 0 次（11 档） | **有**（写在别的类目） |
| `province_modifier` | 0 次（11 档） | **有**（写在别的类目） |
| `remove_god` | 0 次（11 档） | **有**（写在别的类目） |
| `weeks` | 0 次（11 档） | 全库也没有 → 疑似废弃字段 |
| `years` | 0 次（11 档） | **有**（写在别的类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `omens` | 6 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `scale` | 60 | country_modifier |
| `god` | 60 | president_of_games_omen、mother_of_love_omen、the_seductress_omen、the_desired_omen |
| `potential` | 60 | president_of_games_omen、mother_of_love_omen、the_seductress_omen、the_desired_omen |
| `add` | 60 | scale |
| `value` | 60 | scale |
| `country_modifier` | 60 | president_of_games_omen、mother_of_love_omen、the_seductress_omen、the_desired_omen |
| `name_key` | 50 | group、religion |
| `religion` | 47 | religion |
| `stability_cost_efficiency` | 29 | country_modifier |
| `global_population_growth` | 26 | country_modifier |
| `global_life_expectancy` | 21 | country_modifier |
| `global_monthly_prosperity` | 20 | country_modifier |
| `global_hostile_attrition` | 18 | country_modifier |
| `NOT` | 15 | potential_trigger、custom_tooltip |
| `smartism_balanced_gods` | 15 | potential_trigger、custom_tooltip、AND |
