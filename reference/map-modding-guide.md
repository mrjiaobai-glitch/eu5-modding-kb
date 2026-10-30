# 参考资料：地图修改指南（Map modding）

> **来源**：[EU5 Wiki: Map modding](https://eu5.paradoxwikis.com/Map_modding)（页标 **1.1**），`action=raw` 抓取 2026-10-30。
> **性质**：外部资料，**未在本机实测**；与 1.4.5 可能有差异，标 ⚠️ 的地方以本体文件为准。
> **体量**：约 10 分钟

> ⚠️ **格式铁律**：图片格式必须严格（8-bit RGB、无 alpha、位数/图层正确），否则游戏或地图编辑器**崩溃/卡加载**。

## 一、地图文件清单（`in_game\map_data\`）
| 文件 | 作用 |
|---|---|
| `locations.png` | 地点图：每个唯一颜色 = 一个 location |
| `named_locations\00_default.txt` | `地点ID = 十六进制颜色`（大小写不敏感）；**每个颜色必须且只能定义一次** |
| `definitions.txt` | 层级：continent → subcontinent → region → area → province |
| `location_templates.txt` | 每地的 topography / vegetation / climate / religion / culture / `raw_material` / `natural_harbor_suitability` |
| `ports.csv` | 港口坐标（**分号分隔**） |
| `adjacencies.csv` | 不接壤地点间的连接（海峡等），**9 个字段** |
| `default.map` | 海区、湖泊、荒地（`impassable_mountains`）、走廊（`non_ownable`）、地震/火山带；`equator_y` 赤道坐标 |
| `rivers.png` | 河流 |

## 二、关键规则与崩溃点
- **`locations.png`**：**8-bit RGB、无透明、压缩等级 7**；默认 16384×8192（可用 defines `WORLD_EXTENTS_X/Z` 改）。存错格式 → **加载死循环 + 吃满内存**。
- **每个颜色都要在 `00_default.txt` 定义**，漏一个就崩溃；用画笔/非 100% 硬度会产生"杂色"，极难排查。
- **最低地图规模**：≥256 个陆地 location、≥5 个 province、≥1 个海区、≥1 个港口，否则开机崩溃。
- **海区别混进纯陆地 area**（否则整个 area hover 显示为海区）。
- **`location_templates.txt` 的 religion/culture 是占位**：会被 `main_menu\setup\start\06_pops.txt` 覆盖；若 `06_pops.txt` 没定义 POP，游戏会给空地点补 1 个该文化/宗教 POP——**想留无人地就删掉模板里的定义**。
- **荒地/走廊**只有 topography/vegetation/climate；**湖泊/海区**只有 topography/climate。
- **`ports.csv`**：`<陆地 key>;<相邻海区 key>;<x>;<y>;x`；坐标必须落在**相邻海区像素**（对角不算）；**y 从图底起算**：`EU5 y = WORLD_EXTENTS_Z − 编辑器 y`；**每个沿海海区至少要有一个港口**（海上存在计算需要）。
- **`adjacencies.csv`**：`From;To;Type;Through;start_x;start_y;stop_x;stop_y;Comment`；`Through` 用于海线封锁判定；`Type=sea` 是原版唯一样例。

## 三、地图编辑器
- 启动加 **`-map_editor`**；**建议 32GB+ 内存**。
- **必须用编辑器**：heightmap、terrain texture；**不需要**：location 图、河流图。
- 保存时所有红色警报必须清掉；每个 section 都要选输出目录（哪怕不保存）。
- 烘焙 decals 建议选 **userdir**（输出到文档目录的 `baked_streaming_data`，更安全，需自己搬进 mod）。

## 四、locators（改地图后必做）
报错形如：
```
Locator transform overrides modified. Make sure to update them with the command
[MapObjects.GenerateGameLocators city] and copying the 'Paradox Interactive/Europa Universalis V' locators file into the project
```
处理：
1. 控制台执行 `MapObjects.GenerateGameLocators`，参数：**`city` / `vfx` / `combat` / `unit_stack`**（4 类都跑）；
2. 生成文件在 `Documents\Paradox Interactive\Europa Universalis V\`：`generated_locators_*`；
3. 目标在 `in_game\gfx\map\map_objects\`，文件名是 `generated_map_object_locators_*`——**不能直接丢进去**，需粘贴内容并补 **第 6 行 layer**（`layer="cities_layer"` / `layer="vfx_layer"`，见 `reference\settlement-position-guide.md`）；
4. 湖泊：locator 需 `disable_transform_packing = yes` 才正常渲染。

## 五、高度图与地形贴图（decals）
- 原版高度图 = **16 个 8192×4096 decal**（`heightmap_1_16`…`heightmap_16_16`，左上起、先下后右），默认锁定；另有 5 类山脉/火山 decal 多次实例化。
- decal 组成：≤16 张 8-bit 灰度 mask（MASK 0–15 对应海岸/丘陵/高原/山地/湿地/过渡/河流/边界/植被/气候/变化等）+ 16-bit 灰度 bitmask + 16-bit 灰度高度图。
- 地形贴图：`gfx\terrain2\materials.txt` 按 **climate-topography-vegetation 三元组** 映射 16 个 mask 到 `textures\*` 贴图。
- 水位：defines `WATER_LEVEL_PERCENTAGE`（默认 **8.34%** ≈ RGB 21,21,21）。
- 烘焙：`Instance(s) Properties → Baked → Bake decals`，把 `.bin/.info` 拷到 `gfx\terrain2\terrain_cache\`。
- 风险：没烘焙就点某些按钮会**编辑器崩溃**。

## 六、河流/样条
报错：`[jomini_spline_network_graphics_old.cpp:1416]: Could not find spline network strip of type 0 between anchors …`
→ 改地图后需**重新生成 spline network**（原文截断，具体命令以本体为准 ⚠️）。

## 七、和本库的关系
- 现有地图结构（五级树/经济地图等）：`vanilla\vanilla-map-and-geography.md`
- 定局位置/坐标：`reference\settlement-position-guide.md`
- 地图对象生成层：`vanilla\vanilla-dlc-and-assets.md` §六
- 地形修正对战斗：`vanilla\vanilla-combat.md` §四
