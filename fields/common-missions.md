# in_game/common/missions（任务链 · 任务节点）

来源：`in_game\common\missions\____Info.txt`（**61 行；本目录没有 readme.txt，该 .info 就是字段权威**）+ 11 个任务包实查（142 KB / **11 条任务链 / 108 个任务节点**）

> 体系解析与制作用法见 `vanilla\vanilla-events-and-missions.md` §五；本档是**字段权威 + 原版实测 + 审查要点**。

## 文件结构

- 一档可放多条链；原版 11 档 = 11 条链，**文件名 = 链名 + `_mission_pack`**（`generic_capital_economy_mission_pack.txt` → 链 `generic_capital_economy`，原版 11/11 遵守）。
- **链** = 文件顶层块；**任务节点** = 链内一层嵌套块。节点命名约定 `mission_` 前缀（原版 108 个节点中 103 个带前缀，5 个例外：`a_great_capital` / `high_era_of_countryname` / `meritocratic_approach` / `royal_authority` / `raise_a_navy`）——命名只是约定，定序关系靠 `requires`。
- 任务定义**全部写在链内**；`common\mission_task_defs\` 是空壳目录（只有 `.info` 占位，勿删）。
- ⚠️ 原版有一档双扩展名：`generic_infrastructure_mission_pack.txt.txt`（详见"审查要点"最后一条）。

## 链级字段

```
<链名> = {
    header = <图片>              # [String] 选中该链后顶栏显示的图（原版 0 处使用）
    icon = <图片>                # [String] 链选择按钮与顶栏图标（原版 11/11 都用）
    repeatable = <yes/no>        # 默认 no；可重复完成
    visible = { <triggers> }     # 是否出现在候选列表（作用域 = 该国）
    enabled = { <triggers> }     # 该链能否被完成
    abort = { <triggers> }       # 满足即中止
    chance = <script value>      # 权重；同时显示的候选数 = POTENTIAL_MISSION_COUNT（原版 11/11 写 3600）
    ai_will_do = { <script value> }   # AI 在候选中的挑选权重（可用 select_trigger 的全部作用域变量；原版 0 处）
    on_potential = { <effects> }      # 进入"候选池"时执行；作用域在此创建，直到 abort/completion/退出候选（原版 0 处）
    on_start = { <effects> }          # 链开始
    on_abort = { <effects> }          # 链中止（原版 11/11 都有，用来清标记）
    on_completion = { <effects> }     # 链完成
    on_post_completion = { <effects> }# 完成后、链不再属于该国时执行（原版 0 处）
}
```

**`____Info.txt` 未记录、原版却在用的两个链级字段**：

| 字段 | 实测 | 说明 |
|---|---|---|
| `player_playstyle = military / diplomatic / administrative` | 11/11（military 3 / diplomatic 4 / administrative 4） | 标记该链的玩法取向；同一枚举也用于 `scriptable_hints`、`tutorial_lessons`、`scenarios` |
| `select_trigger = { … }` | 8 处（3 处在链级、5 处在节点级） | 让玩家在地图上"选一个目标"并把它存成具名 scope（见下节） |

## 节点级字段

```
<节点名> = {
    icon = <图片>                 # [String] 该节点在链里的图标（原版 108/108 都用）
    requires = { <节点名…> }      # 前置节点（原版 97/108）
    final = <yes/no>              # 默认 no；yes = 完成本节点即完成整条链（原版仅 15/108）
    visible = { <triggers> }      # 是否显示该节点（原版 36）
    highlight = { <triggers> }    # 鼠标悬停时高亮哪些省份；`scope:province` 提供省份（原版 7）
    enabled = { <triggers> }      # 能否完成（原版 104）
    bypass = { <triggers> }       # 满足即"跳过"该节点（原版 21）
    ai_will_do = { <script value> }        # AI 挑选该节点的权重（原版 0 处）
    duration = <天数>             # 限时任务的时长（原版 98/108 —— 原版节点几乎全是限时任务）
    on_monthly = { <effects> }             # 限时期间每月执行（原版 9）
    on_start = { <effects> }               # 限时任务开始时；**即时任务无效**；规则禁用任务奖励时不执行（原版 11）
    on_persistent_start = { <effects> }    # 同上，但**总是执行**（原版 2）
    modifier_while_progressing = { <modifiers> }  # 进度期间挂在该国身上的修正（原版 8）
    on_completion = { <effects> }          # 完成时；规则禁用任务奖励时不执行（原版 95）
    on_persistent_completion = { <effects> }  # 同上，但**总是执行**（原版 9）
    on_bypass = { <effects> }              # 被 bypass 时（原版 0 处）
}
```

## `select_trigger`：目标选择器（`____Info.txt` 完全没写）

唯一实现"任务让玩家在地图上挑一个目标"的机制。原版 8 处，形态（`generic_capital_economy_mission_pack.txt:18-39`）：

```
select_trigger = {
    looking_for_a = market          # 原版 6 种：market / location / goods / province / estate / country
    target_flag = mission_target_market   # 选中项存成 scope:mission_target_market
    # 候选列表二选一：
    interaction_source_list = { scope:actor = { every_market_present_in_country = { add_to_list = source } } }
    # source = actor               # 或直接用 actor 的默认候选集
    name = "mission_select_market"          # 选择器标题（loc 键；值要带引号）
    none_available_msg_key = "mission_no_markets"   # 候选为空时的提示（loc 键，可写 $另一键$ 复用）
    column = { data = name }        # 列表列，可多个（原版用到 name / integration / population）
    visible = { … }                 # 该选择器自身的可见条件（可用 scope:actor）
}
```

- 产出的 `scope:<target_flag>` 在链/节点的效果里直接用，例：`generic_capital_economy_mission_pack.txt:53-54`（`name = mission_target_market` + `value = scope:mission_target_market`）、`:160-161`（`from_market = scope:… / to_market = scope:…`）、`generic_conquer_province_mission_pack.txt:351`（`scope:conquest_province ?= { … }`）。
- 用完要清：原版在 `on_completion` / `on_abort` 里 `remove_variable = <target_flag>`（`:69`、`:90`）。
- 链级与节点级都能写（原版 3 + 5）。

## 门槛 · 钩子 · 调度（全都不在 `____Info.txt` 里）

| 机制 | 位置 | 内容 |
|---|---|---|
| 候选数量 | `loading_screen\common\defines\00_defines.txt:168` | `POTENTIAL_MISSION_COUNT = 10` |
| 任务包总开关 | `main_menu\common\game_rules\00_game_rules.txt:155-168` | `mission_packs_enabled_rule`，**默认 `mission_packs_disabled`**；disabled 选项带 `proficiency_expert` 旗标 |
| 奖励规则 | 同上 `:170-183` | `mission_rewards` 默认 `only_end_mission_rewards`，该选项带 `flag = task_rewards_disabled` |
| 脚本侧开关 | `in_game\common\scripted_triggers\game_triggers.txt:37-52` | `game_has_missions_enabled`（= `NOT = { has_game_rule = mission_packs_disabled }`）、`game_has_mission_rewards_enabled`、`game_has_mission_task_rewards_enabled` |
| 屏蔽单条链 | `scripted_triggers\country_triggers.txt:504` | `has_enabled_mission_trigger` = `NOT = { has_variable = disabled_mission_$type$ }` → **设变量 `disabled_mission_<链名>` 即屏蔽该链** |
| 解锁单条链 | 同上 `:512` | `has_unlocked_mission_task_trigger` = `has_variable = unlocked_mission_task_$type$` |
| 上述两个触发器的 tooltip | `common\trigger_localization\scripted_triggers.txt:187/193` | `has_enabled_mission_trigger_text` / `has_unlocked_mission_task_trigger_text`，各含 none / first / global / third 四种人称 |
| 六个 on_action 钩子 | `common\on_action\_hardcoded.txt:5471-5491` | `on_mission_start` / `on_mission_completion` / `on_mission_abort`（原版自带 `add_stability = stability_mild_penalty`）/ `on_mission_task_start` / `on_mission_task_completion` / `on_mission_task_bypass`（root = country） |
| AI 侧常量 | `00_defines.txt:1269-1277` | `LAW_TARGET_UTILITY_VALUE = 25` / `POLICY_TARGET_UTILITY_VALUE = 50`（AI 为任务而改法/政策的额外效用）、`AI_MISSION_*_UTILITY_*` |

## 本地化键（`main_menu\localization\<lang>\missions\*.yml`）

```
<链名>                          # 链名（"Venturing into the Unknown"）
<链名>_DESCRIPTION              # 链说明
<链名>_CRITERIA_DESCRIPTION     # 候选判据说明
<链名>_BUTTON_TOOLTIP           # 按钮提示
<链名>_BUTTON_DETAILS           # 按钮详情
<节点名>                        # 节点标题（原版命名 mission_xxx）
<节点名>_desc                   # 节点说明
<节点名>_tt                     # 节点自定义提示（custom_tooltip 用）
```

实例：`main_menu\localization\english\missions\generic_colonize_explore_l_english.yml:4-8`（链 5 键）、`:11-22`（节点名 + `_desc`）、`:32-33`（`_tt`）。loc 里引用任务实体写 `[MISSION.GetName]`（`scripted_triggers_l_english.yml:214`）。

## 原版实测（11 链 / 108 节点）

| 项 | 数据 |
|---|---|
| 链 | 11 条，全部 `chance = 3600` + `repeatable = yes` + `icon` + `visible` + `on_completion` + `on_abort` |
| 链级 0 使用 | `header` / `ai_will_do` / `on_potential` / `on_post_completion` |
| 节点 | 108 个；`icon` 108、`enabled` 104、**`duration` 98**、`requires` 97、`on_completion` 95、`visible` 36、`bypass` 21、**`final` 15**、`on_start` 11、`on_persistent_completion` 9、`on_monthly` 9、`modifier_while_progressing` 8、`highlight` 7、`on_persistent_start` 2 |
| 节点级 0 使用 | `ai_will_do` / `on_bypass` |
| 链规模 | 最大 `generic_capital_economy_mission_pack.txt`（26 KB，含 3 个 `select_trigger`） |

## 审查要点

- **默认规则下任务级 `on_start` / `on_completion` 不执行**：`mission_rewards` 默认值 `only_end_mission_rewards` 带 `flag = task_rewards_disabled`，而 `____Info.txt:49/55` 明示这两个字段"规则禁用任务奖励时不执行"。**状态维护（`set_variable` / `remove_variable` / 清 `select_trigger` 的 target_flag）必须放 `on_persistent_start` / `on_persistent_completion` 或链级 `on_completion`**，否则默认设置下断链。原版节点里 `on_completion` 95 处 vs `on_persistent_completion` 9 处——奖励与状态维护是分开写的。
- **`visible` 要先确认总开关**：原版每条链的 `visible` 第一行都是 `game_has_missions_enabled = yes`，而该规则**默认关闭**；漏了它，链在默认设置下也会进候选池。
- **`chance` 不是毫秒也不是百分比**，是候选池权重（配合 `POTENTIAL_MISSION_COUNT = 10`）；原版一律 3600。
- **`requires` 写的是节点名，不是链名**；`final = yes` 的节点决定链何时结束（原版只有 15/108，缺了它整条链会因所有节点完成而挂住）。
- **`select_trigger` 与 `target_flag` 必须成对清理**：flag 是 `scope:<flag>`，链结束后残留会污染下一次链（原版用 `remove_variable` 清）。
- `highlight` 里用 `scope:province`（不是 `scope:location`）——它是"高亮哪些省份"，不是地点。
- **`____Info.txt` 里搜不到 `select_trigger` / `player_playstyle`**：两者只在文件注释里被间接提过（"all scoped variables from the select_triggers"）。审查时勿把这两个字段判成"非法字段"。
- 原版有**双扩展名档** `generic_infrastructure_mission_pack.txt.txt`（13,169 B，全库仅两处此类，另一处是 `gfx\map\map_objects\decal_rock_clusters_01_a.txt.txt`）。它是链 `development_of_infrastructure`（英文名 "Infrastructure Efforts"）与节点 `a_great_capital`（`:194`）的**唯一定义处**，loc 5 键 11 语言齐全，`events\debug\000_johan_debug.txt:1614/1615` 以引擎实体形式引用这两个实体。**结论：文件名手滑，但内容是生效的**——引擎按 `.txt` 后缀通配加载（`x.txt.txt` 仍以 `.txt` 结尾，而 `x.txt.bak` 才会被忽略）；外部佐证是 SteamDB 的补丁 diff 一直在改这个文件的内容（1.0.5 −1 B、1.0.10 +2 B〔同批补丁说明含任务 "control around the capital" 的修正〕、1.2.4 +36 B，见 [1.0.5](https://steamdb.info/patchnotes/20828878/) / [1.0.10](https://steamdb.info/patchnotes/21194479/) / [1.2.4](https://steamdb.info/patchnotes/23310399/)），且粉丝数据库 [EU5DB](https://eu5db.com/mission/development_of_infrastructure) 收录该链时把来源标为这个 `.txt.txt` 档。**自己写档一律用单个 `.txt`**，也别用"改扩展名"的方式停用原版文件（`.txt.txt` 无效，须改成 `.bak` 之类）。

未在 `____Info.txt` 中说明：`select_trigger` 全部字段与用法、`player_playstyle`、`POTENTIAL_MISSION_COUNT` 的调度方式、任务级 `on_persistent_*` 与游戏规则旗标的对应关系、`has_enabled_mission_trigger` / `has_unlocked_mission_task_trigger` 两个门槛触发器、六个 `on_mission_*` on_action 钩子、本地化键全集。
