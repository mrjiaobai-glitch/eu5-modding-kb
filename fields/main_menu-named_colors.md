# main_menu/common/named_colors（具名色板）

> **一句话**：全局具名色板：纹章基础色、地图色与兵模色三档色名，及 rgb 与 hsv360 两种写法。
> **什么时候看**：国家地图色全一样、要加色名或查某色名在哪一档时翻这篇。
> **体量**：44 行 · 约 2 分钟通读

来源：**无 readme**——3 个档实查（`01_coa.txt` 2,031 B / `02_map.txt` **142,164 B / 4,329 行** / `02_units.txt` 102 B）。

## 它是什么

**全局具名色板**：给一个颜色起名，脚本里就能写 `color = <名>`。**`in_game\setup\countries\` 里的国家地图色就是从这里取的**（实测：`color = map_*` 在 46 个国家定义档里用了 **1,004 次**，例如 `ATH` 的 `color = map_athenian` 确实在 `02_map.txt` 里有定义）。

```txt
colors = {                                  # 每档一个容器
    todo_purple = rgb { 1 0 1 }
    white       = hsv360 { 0  0  92 }        # 两种写法都合法
    yellow      = hsv360 { 42 80 85 }
    map_debug   = rgb { 255 0 255 }
    map_arpitan = rgb { 235 196 231 }
}
```

## 原版实测（三档合计 **4,103 个色名**）

| 档 | 色名数 | 用途 |
|---|---|---|
| `01_coa.txt` | **50** | **纹章基础色**（`white` / `yellow` / …——`coat_of_arms` 里 `color1 = "white"` 就取这里） |
| `02_map.txt` | **4,051**（其中 **`map_*` 3,744**） | 地图色（**沿用 EU4 的地图配色命名**，如 `map_austrian`）；国家地图色、地图模式配色 |
| `02_units.txt` | 2 | 兵模配色（`unit_rifle_green` / `unit_cacadore_brown`） |

写法分布：`rgb { r g b }` **3,786** 处 / `hsv360 { h s v }` **202** 处。

## 关联

- **国家地图色**：`in_game\setup\countries\<地区>.txt` 的 `color` / `color2`（见 `fields\setup-countries.md`、`vanilla\vanilla-setup-data.md`）——可用具名色，也可直接写 `rgb { }` / `hsv360 { }`。
- **纹章**：`coat_of_arms` 的 `color1…color5` 用**带引号的色名**（`color1 = "white"`），取值来自 `01_coa.txt` 与 `template_lists\color_lists.txt`（见 `vanilla\vanilla-heraldry-and-flags.md`）。
- **兵模**：`02_units.txt` 的色名供单位外观使用。

## 审查要点

- **加色名要避免重名**：色板是全局命名空间，重名会覆盖（后加载者胜）。
- `rgb` 值域 0–255、`hsv360` 是 `h 0–360 / s 0–100 / v 0–100`；**两者不可混写**（`hsv360` 写成 `hsv`、或把 0–1 的小数写进 `rgb` 都是静默错误）。
- 引用未定义的色名**不报错**——表现为默认色（地图上全是同一个灰/黑），是"国家颜色全一样"类问题的首查点。
- 未在 readme 中说明：本类目**没有 readme**；色名大小写敏感性、地图色的冲突解决顺序、以及 `map_*` 命名是否必须与 tag 同名（原版多为 `map_<形容词>`，与国家 tag 不一一对应）均未文档化。
