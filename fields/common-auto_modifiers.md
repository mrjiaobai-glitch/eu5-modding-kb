# common/auto_modifiers（自动修正）

来源：`in_game\common\auto_modifiers\readme.txt`

## 属性

```
<auto_modifier_id> = {
    category = <修正类别>       # 默认 country；若设置必须在任何 modifiers 之前
    type = <作用域类型>         # potential_trigger 的作用域类型；默认 country；若设置必须在 potential_trigger 之前
    icon = <icon 路径>
    requires_real = <yes/no>    # 是否要求真实国家
    potential_trigger = <trigger>   # 是否应用该自动修正
    scales_with = <script value>    # 评估一个值来乘自动修正效果
    limit = <trigger>           # 检查何时可以应用
    hide_effects = <yes/no>     # 是否隐藏修正
    alert = <yes/no>            # 激活时是否在警报中显示
    <modifiers>                 # 其余全是修正
}
```

## 审查要点

- `category`/`type` 必须放在 modifiers 与 potential_trigger **之前**（顺序约束，readme 明示）。
- 本地化键格式在 readme 中未说明；按实测经验为 `AUTO_MODIFIER_NAME_<名>`（SKILL.md 第 6 节有详细规则）。
- 未在 readme 中说明：`type` 的合法取值集合。
