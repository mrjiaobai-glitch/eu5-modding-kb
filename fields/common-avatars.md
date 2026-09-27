# common/avatars（神祇化身）

来源：`in_game\common\avatars\readme.txt`

## 块格式

```
<avatar_id> = {
    god = <god key>                     # 该化身属于哪个神
    potential = { <trigger> }           # 行动是否可能；root = country
    allow = { <trigger> }               # 行动能否开始；root = country
    years = <int>
    months = <int>
    weeks = <int>
    days = <int>                        # 以上四者定义完全生效所需时间；修正按完成比例缩放
    on_activate = { <effect> }          # 选择该行动时触发；root = country
    on_fully_activated = { <effect> }   # 实现达到 100% 时触发（无时间延迟则立即）
    on_deactivate = { <effect> }        # 行动被移除时触发；root = country
    country_modifier = <scaled & triggered modifier>   # scale = 缩放脚本, potential_trigger = 是否应用；施加于整个国家
    province_modifier = <scaled & triggered modifier>  # 施加于省份
    location_modifier = <scaled & triggered modifier>  # 施加于 location
}
```

## 效果

- `add_avatar = <avatar id>` 为国家添加化身；`remove_avatar = <avatar id>` 移除。

## 审查要点

- `god` 引用须在 common/gods 中存在。
- 三个 modifier 的作用域不同（country/province/location），内容须与目标作用域匹配。
- 时间字段（years/months/weeks/days）均为整数；可组合使用。
- 未在 readme 中说明：本地化键格式。
