# common/country_ranks（国家等级）

来源：`in_game\common\country_ranks\readme.txt`

## 字段

```
<rank_id> = {
    color = <地图颜色>
    rank_modifier = <modifier>     # 施加给国家的修正
    level = <int>
    ai_level = <int>               # 默认 1，取值 1–4（更高按 4 处理）；限制外交 AI 交互范围；在 vanilla 等级间插入新等级时设为与 vanilla 大致相当的值
    allow = <trigger>              # 国家是否允许升到该等级
    language_power_scale = <float> # 该国宫廷语言获得语言力量的倍率
    character_ai_cooldown = <float>  # 默认 1；修改角色交互检查频率
    diplomacy_ai_cooldown = <float>  # 默认 1；修改国家交互检查频率
}
```

## 审查要点

- `ai_level` 取值 1–4（超出按 4 处理）；新等级需给合理 ai_level 否则影响 AI 外交范围。
- 未在 readme 中说明：本地化键格式。
