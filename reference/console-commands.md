# 外部资料：控制台命令（EU5 console commands）

> **来源**：[EU5 Wiki: Console commands](https://eu5.paradoxwikis.com/Console_commands)（抓取 2026-10-30；该页标注"最后校验 1.3"）。
> **性质**：**外部资料，非本库实测**。本档为**释义改写**（非逐字搬运），wiki 原文为 CC BY-SA 3.0；文中"实测/已确认"指本库另做的静态或运行验证。
> **体量**：约 90 行 · 检索用
> **什么时候看**：要开控制台调试、跑数据文档、或测某个事件/触发器时。

## 一、开启 debug mode

- **Steam**：游戏属性 → 启动选项加 `-debug_mode` → 启动（**会禁成就**，需退出重开才能恢复成就）。
- **非 Steam**：给 `binaries\eu5.exe` 建快捷方式，目标末尾加 ` -debug_mode`。
- debug mode 还带：控制台窗口、GUI 编辑器/Script explorer 等调试菜单、地图 tooltip 调试信息、**Ctrl+点击地图国家 = 接管该国**、Ctrl+Alt+点击 = 接管州、**文件监视自动热重载改动过的 mod 文件**（`on_action` 除外——见 `guides\merging.md`）。
- 已确认（本库静态）：`binaries\eu5.exe` 内存在 `-debug_mode`、`script_docs`、`dump_data_types` 字符串。

## 二、控制台

- 打开键**因键盘布局而异**：`Shift+2`、`Alt+2+1`、`Shift+3`、`§`、`~`、`^`、`°`、`²`、`` ` ``（QWERTY 通常是 `` ` ``）；都不行试 `Alt+C` / `Shift+Alt+C`。
- `↑`/`↓` 翻历史；**输入前缀后按 `Tab` 自动补全**（找命令最快的方式）。多数命令再执行一次可关闭。

## 三、命令速查（按用途挑常用的）

### 3.1 事件与脚本（mod 调试核心）
| 命令 | 参数 | 作用 |
|---|---|---|
| `event` | `[eventid] [target]` | 直接触发事件 |
| `test_event_trigger` | `[eventid] [target]` | 测试事件的触发条件 |
| `event_probabilities` | `[on_action key] [lean]` | 把某 on_action 的事件触发/概率拆解**写进 error.log**；仅限 `monthly/yearly/biyearly/four_year country_pulse` |
| `script_docs`（别名 `script_documentation`） | — | **生成脚本文档**（effects/triggers/scopes），输出到 `Documents\Paradox Interactive\Europa Universalis V\docs` |
| `dump_data_types` | — | 导出 GUI 数据类型到 `…\logs\data_types` |
| `Script.Profiling.Start` / `.Stop` / `.Restart` / `.Dump` | — | 脚本性能采样 |

### 3.2 数据与缓存
| 命令 | 作用 |
|---|---|
| `recalc_cached_data` | 重算不写进存档的缓存数据 |
| `update_pop_cache` | 重算玩家国的 POP 缓存 |
| `recalc_connectivity [tag]` | 重算该国附庸领土连通性 |
| `Adjacencies.Rebuild` | 重建全部相邻关系 |

### 3.3 地图与地点
| 命令 | 作用 |
|---|---|
| `discover [location key/all]`（别名 `ti`） | 发现地点 |
| `discover_area [area key/all]`（`da`） | 发现区域 |
| `change_control [location] [0.0–1.0]`（`chg_ctrl`） | 改控制度 |
| `change_devastation [location] [0.0–1.0]`（`chg_deva`） | 改荒废度 |
| `change_winter [location] [0–3]`（`chg_winter`） | 改冬季等级 |
| `weather [location] [front/cyclone/tornado]` | 造天气 |
| `PrintLocationBiome [location id]` | 打印地点 biome |
| `tick_population [location]` | 对某地点做一次月度人口 tick |
| `famine` | 清空市场与省份的粮仓 |
| `destroy_market_stockpiles` | 清空所有市场商品库存 |
| `Map.SavePNG` / `Map.SavePNGScaled` | 按地图模式导出 png |

### 3.4 国家 / 政治 / 外交
| 命令 | 作用 |
|---|---|
| `change_government [政府类型]`（`changeGov`） | 改政体 |
| `form_union [junior]` | 与目标成立联合（union） |
| `parliament_issue [issue]` / `end_parliament_issue` | 改/结束议会议题 |
| `enact_hre_policy [policy_key]` | 强制 HRE 通过政策 |
| `set_ruler [tag] [character_id]` | 指定统治者 |
| `build_everything [地点/国家/区域/all]`（`be`） | 一键建造 |

### 3.5 资源/作弊开关
- 资源：`cash` / `Manpower` / `Sailors` / `Stability` / `Prestige` / `ArmyTradition` / `NavyTradition` / `GovernmentPower` / `WarExhaustion` / `Inflation` / `ReligiousInfluence` 等（直接加对应货币）。
- 开关类：`YesMan` / `NoMan` / `InstantMove` / `InstantSiege` / `InstantWar` / `InstantConstruction` / `InstantMission` / `InstantExploration` / `InstantConquistadors` / `InstantCasusBelli` / `InstantImplementation` / `FastDiplo` / `FastRebels` / `ByPassRequirements` / `RemoveFogOfWar` / `NoRandom` / `WatchWindows` / `LoadingScreen` / `no_peace_deals`（再执行一次关闭）。

### 3.6 图形 / GUI 调试
`portrait_editor`（`pe`，肖像编辑器）、`Portrait.ClearCache`、`gui_data_profiler`、`entity_designer`、`mesh_importer`、`scene_editor`、`animation_editor`、`tweak` / `dump_tweakables`、`log_viewer`、`MapObjects.*` 系列（locator 检查/生成）、`spawn_schematic` / `spawnentity` 等。

### 3.7 其他
`oos_prompt` / `oos_dump` / `force_oos` / `force_resync`（多人不同步诊断）、`achievement [成就]`、`list_dhe_for`（列出某 TAG 已触发的 DHE）、`fake_whisper`（测试私信弹窗）、`save_game_analyzer`（存档分析器）、`copy_mouse_pos`。

## 四、注意事项
- 该 wiki 页**最后校验 1.3**，1.4.5 可能增删命令——**以游戏内 `Tab` 补全为准**。
- `-debug_mode` 会禁成就；mod 热重载不覆盖 `on_action`（本库实测线索：改 on_action 需重启）。
- 很多"编辑器/查看器"命令是引擎开发工具，**不保证在正式版可用**（试不通不代表 mod 写错）。

## 五、与库内其它档的关系
- 修改后重载/合并语义 → `guides\merging.md`
- 日志与 error.log 解读 → `tools\error-log-decoder.md`、`guides\testing.md`
- 脚本参考系（effects/triggers/scopes）→ 建议用 `script_docs` 生成，而不是抄 wiki（见 `reference\README.md` 的分批计划）
