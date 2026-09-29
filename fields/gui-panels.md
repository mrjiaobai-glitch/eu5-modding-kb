# gui/panels/*（面板）

> **一句话**：面板目录的九个主题组结构与官方创建流程，含面板类型名的硬要求与对象名占位文件的约定。
> **什么时候看**：加灾难、局势或国际组织面板、要按官方流程建文件时翻这篇。
> **体量**：54 行 · 约 3 分钟通读

来源：`in_game\gui\panels\disaster\readme.txt`（463 B）、`panels\situation\readme.txt`（540 B）+ `panels\`（**123 文件 / 1.39 MB / 9 个主题组**）实查

## 目录结构（按主题分 9 组）

| 子目录 | 文件 | 体量 | 内容 |
|---|---|---|---|
| `organization\` | 49 | **603 KB** | 各 IO 面板（`parliament.gui` 43 KB、`colonial_federation.gui`、各 `foreign_league_*`…） |
| `situation\` | 24 | 298 KB | 各局势面板（`reformation.gui` 25 KB、`colonial_revolution.gui` 12 KB…） |
| `disaster\` | 38 | 187 KB | 各灾难面板 |
| `religion\` | 3 | 125 KB | 宗教面板 |
| `trade\` | 3 | 79 KB | 贸易面板 |
| `market\` | 2 | 56 KB | 市场面板 |
| `goods\` | 2 | 28 KB | 商品面板 |
| `left_panel\` | 1 | 26 KB | 左侧栏 |
| `right_panel\` | 1 | 16 KB | 右侧栏 |

## 官方创建流程（readme 原文，灾难与局势同构）

**灾难面板**（`panels\disaster\readme.txt`）：

1. 查看 `common.gui` 了解可用的构建块；
2. 在 `disasters` 文件夹新建**与灾难同名**的文件；
3. 新文件结构与现有灾难文件相同，**特别要使用 `type = disaster_panel`**；
4. 建议以"包含相似构建块"的现有灾难为基础布局。

**局势面板**（`panels\situation\readme.txt`）：同上，但 `type = situation_panel`，并**建议参考 `rise_of_the_ottomans`** 作为基础布局。

## ⚠️ 原版的"对象名占位"约定（27 B 存根）

`panels\organization\` 下大量文件只有一行，**文件名与内容不对应**：

```
gui/panels/organization/foreign_league_hre.gui    →   organization_war_theme = {}
gui/panels/organization/jurchen_confederation.gui →   organization_geodiplomatic_theme = {}
gui/panels/organization/location_buildings_view.gui → （47 B）
gui/panels/organization/disaster_view.gui          → （56 B）
```

即：**一对象一文件**，文件里放的是"主题块名"而不是完整界面——真正的界面组件在别处（`ui_library.gui` / `shared\`）。新增 IO、局势、灾难时若漏建这个占位文件，对应面板没有主题可挂。

## 审查要点

- **面板类型名是硬要求**：`type = disaster_panel` / `type = situation_panel`（readme 都用了 "Specially using the type …"）。
- **面板文件名要与对象同名**（灾难/局势/IO 的 id），否则引擎找不到面板。
- 布局建议**以原版相似面板为基**（灾难 readme 明确建议），因为构建块（`using` 库控件）与块名契约都在 `ui_library.gui` 与 `shared\` 里。
- 27 B 存根是**契约不是残迹**——看到 `xxx = {}` 不要"清理"它。
- 未在 readme 中说明：其它主题组（organization / religion / trade / market / goods / left_panel / right_panel）**没有 readme**，字段与结构按同构推断 + 实查。
