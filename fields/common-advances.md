# common/advances（科技/进步）

来源：`in_game\common\advances\readme.txt`

## 块格式

```
<advance ID> = {
    age = <age>                     # 该 advance 在哪个时代可用
    icon = <icon>                   # 图标
    requires = <advance>            # 放置在该 advance 之后
    government = <government_type>  # 仅指定政体可用
    country_type = <location/pop/building/army>  # 仅指定国家类型可用
    allow = { <triggers> }          # 满足才可研究
    potential = { <triggers> }      # 满足才显示；注意：advances 不会追溯可见（研究前不满足就永远不显示）
    for = <adm/dip/mil>             # 仅对时代开始时选定该专精的国家显示
    unlock_unit = <unit>
    unlock_ability = <ability>
    unlock_interaction = <interaction>                    # 角色交互
    unlock_country_interaction = <country_interaction>
    unlock_relation_type = <relation_type>
    unlock_building = <building>
    unlock_law = <law>
    unlock_levy = <levy>
    unlock_government_reform = <government_reform>
    unlock_casus_belli = <casus_belli>
    unlock_subject_type = <subject_type>
    unlock_production_method = <production_method>
    allow_children = <yes/no>       # 强制该 advance 为子节点；违反会产生 error log 输出
    <modifiers>                     # 研究完成后获得的修正
    modifier_while_progressing = {
        potential_trigger = <trigger>
        scale = <maths>
        <modifiers>
    }                               # 研究期间满足 potential_trigger 时应用的比例修正
}
```

## 审查要点

- `unlock_*` 引用的目标（unit/ability/interaction/law/levy/reform/cb/subject_type/production_method）须在对应类目中存在。
- `potential` 不满足的 advance 不会因后续条件满足而出现——这是设计语义，不是 bug。
- 子节点约束：`allow_children = yes` 的 advance 若被其他 advance `requires` 引用不符合预期会报错。
- 未在 readme 中说明：本地化键格式、advance ID 命名规则。
