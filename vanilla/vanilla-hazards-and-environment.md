# 原版解析：天灾人祸与自然环境（vanilla hazards & natural environment）

> **一句话**：覆盖 8 类气候、22 类地形、7 类植被与动态天气、火山地震、7 种疾病及应对行动，说明它们如何互相调用并进入战斗、生产与人口公式。
> **什么时候看**：改地形气候数值、做天气或疫病内容、调人口容量与损耗，或查 `NWeather`／`NDisease` 常量时翻这篇。
> **体量**：404 行 · 约 19 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、气候带（`climates\00_default.txt`，8 类）](#一气候带climates00_defaulttxt8-类)
- [二、地形（22 类）与植被（7 类）](#二地形22-类与植被7-类)
  - [2.1 地形 `topography\00_default.txt`（462 行）](#21-地形-topography00_defaulttxt462-行)
  - [2.2 植被 `vegetation\00_default.txt`（144 行 / 7 类）](#22-植被-vegetation00_defaulttxt144-行--7-类)
  - [2.3 地形/植被是**横切系统**（跨篇接口表）](#23-地形植被是横切系统跨篇接口表)
- [三、天气实体（动态层，引擎硬编码）](#三天气实体动态层引擎硬编码)
  - [3.1 生成效果格式](#31-生成效果格式)
  - [3.2 生成逻辑 `weather_monthly_pulse`（`location_pulses.txt`，2324 行）](#32-生成逻辑-weather_monthly_pulselocation_pulsestxt2324-行)
  - [3.3 强度衰减：`NWeather` + **地形**（不是植被）](#33-强度衰减nweather--地形不是植被)
  - [3.4 Gameplay 挂钩点](#34-gameplay-挂钩点)
- [四、火山与地震（`location_pulses.txt` 的另外两个 pulse）](#四火山与地震location_pulsestxt-的另外两个-pulse)
- [五、疾病（`diseases\`，7 种 + readme 43 行权威）](#五疾病diseases7-种--readme-43-行权威)
  - [5.1 模型与字段](#51-模型与字段)
  - [5.2 七种疾病参数表](#52-七种疾病参数表)
  - [5.3 修正族：**35 个专用修正，原版只用掉 2 个**](#53-修正族35-个专用修正原版只用掉-2-个)
  - [5.4 触发器 · 效果 · 常量](#54-触发器--效果--常量)
  - [5.5 两个局势与 28 个应对行动](#55-两个局势与-28-个应对行动)
  - [5.6 游戏规则（3 个）](#56-游戏规则3-个)
- [六、思潮传播（`movements\`，4 条）](#六思潮传播movements4-条)
  - [6.1 与疾病的同源字段](#61-与疾病的同源字段)
  - [6.2 movements 特有的字段](#62-movements-特有的字段)
  - [6.3 修正族命名（与疾病同一套三档）](#63-修正族命名与疾病同一套三档)
- [七、Mod 改造建议（可改 vs 硬编码）](#七mod-改造建议可改-vs-硬编码)
- [八、中文检索键](#八中文检索键)

版本基准：EU5 1.3.x（行号以实查为准，改版本后用 grep 重新定位）。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 分区 | 文件 | 规模 | 权威 |
|---|---|---|---|
| 气候带 | `in_game\common\climates\00_default.txt` | 167 行 / **8 类** | 无（字段实查） |
| 地形 | `in_game\common\topography\00_default.txt` | 462 行 / **22 类** | 无（字段实查） |
| 植被 | `in_game\common\vegetation\00_default.txt` | 144 行 / **7 类** | 无（字段实查） |
| 天气 | `on_action\location_pulses.txt`（2324 行）+ `defines` `NWeather` | — | 官方在 pulse 注释里给了 `start_weather_system` 格式 |
| 火山地震 | 同上 `location_pulses.txt` + `on_action\country_yearly.txt` | — | 事件文件 `volcano_events` / `earthquake_events` |
| 疾病 | `in_game\common\diseases\` | 7 疾病 + `readme.txt`（**43 行**，本域权威） | 有 readme |
| 常量 | `loading_screen\common\defines\00_defines.txt` | `NWeather`(2564–2573)、`NDisease`(2592–2602) | — |

> **本篇覆盖四大块**：静态地理（气候/地形/植被）→ 动态天气 → 地质灾变（火山/地震）→ 疫病（疾病模型 + 两个局势 + 应对行动）。这四块在原版是**互相调用**的：地形/植被/气候直接进天气衰减与疾病公式，疾病又靠 `winter_level` 与 `has_siege` 做季节性/围城触发。

```
[静态层] climates(8) + topography(22) + vegetation(7)
              │  ├─→ 战斗 defender/frontage、行军 movement_cost、邻近度 proximity
              │  ├─→ 生产 RGO 建造时间/上限、道路建造时间、人口容量、粮食、supply_limit
              │  ├─→ 视野 blocks_vision_*、海军 local_navy_attrition
              │  ↓ 三个 weather_*_strength_change_percent（仅 topography）
[动态层] weather_monthly_pulse → start_weather_system（front/cyclone/tornado）
              ↓ 到达地点
         on_storm_reached_location（原版空）+ 引擎硬编码效果

[灾变层] volcano_location_pulse / earthquake_location_pulse / earthquake_yearly_pulse
[疫病层] diseases(7) ← 地形/植被/气候/winter_level/围城/道路/城镇等级 全进公式
              ↓
         situation:black_death（18 行动）/ situation:great_pestilence（10 行动）
```

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `climate` | 气候 | 8 类：热带/亚热带/海洋性/炎热干旱/寒冷干旱/地中海/大陆性/极地 |
| `topography` | 地形 | 22 类：陆地 5 / 海洋 9 / 荒地 8 |
| `vegetation` | 植被 | 7 类：沙漠/原野/草地/农田/林地/森林/丛林 |
| `winter` / `winter_level` | 冬季等级 | none / mild / normal / severe；被小冰期与**流感**当触发器 |
| `weather_system` | 天气系统 | 引擎实体：锋面 front / 气旋 cyclone / 龙卷风 tornado |
| `disease` | 疾病 | 7 种；`rgo` 式的传播模型 |
| `outbreak` | 疫情 | 一次具体爆发（`disease_outbreak` 作用域，可 `save_scope_as`） |
| `presence` | 存在度 | 0–1，地点感染比例；修正按它缩放 |
| `resistance` | 抵抗 | 0–1，随 `monthly_resistance_reduction` 衰减 |
| `stagnation` | 停滞 | 疾病在原地停止推进（寒带/山地/无路更易停滞） |
| `r0` | 基本传染数 | 一个感染者每周期传染几人 |
| `mortality_rate` | 死亡率 | 0–1 |
| `bubonic_plague` | 腺鼠疫 | 黑死病本体 |
| `great_pestilence` | 大瘟疫 | 美洲殖民灾难（哥伦布大交换） |
| `earthquake` / `volcano` | 地震 / 火山 | 两个独立 pulse |

## 一、气候带（`climates\00_default.txt`，8 类）

| 字段 | 说明 | 原版取值 |
|---|---|---|
| `winter` | 冬季等级 | tropical/subtropical/arid/mediterranean = `none`；oceanic/cold_arid = `mild`；continental = `normal`；arctic = `severe` |
| `has_precipitation` | 是否有降水（视觉） | 仅 `arid`、`cold_arid` = `no` |
| `always_winter` | 永久积雪（**需地形也有该标记**才生效） | 仅 `arctic` |
| `location_modifier` | 常驻修正 | tropical：人口容量 +50%、月度发展 −10%、寿命 −5、粮食衰减 0.002 |
| `colonial_migration_size_modifier` | 殖民迁移规模 | tropical −0.33、arctic −0.3、mediterranean +0.1 |
| `debug_color` / `color` | 调试与地图色 | — |

**`winter_level` 是跨系统的触发键**（三处消费）：
- 局势 `little_ice_age`（`events\situations\little_ice_age.txt`）：severe 省份粮食 −15%、normal −10%、mild −5%
- **流感**：`influenza` 的 `spawn` 里 `OR = { winter_level = normal winter_level = severe }` → 只在寒冬气候地点爆发
- 地形 `blocked_in_winter`（mountains / mountain_wasteland）冬季封路

## 二、地形（22 类）与植被（7 类）

### 2.1 地形 `topography\00_default.txt`（462 行）

分三组，**共 22 类**：

- **陆地 5**：`flatland` 平地 / `hills` 丘陵 / `plateau` 高原 / `wetlands` 湿地 / `mountains` 山地
- **海洋 9**：`ocean` 海洋 / `deep_ocean` 深海 / `coastal_ocean` 近海 / `inland_sea` 内陆海 / `narrows` 海峡 / `lakes` 湖泊 / `high_lakes` 高山湖泊 / `salt_pans` 盐田 / `atoll` 环礁
- **荒地 8**：`flatland_wasteland` / `hills_wasteland` / `plateau_wasteland` / `wetlands_wasteland` / `dune_wasteland` / `mesa_wasteland` / `mountain_wasteland` / `ocean_wasteland`（荒地全组 `movement_cost = 1.0` 且同时 `blocks_vision_from_land` + `blocks_vision_from_sea`）

**陆地五类数值全表**：

| 地形 | movement_cost | proximity | defender | vegetation_density | location_modifier |
|---|---|---|---|---|---|
| flatland 平地 | 1.0 | 0 | — | 1.0 | — |
| plateau 高原 | 1.25 | −0.125 | 1 | 0.8 | frontage −1、`local_rgo_build_time` +0.25、`local_road_building_time` +0.5、发展 −0.10、挡海视 |
| hills 丘陵 | 1.5 | −0.25 | 1 | 0.6 | frontage −3、RGO +0.25、道路 +1.0、发展 −0.25、粮食 −0.1、挡陆+海视 |
| wetlands 湿地 | 1.5 | −0.25 | 1 | 0.6 | frontage −3、RGO +0.25、道路 +1.0、发展 −0.30、粮食 −0.1 |
| **mountains 山地** | **2.0** | **−0.5** | **2** | 0.2 | **`blocked_in_winter = yes`**、frontage −4、RGO +1.0、道路 +3.0、**人口容量 −50%**、发展 −0.50、粮食 −0.2、挡陆+海视 |

**海洋类**：`ocean`（`local_navy_attrition = 2`、`max_attrition = 10`）、`deep_ocean`（**10 / 40**，另有 `is_deep_ocean = yes`）、`coastal_ocean`（`can_have_ice`）、`inland_sea` / `narrows` / `lakes` / `high_lakes` / `salt_pans`（前四者 `can_freeze_over`）、`atoll`；`narrows` 额外 `defender = 1` + frontage −2（海峡卡口）。

**地形字段全集**：`color`、`movement_cost`、`proximity`、`defender`、`vegetation_density`、`blocked_in_winter`、`weather_front_strength_change_percent`、`weather_cyclone_strength_change_percent`、`weather_tornado_strength_change_percent`、`location_modifier`、`colonial_migration_size_modifier`、`has_sand`、`can_have_ice`、`can_freeze_over`、`is_deep_ocean`、`is_lake`、`audio_tags`、`debug_color`（`always_winter` 原版只在注释里）。

**`audio_tags` 是美术/音效驱动键**：`masDataTopologyElevation`（0–1，地形起伏）、`masDataTopologyOcean` / `OceanDeep` / `Coast`、`masDataTopologyLake`；`debug_color` 注释标明供 `topology_screenshot` 地图模式取色。

**荒地两处特殊值**：`dune_wasteland`、`mesa_wasteland` 写 `vegetation_density = -1`，注释说明"低于 Fade_value 以避免随机植被贴片"。

### 2.2 植被 `vegetation\00_default.txt`（144 行 / 7 类）

| 植被 | move | proximity | defender | 人口容量 | 粮食 | supply_limit | frontage | RGO / 道路建造 | 视野 |
|---|---|---|---|---|---|---|---|---|---|
| desert 沙漠 | 1.1 | −0.05 | — | **10** | −0.33 | −5 | — | RGO +0.5 / 道路 +1.0 | — |
| sparse 原野 | 1.0 | — | — | 25 | — | — | — | 道路 −0.10 | — |
| grasslands 草地 | 1.0 | — | — | 50 | **+0.1** | +2 | — | — | — |
| farmland 农田 | 1.1 | −0.05 | — | **100** | **+0.33** | +5 | — | RGO **上限 +0.10**、道路 +0.20 | — |
| woods 林地 | 1.25 | −0.125 | 1 | 50 | +0.1 | +2 | −2 | 道路 +0.5 | 挡海视 |
| forest 森林 | 1.5 | −0.25 | 1 | 25 | — | — | −3 | RGO +0.33 / 道路 +1.0 | 挡陆+海视 |
| **jungle 丛林** | **2.0** | **−0.5** | 1 | 50 | +0.1 | — | **−4** | RGO +0.5 / **道路 +3.0** | 挡陆+海视 |

**植被字段全集**：`color`、`movement_cost`、`proximity`、`defender`、`has_sand`（仅 desert）、`location_modifier`、`colonial_migration_size_modifier`、`audio_tags`（`masDataVegetationDensity` 0.0 desert → 1.0 jungle）、`debug_color`。

> ⚠️ **植被没有** `weather_*_strength_change_percent`、**没有** `blocked_in_winter`、**没有** `vegetation_density`（那是地形的字段）——天气衰减字段**只存在于 topography**。

### 2.3 地形/植被是**横切系统**（跨篇接口表）

| 接到哪一篇 | 用到的字段 |
|---|---|
| 战斗（`vanilla-combat.md` 的 `defender` 骰子表就来自这里） | `defender`（山地 2 / 丘陵·高原·湿地·林地·森林·丛林 1）、`local_frontage_allowed` |
| 行军/补给 | `movement_cost`、`blocked_in_winter`、`can_freeze_over`、`supply_limit` |
| 邻近度与最大控制力 | `proximity`（平地 0 / 高原 −0.125 / 丘陵·湿地·森林 −0.25 / 山地·丛林 −0.5；沙漠 −0.05） |
| 生产建筑篇 | `local_rgo_build_time`、`local_max_rgo_size_modifier`、`local_road_building_time` |
| POP 与粮食 | `local_population_capacity(_modifier)`、`local_monthly_food_modifier` |
| 战争迷雾/情报 | `blocks_vision_from_land` / `blocks_vision_from_sea` |
| 海军 | `local_navy_attrition`、`max_attrition`（深海是近海的 5 倍 / 4 倍） |
| 殖民 | `colonial_migration_size_modifier` |
| 天气（本篇 §三） | 三个 `weather_*_strength_change_percent`（**仅 topography**，原版全 0） |
| 疾病（本篇 §五） | 地形/植被/气候直接进 R0、停滞率、扩散阈值公式 |

## 三、天气实体（动态层，引擎硬编码）

### 3.1 生成效果格式

官方格式写在 `on_action\location_pulses.txt:50` 的注释里：

```
start_weather_system = { width = <pixels> length = <pixels> strength = [0..1]
                         speed = <pixels per day> type = <front/cyclone/tornado>
                         location = <起点> location = <航点> [location = <航点>...] }
```

| 类型 | 用途 | 原版典型参数 |
|---|---|---|
| `front` 锋面 | 季风、寒潮、沙尘暴 | width 800–1200、length 1–100、speed 30–50 |
| `cyclone` 气旋 | 飓风/台风 | width = length = 100、speed 30 |
| `tornado` 龙卷风 | 小范围风暴 | `events\religion\hellenism_religion.txt:1571`（strength 0.5、speed 20） |

效果本地化已注册：`common\effect_localization\weather_effects.txt`（`START_WEATHER_SYSTEM_EFFECT` 等 4 键）。

### 3.2 生成逻辑 `weather_monthly_pulse`（`location_pulses.txt`，2324 行）

`root = location`，每月触发，按 `current_month` 分支：

| 分支 | 条件 | 内容 |
|---|---|---|
| 印度季风·雨季 | `current_month = 2` | 印度洋 → 喜马拉雅/云南，3 条平行锋面（宽 800/1000/1200）+ 3 条西支（经 diu/amet/delhi 入云南）；strength 0.5、speed 30 |
| 印度季风·旱季 | `current_month = 10` | 反向锋面（经 bairab/himalaya12/gaur 出洋）；strength 1、speed 30 |
| 北大西洋飓风季 | `current_month = 5/6/7` | `random_list`（40 权重空 + 十余条路径 10 权重）：塞内加尔/佛得角 → 佛罗里达/百慕大 → 里斯本/南特/布里斯托/苏格兰/法罗 |
| 极地涡旋 | `current_month = 1` | 15 条寒潮锋面（width 1000、length 1、strength 1、speed 50）：欧洲/俄罗斯/西伯利亚×4/白令/阿拉斯加/加拿大×3/格陵兰×2/冰岛/挪威 |
| 撒哈拉沙尘暴 | 文件中段 | 多条北非路径 |
| 热带气旋 | 全年随机 | 东太平洋（约 1/40）、安达曼海（约每 2 年）、阿拉伯海（约每 4 年） |

### 3.3 强度衰减：`NWeather` + **地形**（不是植被）

`defines` 的 `NWeather` 块（2564–2573）：

| 常量 | 值 | 含义 |
|---|---|---|
| `FRONT_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY` | 125 | 锋面每移动 125 像素按地形修正一次强度 |
| `CYCLONE_/TORNADO_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY` | 125 | 同上 |
| `*_DEGRADATION_PER_PIXEL_OF_LATITUDE` | 0 | 纬度衰减（原版禁用） |
| `PIXEL_COUNT_FOR_AVERAGE_LOCATION` | 1600 | 平均地点像素数 |
| `PERCENT_DIFFERENCE_BETWEEN_GOOD_AND_BAD_SIDE_OF_CYCLONE` | 50 | 气旋好侧/坏侧强度差 50% |

**地形侧三个字段**（`flatland` 的注释给出了完整语义：`#every FRONT_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY pixels moved`）：

| 地形 | `weather_front_strength_change_percent` | 注释里保留的设计值 |
|---|---|---|
| flatland | **0** | `-0.08` |
| plateau | **0** | `-0.3` |
| hills | **0** | `-0.4` |
| wetlands | **0** | `-0.1` |
| mountains | **0** | **`-2`** |
| hills_wasteland | **0** | `-0.4` |
| plateau_wasteland | **0** | `-0.15` |
| wetlands_wasteland | **0** | `-0.07` |
| dune_wasteland | **0** | `-0.5` |
| mesa_wasteland | **0** | `-0.07` |
| mountain_wasteland | **0** | **`-4`** |
| ocean_wasteland | **0** | **`+0.01`** |

`weather_cyclone_*` / `weather_tornado_*` 全 22 类一律 `0`（连注释设计值都没留）。

> ⚠️ **这是原版最大的一处"脚本留白"**：天气衰减整条链（3 类型 × 22 地形）原版全填 0，即**天气在移动中不衰减**。填上注释里的设计值就能立刻得到"山地削弱风暴、海面维持强度"的手感——是极低成本的 mod 改造点。

### 3.4 Gameplay 挂钩点

| 钩子 | 位置 | 说明 |
|---|---|---|
| `on_storm_reached_location` | `on_action\_hardcoded.txt:5493-5497` | `root = location`；`scope:weather_system`；**原版 effect 为空** —— 风暴的实际效果由引擎硬编码，脚本只能在此扩展 |
| `has_weather_system` 警报 | `alert_descriptions\00_default.txt:560`（橙色优先级），显示逻辑 `gui\alertmanager.gui:2363-2380` | — |
| 天气地图模式 | `gfx\interface\icons\map_modes\weather.dds` | — |
| 天气 tooltip | `gui\shared\location_tooltips.gui:5984`（`WeatherSystem_tooltip`） | 用 `[WeatherSystem.GetNameWithNoTooltip]`、`[WeatherSystem.GetTooltip]` |
| 天气美术/粒子 | `gfx\models\mapitems\weather\`（desert/sea/snow/volcano 各含 `_*_weather_entities.asset`） | — |

## 四、火山与地震（`location_pulses.txt` 的另外两个 pulse）

| pulse | 机制 |
|---|---|
| `volcano_location_pulse` | **每月检查 `default.map` 的火山地点清单**；`random_events` 四档 `40 = volcano_events.1`（weak，较频繁）/ `30 = .2`（minor）/ `20 = .3`（major）/ `10 = .4`（catastrophic，罕见）；`chance_to_happen = 0.02`（注释：**约每座火山 500 年一次**） |
| `earthquake_location_pulse` | **每月抽 1 个地点**（注释 "checks one random location each month.. yay."）；三档 `16 = earthquake_events.90`（minor）/ `4 = .91`（major）/ `1 = .92`（catastrophic）；`chance_to_happen = 2` |
| `earthquake_yearly_pulse` | `on_action\country_yearly.txt:766`：**40 个历史地震事件**（`earthquake_events.2–.41`），权重 250–400 各一档 |
| 固定事件 | `on_action\country_monthly.txt:43–45`：`earthquake_events.1`（君士坦丁堡地震）、`earthquake_events.1000`（加里波利地震） |

> **火山与地震的地点清单不在这两个 pulse 里，而在 `map_data\default.map`**：`volcanoes`（**102** 个地点，注释带历史喷发与强度）与 `earthquakes`（**3254** 个地点，意大利/巴尔干/安纳托利亚/东地中海最密）。要加/减灾变地点就改那个文件 —— 详见 `vanilla\vanilla-map-and-geography.md` §三。

## 五、疾病（`diseases\`，7 种 + readme 43 行权威）

### 5.1 模型与字段

疾病与 **movements（思潮/运动）共用同一套"传染病模型"**——readme 的字段说明几乎与 `movements\readme.txt` 逐条对应：

| 阶段 | 字段 | 说明（作用域） |
|---|---|---|
| 生成 | `potential` · `monthly_spawn_chance` · `spawn` | `potential` 失败连月度判定都不做；`spawn` 内须调用 `spawn_disease`（scope:disease） |
| 传播 | `r0` · `environmental_infection` · `calc_interval_days` · `location_spread_threshold` · `location_stagnation_chance` · **`sub_unit_stagnation_chance`** | `calc_interval_days` 可为随机区间；`sub_unit_*` 是**军队内部**传播（仅伤寒用） |
| 结算 | `percentage_to_meet_their_fate_on_calc` · `mortality_rate` · `character_mortality_chance` | "每周期有多少感染者被判定命运（获得抗性 or 死亡）" |
| 抵抗 | `monthly_resistance_reduction` | 抵抗每月自然泄漏量（默认 0） |
| 表现 | `location_modifier`（按 presence 缩放） · `map_color` · `secondary_map_color` · **`character_trait`** | — |
| 人群 | `specific_pop_type_effect` | 按 pop 类型 / culture / religion / religion_group / language / language_family 乘倍率 |
| 钩子 | `on_spread_to_country` | root = country |

**readme vs 实查**：
- **readme 漏登记 `character_trait`**（实查 2/7：`bubonic_plague → bubonic_plague_trait`、`smallpox → smallpox_trait`）→ 疾病通过 **traits 接进角色系统**：`traits\08_health.txt:190/202` 里腺鼠疫特质"年痊愈 0.6 / 年死亡 0.55"，天花"0.65 / 0.35"且 `recovery_trait = pockmarked_trait`（中文"麻子"）。也就是说**角色得病后会顶着一个健康特质**。
- **readme 登记了 `potential` 与 `specific_pop_type_effect`，但原版 7 种疾病全都没用**（后者 3/7）。
- **只有 malaria 用 `environmental_infection`**，**只有 typhus 用 `sub_unit_stagnation_chance`**。

**传播规则（readme 原文，与 movements 一字不差）**：
- 不扩散到某地点的条件：目标无人口 / 两国间禁运 / 目标已有 ≥50% 存在度 / 目标已停滞
- 扩散方向：邻接地（人口流动）+ 市场中心（赶集）+ 与之贸易的地点（商人往来）+ 所有者首都（进城）
- 原版额外用到的本地判断：`num_roads`（**有路 → 扩散阈值 ×0.5**，无路且恶劣地形 → **×1.75**）、`has_siege`（围城，伤寒）、`winter_level`（流感）、`location_population_percentage`、`distance_to_squared`（黑死病对起源地 150000 平方距离内强制不衰减）

### 5.2 七种疾病参数表

| 疾病（中文） | `monthly_spawn_chance` | `r0` | `calc_interval_days` | `mortality_rate` | 抵抗衰减/月 | 特点 |
|---|---|---|---|---|---|---|
| `bubonic_plague` **腺鼠疫** | 见下（黑死病局势驱动） | **1.1–1.9** | **25** | **0.3–0.6**（注释：未治疗 40–70%） | 0.0002 | R0 按地形/城镇修正：山地/湿地/沙漠/丛林/极地/热带/乡村 → 1.1–1.3；集镇 1.3–1.6；其余 1.5–1.9。**市场类建筑 ×1.2**（market_village/marketplace/slave_market/entrepot/trading_hub/stock_exchange）；停滞或抵抗 >0.5 → −0.2~0.7。扩散阈值 **0.30**。角色死亡 `0.08 (+0.08 若所有者无 hiding_from_black_death 修正) × presence` |
| `great_pestilence` **大瘟疫** | 0.1（需**美洲出现旧世界所有者**） | 见文件 | 4–16 | **0.75–0.9** | — | **只发生一次**；`spawn` 用 `owner_from_old_world` / `continent:america` |
| `influenza` **流感** | 0.25 | 见文件 | 4–12 | 见文件 | 0.002 | **季节性**：`spawn` 要求 `winter_level = normal/severe`；美洲在 `great_pestilence` 前被排除 |
| `smallpox` **天花** | 见文件 | 见文件 | 4–12 | 见文件 | 0.0007 | 有独立 `location_spread_threshold`；`character_trait = smallpox_trait` |
| `measles` **麻疹** | 见文件 | 见文件 | 20–24 | 见文件 | 0.002 | 与天花/流感同属"大瘟疫"组（`situation:great_pestilence` 分支） |
| `typhus` **伤寒** | 见文件 | 见文件 | 24–36 | 见文件 | 0.002 | **`sub_unit_stagnation_chance = 0.5 − presence×0.5`**（军队内传播）；**`has_siege = yes`**（围城地）；`is_endemic_typhus_location` 触发（3 处） |
| `malaria` **疟疾** | **0（不"爆发"）** | **0（不人传人）** | 10–15 | **0.85–0.95** | 0 | **纯地方病**：靠 `environmental_infection = 0.05` × 气候（tropical **×2**、subtropical ×1.75、oceanic ×0.1、continental/arctic **×0**、arid/cold_arid/mediterranean ×0.15）× 植被（jungle ×1.25、forest ×1、woods ×0.8、farmland/grasslands ×0.75、sparse ×0.25、desert **×0**）× 地形（wetlands **×3**、mountains **×0**）× 沿海 ×0.1；`percentage_to_meet_their_fate_on_calc = 1`（一次结算完） |

**原版"地方病区"用脚本触发器写死**（`scripted_triggers\`）：

```
is_endemic_bubonic_plague_location = {
    OR = { vegetation = grasslands  vegetation = woods  vegetation = forest  vegetation = jungle }
    OR = { region:madagascar / southern_africa / zimbabwe / swahili_coast / great_lakes / kongo /
           western_india / deccan / bengal / indochina / south_china / mongolia / xinjiang /
           tibet / khorasan / caucasus / steppes }
}
is_endemic_typhus_location = { 同植被条件 + 索马里/埃塞俄比亚/几内亚/华西/满洲（流行性）
                               + 南部非洲/马达加斯加/努比亚…（鼠型） }
```

### 5.3 修正族：**35 个专用修正，原版只用掉 2 个**

`modifier_type_definitions\00_modifier_types.txt:10758–11003` 为 7 种疾病各生成 **5 族键**（共 7×5 = **35 个**：21 个 `local_` + 14 个 `national_`）：

| 族 | 键式 | 用途（readme 原话） |
|---|---|---|
| 影响 | `local_<tag>_impact_modifier` | "医院可以带 `local_my_disease_impact_modifier = -0.9`" |
| 抵抗 | `local_/national_<tag>_resistance_modifier` | 提高当地/全国抵抗 |
| 增长 | `local_/national_<tag>_growth_modifier` | 加快/减慢疾病增长 |

另有三项通用抵抗修正：`local_disease_resistance`、`global_disease_resistance`、`rural_disease_resistance`，以及 `losses_to_disease_cost_modifier`（`modifier_types:756`）。

**⚠️ 原版的实际使用情况（实查全 `in_game\common`）**：

| 键 | 原版引用数 |
|---|---|
| `national_malaria_resistance_modifier` | **2**（`0_age_of_absolutism.txt:231` = 0.1、`0_age_of_revolutions.txt:103` = 0.2） |
| `local_disease_resistance` | **1**（`event_only_buildings.txt:2593` = 0.05） |
| 其余 33 个疾病专用键 | **0** |
| `global_disease_resistance`（通用） | 12+（各时代革新 0.03→0.07、文化革新、`artist_types\00_default.txt:121`） |
| `rural_disease_resistance` | 1（`advances\culture_nomadic.txt:10` = 0.25） |

→ **35 个疾病专用修正键 + readme 明示的"医院"用法，原版 33 个完全没用上**：想加"隔离医院 / 检疫站 / 澡堂"等建筑，直接给 `local_<tag>_impact_modifier` 填负值即可，不会与原版内容冲突。

### 5.4 触发器 · 效果 · 常量

**触发器 15 个**（`trigger_localization\disease_outbreak_triggers.txt`）：`num_affected_locations`、`disease_presence`、`disease_outbreak_presence`、`disease_resistance`、`disease_has_stagnated`、`has_any_disease_present`、`disease_is_active`、`disease_outbreak_is_active`、`country_has_disease`、`country_has_disease_outbreak`、`active_outbreak`、`disease_country_deaths`、`disease_outbreak_country_deaths`、`disease_total_deaths`、`disease_outbreak_total_deaths`。
**效果 3 个**（`effect_localization\disease_effects.txt`）：`set_disease_presence`、`change_disease_presence`、`spawn_disease`。

**常量 `NDisease`（`defines:2592–2602`，9 个）**：

| 常量 | 值 | 含义 |
|---|---|---|
| `MAX_DISEASE_PERCENTAGE` | 1 | 存在度上限 |
| `PANDEMIC_LOCATIONS` / `SEVERE_LOCATIONS` / `MODERATE_LOCATIONS` | 1000 / 500 / 200 | 按**感染地点数**分档的定义 |
| `STAGNATION_LENGTH_DAYS` | 1000 | 停滞持续时间 |
| `PANDEMIC_MIN_DEATHS` / `SEVERE_MIN_DEATHS` / `MODERATE_MIN_DEATHS` | 50000 / 1000 / 50（注释：**50M / 1M / 50k**） | 按**死亡人数**分档（显示值 = 内部值 × 1000） |
| `MAX_ACCEPTABLE_PERCENTAGE_DIFFERENCE_TO_AMALGAMATE_POPS` | 0.05 | **疫后 POP 合并容差**（与 POP 篇衔接） |

### 5.5 两个局势与 28 个应对行动

| 局势 | 文件 | 启动/结束条件 | 行动 |
|---|---|---|---|
| `black_death` 黑死病 | `situations\black_death.txt` + 事件 `events\situations\black_death.txt`（**51KB**） | `can_start = disease_is_active = disease:bubonic_plague`；`can_end = NOT { disease_outbreak_is_active = var:original_outbreak }`（用**首个爆发**当锚点）；`visible` 里列出 10 个应对修正 | **18 个**（`generic_actions\black_death.txt`，9 组各带 stop_ 反向）：`hide_from_black_death` / `isolate_cities_black_death` / `control_the_food_market` / `close_the_borders` / `procure_remedies` / `segregate_the_infected` / `strict_quarantines` / `sponsor_sin_forgiveness` / `blame_the_minorities` |
| `great_pestilence` 大瘟疫 | `situations\great_pestilence.txt`（12KB）+ 事件文件 12KB | `can_start = disease_is_active = disease:great_pestilence`；`can_end` 要求 **加勒比/中美洲/安第斯三个区域变量都被感染过**，且美洲"已定居国家 ≤30 且都已历过" | **10 个**（`generic_actions\great_pestilence.txt`）：`great_pestilence_procure_remedies`（仅殖民者）/ `..._segregate_the_infected` / `..._blame_the_minorities`（仅殖民者）/ `sponsor_spiritual_protection` / `no_contact_with_outsiders`，各带 stop_ |

应对产生的国家修正定义在 `main_menu\common\static_modifiers\country.txt:3949` 起（`hiding_from_black_death`、`control_the_food_market`、`close_the_borders`、`segregate_the_infected`、`strict_quarantines`、`procure_remedies`、`sponsor_sin_forgiveness`、`blame_the_minorities`…）。
通用事件：`events\situations\diseases.txt` 的 `diseases.1`（对任一疾病触发，掉繁荣度）。

### 5.6 游戏规则（3 个）

| 规则 | 设置 | 作用 |
|---|---|---|
| `black_death_date_rule` | `black_death_date_historical`（默认）/ `black_death_date_random` / `black_death_date_off` | historical：1346.1.1 起概率 0.2、**1346.10.1 起 = 1.0**；random：文艺复兴时代 0.005 → 发现时代 0.01 → **改革时代 1.0**；off：永不 |
| `black_death_origin_rule` | `black_death_origin_historical`（默认）/ `black_death_origin_random` | historical = **钦察草原 / 阿斯特拉罕 / 下亚伊克** 三区；random = 亚洲 / 欧洲 / 北非 |
| （疾病复发） | 黑死病结束后 `monthly_spawn_chance = 0.005`（约 20 年一次），且目标须满足 `is_endemic_bubonic_plague_location` + `disease_resistance < 0.5` | — |

## 六、思潮传播（`movements\`，4 条）

**这一层和疾病是同一套引擎模型**——观念像瘟疫一样在 POP 之间扩散。原版只有 4 条，但字段结构与疾病逐条对应：

| 条目 | 传播的是 |
|---|---|
| `lutheranism_movement` | 路德宗（宗教） |
| `calvinism_movement` | 加尔文宗（宗教） |
| `hellenism_religion_movement` | 希腊教（宗教） |
| `roman_culture_movement` | 罗马文化（**文化**） |

### 6.1 与疾病的同源字段

| movements 字段 | 疾病里的对应 | 作用域 |
|---|---|---|
| `r0` | `r0` | root = location |
| `environmental_infection` | 同名 | root = location |
| `calc_interval_days` | 同名 | 无作用域 |
| `location_spread_threshold` | 同名 | root = 角色所在 location |
| `location_stagnation_chance` | 同名 | — |
| `specific_pop_type_effect` | 同名（pop_type / culture / religion / language…） | — |
| `location_modifier` | 同名（×presence %） | — |
| `on_spread_to_country` | 同名（事件） | root = country |
| `map_color` | 同名 | — |

**传播规则也与疾病逐条相同**（不传播到：无人口 / 两国间有禁运 / 目标已有 ≥50% / 目标已停滞；传播目标：邻居 · 市场中心 · 市场中心的贸易伙伴 · 所有者的首都）。

### 6.2 movements 特有的字段

| 字段 | 出现率 | 说明 |
|---|---|---|
| `monthly_spawn_chance` | 100% | 每月生成概率（0..1） |
| `spawn` | 100% | 生成效果，**须包含 `spawn_movement`** |
| `religion` **或** `culture` | 75% / 25% | **必须给一个**：传播的是哪种宗教或文化 |
| `development` / `literacy` / `local_control` / `pop_satisfaction` | 各 100% | 四个 `neutral`/`positive`/`negative` 开关：该因素对传播是正还是负（疾病侧没有这层） |
| `on_calc_effect` | 50% | 每个计算日触发（root = movement） |
| `custom_name` | — | 指向 `customizable_localization\` 的自定义名（readme 声明，原版未用） |
| 受众筛选 | 25–75% | `required_{languages / language_families / tags / religions / religion_groups / pop_types / cultures}`（语言与语系、宗教与宗教组可"二选一命中"） |

### 6.3 修正族命名（与疾病同一套三档）

```
local_<tag>_resistance_modifier   / national_<tag>_... / global_<tag>_...
local_<tag>_growth_modifier       / national_<tag>_... / global_<tag>_...
```

原版实例：`national_lutheranism_movement_resistance_modifier`、`national_calvinism_movement_resistance_modifier`——它们正挂在**社会价值观**上（传统/创新轴的传统侧 +0.05、虔诚/人文轴的虔诚侧 +0.1，人文侧 −0.2），形成"社会价值观 ↔ 思潮传播"的双向耦合。

## 七、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 加/改气候 | `common\climates\00_default.txt` | `always_winter` 要地形同标记才生效（仅视觉） |
| 加/改地形 | `common\topography\00_default.txt` | 位置敏感：地图用键名引用；`vegetation_density = -1` 可禁随机植被 |
| 加/改植被 | `common\vegetation\00_default.txt` | 植被**没有**天气衰减与 `blocked_in_winter` |
| **让天气衰减** | 给 22 个地形填 `weather_*_strength_change_percent`（注释里已有设计值）+ `NWeather` 的 125 像素步长 | **原版最大留白**，改完立刻有"山地削弱风暴"手感 |
| 生成新的天气 | 覆盖/新增 on_action 调 `start_weather_system` | 官方格式在 `location_pulses.txt:50` 注释 |
| 风暴到达效果 | 填 `on_storm_reached_location` 的 effect | 唯一脚本钩子（原版空） |
| 火山/地震频率 | `volcano_location_pulse`（0.02）/ `earthquake_location_pulse`（2）的 `chance_to_happen` 与权重 | 火山地点清单在 `default.map`，不在脚本里 |
| 新增疾病 | `common\diseases\<新文件>.txt` | 必须 `spawn` 里调 `spawn_disease`；新 tag 需要自己补三族修正（引擎不会自动生成） |
| 给疾病加"医院" | 用 `local_<tag>_impact_modifier` / `local_<tag>_resistance_modifier`（**原版 40/42 未使用**） | readme 自己举例 `-0.9` |
| 改疾病严重度分档 | `NDisease` 的 6 个分档常量 | 死亡数是内部值 ×1000 |
| 改黑死病时序/起源 | `black_death_date_rule` / `black_death_origin_rule` 游戏规则，或 `diseases\bubonic_plague.txt` 的 `monthly_spawn_chance` 分支 | 起源三区写死在规则文件的脚本根里 |
| 疫情应对行动 | `generic_actions\black_death.txt`（9 组）/ `great_pestilence.txt`（5 组）+ `static_modifiers\country.txt` 的修正 | 行动与修正成对，改一个要改另一个 |

**硬编码不可改**：天气实体的移动、覆盖判定、衰减计算、好/坏侧伤害差异、风暴对单位的实际影响；疾病的月度判定与扩散寻路（邻接+市场+首都）、POP 死亡结算、角色命运判定；火山地点清单（`default.map`）。

## 八、中文检索键

**术语**（`terrains_l_simp_chinese.yml`）：气候 `tropical` 热带 / `subtropical` 亚热带 / `oceanic` 海洋性 / `arid` 炎热干旱 / `cold_arid` 寒冷干旱 / `mediterranean` 地中海 / `continental` 大陆性 / `arctic` 极地；地形 `flatland` 平地 / `hills` 丘陵 / `plateau` 高原 / `wetlands` 湿地 / `mountains` 山地 / `ocean` 海洋 / `deep_ocean` 深海 / `coastal_ocean` 近海 / `inland_sea` 内陆海 / `narrows` 海峡 / `lakes` 湖泊 / `high_lakes` 高山湖泊 / `salt_pans` 盐田 / `atoll` 环礁 / `*_wasteland` 各荒地（平地/丘陵/高原/湿地/**沙丘**/**平顶山**/高山/海洋 荒地）；植被 `desert` 沙漠 / `sparse` 原野 / `grasslands` 草地 / `farmland` 农田 / `woods` 林地 / `forest` 森林 / `jungle` 丛林。

**疾病**（`diseases_l_simp_chinese.yml`）：`bubonic_plague` 腺鼠疫 / `malaria` 疟疾 / `typhus` 伤寒 / `influenza` 流感 / `measles` 麻疹 / `smallpox` 天花；局势 `situations_l_simp_chinese.yml:8` `black_death` 黑死病、`:178` `great_pestilence` 大瘟疫；提示 `hints_l_simp_chinese.yml:461` `hint_black_death` 黑死病、`:579` `hint_great_pestilence`。
**特质**（`traits_l_simp_chinese.yml:260/263/266`）：`smallpox_trait` 天花 / `pockmarked_trait` 麻子 / `bubonic_plague_trait` 腺鼠疫。

**天气**：`WEATHER_RAIN` / `WEATHER_TORNADO` / `WEATHER_CYCLONE` / `WEATHER_SANDSTORM`（`events\debug\qa_debug.txt` 调试选项）；`ALERT_WEATHER_SYSTEM_TITLE`（警报标题）。

**关联字段档**：`fields\common-diseases.md`（疾病 readme 字段）；地形/植被/气候三类目**本体无 readme、字段库亦无档**，字段以本篇 §二 为准。
