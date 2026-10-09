# common/town_rights（城镇特权）+ common/town_setups（城镇建筑模板）

> **一句话**：城镇特权与城镇建筑模板两组字段：地点修正、征服后保留开关，以及建筑名等于等级的模板写法。
> **什么时候看**：写城镇特权、调征服后保留行为，或改城镇自带建筑模板时翻这篇。
> **体量**：72 行 · 约 4 分钟通读

来源：`in_game\common\town_rights\readme.txt`（5 行）+ 9 个数据文件 **50 项特权**；`town_setups\00_default.txt` **117 个模板**（无 readme）

## town_rights 字段

```
<town_right_id> = {
    color = <named color>          # 100% 出现（地图/界面配色）
    location_modifier = { … }      # 100% 出现：作用到被授予的地点
    allow = { … }                  # root = country，scope:target = location（74%）
    kept_at_conquest = yes/no      # ⭐ 被征服后是否保留（64%）
    potential = { … }              # root = country（58%）
    country_modifier = { … }       # 30%：给授予国自身的修正
}
```

## 原版实测（50 项 / 9 文件）

文件按地域/文化分：`00_traditions.txt`、`01_discovery.txt`、`10_country_specific.txt`、`11_scandinavian.txt`、`12_german.txt`、`13_poland.txt`、`14_britain.txt`、`15_iberia.txt`、`byz…`。

| 字段 | 出现 | 率 |
|---|---|---|
| `color` / `location_modifier` | 50 | 100% |
| `allow` | 37 | 74% |
| **`kept_at_conquest`** | 32 | 64% |
| `potential` | 29 | 58% |
| `country_modifier` | 15 | 30% |

原版条目实例：

```
staple_port = {
    location_modifier = { local_marketplace_building_levels = 5  harbor_suitability = 0.2
                          local_maritime_presence = 0.1  local_burghers_estate_power = 0.5 }
    allow = { scope:target = { is_port = yes } }
    color = map_naples
}
granary_town = { location_modifier = { local_food_decay_modifier = -0.25
                                       local_food_capacity_modifier = 0.5  local_migration_speed = -0.5 } … }
market_charter = { location_modifier = { local_marketplace_building_levels = 3
                                         local_trades_per_burgher = 0.25  local_market_access = 0.1 } … }
```

## 成本与脚本面

- 价格（`prices\00_hardcoded.txt`）：**授予 `grant_town_rights = { government_power = 5 }`**、**撤销 `revoke_town_rights = { stability = 10 }`**
- 行动文件：`generic_actions\revoke_town_rights.txt`
- 触发器：`has_town_rights`、`has_any_town_rights`、`has_max_town_rights`、`has_different_town_rights_than`（均在 `location_triggers.txt`）；效果：`grant_town_rights`、`revoke_town_rights_of_type`（`location_effects.txt`）

## town_setups：城镇建筑模板（不是特权）

```
scandinavian_town = { brewery = 1  temple = 1  tools_guild = 1  weapon_guild = 1  mason = 1
                      pottery_guild = 1  tannery = 1  naval_supplies_guild = 1  marketplace = 1 }
```

- **字段就是"建筑名 = 等级"**；原版出现率最高：`marketplace` 86%、`tools_guild` 68%、`mason` 67%、`weapon_guild` 66%、`pottery_guild` 66%、`temple` 65%、`tannery` 62%、`granary` 59%、`fine_cloth_guild` 58%
- **32% 的模板用 `copy_from`** 继承另一个模板再写加法差量（文件注释原文：「Unique city setups use copy_from + additive deltas」）——mod 里复用最省事
- 决定**城镇/城市建成时自带哪些建筑**，是"地方治理 × 生产建筑"的接口

## 审查要点

- `allow` 的作用域是 **`scope:target` = location**、root = country（readme 写作 `target = location`，实测全部用 `scope:target` 写法）；`potential` 是 root = country。
- `location_modifier` 里的修正键必须与"地点"类别匹配（`local_*`），写国家键不会生效。
- `kept_at_conquest` 决定征服后的保留行为——改它就等于改"征服收益"，注意与战争/和平系统的联动。
- `town_setups` 里的建筑名必须是真实 `building_types` id；`copy_from` 的目标必须是已存在的模板。
- 未在 readme 中说明：`town_rights` 的本地化键格式与 `kept_at_conquest` 的默认值；`town_setups` **完全没有 readme**（字段与语义由数据反推）。
