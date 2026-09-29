# gui/attribute_columns（列表列——外观侧）

> **一句话**：列表列的外观侧写法：types 加 type 加 blockoverride 组装列控件，与数据侧列名一一对接。
> **什么时候看**：改列表列长相、要确认与 common 侧列名的对应关系时翻这篇。
> **体量**：65 行 · 约 3 分钟通读

来源：**无 readme**（数据侧权威是 `in_game\common\attribute_columns\readme.txt` 2,142 B，见 `fields\common-attribute_columns.md`）；本档覆盖 `in_game\gui\attribute_columns\`（**42 文件 / 171 KB**）的外观定义。

## 两域分工（关键）

| 目录 | 管什么 | 权威 |
|---|---|---|
| `common\attribute_columns\`（18 文件） | **列有哪些、宽度、排序**：`widget` / `width` / `fixed_height` / `is_constant_width` / `contains_select_target_button` / `single_widget_for_row` / `sort{…}` | `readme.txt` 2,142 B |
| `gui\attribute_columns\`（42 文件） | **列长什么样**：用 `types <Xxx>Columns` + `type <列> = <基控件>` + `blockoverride` 组装 | 无 readme（本档） |

数据侧的 `widget = <名>` 指向的正是 gui 侧 `type` 声明的**控件类型名**——两边靠这个名字对接（改一边名字必须同步改另一边）。

## 结构

```
types <对象>Columns {                       # 例如 types CharacterColumns
    type <列类型> = <基控件> {               # 例如 type character_ruler = character_info
        blockoverride "<块名>" { … }         # 改外观的差异部分
    }
}
```

真实片段（`gui\attribute_columns\character.gui`）：

```
types CharacterColumns
{
	type character_ruler = character_info {
		blockoverride "abilities_tooltip" {
			tagtooltip_enabled = yes
			tooltip = "[InteractionTarget.GetCharacter.GetAbilityTooltipAsRuler]"
		}
	}

	type character_artist = character_info {
		blockoverride "abilities_tooltip" { … }
		blockoverride "abilities_area" {
			artist_abilities = { blockoverride "abilities_bg" {} }
		}
	}
}
```

要点：

- **`InteractionTarget` 是列的根对象**（数据侧 readme 原文：widget "will be given an InteractionTarget to get data from"）——所以在 gui 侧写 tooltip/表达式时可以 `InteractionTarget.GetCountry` / `.GetCharacter` 等。
- 同一基控件（`character_info`）派生多个变体（`character_ruler` / `character_artist` / `character_marriage`），只改差异块——**这是列体系的标准写法**。

## 文件命名

按**对象类型**分文件（与 `common\attribute_columns\` 的命名呼应）：`character.gui`、`location.gui`、`province.gui`、`unit.gui`、`country.gui`、`building.gui`、`cabinet.gui`、`heir_selection.gui`、`parliament_issue.gui`、`bureaucracy_type.gui`、`colonial_charter.gui`、`exploration.gui`…

## 审查要点

- **gui 侧与 common 侧的列名必须一致**：`common` 里 `widget = province_create_colonial_charter` ↔ gui 里必须有同名 `type`。
- 列的排序**不在 gui 侧**：`sort_key` / `sort_text` / `sort_value` / `sort_by_tooltip_key` 全在 `common\attribute_columns\`（另见 `fields\gui-sort_keys.md` 的图标）。
- `InteractionTarget` 只在这套列控件里有意义；复制到别处会取不到数据。
- `fixed_height` 是**性能开关**（数据侧 readme：固定高度"allow for performance optimization when displaying a lot of items"），长列表务必设。
- 未在 readme 中说明：本类目**没有 readme**；基控件清单以 `ui_library.gui` 与各 `.gui` 的 `types` 声明为准。
