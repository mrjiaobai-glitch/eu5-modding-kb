# common/script_values（脚本值：数值与公式）

> **一句话**：脚本值的两种形态与数学 DSL：运算、钳制、区间与列表、跨作用域引用，及三条官方硬规则。
> **什么时候看**：写公式型数值、要确认执行顺序与钳制位置或区间内能否内联公式时翻这篇。
> **体量**：76 行 · 约 4 分钟通读

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
