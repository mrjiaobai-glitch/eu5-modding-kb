# common/attribute_columns（属性列）

> **一句话**：交互界面里"列"的定义：widget 与宽度、是否吸收剩余宽度，以及按文本或数值排序的 sort 块结构。
> **什么时候看**：给通用行动或角色交互的目标列表加列、加排序时翻这篇。
> **体量**：102 行 · 约 5 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\attribute_columns\` 全量 **61 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `contains_select_target_button` | 0 次（61 档） | **有**（写在别的类目） |
| `fixed_height` | 0 次（61 档） | **有**（写在别的类目） |
| `format` | 0 次（61 档） | **有**（写在别的类目） |
| `is_constant_width` | 0 次（61 档） | **有**（写在别的类目） |
| `single_widget_for_row` | 0 次（61 档） | **有**（写在别的类目） |
| `sort` | 0 次（61 档） | **有**（写在别的类目） |
| `sort_by_tooltip_key` | 0 次（61 档） | **有**（写在别的类目） |
| `sort_key` | 0 次（61 档） | **有**（写在别的类目） |
| `sort_text` | 0 次（61 档） | **有**（写在别的类目） |
| `sort_value` | 0 次（61 档） | **有**（写在别的类目） |
| `widget` | 0 次（61 档） | **有**（写在别的类目） |
| `width` | 0 次（61 档） | **有**（写在别的类目） |

### 二、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `name` | 54 | 54 |
| `population` | 9 | 9 |
| `select_target_acceptance_total` | 4 | 4 |
| `owner_flag` | 3 | 3 |
| `integration` | 2 | 2 |
| `peasants_unemployment` | 2 | 2 |
| `control` | 2 | 2 |
| `rebel_progress` | 2 | 2 |
| `power` | 2 | 2 |
| `development` | 2 | 2 |
| `rtr_yuan_allegiance` | 1 | 1 |
| `parliament_agenda_widget` | 1 | 1 |
| `satisfaction` | 1 | 1 |
| `character_artist` | 1 | 1 |
| `shugo_province_name` | 1 | 1 |
| … | 另有 25 种 | |

### 三、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 330 | else_if、scope:actor、if、sort_value |
| `sort_by_tooltip_key` | 233 | sort |
| `sort` | 233 | army_size、gold、right_click_cost、character_explorer |
| `widget` | 147 | army_size、gold、right_click_cost、character_explorer |
| `width` | 130 | army_size、gold、right_click_cost、character_explorer |
| `sort_value` | 128 | sort |
| `fixed_height` | 117 | army_size、right_click_cost、character_explorer、heir_status_icon |
| `limit` | 110 | else_if、if |
| `if` | 104 | sort_text、value、sort_value |
| `else` | 99 | sort_text、sort_value |
| `sort_key` | 99 | sort |
| `sort_text` | 97 | sort |
| `exists` | 96 | limit |
| `is_constant_width` | 94 | gold、character_explorer、control、name_no_rank |
| `sort_order` | 62 | sort |
