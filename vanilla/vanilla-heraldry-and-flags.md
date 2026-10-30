# 原版解析：纹章与旗帜（vanilla heraldry & flags）

> **一句话**：拆开程序化纹章五层：旗帜规则、4,566 个 COA 本体、随机池、图集与 4,138 张贴图，说明改国旗改的是 `main_menu` 哪一层。
> **什么时候看**：给国家换旗、按条件自动换旗、给附属国加角标，或加图案纹章贴图与改随机旗帜取向时翻这篇。
> **体量**：215 行 · 约 10 分钟通读

版本基准：EU5 1.3.x。**本篇为什么存在**：国旗/家徽是 mod 里改得最多、问得最多的一层，而它**不在 `common\`**——它是 `main_menu\common\` 的一套"程序化纹章"系统：**五层文件 + 一张图集 + 4,138 张贴图**。"改国旗"改的到底是哪一层，本篇给出可查的答案。

| 层 | 位置 | 规模 |
|---|---|---|
| ① 旗帜规则 | `main_menu\common\flag_definitions\00_flag_definitions.txt` | 1 档 / 9,622 行 / 183 KB（**文件头自带官方字段文档**） |
| ② 纹章本体（COA） | `main_menu\common\coat_of_arms\coat_of_arms\*.txt` | **9 档 / 4,566 个 COA 键**（`pre_scripted_countries.txt` 1,112 KB 最大） |
| ③ 随机池 | `main_menu\common\coat_of_arms\template_lists\*.txt` | 5 档（`colored_emblem_lists.txt` **297 KB**） |
| ④ 图集参数 | `main_menu\common\coat_of_arms\options\atlases.txt` | 1 档 / 1.4 KB |
| ⑤ 贴图 | `main_menu\gfx\coat_of_arms\` | **4,138 档 / ≈519 MB**（colored_emblems **3,594** / textured_emblems 478 / patterns 66）+ `gfx\interface\coat_of_arms\flag_texture.dds` 512 KB |

> **一句话模型**：**谁用哪面旗**（① flag_definitions，按 tag + 优先级 + 触发器）→ **那面旗长什么样**（② COA 本体：图案 + 5 个颜色槽 + 纹章图层）→ **随机时从哪些池子里抽**（③ 加权列表，可带条件子池）→ **引擎把它们拼成多大的图**（④ 图集分块）→ **贴图本体**（⑤）。

---

## 一、旗帜规则层：`flag_definitions`

**文件头 17 行就是官方字段文档**（原版少见的自解释文件），语义：

```txt
# 国家会去找"与自己 tag 同名"的列表；DEFAULT 列表【永远】被包含；
# 若没有任何 flag_definition 适用，则【直接拿 tag 当 COA_KEY】。
ENG = {                                     # ← 列表名 = tag（原版 258 个 tag 列表 + DEFAULT）
    flag_definition = {
        coa = ENG                           # 主旗：COA 键，或 list "模板名"
        subject_canton = ENG                # 施加给【自己附属国】的角标
        coa_with_overlord_canton = ENG_subject   # 带宗主角标时用的旗
        allow_overlord_canton = yes         # 默认 no
        priority = 1                        # ★ 生效者 = 通过 trigger 的【最高优先级】
    }
    flag_definition = {
        coa = ENG_nordic
        subject_canton = ENG
        allow_overlord_canton = yes
        priority = 2
        trigger = { <triggers> }            # 作用域见下
    }
}
```

**字段实测（1,133 个 `flag_definition` 块 / 259 个列表）**：

| 字段 | 频次 | 说明 |
|---|---|---|
| `coa` | **1,133（100%）** | 主旗；`[list] COA_KEY` 里的 `list` 关键字表示"去 ③ 的池子里随机" |
| `priority` | **1,133（100%）** | 通过 trigger 的候选中取最高 |
| `trigger` | 875 | 生效条件 |
| `subject_canton` | 105 | 给自己的附属国加角标 |
| `allow_overlord_canton` | 108 | 允许在自己的旗角放宗主角标 |
| `overlord_canton_scale` | 91 | 角标缩放（默认 `{ 0.5 0.5 }`） |
| `overlord_canton_offset` | 2 | 角标位移（默认 `{ 0 0 }`） |
| `coa_with_overlord_canton` | 1 | 带角标版本单独指定（默认 = `coa`） |
| `includes` / `allow_revolutionary_indicator` / `revolutionary_canton` | **0** | 文档写了但原版零使用；文件里自注 "revolutionary is not in use for the moment" |

**trigger 作用域表**（文件头原文，三种场合不同）：

| | 已存在的国家 | 释放一个国家 | 成立国家 |
|---|---|---|---|
| `root` | definition | definition | definition |
| `target` | country | N/A | N/A |
| `initiator` / `actor` | N/A | player | player |
| `overlord` | direct overlord（若存在） | player | direct overlord（若存在） |

**文件头还带一套画布常量与纹章学色码**：`@coa_width = 768` / `@coa_height = 512`、角标缩放预设（`cross` 333×205 / `sweden` 255×204 / `norway` 192×192 / `denmark` 220×220，均 +0.001 防抖）、时代触发器别名（1342 `coa_def_renaissance_age_2_trigger` / 1437 / 1537 / 1637 / 1737）、以及纹章学色码 `A`银 `O`金 `B`蓝 `G`红 `S`黑 `V`绿 `P`紫 `M`棕 `Z`黑白相间 `E`貂皮 `N`橙。

---

## 二、纹章本体层：`coat_of_arms\coat_of_arms\*.txt`

**4,566 个顶层 COA 键**（9 档），结构就是"一层底色图案 + 若干纹章图层"：

```txt
@ratio = 1.5            # 画布常量（宽/高）
@height = 512
@width = @[height*ratio]

DUMMY = {               # 占位/无效旗（原版保留）
    pattern = "pattern_diagonal_split_01.dds"
    color1 = "red"
    color2 = "black"
}
MER = {
    pattern = "pattern_solid.dds"    # 底纹：patterns\ 下的 dds
    color1 = "black"                 # 5 个颜色槽（纹章学色名，见 ① 的色码）
    color2 = "yellow"
    color3 = "white"
    color4 = "red"
    colored_emblem = {               # 叠加的彩色纹章图层
        texture = "ce_star_05.dds"   # colored_emblems\ 下的 dds
        color1 = "white"
        instance = { position = { 0.5 0.5 } scale = { 0.4 0.4 } }   # 定位/缩放
    }
}
```

**字段实测（全 9 档）**：`instance` **17,297** / `color1` 13,375 / `color2` 13,203 / `texture` 8,636 / `colored_emblem` 8,135 / `color3` 5,530 / `pattern` 5,033 / `color4` 1,280 / `color5` 526 / `textured_emblem` 501 / `mask` 498；**`emblem` / `canton` / `color` 三个名字原版零使用**（别照 EU4 习惯写）。

文件分工（按体量）：

| 档 | 体量 | 装什么 |
|---|---|---|
| `pre_scripted_countries.txt` | **1,112 KB** | 历史国家国旗 |
| `pre_scripted_dynasties.txt` | 663 KB | 历史王朝家徽 |
| `00_subs_usa.txt` | 587 KB | 美国各州/分部专用 |
| `00_random_countries.txt` | 245 KB | 随机国家池 |
| `pre_scripted_countries_formable.txt` | 205 KB | 可成立国家 |
| `00_random_dynasties.txt` | 103 KB | 随机王朝池 |
| `00_subs.txt` | 91 KB | 通用附属国 |
| `pre_scripted_countries_japanese_clans.txt` | 41 KB | 日本武家 |
| `pre_scripted_countries_usa.txt` | 20 KB | 美国相关 |

---

## 三、随机池层：`template_lists\`

给"随机旗帜/随机王朝"准备的四类加权池，**语法统一为 `权重 = 值`，并支持带触发器的子池**：

```txt
# 图案池（pattern_lists.txt）
pattern_texture_lists = {
    simple = { 20 = "pattern_solid.dds" }
    pattern_canton = { 10 = "pattern_horizontal_split_01.dds"  20 = "pattern_canton_02.dds" }
}

# 颜色池（color_lists.txt）——special_selection 是【带条件的子池】
color_lists = {
    metal_colors = {
        1 = "white"
        1 = "yellow"
        special_selection = {
            trigger = { coa_def_carpathian_trigger = yes }
            10 = "white"
            3  = "yellow"
        }
    }
}

# 纹章池（colored_emblem_lists.txt，297 KB，原版最大的池）
colored_emblem_texture_lists = {
    charge = {
        1 = "ce_star_05.dds"
        1 = "ce_crescent.dds"
        special_selection = { trigger = { coa_def_christian_heraldry_trigger = yes } … }
    }
}

# 模板池（coa_templates.txt）：决定"随机生成时优先拼哪类构图"
coat_of_arms_template_lists = {
    country = {
        5 = template_charge
        2 = template_charge_metal
        5 = template_geometrical
        1 = template_charge_canton_metal
        …
    }
}
```

**模板本身也是数据**（在 ② 层以 `template = { … }` 存在），可用 `list "simple"` / `list "normal_colors"` 引用池子，还能**槽位互引**（`color1 = color2` 让前景跟随某槽）：

```txt
template_charge = {
    pattern = list "simple"
    color1 = list "normal_colors"
    color2 = list "metal_colors"
    colored_emblem = {
        texture = list "charge"
        color1 = color2
        instance = { position = { 0.5 0.5 } scale = { 0.95 0.95 } }
    }
}
```

---

## 四、图集层：`options\atlases.txt`

引擎把生成的旗帜/家徽**烘焙进图集**，尺寸在这里定（4 个 atlas）：

| tile_size | 注释里的用途 | nr_of_tiles |
|---|---|---|
| `{ 192 128 }` | country_flag_big（96×64 显示） | 672 |
| `{ 144 96 }` | country_flag_mid（72×48） | 1,176 |
| `{ 72 48 }` | country_flag_small（36×24） | 1,176 |
| `{ 360 240 }` | dynasty_shield_bigger（180×120） | 1 |

---

## 五、可改点与硬编码边界

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 给某国换/加国旗 | `coat_of_arms\coat_of_arms\pre_scripted_countries.txt` 加一个 **COA 键**，再到 `flag_definitions` 里用 `coa = <键>` 指定 | 列表名必须是 **tag**；同一 tag 多个 `flag_definition` 时**优先级最高者胜** |
| 按条件自动换旗 | `flag_definitions` 的 `flag_definition.trigger` | trigger 作用域随场合变化（见 ① 的作用域表）；**已存在的国家 target 才是 country** |
| 给自己的附属国加角标 | `subject_canton = <COA 键>` + `allow_overlord_canton = yes` | 反过来，旗上出现宗主角标由**宗主**的 `subject_canton` 决定 |
| 加新图案/纹章贴图 | `main_menu\gfx\coat_of_arms\patterns\`（66 档）/ `colored_emblems\`（3,594 档） | 文件名即脚本里 `pattern = "…dds"` / `texture = "…dds"` 的值 |
| 改随机旗帜的取向 | `template_lists\` 四个池的权重；`coa_templates.txt` 决定构图偏好 | 池里可写 `special_selection = { trigger = {…} 权重 = 值 }` 做条件分支 |
| 改旗子清晰度/图集尺寸 | `options\atlases.txt` | 改 `tile_size` 会影响所有旗帜的显存占用（注释已给显示尺寸对照） |
| 家徽（王朝盾） | `pre_scripted_dynasties.txt` / `00_random_dynasties.txt` | 图集第 4 个 atlas 专管 dynasty_shield |

**硬编码**：旗帜的**烘焙与图集分配**在引擎侧（脚本只提供 COA 与规则）；`priority` 相同时的取舍、图集重建时机、`[list]` 随机池的掷骰时机均不可脚本控制；`allow_revolutionary_indicator` / `revolutionary_canton` 目前**未启用**（原版注释自证）。

---

## 六、中文检索键

概念：`[coat_of_arms|e]`、`[flag|e]` 一类概念词条见 `fields\main_menu-game_concepts.md`。
本地化：COA/旗帜本身**不需要 loc 键**（用的是贴图与 tag 名）；相关界面文案在 `interfaces_l_english.yml`。
贴图：`main_menu\gfx\coat_of_arms\{patterns|colored_emblems|textured_emblems}\`、`main_menu\gfx\interface\coat_of_arms\flag_texture.dds`。
相关档：`fields\main_menu-flag_definitions.md`（① 字段权威）、`fields\main_menu-coat_of_arms.md`（②③④ 字段权威）、`fields\main_menu-common-dlc.md`（DLC 也可增补旗帜内容）。
