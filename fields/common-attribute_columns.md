# common/attribute_columns（属性列）

来源：`in_game\common\attribute_columns\readme.txt`

## 用途

为不同对象类型指定"列"的显示方式，供 generic actions / country / character interactions 中的 `select_trigger` 使用。

```
<column_tag> = {
    widget = <gui widget 类型>        # 链接 gui 文件中的 widget；widget 会被给予 InteractionTarget 以取数据（InteractionTarget.GetCountry / GetCharacter 等）
    width = <默认列宽>
    fixed_height = <widget 高度>       # 固定高度，大量条目显示时可优化性能
    is_constant_width = <yes/no>      # 默认 yes；设为 no 让该列吸收剩余宽度
    contains_select_target_button = <yes/no>  # 若 widget 已含 select_target_button 则设 yes，避免重复创建
    single_widget_for_row = <yes/no>  # 整行用一个 widget 时设 yes，多指定了列会报错
    sort = {
        sort_key = <唯一排序键>
        sort_text = <scripted text>   # 文本列排序用；可用全部作用域变量（root = 被选对象，另有 scope:actor/scope:recipient/scope:target 等）
        sort_value = <scripted value> # 数值列排序用；作用域同上
        # sort_text 与 sort_value 二选一（文本列用前者、数值列用后者）
        sort_by_tooltip_key = <排序头 tooltip 文本>
    }
}
```

## 审查要点

- 除列定义外还需提供名称与描述字符串：名称 tag = `<action_tag>`、描述 tag = `<action_tag>_desc`（action_tag 指使用侧的动作）。
- 列 tag 必须唯一。
- 每个 sort 头需要 `sort_key` + 且仅一个 `sort_text`（文本）或 `sort_value`（数值）。
- 未在 readme 中说明：其他字段。
