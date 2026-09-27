# common/area_preferences（区域偏好：AI 探索与征服目标区）

来源：**无 readme**——`in_game\common\area_preferences\` 两个文件的**文件头注释即权威**（原文："area preference definitions - pure data, no country or allowed fields. Use `add_area_preference = <key>` in on_game_start, missions, or events to assign them."）

## 字段

```
<preference key> = {
    preference_type = exploration / conquest    # 100% 出现：探索偏好 / 征服偏好
    modifier = 2                                # 100% 出现；可小于 1 表示"降低欲望"
    area = <area 键>                             # 57%；可多行
    region = <region 键>                         # 51%；可多行
    continent = <continent 键>                   # 5%
    sub_continent = <sub_continent 键>           # 5%
    formable_country = <可成立国家键>              # 5%
    consider_capital_region = yes                # 1%
    allow = { <trigger> }                        # 1%
}
```

**纯数据**：没有 `country` / `allowed` 字段，也不是"自动生效"的——必须用 **`add_area_preference = <key>`** 在 `on_game_start`、任务（missions）或事件里**指派给具体国家**。

## 原版实测（83 条 / 2 文件 / 16.9 KB）

| 文件 | 条数 | 用途 |
|---|---|---|
| `exploration_preferences.txt`（6.2 KB） | **25** | 探索方向 |
| `conquest_preferences.txt`（10.7 KB） | **58** | 征服方向 |

| 字段 | 出现 | 率 |
|---|---|---|
| `preference_type` / `modifier` | 83 | 100% |
| `area` | 47 | 57% |
| `region` | 42 | 51% |
| `sub_continent` / `formable_country` / `continent` | 各 4 | 5% |
| `consider_capital_region` / `allow` | 各 1 | 1% |

`modifier` 取值跨度很大：**−0.5**（抑制）到 **100**（狂热），最常见 3（13 条）/ 5（16 条）/ 10（12 条）/ 15（6 条）/ 25（2 条）。

## 原版实例

```
england_explore_usa = {                # 英格兰：北美
    preference_type = exploration
    modifier = 2
    area = atlantic_north_atlantic_current_area
    area = atlantic_irminger_area
    area = labrador_sea_area
    area = atlantic_labrador_area
    area = azores_sea_area
    region = canada_region
    region = east_coast_region
}

england_explore_india = {              # 英格兰：绕非洲去印度
    preference_type = exploration
    modifier = 2
    area = nw_africa_coast_area
    area = west_africa_coastline
    area = gulf_of_africa_sea
    area = central_africa_coastline_area
    area = southern_africa_coast_area
    area = mozambique_channel_area
    area = somali_sea_area
    area = indian_horn_area
    area = deccan_sea_zones_area
}
```

**各国的历史探索/征服路线就是靠这些条目"写死"的**——英格兰两跳（北美 + 绕非赴印）、法兰西（北美 + 大平原）……这也解释了为什么原版 AI 的殖民方向看起来"很有历史感"。

## 审查要点

- 键名必须通过 `add_area_preference` 被引用才会生效——**只写文件不指派等于没写**（这是与其它类目最大的不同）。
- `area` / `region` / `continent` / `sub_continent` 必须是**真实的地理键**（`map_data\definitions.txt` 的层级；见 `vanilla\vanilla-map-and-geography.md`）；写错不报错、只是从不命中。
- `modifier` 可以小于 1（含负数）用于**抑制**某些方向；`0` 表示"不在意"而非禁止。
- `preference_type` 只有 `exploration` / `conquest` 两种（原版分布 25 / 58）。
- 未在 readme 中说明：**本类目没有 readme**；`consider_capital_region`、`formable_country` 的精确语义、以及 modifier 如何参与最终效用计算，均由引擎决定。
