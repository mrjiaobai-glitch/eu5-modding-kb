# common/script_values（脚本值：数值与公式）

> **一句话**：脚本值的两种形态与数学 DSL：运算、钳制、区间与列表、跨作用域引用，及三条官方硬规则。
> **什么时候看**：写公式型数值、要确认执行顺序与钳制位置或区间内能否内联公式时翻这篇。
> **体量**：150 行 · 约 7 分钟通读

来源：`in_game\common\script_values\_script_values.info`（**3,852 B，完整的数学 DSL 文档**）+ 26 个数据文件（78 KB）实查

> **与 `guides\scripting-core.md` §一 的分工**：那边是**制作用法速查**（怎么定义、怎么内联）；本档是**字段权威**（info 全字段 + 语义细节 + 陷阱），并补上 guide 未展开的边界规则。

## 两种形态

```
# ① 静态值
minor_stress_gain = 10          # 引用：add_stress = minor_stress_gain

# ② 公式
my_formula = {
    value = 0                   # 设为该值
    add = 5 / subtract = … / multiply = … / divide = … / modulo = …
    max = 10                    # 高于则钳到 10
    min = 1                     # 低于则钳到 1
    round = yes / ceiling = yes / floor = yes
    if = { limit = { … }  add = 5 }
    else_if = { limit = { … }  … }
    else = { … }
    fixed_range   = { min = …  max = … }   # 随机定点数（如 1.242）
    integer_range = { min = …  max = … }   # 随机整数
}
```

**`value` 之外的所有运算符都接受"数字 / 另一个脚本值 / `scope.something`"。**

## info 明确的三条硬规则

1. **执行顺序 = 书写顺序**。官方例子：`{ add = 5  multiply = 4  max = 10  add = 5 }` 结果是 **15**（`max = 10` 在最后一次 `add` 之前生效）。→ **钳制类操作要放在最后**。
2. **公式不能用于 true/false 值**（info 原文："Formulas do not work for true/false values"）。
3. **性能**：公式"每一次求值都要重算"（"have to be calculated every single time they're evaluated"）——复杂公式慎用。

## 内联与链式

- **内联**：凡接受脚本值处都可直接写公式，**连运算符里也能嵌套**：
  ```
  add_gold = { value = gold  multiply = { value = 1  multiply = 0.5 } }
  ```
- **链式**：命名公式可跨作用域引用——`add_gold = { value = mother.example_age }`。

## 区间与列表

```
add_gold = { 1 5 }                          # 随机 1–5
add_gold = { named_value another_named_value }   # 引用两个命名值（含公式）
```
⚠️ **区间里不能内联公式**（info 明确举例：`{ { value = 1 add = 2 } some_named_value }` 非法）——需要时改用 `integer_range` / `fixed_range`。

```
add_gold = { every_child = { add = 1 } }     # 按子女人数加钱
add_gold = { ordered_child = { order_by = age  max = 3  add = age } }   # 有序列表
```
所有列表（含脚本自定义列表，见 `fields\main_menu-game_rules.md` 的 `scripted_lists`）都可用。

## 作用域切换（info 原文示例）

```
add_gold = { father = { any_child = { add = 1 } } }   # 按父亲的子女数加钱
```

## 审查要点

- **钳制（`max`/`min`）必须放在会改变数值的运算之后**，否则会被后续运算突破。
- **区间内不能内联公式**（用 `integer_range` / `fixed_range`）。
- `divide` 要防 0（info 原文提醒）；`modulo` 同理。
- 公式**不能返回布尔**——条件判断请用 trigger。
- 公式**每次求值重算**：放进 `ai_will_do`、`select_trigger` 的排序值等高频位置前先估性能。
- 未在 readme 中说明：本文件不是 readme 而是 `.info`；`_` 前缀文件即官方文档本身。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\script_values\` 全量 **30 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **注意**：本类目**本体没有 readme.txt**——下面全部是实测结果，不存在"漏写"一说。

### 一、本体实际在用的字段（无 readme，纯实测）

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `floor` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`min`**（4 种）：0（6）、1（5）、0.25（4）、2（1）
- **`subtract`**（8 种）：80（2）、defensive_alliance_strength（1）、scope:root_offensive_alliance_strength（1）、scope:actor.num_embraced_institutions（1）、scope:actor.modifier:landfriede_flat_cost（1）、1（1）、scope:actor.num_of_advances_researched（1）、modifier:bias_for_diplomat_policies（1）
- **`divide`**（3 种）：2（3）、300（2）、1000（1）
- **`max`**（6 种）：5000（1）、10000（1）、1（1）、scope:gold_bribe_ft_upper_limit（1）、100（1）、scope:parliament_gold_bribe_upper_limit（1）
- **`save_temporary_scope_as`**（1 种）：root_country（2）
- **`floor`**（1 种）：yes（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 2）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `desc` | 34 | 29 |
| `save_temporary_scope_as` | 2 | 14 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `add` | 138 | 12 |
| `if` | 129 | 15 |
| `value` | 88 | 23 |
| `multiply` | 48 | 11 |
| `min` | 17 | 8 |
| `subtract` | 12 | 5 |
| `divide` | 11 | 6 |
| `else_if` | 10 | 4 |
| `max` | 10 | 5 |
| `else` | 9 | 5 |
| `save_temporary_value_as` | 6 | 2 |
| `every_owned_location` | 5 | 2 |
| `scope:country` | 2 | 1 |
| `every_neighbor_country` | 2 | 1 |
| `every_royal_marriage` | 2 | 1 |
| … | 另有 5 种 | |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 375 | scope:second、if、else_if、save_temporary_value_as |
| `desc` | 252 | max、multiply、subtract、divide |
| `limit` | 240 | every_related_country、every_neighbor_country、every_subject_or_below、if |
| `add` | 198 | every_owned_location、every_neighbor_country、every_subject_or_below、if |
| `multiply` | 161 | else、every_neighbor_country、multiply、subtract |
| `if` | 74 | io_get_average_tax_base、else_if、scope:country、if |
| `divide` | 38 | scope:recipient、if、scope:actor、divide |
| `subtract` | 34 | multiply、root、else_if、if |
| `name` | 29 | is_key_in_local_variable_map、save_temporary_value_as |
| `exists` | 29 | limit |
| `min` | 27 | multiply、if、add、value |
| `NOT` | 23 | limit、trigger_if、culture:albanian |
| `save_temporary_value_as` | 22 | ?、if、else_if、strength_ratio_for_garrison_sortie |
| `max` | 18 | multiply、value、add、if |
| `else` | 18 | else、subtract、if、add |

### 六、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `desc` | 34 | 29 |
| `save_temporary_scope_as` | 2 | 14 |
