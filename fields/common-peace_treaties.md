# common/peace_treaties（和平条约）

来源：`in_game\common\peace_treaties\readme.txt`

## 字段

```
<peace_treaty_id> = {
    potential = <trigger>   # scope:winner = taker, scope:loser = giver, scope:war, scope:target = location/country/province
    allow = <trigger>       # 同上
    effect = <effect>       # 执行条约时；同上
    blocks_full_annexation = yes     # 使用该条约时目标不能被完全吞并
    collate_targets = <yes/no>       # 目标能否从所有给方合并（即条约是否与给方无关）
    are_targets_exclusive = yes      # location/province 目标排他，不能与割地条约组合
    category = <country/location/province/area>  # 把脚本化条约放进其他类别
    custom_tags = { <strings> }
    show_tags_in_ui = <yes/no>
    select_trigger = { ... }         # 只能加一个；格式同 character_interactions 的 select_trigger；存入 scope:target
    cost = <script value>            # 战争分数成本；scope:winner/loser/war/target
    base_antagonism = <script value> # 最大敌意获取量，按国调整
    antagonism_type = <bias type key>
    ai_desire = <script value>       # 胜利方想要该条约的程度
    ai_force_add = <yes/no>          # AI 是否尽可能把该条约加进提案
}
```

## 审查要点

- potential/allow/effect/cost/ai_desire 的固定作用域：winner/loser/war/target。
- `select_trigger` 只能一个。
- `antagonism_type` 引用须存在。
- 未在 readme 中说明：本地化键格式。
