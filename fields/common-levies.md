# common/levies（征召部队）

来源：`in_game\common\levies\readme.txt`

## 字段

```
<levy_id> = {
    unit = <unit_type>
    size = <script value>
    country_allow = { <triggers> }   # 作用域：country
    allow = { <triggers> }           # 作用域：pops
    allow_as_crew = { <triggers> }   # 作用域：pops
    allowed_pop_type = <pop type>    # 可多个；不写则所有 pop 类型可用
    allowed_culture = <culture_definition>  # 可多个；不写则所有文化可用
}
```

## 重要排序约束（readme 原文强调）

- 新增征召单位类型时，**条件多的高度特化单位必须放在文件最顶，通用单位放底部**——否则游戏会用第一个找到的征召单位填满所有可用的征召槽。

## 审查要点

- `unit` 引用须在 common/unit_types 存在。
- 顺序约束是审查重点：特化 → 通用。
- 未在 readme 中说明：本地化键格式。
