# gui/sort_keys（排序键图标）

来源：**无 readme**——`in_game\gui\sort_keys\00_sort_keys.txt`（**1,620 B / 37 个键**）实查。

## 字段

```
<sort_key> = {
    icon = <图标键>        # 100% 出现
    tooltip = <loc 键>     # 可选（原版 2 个）
}
```

## 原版实测（37 个键；旧档写 39，是把 `in_range_*` 数成 11 个——实为 9 个）

| 键 | icon | tooltip |
|---|---|---|
| `province_location_wealth` | `wealth` | — |
| `present_pops` | `"market"` | `MARKET_ITEM_SORT_BY_PRESENT_POPS` |
| `their` / `our` | `their_opinion` / `our_opinion` | — |
| `will` | `willingness` | — |
| `price` / `food_income` / `production_efficiency` / `merchant_capacity_profit` | `profit` | — |
| `level` | `number_building` | — |
| `works` | `work_of_art` | — |
| `region` | `location` | — |
| `enthusiasm` | `morale` | — |
| `goods` / `trade_supply_sort` | `trade_good` | — |
| `warscore` | `war_score` | — |
| `production` | `development` | — |
| `relations` | `dip` | — |
| `balance` | `food` | — |
| `gold` | `cost` | — |
| `owner_flag` | `owner` | — |
| `mercenary_tpc` | `transport` | — |
| `goods_local_balance` | `produced` | — |
| `goods_imports` / `goods_exports` | `to_market` / `from_market` | — |
| `in_range_*`（**9 个**：`supplied` / `demanded` / `produced` / `locally_demanded` / `exports` / `imports` / `balance` / `trade_balance` / `local_balance`） | `from_market` | — |
| `relative_power` | `power` | `COUNTRY_SORT_BY_RELATIVE_POWER` |
| `navy_levies` / `establishment` | `navy_levies` / `establishment` | — |

## 用法与联动

- 排序键被 **`common\attribute_columns\` 的 `sort = { sort_key = "…" }`** 引用（列头显示什么图标就由它决定）——见 `fields\common-attribute_columns.md`。
- `icon` 的值是**图标名**（少数带引号如 `"market"`，两种写法原版都有）；图标本体在 `gfx\interface\icons\` 一类目录里。
- 只有 2 个键带 `tooltip`（`present_pops`、`relative_power`）——**绝大多数排序键没有 tooltip**，靠图标表意。

## 审查要点

- 排序键的三件套分工：**排什么**在 `common\attribute_columns` 的 `sort_text` / `sort_value`，**叫什么**在 loc 键（`SORT_TEXT_*` / `*_SORT_BY_*`），**什么图标**在这里。
- 键名要与列定义里的 `sort_key` **逐字一致**；不一致不会报错，只是列表头没有图标。
- 未在 readme 中说明：本类目**没有 readme**；`tooltip` 是可选字段。
