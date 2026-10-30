# common/religious_aspects、religious_factions、religious_focuses（宗教三件套）

> **一句话**：宗教三件套的字段：宗教面相、宗教派系与宗教焦点，标出各自作用域、挂接方式与审查要点。
> **什么时候看**：写宗教面相、派系或焦点，要确认作用域是国家还是国际组织时翻这篇。
> **体量**：117 行 · 约 6 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\religion\` 全量 **20 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `saints_concept` | 2 | 1 | heroes（1）、divine_emperors（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`saints_concept`**（2 种）：heroes（1）、divine_emperors（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `icon` | 95 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `Attributes` | 0 次（20 档） | 全库也没有 → 疑似废弃字段 |
| `potential` | 0 次（20 档） | **有**（出现在 25 个类目） |
| `religious_focuses` | 0 次（20 档） | **有**（出现在 1 个类目） |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `opinions` | 56 | 10 |
| `ai_will_do` | 8 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `add` | 50 | monthly_progress、ai_will_do、value、if |
| `value` | 42 | add、change_institution_progress |
| `desc` | 41 | add |
| `stability_cost_efficiency` | 33 | modifier |
| `global_population_growth` | 23 | modifier |
| `global_life_expectancy` | 21 | modifier |
| `global_monthly_prosperity` | 19 | modifier |
| `NOT` | 19 | enabled、visible、limit |
| `multiply` | 18 | value、add |
| `global_hostile_attrition` | 13 | modifier |
| `has_dlc` | 11 | visible |
| `monthly_towards_spiritualist` | 11 | modifier |
| `tolerance_own` | 9 | modifier_while_progressing、modifier |
| `monthly_religious_influence` | 9 | modifier |
| `if` | 9 | effect_on_completion、monthly_progress |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `icon` | 95 | 9 |
