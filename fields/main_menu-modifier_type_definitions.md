# main_menu/common/modifier_type_definitions（修正键注册表）

> **一句话**：修正键注册表：显示层字段、必填的 game_data 与十种作用域类别，以及两条本地化键。
> **什么时候看**：写任何修正键、要查它归哪类对象或补名称与说明时翻这篇。
> **体量**：79 行 · 约 4 分钟通读

来源：**本类目没有 readme、也没有 info**——3 个档实查（`00_modifier_types.txt` **212,995 B / 2,393 键**、`01_byz.txt` 29 键、`02_generic_bureaucracies.txt` 14 键）。

> ⚠️ **口径提醒**：本库其它篇引用的 "2393 个修正" 是**只算主档**的数字；**目录合计 2,436 个键**（2026-09 体检修正）。凡说"修正键必须在这里注册"的地方，指的都是本档。

## 它是什么

**任何一个 `modifier` 键（脚本里写 `modifier:<键>`、`country_modifier = { <键> = 1 }` 的那个 `<键>`）都必须在这里注册**，否则静默无效——这是本库被引用最多的一条硬规则（14 篇提到），注册表本身长这样：

```txt
shared_border_impact = {          # ← 键名即修正名（loc 键是 MODIFIER_TYPE_NAME_<键>）
    percent = yes                 # 显示层：按百分比渲染
    color = bad                   # 显示层：正负颜色取向
    game_data = {                 # ★ 每一条都有（2,393/2,393）
        category = country        # ★ 作用域类别（枚举见下）
        ai = yes                  # 可选：这个修正参与 AI 评估（原版 48 个）
    }
}
```

## 字段（6 + 2，全量频次实测）

| 字段 | 层级 | 频次 | 说明 |
|---|---|---|---|
| `percent` | 条目 | 1,317 | 按百分比显示（`0.5` → "+50%"） |
| `color` | 条目 | 817 | 颜色取向（原版取值如 `bad`） |
| `boolean` | 条目 | 722 | 布尔型修正（显示为 ✓/✗ 而非数值） |
| `decimals` | 条目 | 126 | 显示小数位（如 `decimals = 0`） |
| `already_percent` | 条目 | 30 | 值本身已是百分数（不再 ×100） |
| `no_difference_sign` | 条目 | 3 | 不显示 +/- 前缀 |
| **`game_data`** | **块（必填）** | **2,393** | 引擎数据块 |
| ├ `category` | game_data | 2,393 级 | **作用域类别**，决定这个修正能挂在哪类对象上 |
| └ `ai = yes` | game_data | 48 | 参与 AI 评估（`vanilla\vanilla-ai.md` 的"48 个带 ai=yes"指此） |

## `category` 枚举与分布（10 值，实查）

| category | 数量 | category | 数量 |
|---|---|---|---|
| `country` | **1,870** | `character` | 43 |
| `location` | 345 | `all` | 5 |
| `internationalorganization` | 94 | `movement` | 4 |
| `unit` | 70 | `religion` | 3 |
| | | `province` / `rebel` | 各 1 |

**读法**（最容易踩的坑）：键名带 `local_` 前缀的多半是 `location`（local ↔ location，不是 province），`global_`/无前缀的多半是 `country`——但**别靠前缀猜，回这个表查 `category`**。写错 category 的表现是"修正挂上了但界面永不显示、取值恒为 0"。

## 本地化键

```
MODIFIER_TYPE_NAME_<键>     # 名称（英文 loc 2,507 条）
MODIFIER_TYPE_DESC_<键>     # 说明（英文 loc 2,477 条）
```

实例：`MODIFIER_TYPE_NAME_shared_border_impact: "Opinion Impact from Shared Borders"`、`MODIFIER_TYPE_NAME_local_nobles_pop_growth: "$nobles$ Growth"`（`modifier_types_l_english.yml:598 / :305`）。**新注册一个键就必须补这两个 loc 键**，否则界面显示 raw key。

## 原版实测

| 项 | 数据 |
|---|---|
| 键总数 | **2,436**（00_modifier_types 2,393 + 01_byz 29 + 02_generic_bureaucracies 14） |
| 带 `ai = yes` | **48**（全部在 00 主档） |
| category 分布 | country 1,870 / location 345 / IO 94 / unit 70 / character 43 / all 5 / movement 4 / religion 3 / province 1 / rebel 1 |
| DLC 追加 | `01_byz.txt`、`02_generic_bureaucracies.txt` 是 DLC/内容包追加的键（44 个）——**mod 也可以照这个方式追加自己的档** |
| 修正名 loc | `MODIFIER_TYPE_NAME_*` 2,507 + `MODIFIER_TYPE_DESC_*` 2,477（英文） |

## 审查要点

- **`game_data` 与 `category` 是必填**（原版 100%）；只写显示层字段（`percent`/`color`）而不给 category，等于没注册。
- **类别前缀与 `category` 必须对得上**：`local_*` ↔ `location`、`global_*`/裸名 ↔ `country`、`ai_*` ↔ `country`。写反了不报错，只是永不生效。
- **`percent` 与 `already_percent` 别混用**：前者表示"数值按比例、显示时 ×100"，后者表示"数值本身就是百分数"——两者都写会导致显示翻倍。
- **跨类目引用**（法律/改革/建筑/阶层特权/事件选项里的 `modifier` 字段）只认**已注册的键**；审查 mod 时必须把所有出现的修正键回本表比对（这是 `tools\review-checklist.md` 第二节列的第一条）。
- **新键要同时给 loc 三件套**（`MODIFIER_TYPE_NAME_` / `_DESC_`，以及用它的对象的 loc）。
- 未在 readme 中说明：本类目**没有任何官方文档**；`color` 的合法取值清单、`boolean` 与 `percent` 的界面渲染细节、`ai = yes` 具体影响哪些 AI 决策，均由引擎决定（`ai = yes` 的清单可反查 `vanilla\vanilla-ai.md`）。
