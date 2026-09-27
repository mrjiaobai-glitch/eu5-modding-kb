# common/religious_aspects、religious_factions、religious_focuses（宗教三件套）

覆盖 readme：`in_game\common\religious_aspects\readme.txt`、`in_game\common\religious_factions\readme.txt`、`in_game\common\religious_focuses\readme.txt`

## religious_aspects（宗教面相）

```
<religious_aspect_id> = {
    religion = <religion>      # 可多个；把该面相加给宗教
    visible = { <triggers> }   # 使面相可见的触发；作用域 = 使用它的国家
    enabled = { <triggers> }   # 使面相可用的触发；作用域 = 使用它的国家
    modifier = { <modifier> }  # 国家修正
}
```

## religious_factions（宗教派系）

```
<religious faction id> = {
    visible = { <io triggers> }
    enabled = { <io triggers> }
    actions = {
        <generic actions>      # 引用 common/generic_actions
    }
}
```

## religious_focuses（宗教焦点）

- 类似 advance：需要像 advance 一样研究，研究期间与完成后都给修正。
- 通过宗教定义的 `religious_focuses` 属性加入宗教（例如 `religious_focuses = { adopt_ometeotl ... }`）。

```
<focus_id> = {
    potential = <trigger>          # 是否在 UI 可见（root = country）
    allow = <trigger>              # 是否可用（root = country）
    monthly_progress = <script value>  # 每月研究进度（root = country）
    modifier_while_progressing = <modifier>  # 研究期间修正
    modifier_on_completion = <modifier>      # 完成后修正
    effect_on_completion = <effect>          # 完成后执行（root = country）
}
```

## 审查要点

- religious_factions 的 trigger 是 **IO 作用域**；aspects/focuses 是 country 作用域。
- factions 的 `actions` 引用须在 common/generic_actions 存在。
- focuses 通过宗教的 `religious_focuses` 属性挂接——只定义 focus 但不挂进任何宗教等于无效。
- 未在 readme 中说明：本地化键格式。
