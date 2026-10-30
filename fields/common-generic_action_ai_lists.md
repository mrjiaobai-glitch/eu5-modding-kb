# common/generic_action_ai_lists（通用行动的 AI 列表）

> **一句话**：把通用行动分组给 AI 的列表：可见条件与行动清单两个字段，未入列表的行动进全局列表，用于缩小 AI 的评估范围。
> **什么时候看**：缩小 AI 对通用行动的评估范围，或核对行动清单引用时看。
> **体量**：55 行 · 约 3 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\generic_action_ai_lists\` 全量 **96 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `can_see_situation` | 21 | potential |
| `religion` | 21 | OR、potential |
| `has_active_disaster` | 17 | OR、potential |
| `is_member_of_international_organization_of_type` | 7 | potential |
| `exists` | 6 | potential、any_international_organizations_member_of |
| `government_type` | 5 | potential |
| `any_international_organizations_member_of` | 4 | potential |
| `OR` | 3 | potential |
| `NOT` | 3 | potential、any_international_organizations_member_of |
| `is_canal_open` | 2 | location:suez、location:panama |
| `is_member_of_international_organization` | 2 | potential |
| `is_active_parliament` | 2 | potential |
| `international_organization_type` | 2 | any_international_organizations_member_of |
| `hre_is_in_formation_period` | 1 | potential |
| `is_emperor` | 1 | potential |
