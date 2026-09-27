# common/join_war_rules（参战规则）

来源：`in_game\common\join_war_rules\readme.txt`

## 字段

```
<rule_id> = {
    join_war_disabled_trigger = <trigger>
    # root = 参战国；scope:war = 正在加入的战争；scope:first_leader = 该国加入方的领袖；scope:second_leader = 该国对抗方的领袖
}
```

## 本地化（readme 声明）

- 规则键本身用于本地化：`<rule_id>: "Example"`，可用预设 `COUNTRY`、`OUR_LEADER_COUNTRY`、`ENEMY_LEADER_COUNTRY`、`WAR`。

## 审查要点

- trigger 作用域固定（root/scope:war/scope:first_leader/scope:second_leader）。
- 未在 readme 中说明：无。
