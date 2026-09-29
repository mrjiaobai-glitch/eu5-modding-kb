# 原版解析：地图与地理层级（vanilla map & geography）

> **一句话**：讲五级静态层级树（大陆／次大陆／区域／地区／预设省份／地点）、运行时省份与预设省份的区别，以及地点键、位图色与寻路数据的实查结论。
> **什么时候看**：加地点或改地图层级、核对 location id 与位图色、写 scripted_geography 地理包，或需要区分 province 与 province_definition 时翻这篇。
> **体量**：269 行 · 约 13 分钟通读

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

**核心文件**：`in_game\map_data\default.map`（181KB / 1629 行，地图总声明 + 特殊地点区）、`map_data\definitions.txt`（491KB，**五级静态层级树**）、`map_data\location_templates.txt`（3.7MB / **28573 条**，地点属性表）、`map_data\named_locations\00_default.txt`（28490 条地点→位图色）、`map_data\locations.png`（8.7MB 位图）、`map_data\rivers.png`、`map_data\nodes.dat`（11MB 二进制寻路）、`map_data\ports.csv`（4421 行）、`map_data\adjacencies.csv`（184 行）；脚本侧 `in_game\common\scripted_geography\`（3 文件 / **27 个地理包** + `scripted_geography.info`）；开局数据 `main_menu\setup\start\`（**25 档**，02–27、缺 01/17；本节只解析到 16）。

> **层级定义与运行时"省份"是两件事**，这是本篇最容易被写错的一点：`definitions.txt` 定义的是**预设省份 province_definition**（静态），而游戏里跑着的 **province（省份）** 是"同一预设省份内被同一国家拥有的那组地点"——**同一块地可以因所有权分裂成多个省**。详见 §二。

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `continent` | **大陆** | 9 个（含 4 个"海洋大陆"） |
| `sub_continent` | **次大陆** | 23 个 |
| `region` | **区域** | 82 个 |
| `area` | **地区** | 805 个 |
| `province_definition` | **预设省份** | 4309 个；静态地图分组，定义在 `definitions.txt` |
| `province` | **省份** | **运行时**：同一预设省份内由同一国家拥有的一组 location；可因所有权分裂 |
| `location` | **地点** | 最小地块，≈2.87 万；总是属于某个 province |
| `geography` | 地理 | 层级总称（词条原文：大陆·次大陆·区域·地区·省份·地点构成世界地理） |
| `scripted_geography` | （脚本地理） | mod/事件用自定义地理包，可混合任意层级 |
| `sea_zone` | 海区 | 在 `default.map` 的 `sea_zones` 段（4821 个），不在 `definitions.txt` 的省定义里单独列 |
| `impassable_mountains` | 不可通行山地 | 1878 个（荒地/山脉障碍） |
| `non_ownable` | 不可拥有（走廊） | 153 个（撒哈拉/叙利亚沙漠走廊等） |

## 一、静态层级树：五级 + 地点

`map_data\definitions.txt` 用**缩进即层级**的嵌套写法（同一 `province` 定义行内直接列出成员 location）：

```
europe = {                                   # continent 大陆
	western_europe = {                       # sub_continent 次大陆
		scandinavian_region = {              # region 区域
			svealand_area = {                # area 地区
				uppland_province = { stockholm norrtalje enkoping uppsala kastelholm tierp heby }   # province_definition → locations
				sodermanland_province = { nykoping kolmarden strangnas oppunda sodertalje malaren hjalmaren_lake }
			}
		}
	}
}
```

**实查计数**（花括号深度解析 `definitions.txt`，文件末深度回到 0）：

| 层级 | 数量 | 备注 |
|---|---|---|
| `continent` | **9** | europe, asia, africa, america, oceania + **atlantic_ocean_continent / indian_ocean_continent / pacific_ocean_continent / antarctic_ocean_continent**（海洋也分大陆） |
| `sub_continent` | **23** | 16 个陆地（western_europe, eastern_europe, central_asia, middle_east, east_asia, north_asia, south_asia, south_east_asia, central/east/north/southern/west_africa, north/south_america, australasia, pacific_islands）+ 7 个海洋 |
| `region` | **82** | scandinavian_region / north_german_region / iberia_region / steppes_region … |
| `area` | **805** | svealand_area / home_counties_area … |
| `province_definition` | **4309** | `uppland_province` 这类键 |
| `location` | **≈2.87 万** | `definitions.txt` 内去重 28729；`location_templates.txt` **28573** 条（1:1 清单，最可靠）；`named_locations` 28490 条 |

**省定义有两种书写形态**：单行 `key = { loc1 loc2 ... }`（2832 个）与**多行块**（`kemi_lappmark_province = {` 起头，逐个列出）——统计时只认单行会漏掉约三分之一（这也是把 location 总数一度算成 17060 的原因）。

## 二、运行时层级：province（省份）≠ province_definition（预设省份）

游戏概念词条给出了权威解释（`game_concepts_l_simp_chinese.yml`）：

| 概念 | 词条原文要点 |
|---|---|
| `province` **省份** | "省份由**同一个国家拥有**的一组 location 构成。省份会向其所属地点**共享和分配粮食**。**army_levies 和 navy_levies 也是同时在整个省份内动员**。**大部分 cabinet_actions 的作用对象都是一个省份**。" |
| `province_definition` **预设省份** | "预设省份是一组预先设定的、**决定了游戏中 provinces 构成**的 locations。**area 划分为省份，省份划分为 location**。同一预设省份的所有 location 若由同一国家拥有，则会属于同一 province。**如果多个国家在同一预设省份拥有 location，则会出现多个 provinces**，每个都拥有自己的 **province_capital** 与 **food_stockpile**。" |
| `area` **地区** | "地区由一系列 **provinces** 组成，并且是 region 的一部分。" |
| `location` **地点** | "地点是最小的陆地地块，**总是属于某一个 province**。一个地点的基本属性由 topography、vegetation 以及 climate 定义。地点也可能包含 river。" |

**脚本侧是两套键，别混用**（vanilla 用例数）：

| 键 | 用法 | vanilla 用例 |
|---|---|---|
| `province_definition = province_definition:montana_province` | 静态分组触发器 | **20** |
| `province = province:x` | — | **0（原版从不这样写）** |
| `any_location_in_province` / `every_location_in_province` | 遍历某省的地点 | **101 / 31** |
| `any_location_in_province_definition` / `every_location_in_province_definition` | 遍历某预设省份的地点 | **23 / 10** |
| `every_province_in_area` / `any_province_in_area` | 遍历某地区内的省 | 8 / 1 |
| `every_province_definition_in_area` / `any_province_definition_in_area` | 遍历某地区内的预设省份 | **13 / 10** |
| GUI：`Location.GetProvince` vs `Location.GetProvinceDefinition` | 两个不同的数据链接（`context_menu.gui` / `cooltip.gui` 同时用到两者） | — |

**为什么要有两个**：`province` 会随战争与征服**实时分裂/合并**（多国各占一半 → 两个省，各有省都与粮仓），所以省粮（`provincial_food`，见贸易篇的三级粮食体系）、levy 动员、内阁行动目标都挂在**运行时省份**上；而 `province_definition` 是**永不变的静态地图片区**，`scripted_geography`、事件与建造成立条件用它。

## 三、`default.map`：地图总声明与七个特殊地点区

文件开头声明了地图各数据文件与全局参数：

```
provinces = "locations.png"          # 地点位图
rivers = "rivers.png"                # 河流位图
topology = "heightmap.heightmap"     # 地形高度（⚠️ 见 §五 注）
adjacencies = "adjacencies.csv"      # 跨海/海峡连接
setup = "definitions.txt"            # ★ 五级层级树
ports = "ports.csv"                  # 港口
location_templates = "location_templates.txt"   # ★ 地点属性表

equator_y = 3340
wrap_x = yes                         # 经度环绕
```

随后是 **7 个特殊地点区清单**（每段都是裸地点键列表，注释里带历史考据）：

| 段 | 数量 | 内容 | 谁在用它 |
|---|---|---|---|
| `sea_zones` | **4821** | 全部海区（波罗的海/北海/地中海…按注释分组） | 海军、贸易、海上存在 |
| `lakes` | **919** | 湖泊（含瑞典/芬兰/英伦/意大利/新西兰…，带注释分组） | `topography = lakes`、`is_adjacent_to_lake` |
| `impassable_mountains` | **1878** | 不可通行山地与荒地（含大量环礁小岛） | 阻断行军（含注释 `# Blocks unit movement.`） |
| `non_ownable` | **153** | 不可拥有走廊（撒哈拉/叙利亚沙漠/阿拉伯半岛等 corridor） | 有注释 `# Can be colored by whoever owns the most of the province's neighbors.` |
| `earthquakes` | **3254** | 地震带地点（意大利/巴尔干/安纳托利亚/东地中海…密集） | `earthquake_location_pulse`（见自然环境篇 §四） |
| `volcanoes` | **102** | 火山地点 + 历史喷发注释（Mount St. Helens / Kelud / Tambora / Vesuvius / Fuji…，含"Strength"列） | `volcano_location_pulse` |
| `sound_toll` | **3** | 通行费海峡：`oresund = helsingor`、`gulf_izmit = constantinople`、`strait_hormuz = hormuz` | 贸易篇的海峡通行费收入 |

## 四、`location_templates.txt`：地点属性表（三篇的汇合点）

**28573 条**，每条一个地点，行内写初始属性：

```
stockholm = { topography = flatland vegetation = grasslands climate = continental religion = catholic culture = swedish raw_material = clay natural_harbor_suitability = 0.75 }
malaren  = { topography = lakes climate = continental }      # 湖泊：无植被/无文化宗教/RGO
```

**字段实查（出现数 / 28573）**：

| 字段 | 数量 | 占比 | 接到哪一篇 |
|---|---|---|---|
| `topography` | **28573** | 100% | 自然环境篇（地形 22 类） |
| `climate` | **28573** | 100% | 自然环境篇（气候 8 类） |
| `vegetation` | 22864 | 80% | 自然环境篇（植被 7 类；海/湖无） |
| `religion` | 20929 | 73% | 文化与宗教篇（开局国教分布） |
| `raw_material` | 20929 | 73% | 生产建筑篇（**开局 RGO 商品**） |
| `culture` | 20922 | 73% | 文化与宗教篇（开局文化分布） |
| `natural_harbor_suitability` | 4418 | 15% | 港口潜力 → `static_modifiers\location.txt:44–75` 的 `location_template_natural_harbor_suitability(_poor/_good)` |
| `movement_assistance` | 749 | 2.6% | 行军辅助（通道/山口） |
| `modifier` | 101 | 0.35% | 地点常驻修正 |

> 三条推论：①**73% ≈ 陆地可拥有地点**（其余是海区/湖泊/荒地，没有文化宗教与 RGO）；②**只有 15% 的地点有港口潜力**（4418 个，与 `ports.csv` 的 4421 行基本对应）；③改地图"历史"最直接的一层就在这里——把某地的 `raw_material` 换成煤，比改任何脚本都彻底。

## 五、其它地图数据文件

| 文件 | 规模 | 格式/作用 |
|---|---|---|
| `locations.png` | 8.7MB | **地点位图**：每地点一种颜色；`named_locations\00_default.txt`（**28490 条**）给出 `地点键 = 6 位十六进制色`（如 `stockholm = dda910`）作为对照 |
| `rivers.png` | 1.5MB | 河流位图（决定 `has_river`、`num_roads` 无关，但影响邻近度与贸易） |
| `nodes.dat` | 11MB | 二进制寻路节点 |
| `ports.csv` | 4421 数据行（文件 4,422 行 = 1 表头 + 数据） | `LandProvince;SeaZone;x;y;`——陆地地点↔海区↔像素坐标；**4421 行全部带海区** |
| `adjacencies.csv` | 184 数据行（文件 185 行 = 1 表头 + 数据） | `From;To;Type;Through;start_x;start_y;stop_x;stop_y;Comment`；例 `messina;reggiocal;sea;strait_messina;8396;5518;8404;5519;xxx`。**184 行的 Type 全部是 `sea`**——原版只有跨海/海峡连接，没有人造陆地连接 |
| `generated_locators_port.txt` | 133 B | 港口 3D 定位器（`game_object_locator{ name="port" ... instances={} }`，实例由引擎生成） |

> ⚠️ `default.map` 声明的 `topology = "heightmap.heightmap"` **在 `game\map_data\` 里不存在**（全盘只有 `in_game\gfx\terrain2\heightmap.png` 及其贴花）——地形高度是引擎侧/生成物，**不是可脚本修改的地图数据**。

## 六、`scripted_geography`：自定义地理包（mod 最实用的地理工具）

权威：`common\scripted_geography\scripted_geography.info`（1685B）。

```
borneo_geography = {
	area = { north_borneo_area south_borneo_area }        # 可混任意层级组件
	province_definition = { riau_islands_province }
	location = { ... }                                    # location 也能直接列
}
```

| 能力 | 写法 |
|---|---|
| 作用域链接 | `scripted_geography:borneo_geography = { ... }` |
| 专属脚本列表 | `every_location_in_scripted_geography`、`every_area_in_scripted_geography`、`every_present_country`（也可用现成列表如 `any_location_in_scripted_geography` / `random_location_in_scripted_geography`） |
| 归属判定 | `is_in_scripted_geography = scripted_geography:x`——**location、province_definition、area、region、sub_continent、continent 六个作用域都能用** |
| 存在度判定 | `has_presence_in = scripted_geography:x` |
| 本地化 | `[ShowScriptedGeographyName(Arg0)]`；可见于游戏时必须写 `<key>: "名字"` |

**原版 3 个文件 / 27 个地理包**：

| 文件 | 包数 | 用到的层级组件 | 内容 |
|---|---|---|---|
| `00_event_scripted_geography.txt` | **8** | area×3、province_definition×5、location×4 | `chinese_canal_geography`（大运河 40 地点）、`ducal_prussia_geography` / `royal_prussia_geography`（条顿事件用）、`albanian_migration_source_geography` / `..._destination_geography`（阿尔巴尼亚移民）、`frankokratia_geography`、`claims_of_thessalonica_geography`、`scaligeri_conquests_geography` |
| `01_europe.txt` | **18** | area×16、province_definition×9、region×3 | `scotland_geography`、`england_geography`、`netherlands_geography`、`portugal_geography`、`baltic_sea_geography`、`mediterranean_sea_geography`、`greece_geography`、`kingdom_of_italy_geography` … |
| `02_great_wall.txt` | **1** | location×1 | `historical_route_of_great_wall`（**长城历史路线**，约 80 个地点，分"古长城线"与"明长城线"，注释说明与 `building_triggers.txt` 的 `is_historical_route_of_great_wall` 镜像） |

## 七、脚本接口

**层级触发器**（vanilla 用例数，写法都是 `<层> = <层>:<键>`）：

| 触发器 | 用例数 | 示例 |
|---|---|---|
| `region` | **297** | `region = region:north_german_region` |
| `sub_continent` | 124 | `sub_continent = sub_continent:north_africa` |
| `area` | 117 | `area = area:desht_kipchak_area` |
| `continent` | 69 | `continent = continent:asia` |
| `province_definition` | 20 | `province_definition = province_definition:montana_province` |

**地理脚本列表 24 种**（原版实际出现的，按用法分三组）：

- **地点级**：`any/every/random/ordered_location_in_` × `province`(101/31/2/1) · `province_definition`(23/10) · `area`(34/16/3) · `region`(13/11) · `sub_continent`(1/1) · `continent`(2/3) · `scripted_geography`(3/2/2)
- **预设省份级**：`any/every_province_definition_in_area`(10/13)
- **省份级**：`any/every/random_province_in_area`(1/8/…)；另见 `every_province_in_area` 8 处
- **地区级**：`any/every_area_in_region`(5/45)、`every_area_in_scripted_geography`(1)

**其它地点级触发器**（`trigger_localization\location_triggers.txt`）：`topography`（:7）、`vegetation`（:1）、`climate`（:451）、`is_coastal`（:80）、`is_land`（:390）、`is_port`（:43）、`is_ownable`（:92）、`is_adjacent_to_lake`（:518）、`has_river`（:500）、`num_roads`（:671）、`distance_to`（:620）、`is_in_scripted_geography`（:812）。
国家侧：`has_presence_in`（`country_triggers.txt:2806`）。

**地图模式**（`main_menu\gfx\interface\icons\map_modes\`，130+ 个 `.dds`）与地理直接相关的：`continent` / `sub_continent` / `region` / `area` / `provinces` / `locations`、`topography` / `vegetation` / `climate` / `winter` / `weather` / `terrain` / `rivers` / `roads` / `disease` / `harbor_capacity` / `natural_harbor_suitability`、以及 `culture` / `culture_group` / `language` / `dialect` / `religion`（文化与宗教篇）、`raw_material` / `market` / `market_access` / `development` / `prosperity` / `proximity` / `control`（生产·贸易·POP 篇）。

## 八、开局数据层：`main_menu\setup\start\`（**25 档**）

> **开局数据层已独立成篇**：`setup\start` **全部 25 档**（编号 `02_`–`27_`，缺 `01_` 与 `17_`）+ `in_game\setup\countries\` 46 档 + 模板层 205 档的逐档解析见 **`vanilla\vanilla-setup-data.md`**。下表只列与地图直接相关的文件；其余 10 档（`18_opinions` / `19_diseases` / `20_rivals` / `21_locations` / `22_situations` / `23_colonies` / `24_town_rights` / `25_area_preferences` / `26_ai_personalities` / `27_armies`）以该篇为准。

地图层级只给"地"，**开局的人口/国家/市场/宗教分布在这一层**（`main_menu\setup\start\`，与 `map_data` 分开）：

| 文件 | 规模 | 作用（抽样格式） |
|---|---|---|
| `02_core.txt` | 1.6KB | `institution_manager`（三个初始思潮的出生地：feudalism→aachen、legalism→rome、meritocracy→dadu）+ `religion_manager`（**学派关系**：kindred/enemy，见文化与宗教篇） |
| `03_markets.txt` | 3.7KB | `market_manager = { add_market = lubeck ... }` → 市场中心清单（贸易篇） |
| `04_dynasties.txt` / `05_characters.txt` | 238KB / **2.4MB** | 王朝与角色 |
| `06_pops.txt` | **5.0MB** | `locations = { stockholm = { define_pop = { type = nobles size = 0.031 culture = swedish religion = catholic } ... } }`——**逐地点逐 POP 定义**（POP 篇的源头） |
| `07_cities_and_buildings.txt` | 257KB | 城市与建筑初始分布（生产建筑篇） |
| `08_institutions.txt` | 662KB | 思潮初始传播状态（科技篇） |
| `09_roads.txt` | 33KB | 初始道路（邻近度/控制力） |
| `10_countries.txt` | 1.6MB | `current_age = age_1_traditions` + 各国领土注释标记 `own_control_core` / `own_control_integrated` / `own_control_conquered` / `own_control_colony` / `own_core` / `own_conquered` |
| `11_art.txt` | 21KB | 初始艺术品（文化与宗教篇的"文化影响"来源） |
| `12_diplomacy.txt` | 50KB | 初始外交关系 |
| `13_religion.txt` | 5.6KB | 宗教初始状态 |
| `14_development.txt` | 6.9KB | 初始发展度 |
| `15_international_organizations.txt` | 67KB | IO 初始成员（灾难局势IO 篇） |
| `16_wars.txt` | 14KB | 开局进行中的战争 |

> 另有 `in_game\setup\countries\`（46 文件 + `00_readme.info`）：**逐国的初始定义**（军队/君主/领土分配），与上面的 `start\` 层配合。

## 九、与其它篇的耦合

| 地理层的什么 | 影响 |
|---|---|
| `province`（运行时省份） | **省粮共享**、levy 整省动员、**大部分内阁行动的作用对象**（`promote_culture`/`promote_religion`/`assimilate_area` 的 `looking_for_a = province`）、`province_culture_percentage` / `province_religion_percentage` |
| `province_definition` | `scripted_geography`、建筑/事件的静态成立条件（如 `montana_province`）、`any_location_in_province_definition` |
| `topography` / `vegetation` / `climate` | 战斗 `defender`/frontage、行军、邻近度、RGO 建造时间与上限、人口容量、粮食、视野、海军损耗、天气衰减、疾病 R0 —— 全在自然环境篇 |
| `raw_material` / `culture` / `religion`（location_templates） | 开局 RGO、文化宗教分布 —— 生产建筑篇 / 文化与宗教篇 |
| `sea_zones` + `ports.csv` + `adjacencies.csv` | 海军、贸易路径、海上存在、封锁 |
| `sound_toll` | 海峡通行费收入（贸易篇） |
| `volcanoes` / `earthquakes` | 两个灾变 pulse（自然环境篇 §四） |
| `impassable_mountains` / `non_ownable` | 行军阻断、控制力与着色规则 |
| `market_manager`（setup） | 市场中心清单（贸易篇） |
| `define_pop`（setup） | POP 初始构成（POP 篇） |

## 十、Mod 改造建议（可改 vs 引擎/位图）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 新增/调整层级归属 | `map_data\definitions.txt`（五级嵌套树） | **位置敏感**：缩进即层级；省定义可单行也可多行 |
| 改地点初始地形/植被/气候/文化/宗教/RGO | `map_data\location_templates.txt` | 一条一行；**这是改"历史地理"最有效的一层** |
| 自定义地理分组 | `common\scripted_geography\*.txt` | 可混任意层级；可见于游戏必须写本地化 `<key>: "名"` |
| 加火山/地震/湖泊/海区/不可通行/走廊 | `default.map` 对应段 | 裸地点键列表，注释可留 |
| 加港口 | `ports.csv`（`LandProvince;SeaZone;x;y;`） | 同时要给地点 `natural_harbor_suitability`（location_templates） |
| 加跨海连接 | `adjacencies.csv`（`From;To;Type;Through;x…`） | 原版 184 行 Type 全为 `sea` |
| 改通行费海峡 | `default.map` 的 `sound_toll`（原版 3 个） | 配套 `prices` 与贸易篇的 `SOUND_TOLL_FACTOR` |
| 改开局人口/国家/市场 | `main_menu\setup\start\*.txt` + `in_game\setup\countries\` | `06_pops.txt` 有 5MB，改动前先备份 |
| 换地图位图 | `locations.png` + `named_locations\00_default.txt`（键=颜色） | 两者必须一致，否则地点无法识别 |

**硬编码/不可脚本化的部分**：`nodes.dat`（寻路，二进制）、`rivers.png`、`heightmap.heightmap`（**本体不存在，引擎侧生成**）、`generated_locators_port.txt` 的实例、`province` 的运行时分裂规则（由所有权驱动）、地图环绕（`wrap_x`）。

## 十一、中文检索键

**地理层级**（`game_concepts_l_simp_chinese.yml`）：`game_concept_geography` 地理、`game_concept_continent` 大陆、`game_concept_sub_continent` 次大陆、`game_concept_region` 区域、`game_concept_area` 地区、`game_concept_province` **省份**、`game_concept_province_definition` **预设省份**、`game_concept_location` 地点（`location_desc` 明确"基本属性由 topography/vegetation/climate 定义"）。

**地形/植被/气候**（`terrains_l_simp_chinese.yml`）：见 `vanilla\vanilla-hazards-and-environment.md` §七。

**地图模式与界面**：`main_menu\gfx\interface\icons\map_modes\`（`continent.dds` / `sub_continent.dds` / `region.dds` / `area.dds` / `provinces.dds` / `locations.dds`）；`attribute_columns\20_province.txt`（省份表格列：`province_population` / `province_culture_percentage` / `province_religion_percentage`）、`attribute_columns\21_province_definition.txt`、`attribute_columns\18_colonial_charter.txt`；GUI 侧 `Location.GetProvince` / `Location.GetProvinceDefinition`。

**脚本本地化函数**：`ShowRegionName` / `ShowSubContinentName` / `ShowScriptedGeographyName` / `ShowDatabase('location_rank')`。
