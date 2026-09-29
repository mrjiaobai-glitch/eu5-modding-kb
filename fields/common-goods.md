# common/goods（商品）

> **一句话**：商品字段：奴隶与通胀标记、基础产量与食物、运输成本与市场默认价、类别与方法枚举，以及按人群类型的需求增减。
> **什么时候看**：加新商品、调市场默认价与食物产出，或排查启动报错时翻这篇。
> **体量**：37 行 · 约 2 分钟通读

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
