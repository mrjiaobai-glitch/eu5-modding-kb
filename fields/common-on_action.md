# common/on_action（事件调度钩子）

> **一句话**：事件调度钩子的字段全集、132 个硬编码钩子清单，以及 21 个原版数据档的顶层条目与作用域实测。
> **什么时候看**：写或审查 on_action、要查某个引擎钩子的确切名字与 root 作用域时翻这篇。
> **体量**：128 行 · 约 6 分钟通读

来源：`in_game\common\on_action\on_actions.info`（**3,263 B，全游戏最完整的调度语法文档**）+ 21 个数据 .txt（**279 KB / 顶层 216 条**）实查（本目录共 22 档 / 283 KB，另一个就是 `on_actions.info`）

> **与 `guides\scripting-core.md` §四 的分工**：那边是**制作用法**（怎么写、怎么追加原版钩子）；本档是**字段权威 + 原版实测清单**（有哪些钩子、每条钩子的作用域与统计）。

## 字段全集（info 声明）

```
<on_action 名> = {
    trigger = { <triggers> }        # 为假 → 本次 on_action 什么都不做
    weight_multiplier = {           # 作为 random_on_action 候选时的权重（base + modifier）
        base = 1
        modifier = { add = 1  <triggers> }
    }

    events = {                      # ★ 列出的事件只要各自 trigger 为真就【全部】触发
        event_id_1
        delay = { days = 365 }      # 之后列出的条目延迟；事件须在“执行时 + 延迟结束时”都有效才 fire
        event_id_2
        delay = { months = { 6 12 } }   # 支持随机范围；★ 新的 delay 覆盖旧的
        event_id_3
    }

    random_events = {               # ★ 只挑【一个】触发
        chance_to_happen = 25       # 百分比：是否进入评估
        chance_of_no_event = {      # 脚本值形式（可用 if 条件）；★ 与 chance_to_happen 分开是“为了性能”
            value = 0
            if = { limit = { <triggers> }  add = 10 }
        }
        100 = event_id_1            # 权重（会乘以该事件的 weight_multiplier，缺省 1）
        200 = event_id_2
        100 = 0                     # ★ “0 条目”= 有机会什么都不发生（防止稀有事件因为别的都无效而必发）
    }

    first_valid = { event_id_1 event_id_2 fallback_event_without_trigger }   # 取第一个 trigger 通过者

    on_actions = { on_action_1 on_action_2 }          # ★ on_action 可以链式触发其它 on_action
    random_on_action = { 100 = on_action_1 100 = 0 }
    first_valid_on_action = { on_action_1 on_action_2 }

    effect = { <effects> }          # ⚠️ 与它触发的事件【并发】执行（不在事件之前）
                                    # ⚠️ 这里的 scope / 局部变量【不会】传给同一 on_action 触发的事件

    fallback = <另一个 on_action>    # 本 on_action 什么都没跑起来时改调它
                                    # ⚠️ 官方警告：别写无限回退循环，否则会【阻止时间推进】
}
```

**所有“触发条目”（事件或 on_action）都支持 `delay`。**

## 原版实测（21 个数据档 / 279 KB / 顶层 216 条）

| 文件 | 体量 | 顶层条目 | 代表条目 |
|---|---|---|---|
| **`_hardcoded.txt`** | **134 KB** | **132** | 引擎内置钩子全集（下节） |
| `location_pulses.txt` | 56 KB | 3 | **`weather_monthly_pulse`** / `volcano_location_pulse` / `earthquake_location_pulse` |
| `country_yearly.txt` | 26 KB | 15 | `yearly_country_pulse`、`earthquake_yearly_pulse`、各国 flavor 脉冲（sic/nap/ven/kor/ira/nav/kni…）、`yearly_hre_circle_leader_pulse` |
| `country_biyearly.txt` | 15 KB | 1 | — |
| `religion_flavor_pulse.txt` | 12 KB | 10 | `religion_flavor_pulse` + 按宗教组分 9 条（muslim / catholic / protestant / orthodox / buddhist / dharmic / nahuatl / hellenism / shinto） |
| `country_four_yearly.txt` | 9 KB | 5 | — |
| `character_death_pulses.txt` | 7 KB | 7 | **`on_character_death`**、`on_cabinet_death`、`on_horde_pretender_death`… |
| `country_monthly.txt` | 6 KB | 10 | `monthly_country_pulse`、`on_papal_opinion_added/removed`、`on_papal_authority_updated` |
| `government_flavor_pulse.txt` | 5 KB | 6 | `government_flavor_pulse` + 按政体分 5 条 |
| `on_country_specific_pulse.txt` / `exploration_mission_monthly.txt` / `appanage_monthly.txt` / `parliament_monthly_pulse.txt` / `treasure_voyage.txt` / `ai_personalities_setup.txt` / `in_regency_yearly_pulse.txt` / `character.txt` / `country_pulse_for_high_infamy.txt` / `settle_the_frontier_monthly.txt` / `colonial_charter_monthly.txt` / `estate_changes.txt` | 各 0.1–3 KB | 各 1–6 | 殖民/探索/议会/摄政/宝物船等专属脉冲 |

### 132 个硬编码钩子（`_hardcoded.txt`）抽样

**战争与战斗**：`on_war_declared` / `on_pre_war_declared` / `on_join_war` / `on_winning_war` / `on_losing_war` / `on_pre_winning_war` / `on_pre_losing_war` / `on_ending_war` / `on_pre_ending_war` / `on_took_location_in_peace_treaty` / `on_battle_won` / `on_battle_lost` / `on_battle_won_character` / `on_battle_lost_character` / `on_great_battle_won` / `on_great_battle_lost` / `in_battle` / `on_overrun_imprisoned`
**围城与占领**：`on_siege_won` / `on_siege_lost` / `on_location_occupied` / `on_location_lost`
**吞并与附属**：`on_annex` / `on_military_annex` / `on_diplomatic_annex` / `on_civil_war_annex` / `on_colonize_annex`（+ 对应 `*_annexed`）/ `on_annexation_start` / `on_annexation_cancel` / `on_subject_created` / `on_dependency_gained` / `on_becoming_free`
**内战与破产**：`on_civil_war_start` / `on_civil_war_won` / `on_civil_war_lost` / `on_bankruptcy` / `on_loan_taken` / `on_loan_repaid` / `on_loan_renewed`
**时代与选举**：`on_new_age` / `on_new_age_global` / `on_election` / `on_reform_change` / `on_bureaucracy_change` / `on_bureaucracy_added` / `on_bureaucracy_removed`
**任务（`_hardcoded.txt:5471-5491`，root = country）**：`on_mission_start` / `on_mission_completion` / `on_mission_abort`（原版自带 `add_stability = stability_mild_penalty`）/ `on_mission_task_start` / `on_mission_task_completion` / `on_mission_task_bypass` —— 六个都**空着**，是给 mod 挂"所有任务统一逻辑"的入口；任务链本身**不靠 on_action 驱动**（引擎按候选池 + `chance` 抽取，见 `fields\common-missions.md`）
**其它**：`on_game_start` / `on_command_gained` / `on_command_lost` / `on_omen_god_selected` / `on_accepted_call_to_arms` / `on_rejected_call_to_arms` / `on_max_doom_reached` / `on_colonial_charter_finished` / `on_colonial_charter_failed`

> **每个钩子段头部都有注释标注 root 作用域与可用 scope**（这正是 `_hardcoded.txt` 值 137 KB 的原因）。mod 要挂引擎事件，**直接写同名块追加内容即可**。

## 审查要点

- **`effect` 与事件并发**（官方原文）：在 `effect` 里改的值**不能**期望同一次触发的事件读到；需要传值就走 `save_scope_as` 或事件自身的 scope。
- **`fallback` 别成环**（官方警告：可能阻止时间推进）。
- **`delay` 是"从这行之后"生效**，且**新的 delay 覆盖旧的**；延迟事件要求"执行时与延迟结束时都有效"——条件会被查两次。
- **`100 = 0` 是必需的设计**（否则稀有事件会在其它候选都无效时必发）；`chance_of_no_event` 与 `chance_to_happen` 是两层独立判定。
- `trigger` 为假 → 整个 on_action 静默跳过（不报错），调试时容易误判为"钩子没触发"。
- 脉冲类文件的**频率由文件名/注释约定**（monthly / yearly / biyearly / four_yearly），不是字段——按目标频率放对文件。
- 未在 readme 中说明：本类目**没有 readme**，只有 `on_actions.info`；硬编码钩子的完整清单（**216 − 84 条非硬编码**）以 `_hardcoded.txt` 为准。（旧档写 217，逐档相加实为 **216**——2026-09 体检修正。）

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\on_action\` 全量 **27 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **注意**：本类目**本体没有 readme.txt**——下面全部是实测结果，不存在"漏写"一说。

### 一、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `effect` | 125 | 17 |
| `trigger` | 59 | 10 |
| `events` | 53 | 11 |
| `random_events` | 41 | 17 |
| `on_actions` | 22 | 13 |
| `first_valid_on_action` | 4 | 4 |
| `random_on_action` | 1 | 1 |

### 二、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `location` | 636 | start_weather_system、if、scope:target |
| `limit` | 549 | random_avatar_for_god、every_cabinet、every_international_organizations_member_of、random_known_country |
| `if` | 440 | every_international_organizations_member_of、on_game_start、on_international_organization_changed_leader、on_character_death |
| `type` | 208 | add_estate_satisfaction、move_building、has_scripted_relation、add_cooldown |
| `speed` | 157 | start_weather_system |
| `width` | 157 | start_weather_system |
| `length` | 157 | start_weather_system |
| `start_weather_system` | 157 | ?、weather_monthly_pulse、if、effect |
| `strength` | 157 | start_weather_system |
| `not` | 122 | scope:winner、scope:location、if、scope:ruler |
| `scope:target` | 100 | limit、?、every_estate_privilege、if |
| `scope:winner` | 93 | limit、?、else、if |
| `trigger_event_non_silently` | 93 | every_attacker、random、every_international_organization_member、every_country |
| `OR` | 85 | scope:winner、scope:target.owner、any_owned_location、any_subject |
| `name` | 78 | change_variable、is_target_in_global_variable_list、remove_list_global_variable、remove_from_variable_map |
