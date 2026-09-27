# common/scripted_diplomatic_objectives（脚本化外交目标）

来源：`in_game\common\scripted_diplomatic_objectives\readme.txt`

## 字段

```
<name> = {
    ai_tick_frequency = <int/script value>   # 重新检查新目标前等待的天数
    actor_trigger = { }                      # 根作用域 = actor
    recipient_trigger = { }                  # scope:recipient = 潜在国家, scope:actor = actor
    recipient_priority = <float/scripted value>  # 排序用；scope:recipient, scope:actor
    recipient_list_builder = { <list builder> }  # 收窄潜在国家；缺省用所有可达国家
    pause_trigger = { }                      # 暂停目标直到下次检查（避免在 cancel_trigger 删除）；scope:recipient, scope:actor
    cancel_trigger = { }                     # 提前取消目标；scope:recipient, scope:actor
    improve_relation = <yes/no>              # 是否改善关系
    improve_relation_limit = <int/scripted value>  # 可选；改善到该意见值；缺省到最大
    defensive_support = <yes/no>             # 是否防守 recipient
    antagonise = <yes/no>                    # 是否对 recipient 使用敌对交互
    destroy = <yes/no>                       # 是否试图摧毁 recipient
    country_interactions = { <country interaction> = <yes/no> }  # yes 鼓励 / no 禁用
    country_relations = { <country relation> = <yes/no> }        # yes 鼓励 / no 禁用并尽量取消
    time_limit = <days/scripted value>       # 完成期限；留空或 0 = 无限；scope:recipient, scope:actor
    max_allowed = <int/scripted value>       # 同类型目标同时数量；0 = 无限（默认）；根作用域 = actor
}
```

## 审查要点

- `country_interactions`/`country_relations` 引用的行动/关系名须存在。
- 未在 readme 中说明：本地化键格式。
