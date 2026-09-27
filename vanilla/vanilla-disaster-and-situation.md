# 原版解析：灾难 · 局势（vanilla disaster & situation）

版本基准：EU5 1.3.x。**本篇为什么只有两件套**：灾难、局势、国际组织原本写在同一章（"三件套"），但 **IO 的机制体量（36 个 IO / 282 KB + 决议 130 KB + 三个子类目 65 KB）远超灾难与局势之和**，且 IO 的议会决议、特殊地位、付款、土地规则都是**独立完整的小系统**，与灾难/局势并无机制血缘——只是"被同一个容器装着"。因此 **IO 已独立成篇：`vanilla\vanilla-international-organizations.md`**；本篇保留三者的**实际耦合关系**（灾难↔局势、灾难/局势↔IO）与灾难、局势各自的完整机制。

| 系统 | 文件 | 中文 | 规模 |
|---|---|---|---|
| 灾难 | `common\disasters\` | **灾难** | 36 个 + readme |
| 局势 | `common\situations\` | **局势** | 22 个 + readme |
| ~~国际组织~~ | 见 `vanilla\vanilla-international-organizations.md` | **国际组织** | 36 个 IO + 3 子类目 + 21 个决议 |

---

## 一、三者耦合关系（灾难 ↔ 局势 ↔ IO，实测）

### 灾难 ↔ 局势：互斥与依赖

| 实例 | 代码 |
|---|---|
| 中华王朝危机 被 红巾军起义**阻止** | `NOT = { is_situation_active = situation:red_turban_rebellions }` |
| 凤凰之命 被 奥斯曼崛起 阻止 + 读局势变量 | `NOT = { is_situation_active = situation:rise_of_the_ottomans }`；`situation:rise_of_the_ottomans.var:strongest_beylik_variable.ruler_or_regent` |
| 德里解体 依赖 德里陷落 局势状态 | `situation:fall_of_delhi ?= { ... }`（多处安全引用） |
| 宗教动荡 依赖 特伦托会议 局势 | `situation:council_of_trent = { ... }` |

### 灾难 ↔ 国际组织

| 实例 | 代码 |
|---|---|
| 中华王朝危机 要求是天朝领袖并读 IO 变量 | `is_leader_of_international_organization = international_organization:middle_kingdom` + `international_organization:middle_kingdom = { var:celestial_authority <= 20 }` |
| 勃兰登堡动荡 要求是 HRE 成员 | `is_member_of_international_organization = international_organization:hre` |

### 局势 ↔ 国际组织（含**同名配对**）

局势侧字段直接绑定 IO：`international_organization_type = <IO type>`、`resolution = <决议>`、`voters = <全球名单>`。

| 局势 | 关联 IO |
|---|---|
| 宗教战争 `war_of_religions` | `catholic_league` / `protestant_union` |
| 圭尔夫与吉伯林 `guelphs_and_ghibellines` | `guelphs_io` / `ghibellines_io`（+ `voters = guelphs_and_ghibellines_voters`） |
| 红巾军起义 `red_turban_rebellions` | **`red_turban_rebels`（同名 IO）** |
| 意大利战争 `italian_wars` | `italian_league_1/2/3` |
| 南北朝 `nanbokuchou`、战国 `sengoku` | `japanese_shogunate` |
| 胡斯战争 `hussite_wars`、百年战争、托尔德西里亚斯条约 | 各自相关 IO / 决议 |

---

## 二、灾难（Disasters）——单国范围的「状态容器」

### 核心认知：灾难 ≠ 惩罚机制（它是一种"个人局势"）

⚠️ **不要把灾难理解成"国家衰退的结算"**——那只是它最常见的用法。灾难与局势在系统层面**同构**：

| 维度 | 灾难 | 局势 |
|---|---|---|
| 结构 | 持续状态 + 修正 + 月钩子 + 事件链 + 结束判定 | **完全一样** |
| 作用范围 | **单个国家**（`root = country`） | 跨国家/跨地区（`root = situation`） |
| 触发源 | 该国自身状态（`can_start` 判国家指标） | 世界大势（`can_start` 判全局） |
| 槽位 | `has_any_active_disaster = no` —— **每国仅 1 个，互斥** | 可多个并存 |
| 呈现 | 国家面板 | 地图着色 + 图例 + 参与国列表 |

**结论**：灾难 = **作用域收缩到单国、槽位唯一的局势**。它承载什么（惩罚 / 改革菜单 / 内容包）完全由脚本决定。

### 字段（权威：`disasters\readme.txt` + 实查补充）

```
custom_description = <customizable_localization 键>
monthly_spawn_chance = <script value>     # 每月爆发概率 0..1（root = country, scope:disaster）
modifier = { ... }                        # 灾难进行中施加于国家的修正
can_start = { <triggers> }                # root = country, scope:disaster
can_end   = { <triggers> }
on_start / on_monthly / on_end = { <effects> }   # root = country, scope:disaster
map_mode = <map mode tag>                 # 可选：查看该灾难时切换的地图模式
fire_only_once = yes/no                   # 同一国家是否会重复发生
content_priority = <int>                  # 实查补充（readme 未列）：DLC 内容调度优先级，如凤凰之命 = 700
```

### 36 个灾难（按主题分组）

| 类别 | 文件 |
|---|---|
| **王朝/帝国危机** | `crisis_of_the_chinese_dynasty`、`crisis_of_the_sayfawa_dynasty`、`decline_of_empire`、`decline_of_majapahit`、`decline_of_mali`、`dissolution_of_delhi`、`twilight_of_the_tsardom`、`time_of_troubles`、`death_of_hayan_wuruk` |
| **内战/叛乱** | `castilian_civil_war`、`english_civil_war`、`horde_civil_war`、`muscovite_succession_war`、`war_of_the_roses`、`war_of_the_aragonese_union`、`aspiration_for_liberty`、`peasants_war`、`ciompi_revolt`、`hook_and_cod_wars` |
| **继承/权力斗争** | `byzantine_succession_crisis`、`succession_crisis`、`struggle_for_royal_power`、`coup_attempt`、`rise_of_the_szlachta`、`court_and_country`、`reform_society` |
| **宗教动荡** | `religious_turmoil`、`french_wars_religion`、`savonarola`、`sinicization_disaster` |
| **革命** | `revolution_disaster`、`revolutionary_chaos` |
| **地区专属 / 内容包** | `ambrosian_republic`、`turmoil_in_brandenburg`、`curse_of_stefan_uros_iii`、**`D008_fate_of_the_phoenix`（DLC；内容包型灾难的范例，见下方 ②）** |

### 三种实际用法（性质是脚本选择，不是机制属性）

#### ① 惩罚期（最常见）
纯负面修正 + 结束时结算。**抽样验证 6 个灾难的 `modifier`**：

| 灾难 | modifier 内容 | 性质 |
|---|---|---|
| 汉化灾难 `sinicization_disaster` | 部落凝聚力 / 正统性 / 共和传统 / 虔诚 各 −0.25 + 叛乱增长 0.005 + 阶层满意度重罚 | 全负 |
| 宫廷与国家 `court_and_country` | 阶层满意度重罚 + 叛乱阈值 +0.1 | 全负 |
| 王权斗争 `struggle_for_royal_power` | 贵族满意度巨罚 + 贵族满意度衰减 0.025 | 全负 |
| 自由渴望 `aspiration_for_liberty` | 阶层满意度惩罚 + 稳定投资惩罚 | 全负 |
| 帝国衰落 `decline_of_empire` | 阶层惩罚 + 叛乱 0.003 + 银行利息 0.05 + 自满机制 | 全负 |
| **改革社会** `reform_society` | 向中央集权显著漂移 + 叛乱 0.005 + **吞并速度 +50%** | **含正面键** |

#### ② 内容包 / 改革菜单 ⭐（`D008_fate_of_the_phoenix` 凤凰之命 = 最佳范例）

**这是一个"灾难形态的改革 DLC"**：

- **modifier 三键两正一负**（`D008_fate_of_the_phoenix.txt:18-22`）：

| 修正 | 值 | 性质 |
|---|---|---|
| `hire_mercenary_premium_cost_modifier` | +0.5 | 负面（雇佣兵贵 50%） |
| `global_bureaucracy_maintenance_efficiency` | **+0.2** | **正面** |
| `casus_belli_creation_speed_modifier` | **+0.2** | **正面** |

- **自带 12 个专属行动**（`generic_actions\D008_fate_of_the_phoenix_actions.txt`），全部以 `disaster_type = disaster_type:fate_of_the_phoenix` + `disaster_is_active = yes` 为前置——**只有这场灾难进行中才能使用**：

  `request_papal_donation` 向教皇募捐 · `grant_latin_merchants_privileges` 授予拉丁商人特权 · `accept_catholic_delegation` 接受天主教使团 · `attract_italian_engineer` 招揽意大利工程师 · `demand_beylik_tribute` 向贝伊索贡 · `summon_patriarchate_member` 召集牧首区成员 · `reform_imperial_armies` 改革帝国军队 · `patronize_orthodox_monastery` 赞助修道院 · `sponsor_troop_feast` 军宴 · `grant_a_triumph` 凯旋式 · `roman_festivals` 罗马节庆 · `greek_festivals` 希腊节庆

  统一价格 `price:fate_of_phoenix_actions_price`，各自带冷却（如向教皇募捐 5 年一次）。

- **开局强制触发**：`tag = BYZ` + `current_age = age_1_traditions` + `has_dlc` + `current_year < 1338`
- **`on_end` 按结果分支**：若奥斯曼崛起局势**未激活**且 `num_locations_owned_or_owned_by_subjects_or_below > 145` → 事件 `.14`（复兴成功），否则 `.15`（失败）
- `content_priority = 700` —— DLC 内容的调度优先级（readme 未列字段）

**设计要点**：引擎提供的"单国持续状态 + 专属行动解锁 + 月事件池"这套骨架，被直接当成**改革菜单的容器**使用。mod 作者完全可以照此把灾难写成"国家改造计划"。

#### ③ 槽位护盾（把互斥约束当资源）

`has_any_active_disaster = no` 是所有灾难互斥的**公共锁**——**占住槽位就等于关闭其他灾难的触发通道**。凤凰之命挂满拜占庭整个传统时代，客观上让拜占庭对其他灾难免疫，同时把槽位从"惩罚"改造成"改革窗口"。

> 机制含义：灾难槽是**每国一份的稀缺资源**。设计时若想保护某国度过关键期，可以用一个"温和灾难"占位（这正是凤凰之命的结构性效果）。

### 实查结构（`crisis_of_the_chinese_dynasty.txt` 完整样例）

```
can_start = {
    current_age_or_later = { age = age_3_discovery }        # 时代门槛
    is_leader_of_international_organization = international_organization:middle_kingdom
    has_any_active_disaster = no                            # 同时只能有一个灾难
    NOT = { is_situation_active = situation:red_turban_rebellions }   # 与局势互斥
    international_organization:middle_kingdom = { var:celestial_authority <= 20 }
    OR = { stability < 0  government_power < 50 }
}
modifier = { monthly_celestial_authority = -0.05  monthly_war_exhaustion = 0.1
             monthly_rebel_growth = 0.01  monthly_inflation = 0.001 }
on_start = { trigger_event_non_silently = crisis_of_the_chinese_dynasty.1 }
on_end = { 清理 4 个变量; 若已非天朝领袖 → 事件 .2（天命丢失）否则 → 事件 .3 }
on_monthly = { random_list { 15 = {...} 85 = {...} } + hidden_effect 里的随机事件 }
```

**关键模式**：① `has_any_active_disaster = no` 保证互斥（反过来说就是**槽位护盾**，见上）；② 灾难**读 IO 变量**做阈值判定；③ `on_end` 用变量清理 + 分支事件；④ `monthly_spawn_chance` 用命名 script value（如 `monthly_spawn_chance_very_high`）。

> 灾难防重复的实测坑见 `pitfalls.md` / `cases\laws-events-and-estates-2026-09.md`：`can_start` 里的 `NOT{xxx_resolved=yes}` 标记**绝不能在 on_end remove**。

## 三、局势（Situations）

### 字段（权威：`situations\readme.txt` + 实查补充）

```
custom_description = <键>
monthly_spawn_chance = <script value>            # scope:situation
international_organization_type = <IO type tag>  # 关联的 IO 类型
resolution = <resolution tag>                    # 引用的具体决议
voters = <global list tag>                       # 有投票权者名单
can_start / can_end = { <triggers> }             # root = situation（⚠️ 不是 country！）
visible = { <triggers> }                         # root = country, scope:target = situation
on_start / on_monthly / on_ending / on_ended = { <effects> }   # root = situation
tooltip = { <effects> }                          # 只生成地图提示，不执行（root = location）
map_color / secondary_map_color = { <script color> }           # 地图着色（root = location）
```

**实查额外字段**（readme 未列但 22 个局势普遍使用）：**`hint_tag`**（提示标签）、**`legend_key`**（图例键）。

⚠️ **作用域差异是最大的坑**：局势的 `can_start` / `on_*` 根作用域是 **situation 本身**（不是 country），而 `visible` / `tooltip` / `map_color` 才用 country 或 location。

### 22 个局势

| 类别 | 局势 |
|---|---|
| **瘟疫/气候** | `black_death`（8.8KB）、`great_pestilence`、`little_ice_age` |
| **宗教** | `reformation`（13KB）、`war_of_religions`（**22.9KB 最大之一**）、`council_of_trent`、`hussite_wars`、`western_schism` |
| **战争/冲突** | `hundred_years_war`、`italian_wars`（**30.2KB 最大**）、`nanbokuchou`、`sengoku`、`guelphs_and_ghibellines`（13.7KB） |
| **王朝/国家兴衰** | `rise_of_the_ottomans`（15.1KB）、`rise_of_timur`、`fall_of_delhi`、`red_turban_rebellions` |
| **革命/殖民** | `the_revolution`、`colonial_revolution` |
| **贸易/交流** | `columbian_exchange`、`treaty_of_tordesillas`、`golden_age_of_piracy` |

## 四、国际组织（IO）—— 已独立成篇

IO 的完整机制见 **`vanilla\vanilla-international-organizations.md`**（36 个 IO 字段对照表、领袖的三种触发 × 五种方法、议会与决议引擎、三个子类目 special statuses / payments / land ownership rules、以及**天朝 IO 作为案例深挖**）。

**本篇只保留它与灾难/局势的实际耦合**（这是原本"三件套合一"的真正价值）：

| 耦合方向 | 机制 | 实例 |
|---|---|---|
| IO → 灾难 | 灾难 `can_start` 读 IO 的 `variables`（帝国健康度做阈值） | 中华王朝危机要求 `is_leader_of_international_organization = international_organization:middle_kingdom` **且** `international_organization:middle_kingdom = { var:celestial_authority <= 20 }` |
| IO → 灾难 | 灾难 `can_start` 判定成员身份 | 勃兰登堡动荡要求 `is_member_of_international_organization = international_organization:hre` |
| 局势 → IO | 局势字段直接绑定 IO：`international_organization_type`、`resolution`、`voters` | 宗教战争 ↔ `catholic_league` / `protestant_union`；圭尔夫与吉伯林 ↔ `guelphs_io` / `ghibellines_io` |
| **同名配对** | 局势与其 IO 同名 | `red_turban_rebellions` 局势 ↔ **`red_turban_rebels` IO**；`italian_wars` ↔ `italian_league_1/2/3`；`nanbokuchou`/`sengoku` ↔ `japanese_shogunate` |
| IO → 局势 | IO 的 `create_visible_trigger` 反过来要求局势激活 | `colonial_federation` 要求 `is_situation_active = situation:colonial_revolution` |

> **写 mod 的实用结论**：灾难/局势脚本里访问 IO 一律用 `?=` 安全引用（组织可能不存在或已解散）；IO 的 `variables`（`var:celestial_authority`、`var:imperial_authority`）是"帝国健康度"的天然载体，灾难拿它当开关阈值最省事。

## 五、三者的设计范式（写 mod 时可复用的模式；标 ★ 的两行详见 IO 篇）

| 范式 | 说明 | 实例 |
|---|---|---|
| **互斥闸门 / 槽位护盾** | 灾难之间用 `has_any_active_disaster = no`（**每国一槽 = 稀缺资源，可用温和灾难占位保护关键期**）；灾难与局势用 `NOT = { is_situation_active = ... }` | 中华王朝危机；凤凰之命（结构上为拜占庭挡灾） |
| **灾难即内容容器** | 灾难可承载专属行动（`disaster_type` + `disaster_is_active` 作前置）、月事件池、结果分支事件——性质由脚本定，可中性偏正 | 凤凰之命（12 专属行动 + modifier 两正一负） |
| **大势绑定 IO 变量** | 灾难/局势读 IO 的 `variables`（`var:xxx`）做阈值判定，形成"帝国健康度"联动 | `celestial_authority <= 20` |
| **局势即 IO 的舞台** | 局势用 `international_organization_type` + `resolution` + `voters` 绑定 IO，把 IO 的投票系统变成事件舞台 | 宗教战争、圭尔夫与吉伯林 |
| **同名配对** | 局势与其 IO 同名（`red_turban_rebellions` 局势 ↔ `red_turban_rebels` IO） | 红巾军 |
| ★ **IO 变量做进度条** | IO `variables` 的 `monthly_change` 承载长期趋势（天威、帝国权威），法律/政策/事件增减它 | middle_kingdom、hre |
| ★ **法律改写 IO 规则** | IO 的 payments/special_statuses 由 IO 政策增删（`payments_implemented` / `special_statuses_implemented`） | 天朝朝贡、HRE |
| **局势作用域陷阱** | `can_start`/`on_*` 根是 **situation**，`visible`/`tooltip`/`map_color` 才是 country/location | 全部 22 个局势 |

## 六、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 新灾难 | `common\disasters\<名>.txt`（字段见上） | `can_start` 的防重复标记勿在 on_end 清除；`scope:disaster` 可用 |
| 新局势 | `common\situations\<名>.txt` | **作用域是 situation**；记得补 `hint_tag` / `legend_key`（不然地图图例缺失） |
| 新 IO / IO 土地·支付·地位 / IO 投票玩法 | 见 `vanilla\vanilla-international-organizations.md` §九 | IO 相关字段与三个子类目都在该篇 |
| 灾难/局势 ↔ IO 联动 | 在灾难/局势脚本里读 IO 变量与成员状态 | 用 `?=` 安全引用避免组织不存在时报错；读变量走 `international_organization:<tag> = { var:… }` |

**硬编码**：`monthly_spawn_chance` 的掷骰时机、灾难/局势的全局调度与互斥判定、IO 侧（议会/决议结算、领袖变更、付款转账、地图着色）见 IO 篇。

## 七、中文检索键

概念：`game_concept_disaster`（灾难）、`game_concept_situation`（局势）；**IO 相关概念（国际组织 / 决议 / 特殊地位）见 `vanilla\vanilla-international-organizations.md`**。
本地化前缀：灾难名直接是块名（如 `war_of_the_roses: "玫瑰战争"`、`time_of_troubles: "大动乱时代"`）；局势的 `hint_tag` / `legend_key` 需对应 loc 键。
界面：`disaster_view.gui`、`situation_view.gui`、`rebels_details.gui`；**IO 界面见 IO 篇**。
