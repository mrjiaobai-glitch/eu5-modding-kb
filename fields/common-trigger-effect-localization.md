# common/trigger_localization 与 effect_localization（tooltip 文本权威）

> **一句话**：tooltip 文本权威：触发器与效果本地化的字段、人称与否定成对规则，及各类触发器的键名推导。
> **什么时候看**：界面上出现 raw key 或人称错乱、要补触发器与效果的 tooltip 文案时翻这篇。
> **体量**：94 行 · 约 5 分钟通读

来源：`in_game\common\trigger_localization\_trigger_localization.info`（**5,756 B / 152 行，GUI 层最完整的文档之一**）；`effect_localization\` **没有 info**，其格式由数据文件头注释（`country_effects.txt` 前 5 行）给出

## 为什么这两档算 GUI

界面上的**每一条 tooltip 文案**都来自这里——`gui\shared\*_tooltips.gui`（51 文件里过半）只负责"怎么摆"，"说什么"由本类目决定。规则错了表现为 tooltip 显示 raw key 或人称错乱。

## trigger_localization 字段（info 全表）

```
<trigger 名> = {
    global = <loc 键>        # 无人称（"Is an adult"，用于任意 trigger 内部）
    first  = <loc 键>        # 第一人称（"I am an adult"）
    third  = <loc 键>        # 第三人称（"[CHARACTER.GetName] is an adult"）
    # 请求的规则不存在时，回退到"列表里最后一个可用的"
    <rule>_not = <loc 键>    # 可选：覆盖否定形式（默认要求 NOT_<loc 键>）
}
```

- 每个 `<loc 键>` 需要**肯定 + 否定**两版：`<键>` 与 **`NOT_<键>`**
- 覆盖否定键的例子：`and = { global = TRIGGER_AND  global_not = TRIGGER_OR }`
- 只写否定版也可（`only_negative_form_used = { first_not = I_AM_NOT_A_CHILD }`）

### 数值比较触发器（info 第 47–67 行）

`dread >= 50` 这类写法按**运算符**找专门条目，找不到就回退到通用条目；可用 `$COMPARATOR$`（greater than / equal to…）与 `$NUM$`：

```
=   → <trigger>_equal          >= → <trigger>_greater_or_equal
>   → <trigger>_greater_than   <= → <trigger>_less_or_equal
<   → <trigger>_less_than
```

### 作用域比较触发器（info 第 69–84 行）

`scope:actor.mother.betrothed = scope:recipient.liege`：左侧**最后一个 `.` 之前**是主语、**之后**决定用哪个条目（`betrothed_equal`），右侧是宾语。

### "Any" 触发器（info 第 86–143 行）

| 写法 | 条目名 |
|---|---|
| `any_child = { … }` | `any_child` |
| `any_child = { count >= 5 … }` | `any_child_count_greater_or_equal`（另有 `_percent_<运算符>`） |
| `any_child = { count = all … }` | `any_child_all` |

**否定形式不另建键**，而是**反转语义**（info 原文）：`not regular any → all`、`not all → regular any`、`not count/percent → count/percent 且运算符取反`；`count = X` 的否定用额外的 `_not_equal` 后缀。

### 链接触发器（info 第 145–151 行）

`xxx = <scope>` 类写法用 `xxx_equal` / `xxx_not_equal` / `NOT_xxx_equal` / `NOT_xxx_not_equal`；数值型还需要 `xxx_greater_than` / `NOT_xxx_greater_than` 等。

## effect_localization 字段（文件头注释权威）

```
_category = { ... }      # 以 _ 开头 = 新建一个分组（组名 = category）
<effect 名> = {
    global / first / third              # 人称（默认无人称）
    global_past / first_past / third_past   # 过去时（默认现在/将来）
    …_neg                              # 负值输出（"gain x gold" vs "lose x gold"）
}
```

关键约定（注释原文）：**`neg` 版本拿到的数值永远是正数**（`the value given to the localization of a neg version will always be positive`）——所以文案里写"失去 $VALUE$"即可，不要在脚本侧预先取负。

原版实例（`country_effects.txt`）：

```
set_age_preference = {
    global = SET_AGE_PREFERENCE_EFFECT        first = WE_SET_AGE_PREFERENCE_EFFECT
    third  = THIRD_SET_AGE_PREFERENCE_EFFECT  global_past = PAST_SET_AGE_PREFERENCE_EFFECT
    first_past = PAST_WE_SET_AGE_PREFERENCE_EFFECT   third_past = PAST_THIRD_SET_AGE_PREFERENCE_EFFECT
}
```

## 原版规模

| 目录 | 文件 | 体量 | 文档 |
|---|---|---|---|
| `trigger_localization\` | 44 | 261 KB | `_trigger_localization.info` 5,756 B |
| `effect_localization\` | 36 | 233 KB | **无 info**（只有数据文件头注释） |

## 审查要点

- **肯定/否定成对**：只写 `<键>` 不写 `NOT_<键>`，则在 `NOT = { … }` 里显示 raw key（除非用 `<rule>_not` 覆盖）。
- **人称回退是"最后一个可用"**而不是第一个——`global` / `first` / `third` 的顺序会影响回退结果。
- `effect_localization` 没有 info：新增效果词条**照抄同类文件的人称/时态组合**（`global`+`first`+`third`+三者 `_past` 是原版常见写法）。
- 以 `_` 开头的键是**分组**不是效果名。
- 未在 readme 中说明：`effect_localization` 的完整字段清单（数值/量化文案规则）在本体里没有文档；`trigger_localization` 的 `_count_` / `_percent_` 运算符后缀见 info 第 100–111 行。
