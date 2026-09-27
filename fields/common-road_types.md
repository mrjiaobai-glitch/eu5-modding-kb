# common/road_types（道路类型）

来源：`in_game\common\road_types\readme.txt`

## 字段

```
<road type id> = {
    level = <integer>                    # 道路等级；用于 has_latest_road_to 触发；一般越高越晚
    pop_movement = <double>              # pop 沿路移动的修正（影响 movements、diseases）
    enabled = { <trigger> }              # 只有 trigger 为真才能建；root = 试图建路的国家
    proximity = <integer>                # 对 location 的"到首都距离"效果的缩减
    movement_cost = <double>             # 修改单位在含该道路的 location 上的移动速度
    build_time_per_unit_distance = <integer>  # 两 location 间修路耗时
    price_per_unit_distance = <price>    # 两 location 间修路费用
    construction_demand = <goods demand> # 修路需要的商品
    color = <named color>                # 道路地图模式中的颜色
}
```

## 审查要点

- `enabled` 的 root 是国家（建造者）。
- 未在 readme 中说明：本地化键格式。
