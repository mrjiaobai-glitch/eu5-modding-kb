# setup/countries（国家定义，00_readme.info）

来源：`in_game\setup\countries\00_readme.info`（注意扩展名为 .info，不是 readme.txt）

## 格式

```
TAG = {
    color = hsv360 { 360 100 100 }
    color2 = hsv360 { 0 0 0 }
    male_regnal_names = {}
    female_regnal_names = {}
    description_category = <administrative>   # 参照 common/country_description_category DB
    difficulty = 2                            # int，1–5
}
```

## 审查要点

- `description_category` 引用须在 `in_game\common\country_description_categories` 存在。
- `difficulty` 取值 1–5。
- 该文件是 .info 扩展名（非 .txt），审查文件发现时应包含。
- 未在 readme 中说明：本地化键格式。

## 原版实测（2,340 个国家块的真实字段分布）

| 字段 | 用到的国家数 | 说明 |
|---|---|---|
| `color` | **2,340（100%）** | 地图色（可用 `map_<名>` 具名色 / `rgb { }` / `hsv360 { }`） |
| `culture_definition` / `religion_definition` | 各 2,337 | **主文化与国教**（⚠️ 字段名都带 `_definition`，readme 模板没写这两个） |
| `color2` | 1,848 | 副色 |
| `description_category` | 67 | 引用 `common\country_description_categories\` |
| `difficulty` | 65 | 1–5 |
| `is_historic` | 55 | 历史国家标记 |
| `unit_color0` / `unit_color1` / `unit_color2` | 各 46 | 兵模配色 |
| `male_regnal_names` / `female_regnal_names` | **0** | ⚠️ readme 模板里有，**原版没有任何国家在用**——别照抄 |

**国家名/形容词不在这里**：在 `main_menu\localization\<lang>\country_names_l_<lang>.yml` 的 `<TAG>` / `<TAG>_ADJ` 两键（11 语言）。

**下两步在别处**：领土与开局状态在 `main_menu\setup\start\10_countries.txt`（`own_control_core = { <location…> }` + `starting_technology_level` + `include = "<模板>"`，模板层 205 档见 `guides\game-layout.md` 的 `setup\` 段）；端到端流程见 **`guides\new-country-tutorial.md`**。
