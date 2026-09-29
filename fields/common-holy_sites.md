# common/holy_site_types 与 common/holy_sites（圣地）

> **一句话**：圣地两档：圣地类型的三个作用域修正与缩放规则，圣地本体的地点、类型、重要度、宗教列表及可选的神祇化身关联。
> **什么时候看**：加圣地，或核对类型、神祇、化身等引用与重要度取值时看。
> **体量**：38 行 · 约 2 分钟通读

覆盖 readme：`in_game\common\holy_site_types\readme.txt`、`in_game\common\holy_sites\readme.txt`

## holy_site_types

```
<type_key> = {
    country_modifier = <modifier>    # 施加于圣地所有者国家
    location_modifier = <modifier>   # 施加于 dominant_religion 的 location；按 importance 缩放
    religion_modifier = <modifier>   # 施加于控制圣地的宗教（若该圣地对该宗教重要）；location 主导宗教占 50% + 所有者宗教占 50%
}
```

## holy_sites

```
<holy_site_id> = {
    location = <location key>        # 圣地所在 location
    type = <type key>                # 类型（见 holy_site_types）
    importance = <integer>           # 对宗教的重要性（1–5）
    religions = { <religion id> ... }  # 认为该地神圣的宗教列表
    god = <god key>                  # 可选；关联神祇
    avatar = <avatar key>            # 可选；关联化身
}
```

## 审查要点

- `type` 引用须在 holy_site_types 存在；`god`/`avatar` 引用须在对应类目存在。
- `importance` 取值 1–5。
- `location` 引用须为真实 location 键。
- 未在 readme 中说明：本地化键格式。
