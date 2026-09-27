# common/wargoals（战争目标）

来源：`in_game\common\wargoals\readme.txt`

## 内置 type 枚举

take_province、superiority（征服成本 < 0 时 AI 总是选择）、naval_superiority、defend_capital、enforce_military_access、independence、take_capital、take_border、take_country

## 字段

```
<war_goal_id> = {
    type = <type>
    war_name = "<war_name_loc_key>"      # 独立 loc 键
    war_name_is_country_order_agnostic = <yes/no>  # 默认 no；yes 则英对法 = 法对英同名
    allow = { <triggers> }
    attacker = {
        call_in_overlord = <yes/no>
        call_in_subjects = <yes/no>
        conquer_cost = <float>       # 和约割地战争分数成本的因子
        subjugate_cost = <float>     # 使目标成为附庸的成本因子
        release_cost = <float>       # 使目标释放国家/地区的成本因子
        antagonism = <float>         # 整个和约的敌意因子
        allowed_locations = { <triggers> }   # 允许攻方拿的 location；scope:loser/scope:winner/scope:war/scope:location
        allowed_subjugation = { <triggers> } # 允许攻方附庸守方；scope:loser/scope:winner/scope:war
    }
    defender = { 同上四 cost 字段 + allowed_locations/allowed_subjugation }
    ticking_war_score = <float>      # 默认 1
}
```

## 审查要点

- `type` 必须是上方内置枚举之一。
- `war_name` 是 loc 键（如 EXPEL_LANDLESS_WAR_NAME），须存在于本地化。
- 未在 readme 中说明：无。
