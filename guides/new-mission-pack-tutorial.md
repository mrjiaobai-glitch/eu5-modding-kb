# 制作教程：从零加一条任务链（new-mission-pack-tutorial）

> **一句话**：以原版实测数据搭一条任务链的最小可用集：三条硬规则、链与节点字段、select_trigger 目标选择、本地化键与图标。
> **什么时候看**：第一次加任务链时按这七步做，写完再用最后一节的表格逐条自检。
> **体量**：134 行 · 约 7 分钟通读

> **E2E 教程**：一条"能看到、能完成、有奖励、有本地化"的任务链，最小可用集。
> 字段权威：`fields\common-missions.md`；机制全貌：`vanilla\vanilla-events-and-missions.md` §五。素材取自原版 11 条链 / 108 个节点的实测。

## 第 0 步：先知道三条硬规则（不遵守会"写了没效果"）

1. **任务包默认是关的**：`mission_packs_enabled_rule` 默认 `mission_packs_disabled`（`main_menu\common\game_rules\00_game_rules.txt:155`）→ 链的 `visible` 第一行必须写 `game_has_missions_enabled = yes`，否则默认设置下不进候选池。
2. **任务级 `on_start` / `on_completion` 默认不执行**：`mission_rewards` 默认 `only_end_mission_rewards`，带 `flag = task_rewards_disabled`（同文件 `:170`）→ **状态维护（`set_variable` / `remove_variable` / 清 `select_trigger` 的 flag）必须放 `on_persistent_start` / `on_persistent_completion` 或链级 `on_completion`**。
3. **链不挂 on_action**：引擎按 `chance` + `POTENTIAL_MISSION_COUNT = 10`（`00_defines.txt:180`）自己抽候选。

## 第 1 步：`common\missions\<链名>_mission_pack.txt`

一个文件可以放多条链，原版惯例是**一档一链**、文件名 = 链名 + `_mission_pack`：

```txt
my_chain = {                                   # ① 链名（顶层块）
    icon = my_chain                            # → main_menu\gfx\interface\illustrations\missions\my_chain.dds
    repeatable = yes                           # 原版 11/11 都写 yes
    player_playstyle = administrative           # military / diplomatic / administrative（原版 11/11 都用；____Info.txt 未记录）
    chance = 3600                              # 候选池权重（原版一律 3600）

    visible = {
        game_has_missions_enabled = yes         # ★ 硬规则 1
        country_rank_level > 2
        NOT = { has_variable = recently_had_my_chain_variable }
    }
    enabled = { exists = capital.market }

    on_completion = {                           # 链级：状态维护放这里最安全
        set_variable = { name = recently_had_my_chain_variable  years = 25 }
    }
    on_abort = {
        set_variable = { name = recently_had_my_chain_variable  years = 10 }
    }

    ###########################################
    mission_do_the_thing = {                    # ② 任务节点（链内一层嵌套块）
        icon = my_advance                       # → main_menu\gfx\interface\advance\my_advance.dds（节点图标复用"革新"图标，原版 108/108）
        duration = 365                          # 限时天数（原版 98/108 是限时任务）
        requires = { }                          # 前置节点（写节点名，不是链名）
        enabled = { treasury > 0 }              # 完成条件
        on_monthly = { }                        # 限时期间每月
        modifier_while_progressing = { <修正键> = 1 }   # 进度期间挂国家身上
        on_persistent_completion = {            # ★ 硬规则 2：一定执行 → 状态维护放这
            remove_variable = my_target_flag
        }
        on_completion = {                       # 奖励放这（默认规则下不执行，属于"规则允许时才发"）
            add_gold = 100
        }
        final = yes                             # 完成即结束整条链（原版仅 15/108 用）
    }
}
```

**节点字段分布参考**（决定你写得像不像原版）：`icon` 108/108、`enabled` 104、`duration` 98、`requires` 97、`on_completion` 95、`visible` 36、`bypass` 21、`final` 15、`on_start` 11、`on_persistent_completion` 9、`on_monthly` 9、`modifier_while_progressing` 8、`highlight` 7。

**注意**：`highlight` 里高亮的是**省份**，用 `scope:province`（不是 `scope:location`）。

## 第 2 步：（可选）让玩家挑目标 → `select_trigger`

这是 `____Info.txt` **完全没写**的字段（原版 8 处），"让玩家在地图上选一个市场/地点/商品/省份/阶层/国家"只能靠它：

```txt
select_trigger = {
    looking_for_a = market                                    # market / location / goods / province / estate / country
    target_flag = my_target_market                            # 选中项存成 scope:my_target_market
    interaction_source_list = {                               # 候选列表（或写 source = actor）
        scope:actor = { every_market_present_in_country = { add_to_list = source } }
    }
    name = "my_chain_select_market"                           # loc 键：选择器标题
    none_available_msg_key = "my_chain_no_markets"            # loc 键：候选为空时
    column = { data = name }                                  # 列表列（可多个：name / integration / population）
    visible = { … }
}
```

链结束后**必须清 flag**（原版用 `remove_variable = <target_flag>`）——清在 `on_persistent_completion` / 链级 `on_completion` / `on_abort`。

## 第 3 步：本地化 → `main_menu\localization\<lang>\missions\<名>_l_<lang>.yml`

**链 5 键 + 节点 3 键**（缺哪个显示 raw key）：

```yml
 my_chain: "My Grand Endeavour"
 my_chain_DESCRIPTION: "……"
 my_chain_CRITERIA_DESCRIPTION: "……"
 my_chain_BUTTON_TOOLTIP: "……"
 my_chain_BUTTON_DETAILS: "……"
 mission_do_the_thing: "Do the Thing"
 mission_do_the_thing_desc: "……"
 mission_do_the_thing_tt: "……"        # custom_tooltip 用
 my_chain_select_market: "Select a [market|e]"    # select_trigger 的 name
 my_chain_no_markets: "No valid markets"
```

- 文件要 **UTF-8 BOM**；语言头放第 1 行，键**缩进一个空格**写在 `l_<lang>:` 之下（顶格会毁文件，见 `guides\localization.md`）。
- 任务 loc **不在** `main_menu\localization\<lang>\` 顶层，而在 `missions\` 子目录里（`tools\loc-keys.md` 判缺键第 3 条）。

## 第 4 步：图标（可后补）

| 用什么 | 放哪 | 命名 |
|---|---|---|
| 链图标 `icon = X` | `main_menu\gfx\interface\illustrations\missions\` | `X.dds`（原版 11/11 命中） |
| 节点图标 `icon = Y` | `main_menu\gfx\interface\advance\` | `Y.dds`（原版 108/108） |
| 任务事件配图 | `main_menu\gfx\interface\illustrations\<主题>\` | 事件里 `image = "gfx/interface/illustrations/<主题>/<名>.dds"` |

缺图标不会报错（只是空白），但**链图标缺失会让链在列表里很显眼地不对劲**。

## 第 5 步：（可选）任务事件

- 事件放 `events\missionevents\` 或自己的 namespace 文件里；原版 3 档 40 个事件（namespace：`generic_mission_events` 25 / `conquest_mission_events` 11 / `generic_colonial_exploration_events` 4）。
- 触发方式：链/节点效果里 `trigger_event_silently = my_ns.1`（或块式 `{ id = my_ns.1  years = 5 }`）。
- 事件字段权威：`fields\common-events.md`；写法：`guides\event-making.md`。

## 第 6 步：自检清单

| 检查 | 怎么看 |
|---|---|
| 链出现在候选池 | `visible` 首行是 `game_has_missions_enabled = yes`；游戏规则里任务包是开的 |
| 候选数对不对 | `POTENTIAL_MISSION_COUNT = 10`（defines）；`chance` 是权重不是概率 |
| 状态没断链 | 状态维护全在 `on_persistent_*` 或链级（**默认规则下任务级 on_start/on_completion 不跑**） |
| 目标 flag 清了 | `remove_variable` 覆盖 completed / aborted 两条路径 |
| 图标对 | `icon` 的值 = 文件名（不是链名）；节点图标来自 `advance\` |
| 键齐 | 链 5 + 节点 3 +（用到的）`name` / `none_available_msg_key`；11 语言 |
| 关卡名/描述错位 | `requires` 写的是**节点名**；`final = yes` 决定链何时结束（漏了整条链会挂住） |

## 第 7 步：相关档

`fields\common-missions.md`（字段权威）· `vanilla\vanilla-events-and-missions.md` §五（机制与默认规则）· `fields\common-on_action.md`（`on_mission_*` 六个钩子）· `tools\review-checklist.md`（审查）· `fields\common-script_values.md`（`chance` / `ai_will_do` 用的脚本数学）
