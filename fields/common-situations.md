# common/situations（局势）

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
