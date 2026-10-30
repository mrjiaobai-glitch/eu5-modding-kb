# main_menu/common/modifier_icons（修正图标映射）

> **一句话**：修正图标映射：把已注册的修正键映到正负两张图标，并给出默认图标的兜底规则与目录。
> **什么时候看**：修正显示默认图标、要给新修正键配图标时翻这篇。
> **体量**：46 行 · 约 3 分钟通读

来源：**无 readme**——`00_modifier_icons.txt`（**287,943 B / 9,388 行**）与 `01_byz.txt`（3,591 B，DLC 追加）实查；**文件头 6 行注释即官方 schema**。

## 它是什么

把**修正键**（`modifier_type_definitions` 里注册的那些）映射到**正/负两张图标**——界面上显示修正时用。

```txt
# 文件头注释给的格式：
#example = {
#    positive = "gfx/interface/icons/modifier_types/example_positive.dds"  # 值 >= 0 时用；negative 未设时也用它
#    negative = "gfx/interface/icons/modifier_types/example_negative.dds"  # 可选：值 < 0 时用
#    default  = yes     # 带此旗标的【第一个】条目作为全局默认图标
#}

default = {                                    # 原版就是这条做默认
    positive = "gfx/interface/icons/modifier_types/_default.dds"
}

contact_patriarch_of_constantinople_cost_modifier = {     # 键名 = 修正键
    positive = "gfx/interface/icons/modifier_types/contact_patriarch_of_constantinople_cost_modifier.dds"
}
```

## 原版实测（2 档）

| 项 | 数据 |
|---|---|
| 条目数 | **2,427**（主档；DLC `01_byz.txt` 另有 30 条左右） |
| `positive` | **2,427（100%）** |
| `negative` | **53**（只有少数修正区分正负） |
| `default` | **2**（第一个生效） |
| 图标目录 | `gfx/interface/icons/modifier_types/`（该目录 1,377 档，见 `vanilla\vanilla-dlc-and-assets.md` §四） |

## 审查要点

- **条目键必须与 `modifier_type_definitions` 的键逐字一致**（大小写、`_modifier` 后缀都要对）——拼错不报错，只是该修正显示默认图标。
- 新增修正键时**配套加一条这里**（`positive` 必填），图标 dds 放 `gfx/interface/icons/modifier_types/`。
- `negative` 只有 53 条在用——**"全写 negative"不是原版惯例**；多数修正只给 `positive`（引擎按值正负自动换色，见 `modifier_type_definitions` 的 `color` 字段）。
- `default` 只应有 1–2 个（原版 2 个），**它是"全局兜底"而不是给每条修正用的**。
- 未在 readme 中说明：本类目**没有任何官方文档**（除文件头 6 行注释）；图标尺寸/格式要求、DLC 档的加载顺序未文档化。
