# 欧陆风云 5 · 区域／地区速查（本 mod 用到的 area）

> **最后校验**：EU5 1.4.1 / 2026-10-17
> 证据源：`game\in_game\map_data\definitions.txt`（全部行号按该文件实查）
> 用途：写 `any_owned_location = { area = area:X }`、`area = area:X`、解锁门槛时**先查这张表**，不要按地名猜归属。

## 一、怎么查（不要凭感觉）

`definitions.txt` 是**四层嵌套**，一个 `}` 管一整层，肉眼极易看错层级：

```
xxxx_region = {                 ← 区域（region）
    xxxx_area = {               ← 地区（area）★ 触发器和修正都用这一层
        xxxx_province = { … }   ← 省份（province），行内直接列 location 名
    }
}
```

- **触发器的层级只有 region / area / province / location**，`area = area:X` 是 location 作用域下的标准写法
  （本体先例 `estate_privileges\peasants_estate.txt:432`）。
- ⚠️ **用 `Select-String` 找 `^xxx_area = {` 时缩进别写死**：本文件里 `tibet_region` 下的 area 是 **3 个 tab**，
  而 `hindustan_region` 下的是 1 个 tab。用 `^\s*` 或 `^\s+`，否则会得到"0 个 area"的假结论（本项目踩过）。
- ⚠️ **地名不可靠**：`himalaya1`…`himalaya22` 这类地名**同时散布在 `monyul_area`（锡金/不丹）与 `tibet_region`（阿里/藏）两边**，
  按名字猜归属必错。

## 二、本 mod 已占用的区域

| 用途 | area | 所属 region | 证据行 |
|---|---|---|---|
| 喜马拉雅山民管理（解锁门槛 + 三区） | `nepal_area` | `hindustan_region`（:3666） | `definitions.txt:3825` |
| 同上 | `monyul_area`（锡金 + 不丹东西） | `bengal_region`（:3837） | `definitions.txt:3879` |
| 同上 | `upper_indus_area`（克什米尔） | `hindustan_region`（:3666） | `definitions.txt:3789` |
| 青藏居民管理（解锁门槛 + 六区） | `amdo_area`／`changtang_area`／`kham_area`／`ngari_area`／`tsang_area`／`u_area` | 同属 `tibet_region`（:3215-3276） | 该块内 6 个 area、48 个 province |

**两条法律的区域零重叠**（实查 `tibet_region` 块内 area 清单不含上述三区）。

**地点级精确核对（2026-10-17，按 definitions.txt + location_templates.txt 全量解析）**：

| 集合 | 组成 | 地点数 |
|---|---|---|
| 喜马拉雅法 `traditional_himalaya_geography` | `nepal_area` 61 + `monyul_area` 35 + `kashmir_province` 20 + `punjab_hills_province` 15 + `kangra_province` 13 + `balor` 10 + `chitral_province` 5 + `hunza` 5 | **164** |
| 青藏法 `tibet_region` | `amdo_area` 80 + `kham_area` 87 + `ngari_area` 68 + `changtang_area` 52 + `u_area` 35 + `tsang_area` 27 | **349** |
| **交集** | —— | **0** |

抽样核对：`kathmandu`／`gorkha`／`srinagar` 只在喜马拉雅侧；`lhasa`／`shigatse`／`samye`／`tsaparang`／`leh` 只在青藏侧。
**汉人法的集合 `han_geography`（2026-10-17，同日改过范围）**：`scripted_geography` 支持三层混用——
`region = { … }`（本体先例 `01_europe.txt:164-172` 的 `roman_borders_geography`）、`area = { … }`、`province_definition = { … }`。

**"整块 region 减几块、加一块" 的标准做法**：整块 region 只能整体要或整体不要，
所以要么列 area，要么把要拆的那一块降级成省/地点列表。本项目实例（作者要求去掉贵州/大理/云南/台湾、加辽东）：

```txt
han_geography = {
    area = { …28 个 area… liaodong_area }        # 四个中国 region 的 area 逐个列出（剔掉 guizhou/dali/yunnan/taiwan）
    province_definition = { …9 个省… }           # jiangnan_area 拆成省，只为剔掉 songjiang_province（松江是省不是 area）
}
```

- 最终 = **27 个 area + 9 个省 / 1619 个地点**，与喜马拉雅（164）、`tibet_region`（349）仍零重叠。
  （作者随后又剔掉 `liangjiang_area` 两江与 `chuannan_area` 川南：两江 54 地点、川南 69 地点，交集均为 0。）
- **省名 ≠ area 名**：作者说的"松江"，本体是省 `songjiang_province`（属 `jiangnan_area`），不是 area——按 area 名找不到时先搜 `province_names_l_*.yml`。
- `liaodong_area`（辽东）本体属 `manchuria_region`，不在四个中国 region 内，所以要单独加。
- 四个 region 的 area 数：north 9 / east 10 / south 7 / west 7（共 33）；保留 27（north 9 / east 8 / south 2 / west 6）+ 辽东。

**印度斯坦法的集合 `gangetic_geography`（2026-10-17）**：恒河四区（`awadh`／`bhojpur`／`rokhilkhand`／`doab`，
属 `hindustan_region`）+ 孟加拉五区（`bengal`／`mithila`／`jharkhand`／`orissa`／`chota_nagpur`，属 `bengal_region`）
= **9 个 area / 316 个地点**；与喜马拉雅（164）、青藏（349）、汉地（1619）零重叠。

⚠️ **两个 region 都不能整体引用**：
`hindustan_region` 含 `nepal_area`／`punjab_area`／`upper_indus_area`（前两个在喜马拉雅集合里）；
`bengal_region` 含 `monyul_area`（门隅，也在喜马拉雅集合）、`bengal_sea_zones_area`（海域）、
以及刻意不要的 `assam_area`（阿萨姆自成一系）与 `hill_area`（米佐丘陵，近缅印边界）。
⇒ 这就是 KB 前面那条"整块 region 只能整体取舍"的第二次实践：**先列 area，再逐个排除**。

**西印法的集合 `western_sea_geography`（2026-10-17）**：`gujarat_area`（20）+ `sindh_area`（46）+
`saurashtra_area`（22）= **3 个 area / 88 个地点**；刻意排除 `rajputana_area`（74，留给拉其普特军事法）
与 `western_india_sea_zones_area`（18 个**海域**地点——海域不吃陆地地点级修正）。
与恒河（316）、喜马拉雅（164）、青藏（349）、汉地（1619）全部零重叠。

**印度大区的完整切分账（1321 个陆地地点）——已完成，覆盖率 100%**：

| 法 | 地理集合 | area | 地点 |
|---|---|---|---|
| 印度斯坦管理 | `gangetic_geography`（含 2026-10-17 并入的印度河—五河平原：punjab 13 省 + upper_indus 3 省） | 9 area + 16 省 | **445** |
| 西印海洋管理 | `western_sea_geography` | 3 | **88** |
| 拉其普特管理 | `rajput_geography`（rajputana + malwa + vidarbha + gondwana） | 4 | **218** |
| 德干管理 | `deccan_geography` | 8 | **347** |
| 阿萨姆管理 | `assam_geography` | 2 | **59** |
| 合计 | —— | —— | **1321 / 1321** |

五块两两零重叠（实查）。**分工方法论**（可复用到别的次大陆）：
1. 先把大区的全部陆地 area 列出来、算地点数（海域 area 一律剔除）；
2. 按**人群原型**分组（商民 / 军事 / 部落 / 神庙社会），不按方位硬切；
3. 相邻且同属一个历史体系的块**合并进一个法**（本例：印度河—五河平原并入印度斯坦，
   因为它们同属德里苏丹国；旁遮普平原单独开法会没有合适原型、还多消耗键族）；
4. 每块的 `scripted_geography` 与其余块**零重叠**，用具名集合而不是 region 直取。

⚠️ 海域 area（`*_sea_zones_area`、`maldives_area`）一律不入陆地地理集合；`sailan_area`（锡兰）在
`deccan_region` 里但属海岛，做德干法时要单独决定收不收。

**高地诸族（佐米亚）集合 `highland_peoples_geography`（2026-10-17）**：
south_china_region 4 个（yunnan 107 / guizhou 74 / dali 68 / liangjiang 54）+
indochina_region 3 个高地（shan_highland 46 / northern_tai_highland 45 / kachin 33）
= **7 个 area / 427 个地点**。

**"佐米亚切法"——可复用到别的高地单元**：
1. 合并依据用**生态位**（刀耕火种 / 流动 / 不立文字 / 逃避编户），不是民族。同一片山地可以住台语支、
   藏缅、苗瑶三批人，但它们的治理逻辑相同 ⇒ 一部法四条生态位路线（土司文治 / 山地集市 / 祭司 / 游耕）。
2. **高地与低地必须切开**。同一 region 里低地河谷（湄南河 41、依善 74、澜沧 50、伊洛瓦底 39+38、
   红河 60、占婆 32、若开 20、马来半岛 48、东南亚海岸 96 = 498 地点）是稻作国家的地盘，
   经济逻辑相反，留给下一部法。切法：只取山地的 area，不取河谷核心。
3. **先算"留给下一部法多少地"**：本例印度支那 622 = 高地 124 + 低地 498，两边都够开法才算切得干净。

**印度支那的低地集合 `indochina_lowland_geography`（2026-10-17）**：
`indochina_region` 13 个 area 减高地 3 个（shan_highland 46 / northern_tai_highland 45 / kachin 33）
= **10 个 area / 498 个地点**（arakhan 20 / champa 32 / chao_phraya 41 / irrawady 39 / irrawady_delta 38 /
khorat_plateau 74 / lower_mekong 50 / malay_peninsula 48 / red_river_delta 60 / southeast_asian_coasts 96）。

**"一个 region 对半切成两部法"的标准打法（622 = 高地 124 + 低地 498）**：
1. 先按**经济逻辑**分，不按方位：高地＝刀耕火种/流动/逃避编户；低地＝河谷稻作/徭役编户/王朝。
2. 高地那部取**山地 area**、低地那部取**河谷与海岸 area**，两片的 area 清单必须**互补且无交集**。
3. 两部法的四政策也要各按自己的经济逻辑取构件：高地＝土司/集市/祭司/游耕；低地＝僧团/河渠/港口/编户。
4. 只有这样，"游耕流徙"与"编户徭役"才不会出现在同一部法里自相矛盾。

**波斯与呼罗珊集合 `persia_geography`（2026-10-17）**：
`persia_region` 10 个陆地 area（azerbaijan 49 / baluchistan 67 / fars 85 / iraq_ajam 75 / kerman 58 /
khuzestan 40 / kordestan 29 / mazandaran 45 / quhestan 42 / sistan 89 = 579）
+ `eastern_khorasan` 56 + `western_khorasan` 52 + `badakhshan` 51 = **13 个 area / 738 个地点**。
与河中（245）、草原（494）零重叠；`persian_gulf_sea_zones_area` 为海域不计。

**"按治理语法切、不按政权切"的第三个实例**（前两个：印度支那高低地、中亚农商）：
1337 年伊儿汗国解体（1335）后波斯碎成札剌亦儿／楚邦／因朱／穆札法尔／萨尔巴达尔／克尔特等小王朝，
**政权碎了一地，但官僚语法是同一套**（波斯语文书、迪万账册、伊克塔军役、苏菲教团、坎儿井）。
判断"能否合成一部法"时，问的是**下层怎么运转**，不是上层谁在当汗。

**南洋商埠 / 岛屿内陆两集合（2026-10-17）**：
`maritime_southeast_asia_geography` = `indonesia_coasts_area` **342** + `moluccas_area` 33 +
`malay_peninsula_area` 48 = **423 地点**；
`island_interior_geography` = 其余 10 个 area（north_borneo 93 / south_sumatra 90 / celebes 73 /
south_borneo 67 / luzon 63 / north_sumatra 55 / java 53 / mindanao 43 / lesser_sunda 35 / visayas 26）= **598 地点**。
两者零重叠，合起来 = `indonesia_region` 全部 973 + 马来半岛 48。

⚠️ **`indonesia_coasts_area` 是"碎岛海岸带"**：它由 40 个 `*_coast*_province` 组成（riau_coast、java_coast、
sumatra_coast、moluccas_coast、philippines_coast…），把所有岛屿的**海岸线单独抽出来**成一个 area。
游戏对群岛的分层（海岸带 + 各岛核心）**恰好就是史实分层**（季风商埠 vs 灌溉国家/长屋部落），
所以做海洋东南亚时**优先按这个两层切，不要按岛切**——按岛切会让同一条贸易网的港口和内陆分到两部法里。

**跨 region 移动 area 的先例**：`malay_peninsula_area` 原本放在「印度支那管理」（河谷稻作国家），
2026-10-17 移入「南洋商埠管理」——判据是**它靠什么吃饭**（马六甲靠季风与转运，不靠湄公河的水稻）。
跨 region 调整 area 归属是正常操作，只要两边集合同步改、且保持零重叠。

**南岛诸族集合 `austronesian_geography`（2026-10-17）**：
台湾 `taiwan_area` 26 + 琉球 `ryuukyuu_area` 12 + 波利尼西亚 19 area 157 +
美拉尼西亚 14 area 298 + 密克罗尼西亚 8 area 85 = **43 个 area / 578 个地点**。

**"出台湾"切法**：南岛语族从台湾扩散到菲律宾、印尼，再到整个太平洋——所以把台湾、琉球、太平洋三区
编成一部法在学术上是正确的（琉球虽属 `japanese_group`，但是靠航海与朝贡贸易的岛屿王国，可并入）。
四政策取远航文明共同的技术与社会：星象导航 / 双体船 / 芋头与地窖 / 环礁承载。

⚠️ **太平洋的 area 地点数必须实查，不能靠"area 数"估**：`polynesia_region` 有 19 个陆地 area 但共 157 地点
（`tuamotu_area` 28、`totaiete_ma_area` 17），`melanesia_region` 14 个 area 却有 **298 地点**
（`fiji_area` 35、`anim_ha_area` 31、`momase_area` 28）。一开始按"每个小岛 1–2 地点"估成 82 是错的，
**用 definitions.txt 逐个 area 解析才准**。

**日本 / 高丽两集合（2026-10-17）**：
`japan_geography` = 9 个 area / **366 地点**（kinai 10 / toukaidou 61 / tousandou 51 / hokurikudou 27 /
sanindou 27 / sanyoudou 38 / nankaidou 31 / saikaidou 56 / ou 65）；
`korea_geography` = `korea_region` 全 8 area / **191 地点**（gwanbuk 55 / yeongnam 27 / honam 23 /
gwanseo 23 / gwandong 22 / hoseo 16 / haeseo 14 / gyeonggi 11）。

⚠️ **`japan_region` 里有两种"假陆地"要剔**：
① `ryuukyuu_sea_area` 104 与 `sea_of_japan_area` 30 —— 名字带 sea，里面**全是海域地点**
（`topography = ocean / coastal_ocean / inland_sea`，如 `japan_high_sea6`、`miyako_cape`），
但它们**照样出现在 definitions.txt 的地点列表里**，直接按 area 解析会算进陆地数。判据：查 `location_templates.txt`
里该地点的 `topography`，ocean/coastal_ocean/inland_sea 一律不算陆地。
② `ezo_area` 78（北海道）—— 1337 年是阿伊努人的地方，**留白**（作者决定等做「满洲 + 虾夷」时一起收）。

**留白清单（跨法通用，别重复踩）**：`steppes_region` 327（金帐—俄罗斯）、`ezo_area` 78、
`manchuria_region`（含已归汉人法的 liaodong 33）、马达加斯加（`malagasy_group`，不属南岛语族）。

⚠️ **选省要看地点构成，不要看省名**：本项目按"旁遮普山麓"挑省时曾把 `karman_province` 一起加进去，
实查它的地点是 `dinkot`／`isakhel`／`bannu`／`akora`／`hangu`／`khost`／`tank`／`thal_daman`／`salt_range`
——**班努/坦克/盐岭一带（开伯尔南部）**，与喜马拉雅无关，作者一眼看出"多了一个省"。
同批正确的两个是 `kangra_province`（pathankot/nurpur/palampur/nagarkot/mandi/kullu，喜马偕尔）与
`punjab_hills_province`（jammu/chamba/pangi/keylong/himalayas_chanda，查谟-拉胡尔）。
⇒ 判定一个省"是不是某片地理"，**列它的地点名 + 查 `topography`**，别凭省名猜。

⚠️ 本体那批 `himalaya1`…`himalaya22` 的占位地名**多在青藏一侧**（ngari/tsang），只有 `himalaya17`／`himalaya22`（门隅）落在喜马拉雅侧——按名字猜归属会错。
⇒ 两法效果**永不叠加到同一地点**（多民族帝国同时持有时，各自只覆盖自己的那一片）。

## 三、已勘察但**未**占用的地皮

- `tibet_region` 之外仍有藏文化/藏传佛教人口的地皮（如拉达克方向已含在 `tibet_region` 的 `ngari_area`）。
- 计划中的「汉人管理法」「和人管理法」需要各自的东西——记得先按第一节的方法查 `definitions.txt`，
  确认与上表**零重叠**再动手。

## 四、与地形（`topography`）的取舍

喜马拉雅法最初想按 `topography = mountains` 圈范围，实测**不可行**：
`nepal_area` 11 个地点里 mountains×9 / **plateau×1（`kathmandu`）/ flatland×1（`upardang`）** ⇒ 首都直接被排除。
结论：**区域类门槛用 `area =`，不要用 `topography` 单独圈范围**（详见 `guides\law-design.md` §十一）。

### 但 `area` 与 `topography` 可以**叠加**用（2026-10-17 实做）

`area` 管"哪一片"，`topography` 管"这片里哪些地块"，两个条件并写就是"该区域内的山区"：

```txt
potential_trigger = {
    custom_tooltip = { text = <范围说明>  OR = { area = area:nepal_area  … } }
    OR = { topography = mountains  topography = plateau  topography = hills }   # 区内再筛
}
```

- 实测三区：喜马拉雅带吃到 **52+27+46** 个山地/高原/丘陵地点，**排除 9+5+0** 个平原（尼泊尔 Terai、门隅低地）。
- ⚠️ 三类别漏：`kathmandu` 是 `plateau`、`hills` 也常见；只写 `mountains` 会把首都排除。
- ⚠️ 想精确到"某几省"，用 `province_definition = province_definition:<省id>`（**location 作用域可用**，
  本体先例 `building_types\unique_buildings.txt:1735-1741` 的 `location_potential`）。
  注意省 id 不一定带 `_province` 后缀（如 `balor`／`hunza`），以 `definitions.txt` 块名为准。

## 五、把区域名做成**可悬停高亮**（比纯文本直观）

loc 里写 `[ShowAreaName('xxx_area')]` / `[ShowRegionName('xxx_region')]`，玩家把鼠标移到名字上，
**舆图会高亮那片地**。本体 simp_chinese loc 里 `ShowAreaName` 用了 **73 处**、`ShowRegionName` **215 处**
（例：`culture_maghrebi_events_l_simp_chinese.yml:4` 的 `[ShowAreaName('desert_area')]`）。

```txt
 himalayan_law_region_tt: "该修正只作用于下列地区，把鼠标移到地区名上即可在舆图上高亮。[ShowAreaName('nepal_area')]、[ShowAreaName('monyul_area')]、[ShowAreaName('upper_indus_area')]"
 tibet_law_region_tt: "…[ShowRegionName('tibet_region')]（[ShowAreaName('amdo_area')]、…、[ShowAreaName('u_area')]）"
```

- **大范围用 `ShowRegionName`（一次高亮整片），小范围用 `ShowAreaName`（逐个点）**。
- 配合 `location_modifier` 的 `potential_trigger = { custom_tooltip = { text = <上面那个键> … } }`：
  玩家在政策 tooltip 里就能**看到并点出**生效范围，不必再猜"地点级到底盖哪几个地点"。
- 同类函数还有 `ShowLocationName`（181 处，单地点高亮）、`ShowProvinceName`（本版本 **0 处**，别用）。
- ⚠️ 这些函数放在 `*_desc` 里同样会走 tooltip 渲染，注意别在里面再写冒号（见 `pitfalls.md` §二十）。

## 六、自定义地理集合：`scripted_geography`（想点名"某一片地方"就用这个）

`common\scripted_geography\*.txt` 可以把 area / province_definition / location **任意混装**成一个具名集合，
判定、遍历、显示都有配套接口——这是"传统喜马拉雅""罗马旧疆"这类**历史/文化地理**唯一干净的表达方式。
（`topography` 只能近似"山区"，无法表达"哪些地方算喜马拉雅"。）

```txt
# in_game\common\scripted_geography\zzz_himalaya.txt
traditional_himalaya_geography = {
    area = { nepal_area  monyul_area }
    province_definition = { kashmir_province  chitral_province  balor  hunza }   # 省 id 不一定带 _province 后缀
}
```

| 用途 | 写法 | 本体先例 |
|---|---|---|
| location 作用域判定 | `is_in_scripted_geography = scripted_geography:<key>` | `building_types\unique_buildings.txt:1373`（`location_potential` 内） |
| country 作用域判定 | `any_owned_location = { is_in_scripted_geography = scripted_geography:<key> }`，或 `scripted_geography:<key> = { any_location_in_scripted_geography = { owner = root } }` | `decisions\coal_prospecting.txt:7-13` |
| 国家是否踏足 | `has_presence_in = scripted_geography:<key>` | `scripted_geography.info:49-51` |
| 遍历 | `scripted_geography:<key> = { every_location_in_scripted_geography = { … } }` | `scripted_geography.info:16-31` |
| **显示 + 悬停高亮整片** | loc 里 `[ShowScriptedGeographyName('<key>')]` | 本体 loc 26 处（如 `cb_lat_reconquest_valid_locations`） |

- ⚠️ **只要集合在游戏里可见，就必须给 `<key>: "名字"` 的本地化**（`scripted_geography.info:53-54`），否则界面露原始键。
- **已有整块 `region` 能表达的，就直接用 `region`**，别拿 area 拼：`region = region:tibet_region` 在 location 作用域可用
  （先例 `building_types\capital_buildings.txt:1059` 的 `location_potential`；国家侧 `casus_belli\italian_wars_secure_italy.txt:14` 的 `any_owned_location = { region = region:X }`），
  tooltip 用 `[ShowRegionName('tibet_region')]` 同样一次悬停高亮整片。area 逐个列举只在"要比 region 更窄"时才需要。
- 与 `area` 的取舍：**`area` 是引擎划分（会变大变小），`scripted_geography` 是你自己点名**。
  做风味内容（"传统 X 地区"）优先用后者——改范围只动一个文件，法律本体不用改。
- 一个集合可以同时被多部法律/决议引用：范围想调整时**只改这一处**，所有引用同步生效。

