# 原版解析：事件 · 任务（vanilla events & missions）

版本基准：EU5 1.3.x。**本篇是什么**：事件与任务这两套"内容容器"的**原版体系实测**——原版怎么摆放 7,470 个事件、任务链由什么驱动、哪些字段文档里有而原版从不用、默认游戏规则会把什么关掉。

> **分工**：字段全集见 `fields\common-events.md`、`fields\common-missions.md`；"怎么写一个事件"见 `guides\event-making.md`；调度语法（`events` / `random_events` / `delay` / `on_action` 全集）见 `fields\common-on_action.md` 与 `guides\scripting-core.md` §四。本篇只讲**原版事实与可改点**。

| 系统 | 文件 | 中文 | 规模 |
|---|---|---|---|
| 事件 | `in_game\events\` | **事件** | 349 档 / 10.52 MB / **7,470 个事件** / **361 个 namespace**（权威：`events\readme.txt` 6.9 KB） |
| 任务链 | `in_game\common\missions\` | **任务链 · 任务节点** | 11 档 / 142 KB / **11 条链 / 108 个节点**（权威：`____Info.txt` 61 行） |
| 任务事件 | `in_game\events\missionevents\` | 任务事件 | 3 档 / 75 KB / 40 个事件 |
| 任务本地化 | `main_menu\localization\<lang>\missions\` | — | 每语言 4 档（en 136 KB / 最大 `generic_missions_l_english.yml` 93 KB） |
| 任务界面 | `in_game\gui\mission_lateralview.gui` + `gui\shared\mission_tooltips.gui` | — | 67 KB + 8 KB（datamodel 类型 `MissionLateralView` / `MissionsTreeView`） |
| 调度 | `in_game\common\on_action\`（21 个 .txt + `on_actions.info`，283 KB） | **事件调度钩子** | 顶层 216 条；`events` 53 处 / `random_events` 38 处 |

---

## 一、事件体量与分布（实测）

| 目录 | 档数 | 体量 | 事件数 | 备注 |
|---|---|---|---|---|
| **`DHE\`** | 159 | **6,239 KB** | **4,145** | 历史事件，**占全部事件 55.5%** |
| `disaster\` | 36 | 808 KB | 469 | 灾难事件 |
| `situations\` | 25 | 768 KB | 456 | 局势事件 |
| `government\` | 20 | 724 KB | 655 | 政体/改革/官僚 |
| `religion\` | 30 | 616 KB | 475 | 宗教 |
| `character\` | 8 | 236 KB | 201 | 角色/王朝 |
| `culture\` | 7 | 225 KB | 142 | 文化 |
| `economy\` | 11 | 130 KB | 124 | 经济 |
| `debug\` | 2 | 100 KB | 77 | 调试（`000_johan_debug.txt` 是实体自检事件） |
| `missionevents\` | 3 | 75 KB | 40 | 任务链专用事件（namespace：`generic_mission_events` 25 / `conquest_mission_events` 11 / `generic_colonial_exploration_events` 4） |
| `diplomacy\` | 7 | 71 KB | 78 | 外交 |
| `colonization\` | 6 | 69 KB | 63 | 殖民 |
| `estates\` | 6 | 69 KB | 68 | 阶层 |
| `exploration\` | 2 | 32 KB | 38 | 探索 |
| `ai_area_conqest_events\` | 1 | 10 KB | 4 | 文件名拼写如此（sic） |
| 根目录散档 | 26 | 598 KB | 435 | 见下 |

**根目录散档是"跨主题单件"**，最大的几档：`earthquake_events.txt` 137 KB / 96 事件、`random_event.txt` 114 KB / 112、`institution_events.txt` 105 KB / 59、`hre.txt` 29 KB / 26、`free_cities.txt` 27 KB / 19、`wokou_events.txt` 21 KB / 13。另有 `readme.txt`（字段权威，本身不是事件档）。

**事件类型**（349 档全量）：`country_event` **7,413** / `exploration_event` 36 / `location_event` 20 / `age_event` 1 / **`unit_event` 0**。→ 原版 99.2% 的事件是国策级事件；`unit_event` 是文档允许但原版零使用的类型。

**namespace**：363 处定义、去重 361 个；一个档一个 namespace 是惯例（`namespace` 必须是文件第一个东西）。

---

## 二、字段的原版使用实况（349 档全量计数）

| 字段 | 次数 | 字段 | 次数 |
|---|---|---|---|
| `option` | 14,603 | `major` | 174 |
| `outcome` | 7,508 | `major_trigger` | 153 |
| `immediate` | 6,911 | `orphan` | 59 |
| `illustration_tags` | 5,176 | `hidden` | **7** |
| `fire_only_once` | 3,802 | `weight_multiplier` | 5 |
| `dynamic_historical_event` | 3,232 | `on_trigger_fail` | **2** |
| `historical_option` | 2,302 | `fallback` | **1** |
| `historical_info` | 1,727 | `exclusive` | **0** |
| `image` | 1,601 | `original_recipient_only` | **0** |
| `ai_chance` | 1,211 | `random_valid` | **0** |
| `category` | 1,103 | `show_as_unavailable` | **0** |
| `hide_portraits` | 975 | **`interface_lock`** | **0** |
| `triggered_desc` | 749 | `first_valid` | 297 |
| `after` | 373 | `ai_will_select` | 180 |

**取值分布**：

- `outcome`：`neutral` 7,331 / `negative` 126 / `positive` 50 → **97.6% 的事件不写或写 neutral**；`negative` / `positive` 合法但罕见（旧档里"negative 是常用值"的说法按全量实测修正）。
- `category`（事件图标）：`disaster_event` 466 / `situation_event` 452 / `international_organization_event` 3。⚠️ 直接 grep `category =` 会捞到 `estate 77` / `pretender 53` / `nationalist 29` 等**别处的同名键**（叛军等），必须白名单过滤取值。
- `image` 路径：`gfx/interface/illustrations/...` 1,394 / `gfx/interface/icons/...` 207。

**"文档写了、原版零使用"清单**（审查时勿当成非法，也别当成惯例）：`interface_lock`（默认 yes 足够）、`exclusive`、`original_recipient_only`、`random_valid`（原版一律用 `first_valid`）、`show_as_unavailable`（readme 自标 NOT IMPLEMENTED）。**`hidden` 只有 7 个**——隐藏事件是异类，不是常规写法；**`major` 174 个（2.3%）**——大事件通知是少数派。

---

## 三、事件是怎么被触发的（实测）

| 触发方式 | 实测用量 | 说明 |
|---|---|---|
| `on_action` 挂接 | 21 个 .txt / 283 KB；`events = { }` **53** 处、`random_events = { }` **38** 处、`first_valid = { }` **0** 处 | 脉冲驱动的主力；`events` 是"各自 trigger 为真就全发"，`random_events` 是"按权重挑一个" |
| 脚本直接触发 | `trigger_event_silently` **939**（events 内）+ 360（其它目录）；`trigger_event_non_silently` **597** + 652 | 事件互调、效果链衔接的主要手段 |
| `dynamic_historical_event` | **3,232** 处（DHE 事件占 78%） | 给 tag + 时间窗 + 月度几率 |
| 玩家操作/其它系统 | — | 无 `trigger_event` 裸形式（原版 0 处） |

两种写法都存在（块式 433 处）:

```txt
trigger_event_silently = settle_the_frontier.1                      # 裸 id
trigger_event_silently = { id = flavor_teu.100 years = 5 }          # 块式：可延迟
trigger_event_non_silently = { id = flavor_gen.36 }                 # 弹窗版
```

---

## 四、DHE（历史事件）的原版布局

- 159 档，文件名 = `flavor_<TAG>.txt`（大小写混用：`flavor_ENG.txt` / `flavor_ach.txt`），另有带编号的 `D008_flavor_BYZ.txt`。
- 单档体量差极大：`flavor_ENG.txt` **491 KB** / `flavor_FRA.txt` 378 KB / `flavor_TUR.txt` 362 KB / `flavor_CAS.txt` 294 KB / `flavor_MOS.txt` 231 KB。
- `tag` 出现最多的：ENG 231 / FRA 206 / TUR 201 / CAS 179 / HAB 146 / MOS 108 / VEN 108 / CHI 103 / RUS 85 / BYZ 76 / POL 73 / DAN 71。
- readme 说 `tag` "可多个"，**原版 `tag = { … }` 块式 0 处**——一律单 tag（要覆盖多国就写多个 `dynamic_historical_event`）。
- DHE 事件 = `dynamic_historical_event`（窗口 + `monthly_chance`）+ `fire_only_once` + `historical_info` + `image` 的固定组合，是"编年史感"的来源。

---

## 五、任务链机制（原版 11 条链 / 108 个节点）

字段与实测明细见 `fields\common-missions.md`，这里只讲机制骨架：

1. **候选池**：链的 `visible` 为真 → 进入候选，同时显示 `POTENTIAL_MISSION_COUNT`（`00_defines.txt:168` = **10**）条；`chance`（原版 11/11 都是 `3600`）是权重，`ai_will_do` 是 AI 的挑选权重。
2. **驱动者不是 on_action**：引擎自己按候选池抽取，任务链不需要挂脉冲。
3. **节点**：链内一层嵌套块，`requires` 定前置、`duration` 定时长（原版 98/108 是限时任务）、`final` 决定链终点（仅 15/108）、`bypass` 可跳过、`highlight` 在地图上标省份。
4. **目标选择器 `select_trigger`**：原版唯一"让玩家挑目标"的机制（6 种 `looking_for_a`，产出 `scope:<target_flag>`），`____Info.txt` 完全没写。
5. **玩家可关、官方默认就关**：`mission_packs_enabled_rule` 默认 `mission_packs_disabled`；`mission_rewards` 默认 `only_end_mission_rewards`（带 `flag = task_rewards_disabled`）→ **默认设置下任务级 `on_start` / `on_completion` 不执行**。做 mod 任务时状态维护必须放 `on_persistent_*` 或链级。
6. **给 mod 的钩子**：`on_mission_start` / `on_mission_completion` / `on_mission_abort` / `on_mission_task_start` / `on_mission_task_completion` / `on_mission_task_bypass`（`_hardcoded.txt:5471-5491`，root = country）；屏蔽/解锁单条链走 `disabled_mission_<链名>` / `unlocked_mission_task_<节点名>` 变量（`country_triggers.txt:504/512`）。
7. **任务专属事件**：`events\missionevents\` 40 个事件由链的 `on_completion` / 任务效果 `trigger_event_*` 拉起来，namespace 与链名不同名（`generic_mission_events.*` 等）。

---

## 六、本地化与界面

- 事件键：`<namespace>.<id>.title` / `.desc` / `.historical_info` / `.a` / `.b` / `.tt`。
- 任务键：链 5 键（`<链名>` / `_DESCRIPTION` / `_CRITERIA_DESCRIPTION` / `_BUTTON_TOOLTIP` / `_BUTTON_DETAILS`）+ 节点 3 键（`<节点名>` / `_desc` / `_tt`）。
- **任务 loc 有独立子目录**：`main_menu\localization\<lang>\missions\`（11 种语言都有），与 `actions_l_english.yml` / `effects_l_english.yml` 那批 114 个 yml 同级但分开放。
- 界面：`gui\mission_lateralview.gui`（侧栏任务树，67 KB）+ `gui\shared\mission_tooltips.gui`；loc 里引用任务实体写 `[MISSION.GetName]`。
- 触发器 tooltip：`has_enabled_mission_trigger_text` / `has_unlocked_mission_task_trigger_text` 定义在 `common\trigger_localization\scripted_triggers.txt:187/193`（none / first / global / third 四人称），实际文案键是 `HAS_ENABLED_MISSION_TRIGGER` 系列（`scripted_triggers_l_english.yml:207-219`）。

---

## 七、可改点与硬编码边界

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 写新事件 | `in_game\events\<自己的档>.txt` | 首行 `namespace`；id 1–9999 全局唯一；`type` 决定 root 作用域 |
| 让事件定时/条件触发 | `common\on_action\` 追加同名钩子，或 `events` / `random_events` | 只追加不覆盖；`trigger` 为假静默跳过 |
| 事件 A 拉事件 B | 效果里 `trigger_event_silently = <id>`（块式可加 `years`） | 排队事件不满足 `trigger` 时会走 `on_trigger_fail` |
| 给某国加历史事件 | `in_game\events\DHE\flavor_<TAG>.txt` 增补 | 单 tag；记得 `fire_only_once` 与 `historical_info` |
| 新任务链 | `common\missions\<名>_mission_pack.txt` | 见 `fields\common-missions.md`；`visible` 首行写 `game_has_missions_enabled = yes` |
| 屏蔽/解锁某条链 | 设变量 `disabled_mission_<链名>` / `unlocked_mission_task_<节点名>` | 判定走 scripted trigger，可被 mod 覆盖 |
| 任务奖励行为 | 游戏规则 `mission_rewards` | 默认 `only_end_mission_rewards` → 任务级 on_start/on_completion 不执行 |
| 查实体引用语法 | `events\debug\000_johan_debug.txt:1600+` | 里面是 `mission:` / `mission_task:` / `goods:` / `road_type:` 等实体自检清单，写 `is_available_for` 时可照抄 |

**硬编码**：候选池抽取与 `POTENTIAL_MISSION_COUNT`、任务奖励规则判定、`dynamic_historical_event` 的月度掷骰、事件弹窗/暂停行为（`interface_lock`）、任务树界面布局（见 `vanilla\vanilla-gui.md`）。

---

## 八、中文检索键

概念：`[event|e]`（事件）、`[mission|e]`（任务）、`[mission_task|e]`（任务节点）、`[exploration|e]`（探索）。
本地化：事件 `<namespace>.<id>.title/.desc/.a/.b/.tt`；任务 `<链名>` + `<链名>_DESCRIPTION` + `<节点名>` + `<节点名>_desc` + `<节点名>_tt`；目录 `main_menu\localization\<lang>\missions\`。
界面：`mission_lateralview.gui`（任务树）、`shared\mission_tooltips.gui`；事件弹窗 `eventwindow.gui` 38.7 KB + `shared\event_tooltips.gui` + 消息系统 `messages.gui` 60 KB / `message_log.gui`（详见 `vanilla\vanilla-gui.md`）。
字段档：`fields\common-events.md`、`fields\common-missions.md`、`fields\common-on_action.md`。
