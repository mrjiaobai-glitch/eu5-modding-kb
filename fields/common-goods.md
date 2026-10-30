# common/goods（商品）

> **一句话**：商品字段：奴隶与通胀标记、基础产量与食物、运输成本与市场默认价、类别与方法枚举，以及按人群类型的需求增减。
> **什么时候看**：加新商品、调市场默认价与食物产出，或排查启动报错时翻这篇。
> **体量**：124 行 · 约 6 分钟通读

来源：`in_game\common\goods\readme.txt`

## 字段

```
<goods id> = {
    is_slaves = <yes/no>                 # 默认 no；标记为奴隶商品
    block_rgo_upgrade = <yes/no>         # 默认 no；阻止该商品 rgo 扩张
    inflation = <yes/no>                 # 默认 no；生产时引发通货膨胀
    base_production = <float>            # 默认 0
    color = <color>                      # 地图模式中的颜色
    food = <float>                       # 默认 0；提供的食物量
    transport_cost = <float>             # 默认 1；运输成本
    default_market_price = <float>       # 默认 1；市场默认价
    category = <raw_material/produced>   # 默认 raw_material
    method = <mining/farming/hunting/gathering/forestry>  # 默认 farming
    ai_rgo_size_importance = <float>     # AI 避免在该 rgo 上建城的偏好
    demand_add = { all = <float> <pop types> = <float> }
    demand_multiply = { upper = <float> <pop types> = <float> }
    location_potential = { <location trigger> }  # setup 时若为 false 会出 error log
    custom_tags = { <strings> }
}
```

## 审查要点

- `category`/`method` 是枚举。
- `demand_add`/`demand_multiply` 中 pop 类型键引用须存在。
- `location_potential` 为 false 会在 setup 时报错——新商品必须保证其适用。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\goods\` 全量 **5 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `origin_in_old_world` | 16 | 3 | yes（16） |
| `development_threshold` | 9 | 2 | 20（7）、30（1）、40（1） |
| `ai_rgo_expansion_priority` | 7 | 2 | 0.025（4）、0.2（2）、0.25（1） |
| `no_demand_if_no_market_availability` | 6 | 1 | yes（6） |
| `demand_only_in_region` | 5 | 1 | nubia_region（1）、sahel_region（1）、western_india_region（1）、somalia_region（1）、indochina_region（1） |
| `origin_in_new_world` | 5 | 3 | yes（5） |
| `demand_only_in_sub_continent` | 4 | 1 | middle_east（1）、south_asia（1）、central_asia（1）、north_africa（1） |
| `literacy_demand_factor` | 2 | 1 | 1（2） |
| `market_unavailability_demand_modifier` | 1 | 1 | 0.5（1） |
| `max_rgo_size` | 1 | 1 | 0.5（1） |
| `disease_demand_modifier` | 1 | 1 | 2（1） |
| `demand_only_in_continent` | 1 | 1 | america（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`category`**（2 种）：raw_material（53）、produced（22）
- **`default_market_price`**（10 种）：3（22）、1（14）、2（11）、4（9）、5（9）、2.5（3）、1.5（2）、6（2）、0.5（2）、8（1）
- **`method`**（5 种）：farming（29）、mining（13）、gathering（7）、hunting（3）、forestry（1）
- **`transport_cost`**（7 种）：0.5（22）、2（9）、1（6）、3（2）、1.0（2）、5（1）、0.25（1）
- **`origin_in_old_world`**（1 种）：yes（16）
- **`ai_rgo_size_importance`**（5 种）：5（5）、3（4）、10（3）、1.5（2）、4（1）
- **`food`**（9 种）：4（2）、8.0（2）、8（2）、5（2）、5.0（1）、2.5（1）、3.5（1）、2.0（1）、10.0（1）
- **`development_threshold`**（3 种）：20（7）、30（1）、40（1）
- **`ai_rgo_expansion_priority`**（3 种）：0.025（4）、0.2（2）、0.25（1）
- **`no_demand_if_no_market_availability`**（1 种）：yes（6）
- **`base_production`**（4 种）：0.01（2）、0.02（1）、0.012（1）、0.0005（1）
- **`origin_in_new_world`**（1 种）：yes（5）
- **`demand_only_in_region`**（5 种）：nubia_region（1）、sahel_region（1）、western_india_region（1）、somalia_region（1）、indochina_region（1）
- **`demand_only_in_sub_continent`**（4 种）：middle_east（1）、south_asia（1）、central_asia（1）、north_africa（1）
- **`literacy_demand_factor`**（1 种）：1（2）
- **`demand_only_in_continent`**（1 种）：america（1）
- **`disease_demand_modifier`**（1 种）：2（1）
- **`max_rgo_size`**（1 种）：0.5（1）
- **`is_slaves`**（1 种）：yes（1）
- **`market_unavailability_demand_modifier`**（1 种）：0.5（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `all` | 0 次（5 档） | **有**（出现在 1 个类目） |
| `block_rgo_upgrade` | 0 次（5 档） | 全库也没有 → 疑似废弃字段 |
| `inflation` | 0 次（5 档） | **有**（出现在 1 个类目） |
| `upper` | 0 次（5 档） | **有**（出现在 2 个类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `wealth_impact_threshold` | 36 | 3 |
| `winter_demand_modifier` | 6 | 3 |
| `sub_continent_demand_modifier` | 5 | 2 |
| `region_demand_modifier` | 4 | 2 |
| `climate_demand_modifier` | 3 | 1 |
| `topography_demand_modifier` | 3 | 2 |
| `continent_demand_modifier` | 1 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `climate` | 73 | OR、NOR、location_potential |
| `vegetation` | 55 | OR、NOR、location_potential、NOT |
| `all` | 53 | demand_add、wealth_impact_threshold |
| `topography` | 50 | OR、NOR |
| `nobles` | 42 | demand_multiply、demand_add、wealth_impact_threshold |
| `OR` | 39 | location_potential |
| `area` | 37 | NOR、location_potential |
| `region` | 32 | NOR |
| `burghers` | 27 | demand_multiply、demand_add、wealth_impact_threshold |
| `tribesmen` | 21 | demand_multiply |
| `NOR` | 21 | location_potential |
| `clergy` | 20 | demand_multiply、demand_add |
| `upper` | 18 | demand_multiply、demand_add |
| `slaves` | 11 | demand_multiply、demand_add |
| `laborers` | 11 | demand_multiply、demand_add、wealth_impact_threshold |
