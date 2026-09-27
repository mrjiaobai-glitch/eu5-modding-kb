# common/generic_action_ai_lists（通用行动的 AI 列表）

来源：`in_game\common\generic_action_ai_lists\readme.txt`

## 用途

把一批 generic actions 分组给 AI，使其能一次性评估"其中任一是否可能"，优化 AI 评估性能。

## 字段

```
<list_id> = {
    potential = <trigger>    # root = 评估行动的国家
    actions = <list>         # 属于该列表的全部行动
}
```

## 语义

- 一个 generic action 可同时属于多个列表；未给任何列表的行动进**全局列表**。
- 用于剔除只对部分国家有用的 generic actions（如百年战争等 situation 类）。

## 审查要点

- `actions` 引用的行动名须在 common/generic_actions 存在。
- 未在 readme 中说明：本地化键格式。
