# common/formable_countries（可成立国家）

来源：`in_game\common\formable_countries\readme.txt`

## 字段

```
<formable_id> = {
    level = <int>                    # 国家等级；玩家/AI 只能成立更高等级的国家
    required_locations_fraction = <float>  # 需要的 location 比例；默认 1.0（100%）
    capital_required = <yes/no>      # 必需地点列表是否必须包含当前首都；默认 no
    potential_requires_own = <yes/no>  # 是否必须拥有列表中的 location 才能在列表看到；默认 yes
    rule = <historical/plausible/fantasy>  # 游戏规则中"多离谱"的筛选；默认 historical
    potential = { <trigger> }        # 能否看到按钮；root = 当前国家
    allow = { <trigger> }            # 能否点按钮；root = 当前国家
    form_effect = { <effect> }       # 点击成立时发生；root = 当前国家
    name = <string>
    flag = <string>
    adjective = <string>
    tag = <string>
    color = <map color>              # 若未设置则保留旧国家地图色
    continents = { ... }             # 必需大陆列表
    sub_continents = { ... }
    regions = { ... }
    areas = { ... }
    locations = { ... }
}
```

## 审查要点

- `rule` 枚举：historical/plausible/fantasy。
- `name`/`flag`/`adjective`/`tag` 四者齐备；adjective 通常是 loc 键形式（如 EXA_ADJ）。
- 未在 readme 中说明：本地化键格式。
