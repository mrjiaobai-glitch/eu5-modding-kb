# common/child_educations（儿童教育）

> **一句话**：儿童教育的七个字段与六种原版教育：角色与国家修正、价格为价格 id 而非金数，以及本类目独有的花括号分行写法。
> **什么时候看**：加儿童教育类型，或统计花括号把本文件数错要复查时看。
> **体量**：58 行 · 约 3 分钟通读

来源：`in_game\common\child_educations\readme.txt`（315 B，7 字段）+ 2 个数据文件 **6 种教育**

## 字段（readme 声明）

```
<child education id> = {
    modifier = { <character modifiers> }          # 角色修正
    country_modifier = { <country modifiers> }    # 国家修正
    price_to_select = <price>
    price_to_deselect = <price>
    potential = { <character triggers> }
    allow = { <character triggers> }
    on_education_start_effect = { <character effects> }   # 选中时触发
}
```

## 原版实测（6 种 / 2 文件）

| 文件 | 教育 |
|---|---|
| `00_default.txt` | `balanced_education`、`administrative_education`、`diplomatic_education`、`military_education`、`expensive_in_depth_education` |
| `D008_orthodox_education.txt` | `orthodox_education`（DLC `d008_fate_of_the_phoenix`） |

| 字段 | 出现 | 率 | 备注 |
|---|---|---|---|
| `modifier` | 6 | 100% | 内容形如 `character_child_education = 0.5` / `character_adm_child_education = 0.5` |
| `allow` | 6 | 100% | 原版全是**空块** `{ }` |
| `country_modifier` | 2 | 33% | 如 `court_spending_efficiency = -0.05`（贵价教育） |
| `price_to_select` / `price_to_deselect` | 2 | 33% | 值不是数字而是**价格 id**：`select_expensive_child_education` / `deselect_expensive_child_education`（在 `common\prices\`） |
| `on_education_start_effect` | 1 | 17% | — |
| `potential` | 1 | 17% | — |

本地化：`<id>` = 名称、`<id>_desc` = 描述（`main_menu\localization\<lang>\character_l_simp_chinese.yml:8` 起）。**基座 5 个在基础包 loc 里，DLC 的 `orthodox_education` 只在 `dlc\D008_fate_of_the_phoenix\main_menu\localization\`**——查键要连 DLC 目录一起搜。

## ⚠️ 本类目的花括号写法（最容易数错/解析错的一处）

```
balanced_education =
{                       # ← 名字与 = 在一行，左花括号另起一行
    allow = { }
    modifier = { character_child_education = 0.5 }
}
```

原版 6 个教育**全部**用这种写法。用 `key = {` 单行正则扫描：6 个会被数成 **16**（块内嵌套块被当成顶层），或整体数成 **0**。全库 `common\` 里只有三处用这种风格（`child_educations` 6 处、`resolutions` 3 处、`biases` 1 处），详见 `pitfalls.md` §十三。

## 审查要点

- `potential` / `allow` / `modifier` 均为**角色作用域**，`country_modifier` / `price_to_*` 才是国家侧。
- `price_to_select` / `price_to_deselect` 引用的是**价格 id**（须在 `common\prices\` 存在），不是金数。
- 原版 `allow` 全是空块——把条件写进 `potential`（是否出现在界面）还是 `allow`（可选但受限）要分清。
- 未在 readme 中说明：本地化键格式（`<id>` / `<id>_desc`）、以及 DLC 教育的 loc 在 DLC 目录里。
