# common/rebel_demands（叛乱诉求）

来源：**本体无 readme**——`in_game\common\rebel_demands\` 2 个数据文件的注释与实测反推（`999_default_rebel_demands.txt` 5 835 B + `900_country_specific_from_startup_or_events.txt` 301 B）

## 字段

```
<demand id> = {
    trigger = { <triggers> }         # 100% 出现；多为 rebel_category = <类别>
    concession_effect = { <effects> } # 100%：玩家"让步"时执行
    victory_effect = { <effects> }    # 15%（原版 2 条）：叛乱方获胜时执行
}
```

让步包内部常见写法：`pacify_rebel_pops = yes`、`owner = { … }`、`change_pop_type`、`add_accepted_culture`、`change_societal_value = { type = <轴>  value = societal_value_minor_move_to_right }`、`grant_benefits_to_estate`、`add_legitimacy`、`rebel_estate_type`。

## 原版实测（13 条顶层 / 2 文件）

| 层级 | 条目 |
|---|---|
| 按**叛乱类别**的默认（4） | `default_slave_demand`、`default_nationalist_demand`、`default_religious_demand`、`default_pretender_demand` |
| 按**阶层**的默认（7） | `default_{nobles\|clergy\|burghers\|peasants\|dhimmi\|tribes\|cossacks}_estate_demand` |
| 兜底（1） | `default_rebel_demand` |
| 国别专属（1） | `the_goals_of_balliol` |

> 文件内另有嵌套的具名 demand 块（全文约 77 个 `= {`）：**顶层 13 条是"每个叛乱类别一个兜底"**，嵌套块是同一类别下的具体诉求变体。

**加载顺序**（文件头注释权威）：

> "Default rebel demands — one fallback per rebel category. **Files named 000_–998_ are loaded first**, so unique demands placed in those files will be evaluated before these defaults."

即：**文件名前缀数字决定优先级**——放 `000_`–`998_` 的专属诉求会先于 `999_default_*` 被评估。

字段族出现次数（全文）：`pacify_rebel_pops` 12、`change_societal_value` 13、`owner` 13、`rebel_estate_type` 7、`grant_benefits_to_estate` 7、`add_legitimacy` 3、`victory_effect` 2。

## 两个原版实例

```
default_slave_demand = {
    trigger = { rebel_category = slave }
    concession_effect = {                       # 让步：解放全部奴隶
        pacify_rebel_pops = yes
        owner = { every_pop = { limit = { pop_type = pop_type:slaves } change_pop_type = pop_type:peasants } }
    }
}
default_nationalist_demand = {
    trigger = { rebel_category = nationalist }
    concession_effect = {                       # 让步：接纳其文化 + 往分权挪一小步
        pacify_rebel_pops = yes
        culture = { save_scope_as = nationalist_culture }
        owner = { if = { limit = { NOT = { has_accepted_culture = scope:nationalist_culture } }
                         add_accepted_culture = scope:nationalist_culture }
                  change_societal_value = { type = centralization_vs_decentralization
                                            value = societal_value_minor_move_to_right } }
    }
}
```

## 本地化

**不需要键**：原版没有任何 `REBEL_DEMAND_*` 之类的本地化键（`tools\loc-keys.md` 已把 rebel demand 名列入"不需要 loc 的类型（不要误报）"）。诉求的表现来自 `concession_effect` 自身的效果提示。

## 审查要点

- `trigger` 通常按 **`rebel_category`** 匹配（slave / nationalist / religious / pretender / 各阶层）；写别的条件必须保证互斥，否则同一场叛乱会命中多条诉求。
- `concession_effect` **务必包含 `pacify_rebel_pops`**（原版 12/13 都有），否则让步后叛军不会平息。
- 想让专属诉求优先，**用 `000_`–`998_` 前缀命名文件**；前缀不是装饰，是加载顺序。
- 上层接口：叛乱类别、`rebel_estate_type`、`grant_benefits_to_estate` 都引用真实 id；与社会价值观联动走 `change_societal_value`。
- 未在 readme 中说明：**本类目没有 readme**，字段与优先级规则以本文件为准。
