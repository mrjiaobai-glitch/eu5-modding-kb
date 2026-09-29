# common/subject_types（附庸类型）

> **一句话**：附庸类型的 74 字段全景与作用域表，含纳贡、吞并速度、忠诚、解约路径与解约义务实测。
> **什么时候看**：新增附庸类型、要调吞并或忠诚，或核对各触发器 root 作用域时翻这篇。
> **体量**：508 行 · 约 24 分钟通读

## 目录

- [触发类字段（作用域各异，重点核对）](#触发类字段作用域各异重点核对)
- [数值/枚举字段](#数值枚举字段)
- [审查要点](#审查要点)
- [subject_pays 实现（属国每月付宗主，2026-09 实测）](#subject_pays-实现属国每月付宗主2026-09-实测)
- [四种"殖民 / 海外"附属国（2026-09 实测）](#四种殖民--海外附属国2026-09-实测)
- [实查补缺（2026-09，readme 未写但实测确认）](#实查补缺2026-09readme-未写但实测确认)
  - [`annexation_speed` 可以是动态的（2026-09 实测，含"越久越难吞"的做法）](#annexation_speed-可以是动态的2026-09-实测含越久越难吞的做法)
  - [❌ `subject_territory_connectivity` 不可脚本读（2026-09 实测）](#-subject_territory_connectivity-不可脚本读2026-09-实测)
  - [附庸吞并的完整机制与反制（2026-09 实测）](#附庸吞并的完整机制与反制2026-09-实测)
  - [脚本值里可读的"附庸状态"（2026-09 实测）](#脚本值里可读的附庸状态2026-09-实测)
  - [三个"谈判成本"字段的实测校准（2026-09；含一处 KB 更正）](#三个谈判成本字段的实测校准2026-09含一处-kb-更正)
- [义务轴：`vassal`（紧极）↔ `tributary`（松极）](#义务轴vassal紧极-tributary松极)
- [解约路径：引擎白送的 CB（**不要自建**）](#解约路径引擎白送的-cb不要自建)
  - [解约动作带来的 loc 义务](#解约动作带来的-loc-义务)
- [忠诚：`subject_modifier` 里的 `loyalty_to_overlord`](#忠诚subject_modifier-里的-loyalty_to_overlord)
  - [⚠️ 别和 `subject_loyalty` 搞混（两个键都是 `category=country`，写错不报错）](#️-别和-subject_loyalty-搞混两个键都是-categorycountry写错不报错)
  - [忠诚是**算出来的**，没有"初始值"这个库存](#忠诚是算出来的没有初始值这个库存)
  - [集权／分权轴：一个 `subject_loyalty` 的 ±大坑](#集权分权轴一个-subject_loyalty-的-大坑)
  - [本体镜像范例：`fiefdom` 就是"漂移 + 补忠诚"的完整写法](#本体镜像范例fiefdom-就是漂移--补忠诚的完整写法)
- [74 字段全景：别只写 readme 上那十几个](#74-字段全景别只写-readme-上那十几个)
  - [⚠️ 事实上的必填：20/20 全用的 15 个](#️-事实上的必填2020-全用的-15-个)
  - [紧↔松轴上的字段实测（`vassal` 紧极 vs `tributary` 松极）](#紧松轴上的字段实测vassal-紧极-vs-tributary-松极)
  - [⚠️ "只写过 `yes`"的字段：省略 = 否](#️-只写过-yes的字段省略--否)
  - [`color`：类型自己的字段，指向命名色](#color类型自己的字段指向命名色)
  - [其他易漏项](#其他易漏项)
  - [可复现脚本（pwsh，非递归，单目录）](#可复现脚本pwsh非递归单目录)

来源：`in_game\common\subject_types\readme.txt`

## 触发类字段（作用域各异，重点核对）

```
<subject_type> = {
  visible=<trigger>(root=overlord,target=subject); enabled=<trigger>(root=overlord,target=potential subject)
  visible_through_diplomacy=<trigger>(root=overlord,target=subject); enabled_through_diplomacy=<trigger>(root=overlord,target=potential subject)
  visible_through_treaty/enabled_through_treaty=<trigger>(可选;root=overlord,target,recipient=和约对象,war)
  creation_visible=<trigger>(root=overlord); subject_creation_enabled=<trigger>(root=overlord,target_province=潜在地理)
  release_country_enabled=<trigger>(root=overlord,target=potential subject)
  can_attack=<trigger>(root=subject type,overlord/subject/attacker/defender)
  can_rival/can_marry=<trigger>(root=subject type,overlord/subject/actor/recipient)
  allow_declaring_wars=<trigger>(root=subject type,scope:attacker,scope:defender)
  join_offensive_wars_always/auto_call/can_call=<trigger>(root=subject type,scope:actor=caller,scope:recipient=callee,scope:target)
  join_defensive_wars_always/auto_call/can_call=<trigger>(同上)
}
```

## 数值/枚举字段

```
  minimum_opinion_for_offer=<int>; type=<location/pop/building/army>
  overlord_modifier/subject_modifier=<modifier>; great_power_score_transfer=<float>; government=<government key>
  level=<int>(低等级自主度高;经验:3=可吞并附庸,0=名义附庸)
  can_be_annexed=<yes/no>(默认yes); annexation_speed=<float script value>(默认1)
  annexation_min_years_before=<int>; annexation_min_opinion=<int>; annexation_stall_opinion=<int>
  subject_pays=<price>(附庸每月付宗主); diplomatic_capacity_cost_scale=<float>(非 scriptvalue)
  subject_can_cancel/overlord_can_cancel=<yes/no>; will_join_independence_wars=<yes/no>
  fleet_basing_rights=<yes/no>; food_access=<yes/no>; use_overlord_laws=<yes/no>(共用法律政策)
  on_overlord_becomes_a_subject=<cancel_subjects/transfer_subjects/nothing>(默认nothing)
  annulled_by_peace_treaty=<yes/no>(默认yes); annullment_favours_required=<int>
  use_overlord_map_color/use_overlord_map_name=<yes/no>; only_overlord_culture/only_overlord_or_kindred_culture/only_overlord_court_language=<yes/no>
  can_overlord_recruit_regiments/_build_ships/_build_roads/_build_buildings/_build_rgos=<yes/no>
  overlord_share_exploration=<yes/no>; overlord_protects_external=<yes/no>(默认yes); overlord_protects_other_subjects=<yes/no>(默认no)
  counts_as_external=<yes/no>(默认no); can_be_force_broken_in_peace_treaty=<yes/no>; overlord_can_enforce_peace_on_subject=<yes/no>
  war_score_cost=<script value>; base_antagonism=<script value>(≤0 用代码计算值)
  monthly_favor_gain=<script value>(root=人情方,scope:overlord,scope:subject)
  diplo_chance_accept_subject/diplo_chance_accept_overlord=<tag=values 列表>(如 border_distance=-0.1)
  ai_wants_to_be_overlord/ai_wants_to_be_subject=<effect script>(scope:overlord,scope:subject)
  institution_spread_to_overlord/institution_spread_to_subject=<script value>
  has_overlords_ruler=<yes/no>(宗主统治者兼任附庸统治者;创建时宗主无统治者则附庸脱离联合进摄政)
  has_overlords_religion=<yes/no>; has_limited_diplomacy=<yes/no>; can_change_rank=<yes/no>; can_change_heir_selection=<yes/no>
  on_enable=<effect>(root=subject,future_overlord=overlord); on_disable=<effect>(root=subject,former_overlord=overlord)
  on_monthly=<effect>(root=subject)
}
```

## 审查要点

- 各 trigger root 不同（visible 是 overlord；can_attack 是 subject type）——混用作用域是高频错误。
- `type` 枚举：location/pop/building/army；`on_overlord_becomes_a_subject` 枚举：cancel_subjects/transfer_subjects/nothing。
- 未在 readme 中说明：本地化键格式（实测见 SKILL.md 第 6 节：顶层键即 loc 键 + `LEAD_<名>`/`AM_<名>`）。

## subject_pays 实现（属国每月付宗主，2026-09 实测）

- 字段写法：`subject_pays = <price 键>`（原版裸键引用，如 `subject_pays = subject_pays_vassal`）。
- price 定义在 `in_game\common\prices\`（原版 03_diplomacy.txt 顶部）：`scaled_gold = 0.2` = 附庸月收入 20% 自动转宗主；可配 `scaled_manpower`/`scaled_sailors` 同时抽人力/水手；`ignore_inflation = yes` 防通胀缩放。原版比例：vassal 0.2 / colonial 0.025(+水手人力 0.1) / tributary 0.2(+0.05+0.05) / march 0.1 / trade_company 0.5 / pronoia 0.2。
- mod 自定义：新建 `in_game\common\prices\zzz_*.txt` 定义自有 price（scaled_gold 比例或 `gold = N` 固定额）→ subject type 里改指。
- 注意与 overlord_modifier 的 `monthly_gold_income`（拨款/收费修正）是两套独立机制，可并存（拨款 + 缴税双轨）。

## 四种"殖民 / 海外"附属国（2026-09 实测）

| | `colonial_nation` 殖民领 | `conquistador` 征服者 | `dominion` 自治领 | `trade_company` 贸易公司 |
|---|---|---|---|---|
| level | 1 | 1 | **3** | 1 |
| 纳贡 | `subject_pays_colonial` | `subject_pays_vassal` | `subject_pays_vassal` | `subject_pays_trade_company` |
| 可否吞并 | **`can_be_annexed = no`** | 可（20 年 + 好感 150） | — | **no** |
| 外交容量系数 | 0.5 | **0.1** | 0.5 | 0.5 |
| 列强分转移 | 0.75 | 0.5 | 0.5 | 0.75 |
| 政体 | republic | — | — | republic |

- **`colonial_nation` 的关键标记是 `is_colonial_subject = yes`**——殖民地联邦 IO、殖民革命局势、`merge_colonies`、`send_people_to_the_colonies` 全都用它来识别"殖民附属国"；写自定义殖民附属国类型时**必须带上这个字段**。
- 殖民领另有：宗主兼任统治者 = no、`will_join_independence_wars = yes`、`shares_exploration_with_overlord = yes`、`merchants_to_overlord_fraction = 0.33`（贸易优势上交）、`can_change_heir_selection = no`；`subject_creation_enabled` / `release_country_enabled` 都要求目标省份 `is_overseas_for_owner = yes`。
- 机制全貌见 `vanilla\vanilla-colonization-and-exploration.md` §6。

## 实查补缺（2026-09，readme 未写但实测确认）

**`overlord_modifier` / `subject_modifier` 是仅有的两个修正挂点，都只作用于国家。** readme 里**不存在** location / area / province 级 modifier 字段——想在附庸领土上挂地点修正是没有声明式钩子的（`on_monthly` 是命令式 effect，root = subject，只能迂回触及地点）。

**"附庸推动宗主分权"是逐类型 opt-in，不是全局机制。** 实测（EU5 1.3.x）：

- 只有 **4 / 20** 个类型给宗主 `monthly_towards_decentralization`：`vassal`、`fiefdom`、`uc_bey`、`pronoia`（都在 `overlord_modifier` 内）
- 值 = `societal_value_tiny_monthly_move` = **0.025**／每个附庸
- **mod 新增类型不会自动继承**——不写该键就没有；原版 `march` / `colonial_nation` / `trade_company` 的 `overlord_modifier` 为空，就是 opt-out 的先例
- **全局 auto_modifier 假设已排除**：`auto_modifiers\` 全目录 grep `decentraliz` = **0 命中**；`defines\` 无 CENTRALIZ / DECENTRALIZ 常量；`societal_values\00_default.txt` 无按附庸数量缩放的逻辑
- 20 个定义的 `overlord_modifier` 其余内容：多数是 `monthly_prestige = 0.01` 或空 `{ }`；`appanage` 给 stability_decay + legitimacy + 阶层力量；`hanseatic_member` 给 merchant capacity；`state_bank` 给 bank_interest + bonds；`tusi` 给 prestige + legislative_efficiency + subject_income

> **grep 陷阱**：对 `subject_types\` grep `monthly_towards` 会得到 6 处命中，但**不是同一回事**——`vassal`/`fiefdom`/`uc_bey`/`pronoia` 在 `overlord_modifier` 内推**宗主**分权 ✅；`samanta.txt:266` 虽在 `overlord_modifier` 内但推的是**和解轴**；`tusi.txt:102` 在 **`subject_modifier`** 内，推的是**附庸自己**的汉化轴，与宗主无关。

> 想按"附庸数量"推某条社会价值轴，原版**唯一**的模板在 DLC 的 `auto_modifiers\byzantium.txt:554-582`：`potential_trigger` 里 `any_subject` + `scales_with = <计数脚本值>` + `monthly_towards_<轴>`，计数用 `script_values\byz_values.txt:79-105` 的 `every_subject { root = { add = 1 } }`。**不是**用 define，也不缩放分权轴。

**`monthly_towards_centralization` 与 `_decentralization` 是一条轴的两个键**（轴内部名 `centralization_vs_decentralization`），两者都 `min = 0` → **不能写负值**，反向推必须换用另一侧的键。全库共 **34 个 `monthly_towards_*` 键 = 17 轴 × 2 端**，全部 `category = country`（行号连续 15452–15749）。

**readme 未文档化但原版在用的字段恰好 9 个**（与 101 档字段库口径一致）：`strength_vs_overlord`、`is_colonial_subject`、`shares_exploration_with_overlord`、`merchants_to_overlord_fraction`、`allow_subjects`、`content_priority`、`overlord_inherit_if_no_heir`、`maritime_path_tolerance`、`color`。

**`color` 是可选字段**：20 个定义里 2 个不写（`secessionists`、`uc_bey`），图标另有 `main_menu\gfx\interface\icons\subject_types\_default.dds` 兜底。写了 `color = subject_X` 才需要在 `main_menu\common\named_colors\02_map.txt`（`#SUBJECT TYPES` 段，4201–4219 行）加对应色值。

**readme 有文档但 20 个定义一个都没用**的字段 10 个：`can_marry`、`on_monthly`、`on_overlord_becomes_a_subject`、`annulled_by_peace_treaty`、`base_antagonism`、`monthly_favor_gain`、`ai_wants_to_be_subject`、`join_offensive_wars_auto_call`、`join_defensive_wars_auto_call`（+ 拼错的 `..._scape`）。**用这些字段等于没有原版校准数据可参照。**
### `annexation_speed` 可以是动态的（2026-09 实测，含"越久越难吞"的做法）

**`annexation_speed` 接受 `<float script value>`**（`subject_types\readme.txt:26`）。这不是套话——readme 对**非**脚本值字段会显式标注，见 `:43` 的 `diplomatic_capacity_cost_scape = <float> ... (not a scriptvalue due to checked so much)`。**但本体 10 处用法全是裸字面量，命名脚本值零先例。**

| 类型 | `annexation_speed` | `annexation_min_years_before` |
|---|---|---|
| `secessionists` | **5** | 0 |
| `maha_samanta` / `direct_imperial_free_city` | **2** | 10 |
| `vassal` / `fiefdom` / `appanage` / `dominion` / `conquistador` / `uc_bey` | **1** | 10 / 15 / 50 / 20 / — |
| `pronoia` | **0.5** | 20 |
| 9 个 `can_be_annexed = no` 的类型 | —（不设） | — |

本体速度取值域 = 恰好 **`{0.5, 1, 2, 5}`**（不设则默认 1）；`annexation_min_years_before` 域 = `{0, 10, 15, 20, 50}`；`annexation_min_opinion` 在用值 = 125 / 150 / 190。

**⭐ 关键：附庸关系的"年龄"有原生触发器 —— `dependency_length_days`**

```
# in_game\common\trigger_localization\country_triggers.txt:1705-1710
# loc: main_menu\localization\english\triggers_l_english.yml:4209
dependency_length_days = {
    target = scope:actor
    value >= MIN_DAYS_AS_SUBJECT_FOR_ENFORCEMENT
}
```

- **root = 附庸国，`target = ` 宗主国**；**数值比较（天数）**
- 本体活用法：`country_interactions\enforce_culture.txt:37-40` 与 `enforce_religion.txt:37`（两处交互都是 `type = subject`、目标取 `every_subject`，root/target 约定由用法反证）
- 阈值本体用脚本值：`common\script_values\define_values.txt:37` → `MIN_DAYS_AS_SUBJECT_FOR_ENFORCEMENT = 3650`（10 年）
- 君合版姊妹键：`union_length_days`（`country_triggers.txt:1698-1703`；本体用法 `subject_types\dominion.txt:121`）

⇒ **"当了 N 年附庸"原生可读**，不需要 dated-flag 阶梯，也不需要 `on_monthly`。

**⚠️ 反面：dated flag 读不出时长。** 全库变量触发器只有存在性/列表形态类（`has_variable`、`has_global_variable`、`variable_list_size`、`is_target_in_variable_list`…），**没有任何"变量已存在 N 天"的读法**；`in_game\common\trigger_localization\` 里 `for_years` 只有一个命中（`had_disaster_for_years`，灾难专用）。

**脚本值里可以写触发器与变量**（做动态 `annexation_speed` 的地基，均为本体实例）：
- `script_values\building_caps.txt:37-46` → `if = { limit = { … has_variable = extra_levels_clothmaker_variable } add = { … } }`
- `script_values\byz_values.txt:90-96` → `limit` 内直接写附庸类触发器
- `script_values\economy_satisfaction_target.txt:5` → `limit = { exists = var:nobles_estate_target }`
- 作用域切换与具名 scope：`hre_action_values.txt:92-99`、`io_policy.txt:111-114`
- 语法权威：`script_values\_script_values.info:36-48`（`if`/`else_if`/`else` + `limit`）；`:14` = **每次求值都重算，不缓存**
- ⚠️ `annexation_speed` 的 root 是**吞并方**，要读附庸侧必须 `scope:target = { … }`。同款写法本体见 `samanta.txt:126`（在 `enabled_through_diplomacy` 里用 `scope:target = { NOT = { has_variable = … } }`）

**两个未验证的运行时风险**（做动态值前应实机确认）：
1. 引擎是**每月重算** `annexation_speed` 的脚本值，还是**在吞并开始时快照一次**？`readme.txt:26`「per month」与 `on_annexing_subject_monthly_pulse` 的存在都指向每月，但调用点在引擎侧。**若快照，动态值会被冻住。**
2. `dependency_length_days` 在 **`annexation_speed` 的求值上下文**里是否合法（写在 `limit` 内）。结构上一致，但**没有这个确切组合的本体先例**。

**其他可用挂点**：
- `annexation_speed_base`（平坦）与 `annexation_speed_modifier`（百分比），均 `category=country`，`main_menu\common\modifier_type_definitions\00_modifier_types.txt:5919-5930`；本体脚本用法 `government_l_english.yml:1213-1217`（俄国某行动授予）
- `on_action\country_monthly.txt:248-263` → **`on_annexing_subject_monthly_pulse`**（**root = 宗主，`scope:target` = 附庸**，吞并进行中每月触发；当前 body 只有 `random_events`，**可安全追加 `effect`**）
- `on_annexation_start` / `on_annexation_cancel`（`on_action\_hardcoded.txt:959-977`、`:980-989`）；`is_being_annexed` / `is_annexing`（`country_triggers.txt:2048`、`:2056`）
- ⚠️ 本体本地化有个笔误别照抄：`subject_interactions_l_english.yml:16` 写 `ShowModifierTypeName('annexation_speed')`，但**不存在**这个名字的修正类型（只有 `_base` / `_modifier`）

### ❌ `subject_territory_connectivity` 不可脚本读（2026-09 实测）

「Territory Connectivity」（附庸领土连通性）是**引擎内部计算的 0–100 分**，**没有触发器、没有脚本值、没有修正键、没有 define**，任何作用域都读不到，只在 GUI/本地化层暴露（`main_menu\gui\shared\diplomatic_tooltips.gui:192` 是个 `[subject_territory_connectivity|E]` 本地化常量，不是取值函数）。

- 概念定义：`main_menu\common\game_concepts\00_game_concepts.txt:2619-2621`；词条 `game_concepts_l_english.yml:1831-1832`
- 唯一的数据结构是同名 **static modifier**（`main_menu\common\static_modifiers\country.txt:215-221`），body 是 `loyalty_to_overlord = 20`，且**未注册**进 `modifier_type_definitions`（属隐藏修正）
- **它唯一的作用就是那 +20 忠诚**；而忠诚经 `SUBJECT_LOYALTY_ANNEXATION_SPEED_FACTOR = 0.2`（`define_values` 隔壁的 `NDiplomacy`）**让吞并更快**
- ⇒ **本体里"高连通 = 快吞"**。任何"高连通 → 难吞"的设计都是在**反接**引擎已有的耦合，且会与连通性面板上的说明文字（"Higher connectivity increases its subject_loyalty"）**在 UI 上直接矛盾**
- 唯一相关的逐类型旋钮是 `maritime_path_tolerance`（**字面量 float，非脚本值**；20 个类型里只有 6 个设了它：`vassal`/`march` −0.25、`fiefdom`/`dominion` −0.2、`tributary` +0.25、`colonial_nation` +0.5），它调的是**海上路径容忍距离**，不是分数
- **没有任何修正键能改连通性**（`modifier_type_definitions` 里 grep `connect` = 0 命中）

**可近似替代（连通性的输入本身可读）**：`average_control`（国级）、`average_control_in_home_region`（国级）、`proximity`（**地点级**，到首都距离）、`location_maritime_presence_power = { target = <国> value >= X }`（地点级）。**只能重新推导，不能直接读。**
### 附庸吞并的完整机制与反制（2026-09 实测）

**⭐ 原版已实现"敢吞就开战"，不需要自己写**（`on_action\_hardcoded.txt:959-977`，root = **吞并方**，`scope:target` = 被吞方）：

```
on_annexation_start = {
    scope:target = {
        add_casus_belli = { target = root type = casus_belli:cb_resist_annexation }
    }
    every_related_country = {
        type = guarantee_vassal_independence
        add_casus_belli = { target = root type = casus_belli:cb_prevent_annexation }
    }
}
```

- 被吞的附庸自己拿到 **`cb_resist_annexation`**（抵抗吞并 CB）
- **所有担保它独立的国家**拿到 **`cb_prevent_annexation`**（阻止吞并 CB）—— 这就是"被吞时拉独立运动"
- `on_annexation_cancel`（`:979-989`）在取消吞并时收回 `cb_resist_annexation`
- 另有 `LOYALTY_FROM_INDEPENDENCE_MOVEMENT_SCALE = 10` / `_MAX = 20`：**独立运动的相对强度直接扣忠诚**
- **硬停**（`diplomacy_l_english.yml:1531-1533`）：内战 / 叛乱战争 / **不忠**（`is_disloyal_subject`）→ 根本吞不了
- 门槛：`ANNEX_TOO_LOW`（最低观感）、`ANNEX_DISLOYAL`

**吞并速度的完整叠加栈**（新增项就是叠在这上面）：

| 项 | 值／来源 |
|---|---|
| `annexation_speed_base` | **0.5**，所有国家自动（`auto_modifiers\country.txt:51`，`country_base_values` 块）；另有 `diplomatic_maintenance_mod` 再给 +0.5（`static_modifiers\country.txt:400`） |
| **`annexation_speed`** | **subject type 的字面量**（默认 1）——**这就是"按类型调吞并速度"的唯一旋钮** |
| `annexation_speed_modifier` | 百分比，**宗主全域**（见下） |
| `SUBJECT_LOYALTY_ANNEXATION_SPEED_FACTOR` | **0.2** × 忠诚（`defines:2018`） |
| `MULTIPLE_ANNEX_PENALTY` | **−0.5**／每个并发吞并（`:2017`） |
| 小国／相对体量项 | 见 `ANNEX_TINY_SUBJECT`（`diplomacy_l_english.yml:1527`） |

其余相关 define（`NDiplomacy` 块 = `00_defines.txt:1992-2417`，完整）：
`MULTIPLE_ANNEX_PENALTY −0.5`(2017) · `SUBJECT_LOYALTY_ANNEXATION_SPEED_FACTOR 0.2`(2018) · `SUBJECTS_STRENGTH_THRESHOLD 0.25`(2020) · `SUBJECTS_STRENGTH_SCALE −2`(2021) · `LOYALTY_FROM_INDEPENDENCE_MOVEMENT_SCALE 10`(2022) · `_MAX 20`(2023) · `MAX_ANNEX_SIZE 2`(2170) · `DEFAULT_ANNEX_MIN_RELATION 190`(2173) · `DEFAULT_INTERNATIONAL_ORGANIZATION_ANNEX_MIN_RELATION 150`(2174) · `DEFAULT_ANNEX_MIN_YEARS 10`(2175) · `PEACE_TREATY_ANNEX_REVOLTER_MAX_COST 70`(2306)
块外（成本/AI，非月速度）：`ANNEX_BASE_COST 200`(212, NCountry) · `ANNEX_COST_PER_LOCATION 10.0`(213) · `POP_SATISFACTION_NUDGE_AFTER_REANEX 0.5`(256) · `AI_ANNEX_SUBJECT_DISTANCE_THRESHOLD 50`(1035) · `AI_ANNEX_SUBJECT_BORDERING_CONTROL_NEEDED 0.35`(1036) · `AI_ANNEX_UTILITY 0.25`(1311) · `AI_ANNEX_UTILITY_OTHER_SUBJECTS 0.05`(1312)

**引擎效果**：`change_annexation_progress = { target = <国> value = ±X }`（本体仅 `events\missionevents\generic_mission_events.txt:413` +0.05 与 `:480` −0.05 两处）；`annex_country = { country = <国> reason = Diplomatic }`（包装版 `scripted_effects\country_effects.txt:2345-2354` `annex_country_diplomatically`）

**注册的 `annex` 修正键共 8 个**（`00_modifier_types.txt`）：
`diplomatic_annexation_efficiency`(3816, good/percent) · `hostile_diplomatic_annexation_efficiency`(3824, percent) · `annexation_speed_base`(5919) · `annexation_speed_modifier`(5925, percent) · `ai_force_annexation_modifier`(12551, ai) · `rtr_demand_annexation_price_cost_modifier`(13120, bad) · `years_to_annex_members`(15436, **IO 类目**) · `enable_annexation_of_members`(15444, **IO 类目, boolean**)

> ⚠️ **`annexation_speed_modifier` 不是"按类型"的旋钮。** 它 `category=country`、**宗主全域**，而且经 `overlord_modifier` 施加时**按该类型附庸的数量叠加**（`overlord_modifier` 每个附庸关系发一份——本体 `hanseatic_member` 每个成员 +2.5% 贸易容量、`appanage` 每个 −0.025 征召规模、`vassal` 每个推 0.025 分权，都是这个用法）。**结果是"这类附庸越多，宗主吞并*所有*附庸都越快"，做不到"就这个类型更难吞"。** 要按类型定向，只能用 `annexation_speed` 字段本身。

> ⚠️ **附庸侧的反制键是另外两个**：`frustrate_annexation` 内阁行动给 `hostile_diplomatic_annexation_efficiency = -0.2`（`cabinet_actions\frustrate_annexation.txt:27`），`sow_disloyalty` 给 `loyalty_to_overlord = -10`（`sow_disloyalty.txt:16-18`）。**附庸侧永远不用 `annexation_speed_modifier`。**

### 脚本值里可读的"附庸状态"（2026-09 实测）

`annexation_speed` 的求值语境是 **root/`scope:actor` = 吞并方，`scope:target` = 被吞方**。要读附庸侧必须 `scope:target = { … }`——**脚本值里换作用域是官方支持的**（`script_values\_script_values.info:114-122`「You can change scope within script values just as you can in regular script」），本体实例：`hre_action_values.txt:204-207`、`io_policy.txt:873-880`；`scope:target` 在 `subject_types` 自身也有先例（`hanseatic_member.txt:59-64`、`dominion.txt:115-129`）。

| 标识 | 类型 | root 作用域 | 数值/布尔 | 本体用法 |
|---|---|---|---|---|
| `subject_loyalty` | **触发器**（同时也是修正键，但语义相反：修正键 = "我附庸的忠诚"） | **附庸国** | 数值比较 | `subject_interaction_events.txt:1005` `subject_loyalty > 50`；`sow_disloyalty.txt:9` |
| `liberty_desire` | 触发器 | **附庸国** | 数值 | `disaster_triggers.txt:401` `any_subject = { liberty_desire > 50 }` |
| `relative_strength` | 触发器（**需要 target**） | 国 vs target | 数值 % | `dominion.txt:125-128` `relative_strength = { target = scope:target value > 0.7 }` |
| `num_locations_owned_or_owned_by_subjects` | 触发器 | 国 | 数值 | `hanseatic_member.txt:62/69/76` |
| `country_tax_base` / `country_total_development` / `country_average_control` | 触发器 | 国 | 数值 | `exploration_triggers.txt:6`；`triggers_l_english.yml:3689`、`:7221` |
| `opinion = { target = X value >= N }` | 触发器 | 国 vs target | 数值 | `subject_interaction_events.txt:1004` |
| `is_being_annexed` / `is_annexing` / `is_disloyal_subject` | 触发器 | 国 | 布尔 | `frustrate_annexation.txt:7/12/15`；`country_triggers.txt:2048/2056` |
| ⚠️ `subject_opinions` | **只是修正键，不是触发器**（`00_modifier_types.txt:6192`，`bias_type="opinion"`） | — | — | 要读用 `opinion = { target = … }` |
| ⚠️ `strength_vs_overlord` / `great_power_score_transfer` | **只是 subject type 的数值字段**，**不可读**（无触发器/脚本值证据） | — | — | — |
| ⚠️ `tax_base` | 歧义：在 `subject_types` 里是 **`diplo_chance` 权重键**（`vassal.txt:131`），**不是**触发器；国家税基比较本体一律写 **`country_tax_base`** | — | — | — |
| ⚠️ `development` / `average_control` | root 是 **location / province**；国家级等价物是 `country_total_development` / `country_average_control` | — | — | — |

**性能**：`_script_values.info:14`「每次求值都重算，复杂公式有性能代价」；`:61-70` 运算按**书写顺序**执行（`max`/`min` 要放最后）；**没有任何"不许在脚本值里换作用域/写触发器"的警告**。
### 三个"谈判成本"字段的实测校准（2026-09；含一处 KB 更正）

#### ⚠️ 更正：`base_antagonism` 不是附庸类型的字段，是**和约**的

- `subject_types\readme.txt:71` 确实写了 `base_antagonism`，但 **20 个本体类型里 0 个使用**（DLC 里也没有；唯一 DLC 类型 `pronoia` 不设）
- **真正的挂点在和约**：`peace_treaties\readme.txt:54` `base_antagonism = script value for the max amount of antagonism gained, will be adjusted by various factors per country`；`:55` `antagonism_type = key of a bias type for the antagonism that will be added`
- 本 KB 的 `vanilla\vanilla-diplomacy.md:184` 与本文档早前的表把 `base_antagonism` 归到了 subject type 名下——**readme 为真、本体为空**，实战应在和约里写

**本体四个"造附庸"的和约及其敌意值**：

| 和约 | 类型 | `base_antagonism` | `antagonism_type` |
|---|---|---|---|
| `force_tributary` | `tributary` | **20** | `antagonism_force_tributary` |
| `subjugate_neighbor_native` | **`vassal`** | **5** | `antagonism_subjugate_neighbor_native` |
| `league_of_public_weal_royalist` / `_appanage` | `appanage` | **不写** | 不写 |
| （`subjugate_natives` **不造附庸**，只转移 pop） | — | 1 | `antagonism_subjugate_natives` |

⇒ **本体一半的造附庸和约什么都不写，直接吃引擎算出来的值。**

**敌意尺度**：`ANTAGONISM_MIN 0` / `_BIAS_MIN −100` / `_MAX 1000`（`defines:2163-2165`）；**coalition 门槛 = 50**（警告 40，`defines:2122-2123`）；AI 自我约束 `SAFE_AMOUNT_OF_ANTAGONISM 30`（`:1248`）。本体条约实取值域 = **0.5 ~ 50**（50 = `claim_french_throne`、`delhi_peace_treaties` 的 coalition 级）。**`base_antagonism` 是上限（cap），不是加项**——readme 的 "max amount … will be adjusted" 与 subject-type readme 的 "override … unless the value is 0 or less" 都指向替换语义。

**⭐ 引擎已经给了附庸折扣**：`defines:2104` `ANTAGONISM_ADDITIONAL_MODIFIER_PEACE_SUBJECT_REDUCTION = -0.5`（"AE reduction if it is taken for a subject"）；另有 `:2105 SUBJECT_OF_AGGRESSOR = -0.80`、`:2081 WE_ARE_OVERLORD = -0.9`。**所以"变成附庸"本身已经比"割地"少一半敌意。**

**`antagonism_type` 的取值是开放集合**：全部定义在 `in_game\common\biases\05_antagonism_hardcoded.txt`（88 个顶层条目），形如 `antagonism_force_tributary = { value = 1 yearly_decay = 2 }`。**不是 define、不是脚本值、不是引擎 enum**——`common\biases\` 里的普通脚本块，**mod 可在自己文件里新增**（前缀 `antagonism_` 只是惯例，本体也有 `subjugated_elector_antagonism`、`rus_claimed_land_antagonism` 这种不带前缀的）。

**可读性**：`peace_treaty_antagonism` 触发器（`trigger_localization\peace_treaty_triggers.txt:8-13`）可**查询某和约会造成多少敌意**；`antagonism` 触发器（`country_triggers.txt:655-660`，国 vs target）可读现存量。写入用 `add_antagonism = { target = <国> modifier = <bias 类型> }` / `remove_antagonism` / `add_antagonism_no_duration` / `drop_antagonism_bomb`。

#### `war_score_cost`：本体基线是"不写"，不是 1

`subject_types\readme.txt:70`：`war_score_cost = <script_value> how much will this subject cost to establish in wars, **modifies the base war score cost calculation**`。

**20 个类型里只有 4 个写了它**：`fiefdom` **0.5**、`dominion` **0.5**、`samanta` **0.25**、`maha_samanta` **0.5**。**`vassal` / `march` / `tributary` / `appanage` 等 16 个全都不写** ⇒ **vassal 基线 = 省略该字段 = 引擎默认（恒等 = 1.0）**。**不要在文件里找 "vassal = 1"，它不存在**；"比附庸便宜"就是**任何 < 1.0 的字面量**，本体的便宜档是 0.25~0.5。

**注意还有两层乘子**（不是同一个字段）：
- **wargoal**：`wargoals\readme.txt:23/:33` `subjugate_cost = <float> # factor applied to warscore cost when making the target a subject`（本体约 95 处；通用臣服 wargoal `:163` = **0.25**，`take_country` = 0.5，`conquer_province` = 0.75，`demand_military_access` = 20.0）
- **和约自身的 `cost` 块**（readme:53）：`force_tributary.txt:2-18` = 20 + 100×经济比（钳 0~100）；`subjugate_neighbor_native.txt:3-5` = **平坦 75**；`league_of_public_weal_*` = 50
- ⇒ 总显示成本 ≈ base × `war_score_cost` × wargoal `subjugate_cost`，**除非和约硬写了 `cost` 块**（`force_tributary` 与两个 League 就是硬写）

#### `diplo_chance_accept_subject` / `_overlord`：`base` 是锚，其余全是可选覆盖

readme `:73-74`：`diplo_chance_accept_subject = <list of tag = values>`——"multiplier for various values … that lead to the overall acceptance"，例 `border_distance = -0.1`。**`base` 是每个块都有的锚点**（本体 44 处 `base =` 全在这些列表里）；其余键是**对引擎默认值的覆盖**（`country_interactions\readme.txt:6` "overrides of default acceptance values"），省略即保持默认。

**本体 20 个类型的 `base` 全表**：

| 类型 | `accept_subject.base` | `accept_overlord.base` |
|---|---|---|
| `vassal` | **−92** | **−52** |
| `march` | −92 | **+16** |
| `tributary` | −72 | +9 |
| `fiefdom` | −92 | +8 |
| `dominion` | −72 | +10 |
| `samanta` / `maha_samanta` | −92 | +8 |
| `pradhana_maha_samanta` | **−12**（本体最容易） | +8 |
| `pronoia` | −90 | −50 |
| `tusi` | −92 | **+18** |
| `uc_bey` | −92 | **+18** |
| `state_bank` | −50 | +20 |
| `hanseatic_member` | −50 | +8 |
| `appanage` / `conquistador` | **−9999**（外交上不可能） | **−9999** |
| `secessionists` | 无该块 | +10 |
| `colonial_nation` / `trade_company` / `imperial_free_city` / `direct_imperial_free_city` | **完全没有 `diplo_chance_*` 块**（纯条约/创建路径） | 同左 |

⇒ **"接受度更高"的实测做法：把 `accept_subject.base` 从附庸的 −92 抬到 −12**（对齐本体最容易的 `pradhana_maha_samanta`）；`accept_overlord.base` 用 +16~+18（对齐 `march` / `tusi` / `uc_bey`）。

⚠️ **但别只靠这个数。** 接受度高必须有语义撑着，否则新类型就是"没有代价的白拿"。本体自己给了现成的理由：**纽带越弱 ⇒ 进得容易、出得也容易**（见下面「义务轴」）。抬数值之前先问：这类型凭什么好谈？答案应该是它给对方上的枷锁也更松。

**UI 层面零新键**：拒绝/接受理由由两个**已注册的修正键**驱动——`reject_subjugation_reasons` / `accept_subjugation_reasons`（`00_modifier_types.txt:15222-15234`，本体 13 个块都在用，值恒为 −1 / 1），分解提示走 `DIPLOREASON_REJECT_SUBJECTION_REASONS`（`diplomacy_l_english.yml:905-906`）+ `ShowModifierTypeNameWithBreakdown`。**新附庸类型不需要新的理由 loc 键。**

---

## 义务轴：`vassal`（紧极）↔ `tributary`（松极）

新做一个主体类型时，**义务不要自己发明一套，直接在本体这条已有轴上取点**。两端实测（`in_game\common\subject_types\`）：

| 字段 | `vassal`（紧极，`vassal.txt`） | `tributary`（松极，`tributary.txt`） | 备注 |
|---|---|---|---|
| `join_offensive_wars_always` | 条件块（默认自动加入，少数特例豁免）`:41` | **整条不写** | ⚠️ **是 trigger 不是布尔**，写 `= yes` 会静默失败 |
| `join_defensive_wars_always` | 条件块（自动加入）`:53` | 条件块（自动加入）`:60` | 松极也保留防御义务 |
| `subject_can_cancel` | 不写 | **yes** `:86` | 本体 20 个类型 13 个写 `no`；写 `yes` 的只有 `tributary:86` 和 `samanta:33` |
| `overlord_can_cancel` | **yes** `:72` | **yes** `:87` | 只有这两个类型写；放人权是两极共有的 |
| `will_join_independence_wars` | **yes** `:67` | **yes** `:78` | **12 个类型全是 yes**（含 `march:43`、`hre:19`、`appanage:53`）。设 `no` 是反本体的少数派 |
| `overlord_protects_external` | 不写（readme：默认 yes） | **no** `:80` | 松极 = 不给外部保护 |
| `has_limited_diplomacy` | **yes** `:78` | **no** `:100` | |
| `can_change_rank` | **no** `:91` | **yes** `:89` | |
| `allow_declaring_wars` | 限制性条件块 `:80`（普通附庸不能宣战） | **`{ always = yes }`** `:88` | `appanage:62`、`hre:30` 同 |
| `strength_vs_overlord` | **−0.5** `:64` | **−0.25** `:84` | 全域实测值 `{−1, −0.5, −0.40, −0.33, −0.25, −0.2, −0.1, −0.05}`（`march` −1 最紧，`colonial_nation` −0.05 最松）。⚠️ readme 未文档化 |
| `diplomatic_capacity_cost_scale` | **1.0** `:60` | **0.2** `:91` | 本体档位 `0 / 0.05 / 0.1 / 0.2 / 0.25 / 0.5 / 0.75 / 1.0 / 1.25` 全有 |
| `can_be_annexed` | 默认（可吞） | **no** `:79` | 松极不可吞并 |

**要点**
1. `tributary` 是本体现成的「松主体」模板——做"自治/朝贡/藩属"类主体，**抄它的整套自由度**比自创更安全，也天然有校准。
2. `join_offensive_wars_always` / `join_defensive_wars_always` / `allow_declaring_wars` / `join_*_can_call` / `join_*_auto_call` **全是 trigger 块**（root = subject type，`scope:actor` / `scope:recipient` / `scope:target`）。短写即 `{ always = yes }` / `{ always = no }`。
3. **`subject_can_cancel` 的默认值 readme 没写**（`readme.txt:44` 只登记字段不写默认；`overlord_protects_external` 反倒写了 "defaults to yes"）。13/20 显式写 `no`、只有 2 个写 `yes`、`vassal` 干脆不写 ⇒ **不要赌默认值，需要的级一律显式写**。
4. `will_join_independence_wars` 是**自由度不是义务**，本体倾向 `yes`。想做"永不脱离"只能靠别的机制；靠这个字段设 `no` 是反本体的。
5. 反过来，**"升级链"的方向决定这些字段往哪走**：`samanta` 链是**升级=更紧**（`subject_can_cancel` 从 `yes:33` 变 `no:145/:252`，`strength_vs_overlord` 从 −0.25 变 −0.5）；如果设计意图是"升级=更自治"，这些字段必须**反向**走。
6. ⚠️ **`visible` 里本体自己有一道常被漏掉的「相对等级门」**：`country_rank_level >= scope:target.country_rank_level`，出现在 `vassal.txt:9`、`march.txt:9`、`tributary.txt:11`、`fiefdom.txt:80`、`dominion.txt:73`、`D008_pronoia.txt:7,40`。本体注释写在 `vassal.txt:7`：*"# High Kingship members can subjugate each other regardless of rank"*。作用 = **防止低等级国家把高等级国家收成附庸**。自建主体类型忘了写它，就会出现"伯爵把皇帝收成附庸"。
7. **等级相关触发器与枚举**：`country_rank`（`trigger_localization\country_triggers.txt:1056`，有 `none`/`global`/`first`/`third` 变体）、`country_rank_level`（`:1063`）、`country_rank_level_less_or_equal`（`:1070`）；等级枚举 `rank_empire` / `rank_kingdom` / `rank_duchy` / `rank_county`（`country_ranks\00_default.txt:1 / :52 / :95 / :140`）。⚠️ **`country_rank_level` 的数值映射（各等级 = 几）未证**，且本体在 `dominion.txt:70-71` 里先写 `exists = country_rank_level` 才比较，暗示它可能为空 ⇒ **想设"XX 级以上"的绝对地板，必须先实机确认数值**；相对门则不需要任何魔数（这是它的好处）。
8. **等级决定 `cultures_capacity` 基线**：`rank_county` 无（=0）／`rank_duchy = 0.5`／`rank_kingdom = 1`／`rank_empire = 2`（`country_ranks\00_default.txt:104 / :65 / :15`）。所以拿等级当主体类型的门，与"文化容量"这条线天然自洽。
9. **`diplo_chance` 里本体每个类型都写 `rank_difference = -5`**（`vassal.txt:115`、`march.txt:95`、`samanta.txt:64`、`tributary.txt:119`…）。自建类型漏掉它 = 接受度算子里少一项本体的标准权重。
10. **别拿 societal value 当主体类型的门**。本体 20 个类型的结构性门清一色是 `government_type`（`tributary.txt:52-58`）、`government = monarchy`（`uc_bey.txt:24`）、等级、`is_overseas_for_owner`（殖民领）这类**看得见、能主动改变**的条件；价值观滑块玩家几乎无法操控，做成门就是"不知道该干什么才能解锁"。

---

## 解约路径：引擎白送的 CB（**不要自建**）

设 `subject_can_cancel = yes` 后，**本体引擎会自动给原宗主发一个专用 CB，零脚本**：

| 证据 | 内容 |
|---|---|
| `casus_belli\00_hardcoded.txt:1` | 文件头 `#generate through code`——这一档全是代码直接发的 CB |
| 同上 `:18-23` | `cb_subject_broke_free = { create_visible = { always = no }  war_goal_type = superiority }`。`create_visible = always = no` ⇒ 不走常规可见性门，只可能由代码授予。**全本体 `common\` 里没有任何脚本 `add_casus_belli` 引用它** ⇒ 授予完全在引擎侧 |
| `casus_belli_l_english.yml:17 / :66` | `"Subject Broke Free"` / desc 原文 *"…and **return them to their place under the yoke**"*——**本体自己把"夺回来"写成了设计意图** |
| `diplomacy_l_english.yml:1625` + 14 个同类 | `BREAK_vassal_NEWDESC` = *"…relations will worsen, and **they will get a [casus_belli\|e] against us!**"*，按类型自动生成（`1671/1734/1779/1867/1909/1951/1995/2029/2064/2101/2133/2174/2215/3133`）。**`CANCEL_*_NEWDESC`（宗主主动放人，`:1624`）里没有这句承诺** ⇒ 只有"附庸自己走"才发 CB |

**关键推论**
1. **解约不产生停战。** 全本体 `on_action` / `casus_belli` / `country_interactions` / `events` 里都没有在解约路径上加停战；`on_becoming_free` 块（`_hardcoded.txt:2981-3068`）也不加。停战只是**禁止**解约的门（`diplomacy_l_english.yml:9` `not_possible_to_break_subject_in_truce`），**不是解约的后果** ⇒ 解约后可以立刻开打。加停战的正确效果名是 `add_truce_with` / `add_truce_with_mutual`（`effect_localization\country_effects.txt:1300-1324`），**没有 `add_truce`**；`TRUCE_YEARS = 5` / `SCALED_TRUCE_YEARS = 10`（`00_defines.txt:2191-2192`）。
2. **"破停战 = 稳定 50" 属实，且它是 price 不是 define**：`prices\00_hardcoded.txt:107-110` `war_breaking_truce = { stability = 50  war_exhaustion = 1 }`；另有 `war_no_cb = { stability = 15 }`（`:128`）、`declaring_war = { karma = 10 }`（`:99`）、`war_on_subject = { stability = 40 righteousness = 40 }`（`:135`）。
3. **想换成"打回附庸"的 CB，用 `cb_subjugation`**：`casus_belli\01_event_triggered.txt:103-112` = `years = 15`、`ai_subjugation_desire = 1000`、`ai_cede_location_desire = -1000`、`war_goal_type = take_capital_subjugation`，**loc 已有、不用新造**。挂载点优先用**类型自己的 `on_disable`**（`subject_types\readme.txt:22`：*"what happens when the subject type is broken. root = subject, former_overlord = overlord"*）——它天然只对本类型触发，比 `on_becoming_free` 干净。⚠️ `on_disable` 在**升级**（`change_subject_type`）时可能也触发 → 必须包 `if = { limit = { is_subject = no } }`。
4. **`on_becoming_free` 抓不到转移**：转移有独立钩子 `on_transfer_subject`（`_hardcoded.txt:5630-5631`，`root = subject, scope:overlord = new overlord, scope:former_overlord = old overlord`）；吞并、释放成国家、类型变更各自也都有独立钩子（`on_annexation_start` / `on_released_country` / `on_subject_type_changed`）。且 `_hardcoded.txt` 里约 110 个钩子**没有一个用 `trigger = {`** ⇒ 若要门控，写在 `effect = { if = { limit = { … } } }` 里，别加 `trigger`。
5. **前缀不对称（会静默出错）**：`is_subject_type = <键>` **不带**前缀（`trigger_localization\country_triggers.txt:676-680`），但 `change_subject_type = subject_type:<键>` **带**（`_hardcoded.txt:5642`）。相关：`is_subject`（`country_triggers.txt:566`）、`is_subject_of`（`:830`）、`is_subject_or_below_of`（`:837`）、`overlord = <国>`（`casus_belli\disloyal_subject.txt:10`）、`subject_loyalty`（`:16`）、`make_subject_of`（`country_interactions\samanta_upgrades.txt:51`）。

### 解约动作带来的 loc 义务

引擎按类型生成 `CANCEL_<键>` / `BREAK_<键>` 两个交互，读标准 **17 键**族（CANCEL 9 + BREAK 8：`TOOLTIP_HEADER` / `_TOOLTIP_HEADER_NO_TARGET` / `TITLE` / `FLAVOR` / `CATEGORY` / `NEWDESC` / `DESC` / `REQDESC` / `NOT_IN_TRUCE`；BREAK 无 `FLAVOR`）。模板见 `diplomacy_l_english.yml:1603-1646`（`vassal` 一整块）。

- 本体 20 个类型里 **16 个有**；没有的 4 个（`conquistador` / `colonial_nation` / `trade_company` / `secessionists`）正是**没有解约动作**的类型。
- ⚠️ **键是按"有没有这个动作"写的，不按 flag**：`BREAK_uc_bey_*`（`:3144-3145`）存在，但 `uc_bey.txt` 并没写 `subject_can_cancel`。
- ⚠️ **是否必需仍未证**：`diplomacy_l_english.yml:1697-1698` 有通用键 `CANCEL_SUBJECT_STATUS` / `BREAK_SUBJECT_STATUS`（用 `[SUBJECT_TYPE.GetNameWithNoTooltip]`），**看起来**正是给新类型准备的兜底，但没找到引用它的 GUI/交互文件，无法定论。**判定法：一个键都不写，进游戏点按钮——缺键会原样显示 `BREAK_<键>_NEWDESC`，一眼可见。**
- 附庸类类型**另有** `OFFER_<键>` / `REQUEST_<键>` 外交行动族（与上面的 CANCEL/BREAK 族是两套）。

---

## 忠诚：`subject_modifier` 里的 `loyalty_to_overlord`

**想给一个主体类型"自带忠诚"，字段就是 `loyalty_to_overlord`，写在 `subject_modifier = { }` 里。** 本体 4 例，全在这个位置：`fiefdom.txt:58 = 10`、`colonial_nation.txt:98 = 20`、`hanseatic_member.txt:48 = 30`、`secessionists.txt:42 = 50`。⇒ 阶梯 **10 / 20 / 30 / 50** 全部有本体先例（注意 `vassal` / `march` / `tributary` **都没写**）。

### ⚠️ 别和 `subject_loyalty` 搞混（两个键都是 `category=country`，写错不报错）

| 键 | 谁的修正 | 用在哪 | 本体用法 |
|---|---|---|---|
| `subject_loyalty` | **宗主**——抬高它**所有**属国的忠诚 | `overlord_modifier`、法律、进阶、政体改革、特质 | `government_reforms\common.txt:276 = 15`、`advances\0_age_of_absolutism.txt:280 = 5`、`traits\00_ruler.txt:267 = 5` |
| **`loyalty_to_overlord`** | **附庸**——只对它自己的宗主 | **`subject_modifier`** | 上表 4 例 |

两个键的注册：`main_menu\common\modifier_type_definitions\00_modifier_types.txt:6129` / `:6136`（都 `decimals=2`、`category=country`）。

### 忠诚是**算出来的**，没有"初始值"这个库存

`loading_screen\common\defines\00_defines.txt`：`LOYALTY_FROM_OPINION = 0.15`（`:2223`）、`LOYALTY_FROM_TRUST = 0.1`（`:2224`）、`LOYALTY_FROM_INDEPENDENCE_MOVEMENT_SCALE = 10` / `_MAX = 20`（`:2022-2023`）、`SUBJECT_LOYALTY_ANNEXATION_SPEED_FACTOR = 0.2`（`:2018`）。

- ⇒ **想要"初始忠诚偏高"只能靠常驻修正**（`subject_modifier`），一次性加成无处可加。
- 想一次性加，能加的是 `liberty_desire`（`default_values.txt:529-538` 有对称的 `liberty_desire_ultimate_minus = -100` … `_ultimate_plus = 100`，步长 −100/−50/−20/−10/−5/+5/+10/+20/+50/+100），**但它有 `LIBERTY_DESIRE_MONTHLY_DECAY = -0.5`**（`:2290`）⇒ 一次 −10 约 **20 个月就蒸发干净**，等于白给。

### 集权／分权轴：一个 `subject_loyalty` 的 ±大坑

`societal_values\00_default.txt:1-23` —— 本体第一个轴 `centralization_vs_decentralization` 的两端**各自捆了三个**修正：

| | `left_modifier`（**集权端**） | `right_modifier`（分权端） |
|---|---|---|
| **`subject_loyalty`** | **−20** | **+30** |
| `annexation_speed_modifier` | **+0.33** | −0.33 |
| `control_importance_modifier` | 0.2 | −0.1 |
| `global_crown_estate_power` | 0.5 | — |
| 其他 | `global_distance_from_capital_speed_propagation = 0.2` | `global_estate_target_satisfaction`、`global_estate_satisfaction_recovery = 0.002` |

**跨度 50 点忠诚**，而且与吞并速度**反号**：集权 = 属国更不忠、但吞并快 33%。任何"把宗主往某一端推"的主体类型都必须连算这三个后果。

### 本体镜像范例：`fiefdom` 就是"漂移 + 补忠诚"的完整写法

```
# subject_types\fiefdom.txt:52-58
overlord_modifier = {
	monthly_towards_decentralization = societal_value_tiny_monthly_move
}
subject_modifier = {
	country_cabinet_efficiency = -0.50
	loyalty_to_overlord = 10
}
```
⇒ 采邑把宗主推向**分权**（轴上是 +30 忠诚）**并**补 10 点忠诚。想做反向（推向集权、−20 忠诚）的类型，就照这个模子补 `loyalty_to_overlord`。**顺带：这也给 `monthly_towards_*` 的取值提供了本体锚点 = `societal_value_tiny_monthly_move`。**

---

## 74 字段全景：别只写 readme 上那十几个

**做法**（可复现）：对 `in_game\common\subject_types\*.txt`（排除 `readme.txt`）逐文件取**顶层字段**——正则 `^\t([a-z_0-9]+)\s*=`（**恰好一个 Tab**；两个 Tab 是嵌套，不会误捕）——再计数。本体 20 个类型合计 **74 个顶层字段**。

### ⚠️ 事实上的必填：20/20 全用的 15 个

`can_change_rank`、**`has_overlords_ruler`**、`institution_spread_to_overlord`、`level`、`subject_pays`、`subject_modifier`、`overlord_can_cancel`、**`great_power_score_transfer`**、`institution_spread_to_subject`、`overlord_modifier`、`has_limited_diplomacy`、`join_defensive_wars_always`、`diplomatic_capacity_cost_scale`、`strength_vs_overlord`、`can_change_heir_selection`

⇒ **20 个类型无一例外都写了这 15 个**。自建类型漏掉其中任何一个，都是在裸奔吃默认值——而 readme 大多没写默认。这一条比 readme 重要：**readme 是"有哪些字段"，频次表是"哪些字段不写会出事"。**

### 紧↔松轴上的字段实测（`vassal` 紧极 vs `tributary` 松极）

| 字段 | `vassal` | `tributary` | 是否在轴上 |
|---|---|---|---|
| `annullment_favours_required` | 20 | 5 | ✅ | 
| `institution_spread_to_overlord` / `_to_subject` | `..._mild` | `..._weak` | ✅（`severe` 是最紧档，10/20 在用） |
| `food_access` | yes | **不写** | ✅ |
| `fleet_basing_rights` | yes | **不写** | ✅ |
| `maritime_path_tolerance` | −0.25 | **+0.25** | ✅ 两极现成 |
| `overlord_protects_external` | 默认 yes | no | ✅ |
| `has_limited_diplomacy` | yes | no | ✅ |
| `can_change_rank` | no | yes | ✅ |
| `strength_vs_overlord` | −0.5 | −0.25 | ✅ |
| `diplomatic_capacity_cost_scale` | 1.0 | 0.2 | ✅（本体档位 `0/0.05/0.1/0.2/0.25/0.5/0.75/1.0/1.25` 全有） |
| ⚠️ `can_change_heir_selection` | **yes** | **yes** | ❌ **不在轴上**（13/20 yes、7/20 no） |
| ⚠️ `has_overlords_ruler` | **no** | **no** | ❌ 不在轴上（17/20 no；写 yes 的只有 `dominion`/`fiefdom`/`state_bank` 三个最一体化的） |
| `great_power_score_transfer` | 0.25 | 0.25 | ❌ 两极同值（众数 0.25，域 `0.1/0.25/0.5/0.75`；**0.1 = 松端值**，`hre`/`samanta`/`tusi`） |
| `minimum_opinion_for_offer` | 150 | 150 | ❌ 同值（众数 150，另有 100/175/200） |

**⇒ 教训：不要凭"名字听起来像紧松"就把它排进阶梯。** 上表有 4 个字段经实测**不在轴上**，其中 2 个（`can_change_heir_selection` 两级都 yes、`has_overlords_ruler` 两级都 no）与直觉相反。

### ⚠️ "只写过 `yes`"的字段：省略 = 否

这几个字段本体**只有 `yes`、零个 `no`**：`can_overlord_build_rgos`(8)、`can_overlord_build_buildings`(7)、`can_overlord_build_roads`(7)、`can_overlord_recruit_regiments`(6)、`can_overlord_build_ships`(5)、`food_access`(15)、`fleet_basing_rights`(14)、`overlord_protects_other_subjects`(2)、`overlord_can_enforce_peace_on_subject`(2)、`has_overlords_religion`(2)、`overlord_inherit_if_no_heir`(2)。

⇒ 这类字段的正确用法是**写 = 授予、不写 = 拒绝**，而不是写 `= no`。反向的也有：`can_be_force_broken_in_peace_treaty`(2) 与 `allow_subjects`(2) **只写过 `no`**。

### `color`：类型自己的字段，指向命名色

`subject_types` 里 **18/20 写 `color = subject_<类型键>`** ⇒ 颜色要**两处**落地：先在 `main_menu\common\named_colors\` 里定义 `subject_<键>`，再在类型块里写 `color = subject_<键>`。**不是只做一处。**

### 其他易漏项

- ⚠️ **本体拼写是双 l 的 `annullment_favours_required`**（17/20 在用）。
- `join_offensive_wars_can_call`（6/20，触发块）常被漏——很多人只写 `join_*_always`。**`join_defensive_wars_can_call` 本体 0 用**（readme 有、20 个类型无一写）。
- `level` 本体取值 `0/1/2/3`（各 1/8/6/5 次）。
- `type` 本体只写过 `location`(6) 与 `building`(2)，**12 个类型不写**。
- `government`（3/20）是**创建门**的一种（`uc_bey.txt:24 = monarchy`），与 `government_type`（`tributary.txt:52-58` 用）并列——想做结构性创建门，这两个都能用。

### 可复现脚本（pwsh，非递归，单目录）

```powershell
$d='<game>\in_game\common\subject_types'
$f=@{}
Get-ChildItem $d -File -Filter *.txt | Where-Object { $_.Name -ne 'readme.txt' } | ForEach-Object {
  Get-Content $_.FullName | ForEach-Object {
    if($_ -match "^\t([a-z_0-9]+)\s*="){ $n=$Matches[1]; if($f.ContainsKey($n)){$f[$n]++}else{$f[$n]=1} }
  }
}
$f.GetEnumerator() | Sort-Object Value -Descending | ForEach-Object { "{0,3}  {1}" -f $_.Value, $_.Key }
```
