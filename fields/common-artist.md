# common/artist_types 与 common/artist_work

> **一句话**：艺术家两档字段：学科门类的门槛与宫廷修正，艺术品类型的夺取开关、三类 modifier 与按角色作用域的可用条件。
> **什么时候看**：做艺术家与艺术品内容，或核对修正作用域与本地化前缀时看。
> **体量**：98 行 · 约 5 分钟通读

来源：`in_game\common\artist_types\readme.txt`、`in_game\common\artist_work\readme.txt`

## artist_types（艺术家学科门类）

```
<artist_type_id> = {
    potential = { <triggers> }   # 国家作用域 trigger；省略则所有国家可用（用于按文化/宗教/advance 门槛）
    modifier = { ... }           # 艺术家任职宫廷期间应用的国家修正；作为角色修正加在艺术家身上，死亡/解雇时移除；支持全部标准国家修正字段
}
```

- 本地化键：`ARTIST_TYPE_NAME_<id>`（名称）、`ARTIST_TYPE_DESC_<id>`（说明）。
- 一个国家可同时雇用多种门类的艺术家；同门类修正叠加。
- 作品亲和性（艺术家偏好哪类作品）由 **artist_work 侧**的 `allow = { artist_type = <id> }` 定义，不在这里。
- `dismiss_artist` 角色交互把艺术家从宫廷移除（代价：威望招募）。

## artist_work（艺术品类型）

```
<work_of_art_type> = {
    captured = <yes/no>                    # 能否被夺取
    allow = { <trigger> }                  # 是否允许该类型；root = character
    location_modifier = <modifier>         # 施加于艺术品所在 location
    country_modifier = <modifier>          # 施加于艺术品所有者
    religion_scale_modifier = <modifier>   # 施加于整个宗教
}
```

## 审查要点

- artist_types 的 `modifier` 是"角色修正"（施加于艺术家角色），artist_work 的三类 modifier 作用域不同（location/country/religion）——按目标检查作用域与键。
- artist_work 的 `allow` 中 root = character（不是 country）。
- 本地化前缀 `ARTIST_TYPE_NAME_`/`ARTIST_TYPE_DESC_` 缺失会显示 raw key。
- 未在 readme 中说明：artist_work 的本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\artist\` 全量 **2 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `woa_influence_multiplier` | 22 | 1 | 1.0（3）、0.8（3）、0.5（2）、0.3（2）、0.4（2） |
| `woa_age_influence_per_year` | 22 | 1 | 0（22） |
| `woa_type_worth` | 22 | 1 | -1（5）、500（2）、400（1）、250（1）、80（1） |
| `woa_age_gold_per_year` | 22 | 1 | 0（5）、2（3）、6（3）、3（3）、4（2） |
| `woa_age_tradition_per_year` | 22 | 1 | 0（22） |
| `woa_tradition_multiplier` | 22 | 1 | 2.0（4）、1.0（3）、0.3（2）、1.3（2）、0.4（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`woa_influence_multiplier`**（15 种）：1.0（3）、0.8（3）、0.5（2）、0.3（2）、0.4（2）、0.7（1）、2.5（1）、0.6（1）、1.2（1）、0.2（1）、1.4（1）、2.0（1）、1.8（1）、1.5（1）、1.6（1）
- **`woa_age_influence_per_year`**（1 种）：0（22）
- **`captured`**（2 种）：yes（16）、no（6）
- **`woa_age_gold_per_year`**（9 种）：0（5）、2（3）、6（3）、3（3）、4（2）、5（2）、1（2）、7（1）、8（1）
- **`woa_age_tradition_per_year`**（1 种）：0（22）
- **`woa_tradition_multiplier`**（13 种）：2.0（4）、1.0（3）、0.3（2）、1.3（2）、0.4（2）、0.8（2）、0.5（1）、0.7（1）、2.5（1）、0.6（1）、1.2（1）、1.4（1）、1.8（1）
- **`religion_scale_modifier`**（1 种）：religious_icon_power_modifier（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `A` | 0 次（2 档） | 全库也没有 → 疑似废弃字段 |
| `country_modifier` | 0 次（2 档） | **有**（出现在 14 个类目） |
| `The` | 0 次（2 档） | 全库也没有 → 疑似废弃字段 |
| `Which` | 0 次（2 档） | 全库也没有 → 疑似废弃字段 |
| `Works` | 0 次（2 档） | 全库也没有 → 疑似废弃字段 |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `artist_type` | 28 | OR、allow |
| `OR` | 10 | location、potential、allow |
| `court_language` | 9 | OR |
| `religion` | 7 | OR |
| `current_age` | 6 | OR、NOR |
| `has_advance` | 3 | OR、potential |
| `always` | 2 | allow |
| `has_building_with_at_least_one_level` | 2 | OR |
| `legislative_efficiency` | 1 | modifier |
| `change_policy_cost_modifier` | 1 | modifier |
| `stability_cost_efficiency` | 1 | modifier |
| `monthly_tribal_cohesion` | 1 | modifier |
| `global_disease_resistance` | 1 | modifier |
| `global_estate_target_satisfaction` | 1 | modifier |
| `monthly_horde_unity` | 1 | modifier |
