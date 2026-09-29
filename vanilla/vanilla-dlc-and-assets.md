# 原版解析：DLC 层与美术资源层（vanilla DLC & assets）

> **一句话**：讲 DLC 描述符与三区镜像的装载方式，以及图片/模型不靠 `.gfx` 注册、按路径直取时的引用规则与资产规模。
> **什么时候看**：加事件插图或任务图标、给单位换外观，或研究 DLC 挂载与资产路径怎么引用时翻这篇。
> **体量**：306 行 · 约 14 分钟通读

## 目录

- [一、DLC 层的结构](#一dlc-层的结构)
  - [cosmetic 包（D015 / D017）：**纯资产，脚本侧没有门控**](#cosmetic-包d015--d017纯资产脚本侧没有门控)
- [二、DLC 定义档（`main_menu\common\dlc\<pack>.txt`，实查字段表）](#二dlc-定义档main_menucommondlcpacktxt实查字段表)
  - [DLC 描述符 `.dlc.json`（与 mod 描述符同形）](#dlc-描述符-dlcjson与-mod-描述符同形)
- [三、图片是怎么被找到的（实测规则）](#三图片是怎么被找到的实测规则)
- [四、2D 资产的目录主干（`main_menu\gfx\interface\`）](#四2d-资产的目录主干main_menugfxinterface)
- [五、单位外观：唯一的官方资产文档在这里](#五单位外观唯一的官方资产文档在这里)
- [六、地图对象生成层（`in_game\content_source\map_objects\`，14 档 / 20 MB）](#六地图对象生成层in_gamecontent_sourcemap_objects14-档--20-mb)
- [七、三个易漏的资产类型（第八轮补齐）](#七三个易漏的资产类型第八轮补齐)
- [八、纹章与旗帜（**已成独立篇**）](#八纹章与旗帜已成独立篇)
- [九、可改点与硬编码边界](#九可改点与硬编码边界)
- [十、中文检索键](#十中文检索键)

版本基准：EU5 1.3.x。**本篇为什么存在**：`vanilla\` 下其它 19 篇都在讲"脚本怎么运作"，而 **DLC 的装载方式**与**图片/模型怎么被脚本引用**这两层此前完全空白——它们却是加一张事件图、给任务配图标、改单位外观时的必经之路。本篇基于三个区与 `dlc\` 的实查（含 170 条事件图片路径的解析实测）。

| 层 | 位置 | 规模 |
|---|---|---|
| DLC 元数据 | `dlc\D000_shared\` | 24 档 / 6.2 MB（含 3 个 DLC 定义 + 11 语言文案 + 图标） |
| DLC 内容 | `dlc\D008_fate_of_the_phoenix\` | **1,141 档 / 539 MB**（`in_game` 765 / `main_menu` 350 / `loading_screen` 23） |
| DLC 内容 | `dlc\D015_ancient_monuments_pack\` / `D017_sacred_sites_pack\` | 107 档 / 41 MB、90 档 / 49 MB（**cosmetic** 型） |
| 2D 资产 | `main_menu\gfx\` | **16,821 档 / 2,124 MB**（`interface\` 下：icons 6,705 / illustrations 1,065 / advance 1,634） |
| 3D 资产 | `in_game\gfx\` | **22,369 档 / 7,614 MB**（models 17,327 / terrain2 4,545 / map 348） |
| 启动界面资产 | `loading_screen\gfx\` | 534 档 / 1,148 MB |
| 单位外观 | `main_menu\gfx\unit_graphics\` | 附件 46 + 材质纹理 309 + 单位构造器 3（**含 3 份官方 .info**） |

> ⚠️ **全库没有任何 `.gfx` 文件**——EU5 不走 CK3/EU4 的 `interface\*.gfx` + `spriteType` 注册制。图片就是**按路径/按名字直接找文件**，没有注册表这一层。

---

## 一、DLC 层的结构

**DLC 目录镜像三区**（和 mod 一样）：

```
dlc\D008_fate_of_the_phoenix\
├── D008_fate_of_the_phoenix.dlc.json     # 描述符（见 §三）
├── in_game\        765 档   ← 脚本内容：advances / bureaucracies / disasters / events …
├── main_menu\      350 档   ← 数据 + 2D 资产
└── loading_screen\  23 档
```

**⚠️ DLC 只新增、不覆盖基础包**（实测：D008 的 **1,141 档与基础包 0 路径碰撞**）。DLC 内容分两种落地方式：

1. **新文件落在 DLC 目录**（上表）；
2. **基础包文件里写 `has_dlc` 分支**——原版 **44 个基础包文件 / 263 处**用到它，**且全部是 `d008_fate_of_the_phoenix`**（实测：基础包里 `d015_*` / `d017_*` 的引用 **0 处**，见下节）：

```txt
potential = {
    has_or_had_tag = BYZ
    has_dlc = "d008_fate_of_the_phoenix"      # ← 带引号，值是 .dlc.json 里的目录名
}
age = age_1_traditions
```

词条文本：`has_dlc_trigger: "Has DLC: $DLC$"` / `NOT_has_dlc_trigger`（`triggers_l_english.yml:1753-1754`）。实例分布：`advances\country_byz.txt:35`、`bureaucracies\byz.txt:11`、`disasters\D008_fate_of_the_phoenix.txt:10`、`child_educations\D008_orthodox_education.txt:6`、`building_types\unique_buildings.txt:5188`、`formable_countries\00_formable_countries.txt:1903` 等。

### cosmetic 包（D015 / D017）：**纯资产，脚本侧没有门控**

| 项 | D015 ancient_monuments | D017 sacred_sites |
|---|---|---|
| 档数 | 107 | 90 |
| 扩展名分布 | `.dds` 85 / `.asset` 9 / `.mesh` 9 / **`.txt` 3** / `.json`（描述符） | `.dds` 70 / `.asset` 8 / `.mesh` 8 / **`.txt` 3** / `.json` |
| 有 `common\` 吗 | **没有**（无 `in_game\common`、无 `main_menu\common`） | 同上 |
| 基础包里的引用 | **0 处**（`has_dlc` 与名字都查不到） | 0 处 |

包内那 3 个 `.txt` 就是它们与游戏的全部接口（**都是资产侧**）：

```txt
in_game\gfx\images\D015_images_location.txt         # 地点视图贴图标签
    d015_monuments1 = { tag = "monuments1"
                        illustration_image = { texture_file = "gfx/illustrations/assets/…/empty.dds" } … }

in_game\gfx\map\city_data\D015_ancient_monuments.txt # 城市/地点的 3D 摆放
    mesh_list = { name = "d015_pyramids_mesh"
                  scale_settings_large = { bias = 0 min = 1.3 max = 1.5 }
                  should_spawn_flags = no … }

in_game\gfx\map\city_data\D017_holy_sites.txt        # 文件头声明归属
    @dlc = "d017_sacred_sites_pack"
```

**推论（对 mod 的实操含义）**：cosmetic 包的机制条目（建筑/圣地的**定义**）在**基础包里**，DLC 只提供模型与插图；门控发生在**文件加载层**（没买就不加载该目录），所以**脚本里查不到、也无法用 `has_dlc` 检测**"玩家是否拥有某个 cosmetic 包"。要在 mod 里判断 DLC，只能用 `has_dlc = "<内容包名>"`（原版只对 D008 这么做）。

**基础包不含 `main_menu\common\dlc\`**——该目录只存在于 DLC 里。

---

## 二、DLC 定义档（`main_menu\common\dlc\<pack>.txt`，实查字段表）

> 逐字段权威表与 3 档对照见 **`fields\main_menu-common-dlc.md`**（本节只给机制要点与实例）。

每个 DLC 一个档，原版 3 个（D000_shared 里）：`fate_of_the_phoenix.txt` / `ancient_monuments_pack.txt` / `sacred_sites_pack.txt`。**没有 readme/info，以下为 3 档全字段实查**：

```txt
fate_of_the_phoenix = {                        # 块名 = DLC 键（= .dlc.json 的 name）
    id = "d008_fate_of_the_phoenix/d008_fate_of_the_phoenix.dlc.json"   # 相对 dlc\ 的路径
    name = "fate_of_the_phoenix_key"           # loc 键（DLC 名）
    desc = "fate_of_the_phoenix_dlc_desc"      # loc 键（长描述）
    category = "dlc_immersion_pack"            # 原版取值：dlc_immersion_pack / dlc_cosmetic_pack
    activate = "ancient_monuments_pack_activate_desc"   # loc 键（激活说明；D015 有、D008 无）
    item = "monuments_signup_reward"           # 关联的"订阅奖励"条目（仅 D015）
    type = content                             # content / cosmetic
    color = hsv360 { 287 62 55 }               # 或 rgb { 84 140 85 }——UI 配色
    desc_id = "d008_fate_of_the_phoenix"       # 用于拼 DLC 图标的目录名
    order = 3                                  # UI 排序
    steam_id = "3699010"                       # Steam 商店 ID（D008；cosmetic 包也有）
    review_steam_id = "4097770"                # 评测版 ID（D015/D017 有）
    show_tags_in_ui = { "BYZ" }                # 在 UI 上显示的 tag 标签（仅 D008）
}
```

- 只有 `type = content` 的 DLC 带内容目录（D008）；`type = cosmetic` 的只影响资产/UI。
- 对应的 DLC 图标在 `D000_shared\main_menu\gfx\interface\icons\dlcs\<desc_id>.dds` + `_background.dds`。
- loc 在 `D000_shared\main_menu\localization\dlc\<lang>\dlc_localizations_l_<lang>.yml`（**11 语言全有**）。
- ⚠️ **mod 不能靠这个类目注册自己**：字段里的 `steam_id` / `id`（指向 `.dlc.json`）都是 Steam/DLC 体系的东西。mod 走 `metadata.json`（见 `guides\mod-skeleton.md`）。

### DLC 描述符 `.dlc.json`（与 mod 描述符同形）

```json
{
    "name": "d008_fate_of_the_phoenix",
    "localizable_name": "…", "path": "dlc/d008_fate_of_the_phoenix",
    "picture": "thumbnail.dds", "checksum": "…",
    "supported_version": "", "dependencies": [], "replace_path": [], "tags": [],
    "steam_id": 0, "review_steam_id": 0,
    "affects_save_compatibility": false, "mp_synced": false,
    "third_party_content": false, "enabled": true, "hidden": true, "verify": false
}
```

`D000_shared` 另含 `checksum_manifest.txt`（83 B，只是"这个目录里有这些文件"的清单树）与 `thumbnail.dds`。

---

## 三、图片是怎么被找到的（实测规则）

**规则 1：脚本里的 `image = "gfx/…"` 是"区根相对路径"，引擎在基础包与 DLC 的同一区根下找。**

对原版事件里 **170 条唯一 `image` 路径**实测：

| 落点 | 命中 |
|---|---|
| `main_menu\gfx\…` | **167 / 170** |
| `in_game\gfx\…` | 0 |
| `loading_screen\gfx\…` | 0 |
| `dlc\D008_fate_of_the_phoenix\main_menu\gfx\…` | **3 / 170**（`disaster/fate_of_the_phoenix.dds`、`event/backgrounds/special/greek_fire.dds`、`government/throne_rooms/throne_room_byzantium.dds`） |

→ 脚本里**不写区名、不写 `dlc\`**；UI 类插图全部落在 `main_menu`，DLC 图靠"同一个 `main_menu\gfx\` 相对路径"叠加（D008 自带 **1,121 档 gfx**）。

**规则 2：`icon = <名字>` 按概念去固定目录按名找，可跨主题。**

| 谁在用 | 原版实测 | 解析到 |
|---|---|---|
| 任务链级 `icon` | 11/11 | `main_menu\gfx\interface\illustrations\missions\<icon 值>.dds` |
| 任务节点级 `icon` | **108/108** | `main_menu\gfx\interface\advance\<icon 值>.dds`（节点图标复用"革新"图标） |

链级 `icon` 的值不总等于链名（如链 `generic_infrastructure` 的图标档就叫 `generic_infrastructure.dds`，而 `generic_humiliate_rival_mission_pack.txt` 用的是 `generic_humiliate_rival_mission_pack.dds`）——**档名跟 `icon` 的值走**。

**规则 3：`illustration_tags` 是"权重 = 标签"的加权列表，不是普通标签集合。**

```txt
illustration_tags = {
    10 = exterior
    10 = happy
}
```

原版 **5,177 个事件**用这个块，权重实测 **10 出现 10,245 次、20 只出现 3 次**——实际就是"等权候选列表 + 一个几乎不用的权重位"。

**标签词汇表**（原版 6 个高频 + 少量长尾）与对应素材：

| 标签 | 用次 | 对应素材 |
|---|---|---|
| `interior` / `exterior` | 2,983 / 2,179 | `illustrations\event\backgrounds\{interior\|exterior}\`（exterior 34 档 / interior 26 档） |
| `regular` | 2,449 | `event\characters\`（128 档，无前缀的常规人物图） |
| `happy` / `angry` | 1,026 / 1,226 | 同目录带 `_happy` / `_angry` 后缀的变体 |
| `armed` | 396 | 武装变体 |
| 长尾（各 1 次） | — | `burghers` / `clergy` / `professional` / `military` / `institution` / `economy` / `fire` / `bank` / `interior_peasant` / `characters_discussing` … |

`event\backgrounds\exterior\` 下按阶层再分：`burghers` / `clergy` / `nobles` / `peasants` / `soldiers`（各 6–7 档），命名形如 `ashanti_burghers_exterior.dds`。另有 `backgrounds\special\`（6 档，如 `greek_fire.dds`）与 `event\frontobjects\`（21 档）、`event\vfx\`（5 档）。

---

## 四、2D 资产的目录主干（`main_menu\gfx\interface\`）

| 目录 | 档数 | 用途 |
|---|---|---|
| `icons\` | **6,705** | **112 个主题子目录**的小图标：`modifier_types` **1,377** / `buildings` 473 / `flat_icons` 461 / `government_reforms` 337 / `religion` 294 / `privileges` 259 / `sort` 242 / `laws` 240 / `traits` 160 / `achievements` 151 / `trade_goods` 150 / `map_modes` 129 / `generic_actions` 125 / `diplomatic_actions` 124 / `text_icons` 109 / `international_organizations` 107 … |
| `illustrations\` | **1,065** | **25 个主题子目录**的大插图（根下 0 档，全在子目录）：`units` 293 / `location` 241 / `event` 220 / `international_organization_types` 44 / `societal_values` 33 / `disaster` 32 / `government` 31 / `situation` 23 / `work_of_art` 22 / `institutions` 18 / `advances` 12 / `missions` 11 … |
| `advance\` | **1,634** | 革新/科技图标（`a_central_power.dds`…），**任务节点图标也取自这里** |
| `buttons` 350 / `component_tiles` 301 / `component_decoration` 164 / `graphical_cultures` 165 / `component_masks` 125 / `progressbars` 65 / `colors` 49 / `topography` 35 / `mapitems` 34 / `alerts` 26 / `vegetation` 15 … | | UI 皮肤与地图小图 |

**3D 资产在另一个区**：`in_game\gfx\`（**22,369 档 / 7,614 MB**）——`models` 17,327 档、`terrain2` 4,545 档、`map` 348 档 1.4 GB、`city_materials` 37、`compound_nodes` 82。文件类型以 `.dds` 9,008 / `.json` 3,849 / `.mesh` 3,171 / `.asset` 2,844 / `.anim` 671 为主（另有 `.lnk` 1,025 个，是官方打包时留下的快捷方式残迹，**别照抄**）。**角色"肖像"是 3D 模型**，2D 侧没有 `portraits\` 目录——这正是事件字段 `hide_portraits` 存在的原因。

---

## 五、单位外观：唯一的官方资产文档在这里

`main_menu\gfx\unit_graphics\`（附件 / 材质纹理 / 单位构造器），配套 **3 份官方 `.info`**：

1. **`units\00_units.info`（674 B）——单位构造器**：

```txt
age_5_absolutism:a_militiamen = {     # 键 = <时代>:<unit_type 或 unit_category>，可加 gfx_tag
    entity = "test_unit_shader_entity" # 基础实体（必须）
    attach = {
        use_uniformity = no            # 默认 yes：高统一度单位是否强求同一附件
        70 = helmets                   # 权重 = 附件列表名，可多个
        30 = standard_hats
    }
}
# 附件按脚本顺序求值 → add_tags / require_tags 的顺序有意义
```

数据档：`units\01_army.txt`（7.7 KB）、`units\01_navy.txt`（3.3 KB）；附件清单在 `attachments\`（headgear 7 / torsos 7 / legs 7 / weapons 7 / cavalry 3 / artillery / backpacks / beards / hairstyles / heads / offhand / shields），纹理在 `materials\textures\`（metals 75 / woods 34 / cloth 32 / gambeson_pattern 29 / hair_fur 22 / leather 19 / tattoos 10 / generic 6）。

2. **`illustrations\units\00_naming_convention.info`（683 B）——单位插图 9 级命名优先级**（从最具体到最泛，找不到就往下退）：

```
[unit_category]_[unit_type]_[culture_tag]      # army_infantry_a_genoese_crossbowmen_catalan_gfx
[unit_category]_[unit_type]                    # army_infantry_a_genoese_crossbowmen
[unit_category]_[age_number]_[gfx_tag]_[culture_tag]
[unit_category]_[age_number]_[gfx_tag]
[unit_category]_[age_number]_[culture_tag]
[unit_category]_[age_number]
[unit_category]_[gfx_tag]
[unit_category]_[culture_tag]
[unit_category]                                # army_infantry
```

3. **`attachments\00_audiotags_units.info`（2,630 B）——单位装备状态音频 tag 参考表**（官方自注"仅供团队参考"）。

另有 `loading_screen\input_profile\_input_profile.info`（4,772 B）：输入配置（input context → input action → 键/鼠标/手柄映射）的结构文档——**改按键/加自定义快捷键的唯一起点**。

---

## 六、地图对象生成层（`in_game\content_source\map_objects\`，14 档 / 20 MB）

**地图上的植被、岩石、装饰、环境音对象是"程序化生成"的**，数据在这一层（**不在 `map_data`，也不在 `gfx`**）：

| 子目录 | 档 | 内容 |
|---|---|---|
| `generators\vegetation_generators.txt` | 1 档 / **11.4 KB** | **植被生成器**：每个生成器带 `layer` / `max_density` / `mask = "<掩码名>"` / `meshes = { "<mesh 名>" = 权重 }` |
| `ambience_generators\ambience_generators.txt` | 1 档 / 2.2 KB | **环境音对象生成器**（文件头即字段文档：`entity` + 可选 `topography` / `vegetation` / `climate` / `raw_material` + `avoid_sea`） |
| `masks\*.png` | **12 档 / 20 MB** | 掩码图：`pine_mask` / `jungle_mask` / `rock_mask` / `reeds_mask` / `dirt_mask` / `grassmesh_mask` / `decals_mask` + 4 张 `vegetation_*` |

**原版注释写明的设计法**（`vegetation_generators.txt` 头 9 行）：生成器被拆成 **low / medium / high 三件套**，用宏控制密度——

```txt
@density_factor_low = 0.25     @density_factor_medium = 0.5     @density_factor_high = 1.0
@pine_density_base = 0.95

pine_generator_low = {
    layer = "vegetation_low"
    max_density = @[pine_density_base * density_factor_low]     # 宏算式
    mask = "pine_mask"                                          # → masks\pine_mask.png
    meshes = { "vegetation_diorama_arctic_tree_mesh" = 1  "…tree2_mesh" = 0.7 … }   # 权重
}
```

> 官方注释原文意思：**"每个点位上密度最低的生成器优先"**——三件套是同一批生成器、不同 `max_density`，低密度的会**"抢走"高密度的位置**，于是美术只需为高画质作画，中/低画质自动变稀。

**改法**：加/改生成器（`layer` + `max_density` + `mask` + 加权 `meshes`）→ 掩码图放 `masks\`（**键同名**，脚本里写掩码名不带扩展名）→ mesh 本体在 `in_game\gfx\models\`。掩码图可用官方在音频层随包发的 `sensorgen.py` 同法生成（PIL 脚本，见 `vanilla\vanilla-audio-and-fonts.md` §一.5）。

---

## 七、三个易漏的资产类型（第八轮补齐）

除了 `.dds` / `.mesh` / `.asset`，还有三类**从来没被写过**的资产：

| 类型 | 档数 | 位置 | 说明 |
|---|---|---|---|
| **`.animsm`** | **155** | `main_menu\gfx\animation_state_machines\` **149** + `in_game\gfx\` 4 + DLC 1 + `loading_screen\gfx\` 1 | **模型动画状态机**（每档配一个 `.editordata` 编辑器元数据）；原版实例：`resource_lion_statemachine` / `resources_turkey_idle_statemachine` / `building_han_factory_a_state` |
| **`.particle2`** | **130** | `main_menu\gfx\particles\`（`environment` 43 / `arms` 21 / `legacy` 13 / `particle_effects` 12 / 根 25 …） | **粒子特效**资产 |
| `terrain_decals.json` | 1 | **`in_game\` 区根**（不在 `gfx\` 里） | **地形贴花**：`decal_name: "heightmap_1_16"` + `decals[]`——与 `gfx\map\map_objects\decal_*.txt` 的贴花定义呼应 |

> 另有一个**加载层开关**值得知道：`loading_screen\vfs_skip_files.config`（97 B / 4 行）——**告诉 VFS 跳过哪些文件**，原版列了 `gfx/FX/cw/particle2.fxh`、`gfx/FX/cw/particle2.shader`、`gui/multiplayer_lobby.gui`、`/settings_layout.txt`。排查"某个文件改了却不生效"时它是嫌疑对象之一。

## 八、纹章与旗帜（**已成独立篇**）

**本篇不再覆盖**——`flag_definitions`（259 列表 / 1,133 定义，文件头自带官方 schema）、`coat_of_arms`（9 档 / **4,566 个 COA 键** + 5 池档 + 图集）、贴图 4,138 档 ≈519 MB 的完整机制见：

- **`vanilla\vanilla-heraldry-and-flags.md`**（五层模型 + 可改点表）
- `fields\main_menu-flag_definitions.md`、`fields\main_menu-coat_of_arms.md`（字段权威）

---

## 九、可改点与硬编码边界

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 给事件加一张图 | `main_menu\gfx\interface\illustrations\<主题>\<名>.dds` + 事件里 `image = "gfx/interface/illustrations/<主题>/<名>.dds"` | **路径 = 区根相对路径**，不写区名；`.dds` 为主（全库 `.gfx` 注册表不存在） |
| 事件按情景配图 | 事件里 `illustration_tags = { 10 = happy  10 = exterior }` | **权重 = 标签**；标签走固定词汇表（见 §三），乱写标签等于没配 |
| 给任务链/节点配图标 | 链级 `icon` → `illustrations\missions\`；节点级 `icon` → `advance\` | 档名 = `icon` 的值，不是链名 |
| 给革新/建筑/法律等配图标 | `icons\<主题目录>\<名>.dds`（112 个主题目录） | 目录名按概念固定，改名前先确认脚本里的键 |
| 改单位外观 | `main_menu\gfx\unit_graphics\`（构造器 + `attach` 权重） | 键 `<时代>:<unit_type>`；`entity` 必须存在；附件顺序影响 `add_tags`/`require_tags` |
| 加 3D 模型/地形 | `in_game\gfx\models\` / `terrain2\` | `.mesh` + `.asset` 成对；体量 7.6 GB，改前备份 |
| 改地图上的植被/岩石/装饰/环境音对象 | `in_game\content_source\map_objects\`：生成器（`layer` + `max_density` + `mask` + 加权 `meshes`）+ `masks\*.png`（**掩码名与脚本里同键**） | mesh 本体在 `in_game\gfx\models\`；三件套 low/medium/high 靠 `@density_factor_*` 宏缩放 |
| 让内容只在某 DLC 下出现 | 基础包文件里 `has_dlc = "<dlc 名>"`（配合 `potential`/`trigger`） | 值 = DLC 目录名；mod 不能注册 DLC，只能检测 |
| 改 DLC 名称/图标/顺序 | `dlc\D000_shared\main_menu\common\dlc\<pack>.txt` + `icons\dlcs\` + 11 语言 yml | 只对官方 DLC 有意义 |
| 改按键映射 | `loading_screen\input_profile\`（配 `_input_profile.info`） | — |

**硬编码**：`.dds` 之外只少量接受 `.png`（`loading_screen\gfx` 有 23 个 `.png`、`.cur` 22 个）；DLC 的 `steam_id` 校验、`checksum_manifest.txt` 校验、`third_party_content` 标记、`.dlc.json` 的 `mp_synced` / `affects_save_compatibility` 由启动器与引擎读取。

---

## 十、中文检索键

概念：`[dynamic_historical_events|e]`（DLC 相关 loc 里出现过）、DLC 类目词条（`dlc_immersion_pack` / `dlc_cosmetic_pack`）。
本地化：DLC 名/描述 `dlc_localizations_l_<lang>.yml`（11 语言，`<pack>_key` + `<pack>_desc` + `<pack>_activate_desc`）；`has_dlc_trigger` / `NOT_has_dlc_trigger`。
界面：`icons\dlcs\`（DLC 图标）、任务树与插图见 `vanilla\vanilla-gui.md`。
相关档：`guides\mod-skeleton.md`（mod 描述符 `metadata.json`）、`fields\common-events.md`（`image` / `illustration_tags` 字段）、`vanilla\vanilla-gui.md`（界面层）。
