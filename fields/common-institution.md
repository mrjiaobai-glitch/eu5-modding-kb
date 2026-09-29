# common/institution（制度）

> **一句话**：制度字段：所属时代、生成条件，以及从海岸、边境、进口、市场与首都等各条渠道的制度传播速度脚本值。
> **什么时候看**：调制度生成与传播速度，或核对生成条件作用域时翻这篇。
> **体量**：32 行 · 约 2 分钟通读

来源：`in_game\common\institution\readme.txt`

## 字段

```
<institution_id> = {
    age = <age_id>
    can_spawn = { <triggers> }          # root = location 作用域
    promote_chance = <scripted value>
    spread_from_friendly_coast_border_location = <scripted value>
    spread_from_any_coast_border_location = <scripted value>
    spread_from_any_import = <scripted value>
    spread_scale_on_control_if_owner_embraced = <scripted value>
    spread_embraced_to_capital = <scripted value>
    spread_to_market_member = <scripted value>
    spread_to_market_center = <scripted value>
    spread = <scripted value>           # 制度传播速度
}
```

## 审查要点

- `can_spawn` 是 location 作用域 trigger（root = location）。
- 其余传播相关均为 scripted value。
- 未在 readme 中说明：本地化键格式。
