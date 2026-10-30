# 参考资料：修正名速查（modifier 键）

> **用途**：写 mod 时要查"这个修正叫什么键、是不是 percent、能不能加在 country/location 上"——**不用再翻 `main_menu\common\modifier_type_definitions\` 的三个注册表档**。
> **来源**：本体 `main_menu\common\modifier_type_definitions\{00_modifier_types,01_byz,02_generic_bureaucracies}.txt` + `main_menu\localization\{simp_chinese,english}\*_l_*.yml` 的 `MODIFIER_TYPE_NAME_*`，**脚本生成自 EU5 1.4.5**（2026-10-30）。
> **体量**：约 6 分钟浏览（索引本身）

## 一、按 category 拆成三份

| 文件 | 内容 | 键数 |
|---|---|---|
| `reference\modifiers-country.md` | `category = country`（国家修正） | **2,049** |
| `reference\modifiers-location.md` | `category = location`（地点修正） | **428** |
| `reference\modifiers-other.md` | IO **100** / unit **70** / character **47** / all 7 / movement 4 / religion 3 / province 1 / rebel 1 | **233** |
| 合计 | 注册表 3 档 | **2,710** |

## 二、每列怎么读

| 列 | 含义 |
|---|---|
| 键 | 写进脚本里的键名（`modifier:<键>` 或修正块里的裸键） |
| 简中名 | 游戏内显示名（`MODIFIER_TYPE_NAME_<键>`，2,691/2,710 有） |
| 类型 | `percent`（游戏里按百分比显示，**写 0.1 = 10%**）/ `boolean`（开关，写 `yes`）/ `number`（纯数值） |
| 标记 | `ai=yes`（AI 用）/ `bias=<opinion\|trust\|voting>` / `decimals=N` / `范围<min~max>` |

## 三、注册表头注释里的字段含义（原文规则）

| 字段 | 默认 | 说明 |
|---|---|---|
| `color` | good / neutral / bad | UI 颜色语义 |
| `boolean` | no | 是开关型修正（写 `yes`） |
| `percent` | no | 按百分比显示（值 0.1 = 10%） |
| `already_percent` | no | 值本身已是百分数（不再 ×100 显示） |
| `decimals` | 2 | 显示小数位 |
| `category` | all | 可用的 scope：country/location/province/character/unit/mercenary/religion/internationalorganization/rebel/dynasty/all |
| `min` / `max` | ±∞ | 数值钳制 |
| `cap_zero_to_one` | no | 钳到 0–1 |
| `scale_with_pop` | no | 按人口缩放 |
| `format` | "" | `FormatPopCaps` / `FormatManPower` / `FormatGold` |
| `ai` | no | AI 是否使用该修正类型 |
| `bias_type` | 无 | `opinion` / `trust` / `voting`（可多个） |
| `should_show_in_modifiers_tab` | yes | 是否出现在政府修正页 |

## 四、使用提醒

1. **键名 ≠ 显示名**：显示名走本地化键 `MODIFIER_TYPE_NAME_<键>`；名字里有 `$...$` 的是**游戏内占位符**（如 `开放$mysticism_vs_jurisprudence$`），不是错字。
2. **没注册 = 静默无效**：任何 `modifier:<键>` 必须能在这三份表里找到，否则不报错、只是没效果。
3. **scope 要匹配**：`category = location` 的键写进国家修正是没有意义的（反之亦然）；`all` 类两者通用。
4. **数值口径**：`percent` 类写 `0.1`（=10%），不要写 `10`；`number` 类按原值。
5. 机制说明（注册表怎么被引擎读、`game_data` 必填、`category` 10 值）见 `fields\main_menu-modifier_type_definitions.md`；本档只解决"查键名"。

## 五、复跑生成

游戏更新后重跑同一套生成逻辑即可（解析 3 个注册表档 + 拉 `MODIFIER_TYPE_NAME_*`），产物覆盖本目录三档。生成脚本逻辑记录在 `tools\kb-self-audit.md` §十五（若尚未记，见下条）。

> 待办：把生成器脚本落到 `kb\scripts\gen-modifier-reference.ps1`（当前为一次性内联执行）。
