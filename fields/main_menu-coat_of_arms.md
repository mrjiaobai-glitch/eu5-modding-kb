# main_menu/common/coat_of_arms（纹章本体 + 随机池 + 图集）

> **一句话**：纹章本体、随机池与图集三层结构：字段频次、模板写法、池名引用语法与图集规格，另附贴图路径。
> **什么时候看**：写或改纹章、要接随机池，或核对贴图文件名与路径写法时翻这篇。
> **体量**：114 行 · 约 6 分钟通读

来源：**本类目没有 readme**——但**文件头带自定义常量与色码注释**；以下为 9 个 COA 档（**4,566 个顶层键**）+ 5 个池档 + 1 个图集档实查。

> 机制全貌（旗帜规则层、烘焙流程、可改点表）见 `vanilla\vanilla-heraldry-and-flags.md`。

## 目录构成

```
main_menu\common\coat_of_arms\
├── coat_of_arms\              ← ② 纹章本体（9 档 / 4,566 键）
│   ├── pre_scripted_countries.txt            1,112 KB（历史国家）
│   ├── pre_scripted_dynasties.txt              663 KB（历史王朝）
│   ├── 00_subs_usa.txt                         587 KB
│   ├── 00_random_countries.txt                 245 KB
│   ├── pre_scripted_countries_formable.txt     205 KB
│   ├── 00_random_dynasties.txt                 103 KB
│   ├── 00_subs.txt                              91 KB
│   ├── pre_scripted_countries_japanese_clans.txt 41 KB
│   └── pre_scripted_countries_usa.txt           20 KB
├── template_lists\            ← ③ 随机池（5 档）
│   ├── colored_emblem_lists.txt   297 KB  ← 原版最大的池
│   ├── coa_templates.txt           64 KB（构图模板权重）
│   ├── textured_emblem_lists.txt   19 KB
│   ├── color_lists.txt             17 KB
│   └── pattern_lists.txt          1.4 KB
└── options\atlases.txt        ← ④ 图集（4 个 atlas）
```

贴图（不在本目录）：`main_menu\gfx\coat_of_arms\{patterns 66 档|colored_emblems 3,594 档|textured_emblems 478 档}` ≈ **519 MB**，另有 `main_menu\gfx\interface\coat_of_arms\flag_texture.dds`（512 KB）。

## ② 纹章本体：字段（实查频次，共 4,566 键）

```txt
@ratio = 1.5                 # 画布常量（文件头）
@height = 512
@width = @[height*ratio]

<COA 键> = {
    pattern = "pattern_solid.dds"      # 底纹（patterns\ 下的文件名）
    color1 … color5 = "red"            # 5 个颜色槽（纹章学色名 / 或写 list "<颜色池>"）
    colored_emblem = {                 # 彩色纹章图层（可多个）
        texture = "ce_star_05.dds"     # colored_emblems\ 下的文件名
        color1 … color3 = "white"      # 图层自己的色槽
        instance = { position = { 0.5 0.5 }  scale = { 0.4 0.4 } }
    }
    textured_emblem = { … }            # 带贴图的纹章图层（501 处）
    mask = { … }                       # 蒙版（498 处）
}
```

| 字段 | 频次 | | 字段 | 频次 |
|---|---|---|---|---|
| `instance` | **17,297** | | `color3` | 5,530 |
| `color1` | 13,375 | | `pattern` | 5,033 |
| `color2` | 13,203 | | `color4` | 1,280 |
| `texture` | 8,636 | | `color5` | 526 |
| `colored_emblem` | 8,135 | | `textured_emblem` / `mask` | 501 / 498 |

**`emblem` / `canton` / `color` 三个名字原版零使用**（EU4 习惯写法，别写）。

**模板条目**（供 ③ 的 `coa_templates` 引用，同样在 ② 层）：

```txt
template = {
    template_charge = {
        pattern = list "simple"            # 引用池：pattern_texture_lists.simple
        color1 = list "normal_colors"
        color2 = list "metal_colors"
        colored_emblem = {
            texture = list "charge"        # 引用 colored_emblem_texture_lists.charge
            color1 = color2                # ★ 槽位互引：前景跟随某色槽
            instance = { position = { 0.5 0.5 } scale = { 0.95 0.95 } }
        }
    }
}
```

## ③ 随机池：语法与三个池名

统一写法 `权重 = 值`，并支持**带触发器的条件子池** `special_selection = { trigger = {…} 权重 = 值 }`：

| 池档 | 顶层块名 | 池例 | 被谁引用 |
|---|---|---|---|
| `pattern_lists.txt` | `pattern_texture_lists` | `simple` / `pattern_canton` / `pattern_maori`… | `pattern = list "<池名>"` |
| `color_lists.txt` | `color_lists` | `metal_colors` / `normal_colors`… | `colorN = list "<池名>"` |
| `colored_emblem_lists.txt` | `colored_emblem_texture_lists` | `charge` / `ordinaries_shield`… | `colored_emblem.texture = list "<池名>"` |
| `textured_emblem_lists.txt` | `textured_emblem_texture_lists` | — | `textured_emblem.texture = list "<池名>"` |
| `coa_templates.txt` | `coat_of_arms_template_lists` | `country = { 5 = template_charge  5 = template_geometrical … }` | 随机生成时的构图取向 |

条件子池的 `trigger` 用的是与 `flag_definitions` **同一批 `coa_def_*_trigger` 脚本触发器**（如 `coa_def_carpathian_trigger`、`coa_def_christian_heraldry_trigger`）。

## ④ 图集：`options\atlases.txt`

| `tile_size` | 用途（注释） | `nr_of_tiles` |
|---|---|---|
| `{ 192 128 }` | country_flag_big（显示 96×64） | 672 |
| `{ 144 96 }` | country_flag_mid（72×48） | 1,176 |
| `{ 72 48 }` | country_flag_small（36×24） | 1,176 |
| `{ 360 240 }` | dynasty_shield_bigger（180×120） | 1 |

## 审查要点

- **`pattern` / `texture` 只写文件名**（`"pattern_solid.dds"`、`"ce_star_05.dds"`），引擎分别去 `gfx\coat_of_arms\patterns\` 与 `colored_emblems\` 找；**写路径会找不到**。
- **`list "<池名>"` 的池名必须在 ③ 的对应档里存在**（图案池 ↔ `pattern_texture_lists`、颜色池 ↔ `color_lists`、纹章池 ↔ `colored_emblem_texture_lists`）；跨类引用不报错、只是随机结果为空。
- **颜色槽可互引**（`color1 = color2`）——这是模板实现"前景跟随底色"的正规做法，别当成笔误。
- **新增 COA 要同时决定"谁用它"**：只加 COA 键不加 `flag_definitions` 规则，等于没有国家会显示它（随机池除外）。
- **`mask` / `textured_emblem` 用得少但真实存在**（各约 500 处），照抄同类原版条目再改。
- 未在 readme 中说明：本类目**没有任何官方文档**；`instance` 支持的全部子字段、`mask` 的语义、`@[ … ]` 表达式的可用运算符，均由引擎决定（文件头只给了常量与色码样例）。
