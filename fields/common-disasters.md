# common/disasters（灾难）

来源：`in_game\common\disasters\readme.txt`

## 字段

```
<disaster_id> = {
    custom_description = <string>      # customizable_localization 中的自定义描述键
    monthly_spawn_chance = <script value>  # 每月生成概率（0..1）；root = country, scope:disaster = 灾难
    modifier = <modifier>              # 灾难进行中施加给国家的修正
    can_start = <trigger>              # 能否开始（root = country, scope:disaster = 灾难类型）
    can_end = <trigger>                # 是否应结束（root = country, scope:disaster）
    on_start = <effect>                # 开始时（root = country, scope:disaster）
    on_monthly = <effect>              # 进行中每月（root = country, scope:disaster）
    on_end = <effect>                  # 结束时（root = country, scope:disaster）
    map_mode = <map mode tag>          # 可选；查看该灾难时显示的地图模式
    fire_only_once = <yes/no>          # 同一国家是否只能触发一次
}
```

## 审查要点

- 防重复标记：`fire_only_once = no` 的灾难若用变量/flag 防重复（如 `can_start` 里 `NOT { xxx_resolved = yes }`），**不得在 `on_end` 里移除该标记**——否则灾难无限重复（实测教训，见 SKILL.md 第 8 节）。
- `monthly_spawn_chance` 是 script value（0..1），不是字面整数概率。
- 未在 readme 中说明：本地化键格式。
