# 原版解析：法律 · 阶层 · 议会（vanilla law, estate & parliament）

版本基准：EU5 1.3.x。核心文件：`common\estates\00_default.txt`（1275 行，8 个阶层）、`common\estate_privileges\`（8 文件，`nobles_estate.txt` 40KB 最大）、`common\laws\`（33 文件 + `readme.txt`）、`common\parliament_types|parliament_issues|parliament_agendas\`（各带 readme）、`common\rebel_demands\`（**13 条诉求**，含 12 条按叛乱类别的默认诉求）、`common\generic_actions\estates.txt` / `estate_emergency_actions.txt` / `parliament.txt` / `io_parliament.txt`、defines **`NEstate`（1639–1687）**。机制描述引自游戏内百科词条。

> **三篇的分工**：本篇是**机制层**（系统怎么运转、数值在哪、什么被硬编码）；`guides\law-design.md` 是**制作层**（怎么写一条法律：字段用法、解锁链三处同步、政策配方、池子大小、命名三禁）；**政体与改革、官僚部门、社会价值观、国家等级**另见 `vanilla\vanilla-government-and-reform.md`（阶层的"上游"在那里）。三篇配合读。

## 术语对照

| 内部名 | 中文 | 说明 |
|---|---|---|
| `estate` | **阶层** | 国家内部利益集团（8 个） |
| `estate_power` | 阶层力量 | 政府内政治力量 |
| `estate_satisfaction` | 阶层满意度 | 需求满足程度 |
| `estate_opinions` | 阶层观感 | 阶层**对他国**的观感 |
| `estate_privilege` | 阶层特权 | 提高阶层力量的特殊权利 |
| `crown_power` | 王室力量 | 被阶层力量总和削弱 |
| `law` | **法律** | 容器，含多个政策 |
| `policy` | **政策** | 法律内的具体选择（同时只能选一个） |
| `parliament` | 议会 | 阶层表达诉求的场所 |
| `parliament_agenda` | 议会议程 | 阶层提要求，解决它换支持 |
| `parliament_issue` | 议会诉求 | 需表决的事项，未通过有损失 |
| `parliament_support` | 议会支持 | 通过诉求的意愿值 |

## 一、三环总览

```
阶层（estates）—— 8 个利益集团；提供税收 / 陆军征召 / 海军征召；握有力量与满意度
   │
   ├─【短周期·可反复】阶层互动（Estate Interactions）—— 直接作用于阶层本身
   │     贿赂阶层（金币→满意度）· 削减阶层力量 · 8 个紧急行动（5 年冷却）
   │     压制各阶层（court_and_country）· 灾难/局势专属索取
   │
   └─【长周期·制度化】议会（Parliament）
         议程（agenda）→ 解决换「议会支持度」→ 用支持度通过诉求（issue）
              ↓ ask_for_law_changes（无损换政策 —— 另有 7 条途径，见第六节）
         法律（law，容器）→ 政策（policy，具体选择）→ 影响社会价值轴
```

**两条通道的分工**：阶层互动是**高频短周期**操作（有冷却但可反复，直接改满意度/力量）；议会是**长周期制度化**通道（攒议程 → 换支持度 → 换政策/税收/征召/CB）。两者叠加才是完整的阶层玩法。

## 二、阶层（`common\estates\00_default.txt`）

### 8 个阶层与基础数值

| 阶层 | `power_per_pop` | `tax_per_pop` | 特性 |
|---|---|---|---|
| `crown_estate` 王室 | 0 | 0 | `ruler = yes`；满意度恒满、UI 不显示 |
| `nobles_estate` 贵族 | **25** | **100** | `characters_have_dynasty = always`、`bank = yes`、可产生雇佣兵领袖 |
| `clergy_estate` 教士 | 10 | 25 | dynasty = sometimes、bank yes |
| `burghers_estate` 市民 | 4 | 40 | dynasty = sometimes、bank yes |
| `peasants_estate` 农民 | —（未设，引擎默认） | 1 | dynasty = **never**、bank yes |
| `dhimmi_estate` 齐米 | — | 1 | — |
| `tribes_estate` 部落 | — | 0.01 | — |
| `cossacks_estate` 哥萨克 | — | 0.02 | — |

各阶层通用字段：`color`、`rival`（默认 −0.01）/ `alliance`（默认 0.01）（阶层间竞争/结盟倾向）、`characters_have_dynasty`、`bank`（可放贷）、`satisfaction` / `high_power` / `low_power` / `opinion` 四个子块。

### 三个核心数值（官方词条）

| 数值 | 机制 |
|---|---|
| **阶层力量** | 很大程度取决于该阶层**下辖 POP 类型的人口规模** + 享有的**特权**；不同 POP 类型权重差异巨大（贵族远高于奴隶）。**所有阶层力量之和削弱王室力量**；地点的**人口规模 + 阶层力量 + POP 类型**决定该地**税基如何分配** |
| **阶层满意度** | 高 → 国家获奖励修正；低 → 惩罚修正；**过低 → 无法从该阶层招募陆军/海军征召**；**并直接影响该阶层下辖 POP 的满意度**（→ 传导到叛乱） |
| **阶层观感** | `opinion = { }` 块按来源逐项列：基础 opinion、威望差、权力投射差、社会价值轴偏差（`aristocracy_vs_plutocracy`、`serfdom_vs_free_subjects` 等） |

### 三套修正块（线性差值模型）

`NEstate`：`LOW_SATISFACTION_THRESHOLD = 0.5`、`LOW_POWER_THRESHOLD = 0.25`

| 块 | 倍率（注释原文） | 实例（教士 / 市民） |
|---|---|---|
| `satisfaction` | ×(满意度 − 0.5) | 教士：研究速度 +0.5、外交声誉 +2；市民：商人力量 +0.5、生产效率 |
| `high_power` | ×(相对力量 − 0.25)，仅当 > 阈值 | 教士：`clergy_estate_max_tax = -0.5`、改宗速度 **+2.0**、稳定度成本效率 +1、朝灵性主义漂移、`court_language_is_liturgical_language_importance_modifier = 12`；市民：`burghers_estate_max_tax = -0.5`、**商人容量 +100%**、海军维护效率 +0.5、宫廷语言=市场语言重要性 +12 |
| `low_power` | ×−(相对力量 − 0.25)，仅当 < 阈值 | 教士：`clergy_estate_max_tax = +0.25`；市民：+0.5、语言重要性 −6 |

⚠️ **`high_power` 的代价设计**：阶层强大给机制性加成，但**同步扩大其免税额度**（贵族 `high_power` 时 `nobles_estate_max_tax = -1.0` 即完全免税）。"强阶层 = 收不到它的税，换机制加成"是原版的平衡轴。

**隐性联动**：宫廷语言的重要性受阶层力量影响——教士强 → 礼仪语言重要（+12），市民强 → 市场语言重要（+12），弱则各 −6。

### 经济角色

- 阶层从**地点**取税基 + 一部分**贸易利润**，用财富**建造建筑**、满足 POP 需求（`NEstate`：`POP_DEMAND_DEFICIT_SCALE = 0.5`，收入不足时 POP 需求按此收缩）
- **满意度过低 → 阶层只建造支持自身力量的"阶层建筑"**（不再为国家经济服务）
- `bank = yes` → 阶层可放贷（`NEstate`：`ESTATE_LOAN_SIZE = 0.5`、`MIN_ESTATE_LOAN = 2`、`ESTATE_LOAN_DURATION_IN_MONTHS = 60`、`ESTATE_LOAN_MIN_INTEREST = 0.01`）

### 阶层与角色

- **每个角色都有所属阶层**；获**内阁任命**或**军队/海军指挥权**会**提高其阶层力量**
- **一些继承选择（`heir_selections`）会限制继承人的阶层**（参见 `common\heir_selections\`）
- 角色/王朝/内阁/继承法/摄政的**完整机制**见 `vanilla\vanilla-character-dynasty-cabinet.md`（含 147 个特质按 **ruler/cabinet/general/…** 分类、`government_size` 决定内阁席位、`CABINET_ACTION_SKILL_MODIFIER` 本体值 0.005）

### 文化与宗教影响（`NEstate`）

| 因素 | 对满意度 | 对力量 |
|---|---|---|
| 文化被接纳 | −0.05 | −0.05 |
| 文化被容忍 | −0.10 | −0.10 |
| 文化被歧视 | −0.20 | −0.20 |
| 宗教容忍度 | ×`ESTATE_RELIGION_TOLERANCE_SCALE = 0.0025` | — |

**阶层主导（dominance）权重**：主文化 **1.20** / 接纳 1.00 / 容忍 0.50 / 歧视 0.25；`ESTATE_DOMINANCE_RELIGION_TOLERANCE_SCALE = 1.00`、`ESTATE_DOMINANCE_RELIGION_MIN_WEIGHT = 0.10`、**`ESTATE_DOMINANCE_OVERRIDE_THRESHOLD = 1.10`**（超阈值则主导阶层易主）。
相关：`ESTATE_CULTURE_RELIGION_CHANGE_COOLDOWN_MONTHS = 60`（防反复横跳）、`ESTATE_DISCRIMINATED_CULTURE_POWER_ALERT_THRESHOLD = 0.10` / `ESTATE_TOLERATED_CULTURE_POWER_ALERT_THRESHOLD = 0.20` / `ESTATE_DISCRIMINATED_CULTURE_POWER_ERROR_THRESHOLD = 0.30`（警告与报错阈值）。

### 其他常量（`NEstate`）

| 常量 | 值 | 含义 |
|---|---|---|
| `PARLIAMENT_MONTHS_NOT_CALLED_THRESHOLD` | **120** | 10 年不召集议会的阈值 |
| `UPKEEP_PER_BUILDING` | −1.0 | — |
| `MIN/MAX_PEASANT_ENFRANCHISMENT` | 0.1 / 1.0 | 农民解放范围 |
| `COMPARATIVE_WEALTH_POWER_FACTOR` / `_CAP` | 0.01 / 1.0 | 相对财富 → 力量 |
| `POWER_TO_SATISFACTION_PENALTY` | −0.5 | 力量转满意度的惩罚 |
| `BUILDING_DESTRUCTION_IMPACT` | −0.20 | 建筑被毁对阶层的影响 |

## 三、阶层特权（`common\estate_privileges\`）

8 个文件按阶层分：`nobles_estate.txt`(40KB) / `burghers_estate.txt`(24KB) / `clergy_estate.txt`(18KB) / `peasants_estate.txt`(16KB) / `tribes_estate.txt` / `dhimmi_estate.txt` / `cossacks_estate.txt` + `readme.txt`。

**字段**（`readme.txt` 权威）：
```
<privilege> = {
    estate = <阶层>
    potential / allow = { <triggers> }        # root = country
    years/months/weeks/days = <int>           # 实施时长，修正按完成度缩放
    on_activate / on_fully_activated / on_deactivate = { <effect> }
    country_modifier / province_modifier / location_modifier = { ... }   # 三类可缩放修正
    can_revoke = { <trigger> }                # 能否撤销
}
```

**代价结构**（官方词条）："授予特权往往需要付出某种代价，通常包括**王室力量的削弱、阶层力量的增强，以及额外的负面效果**"（典型形式：税收减免、经营垄断）。

## 四、阶层互动（Estate Interactions）——「阶层行动」面板

**入口**：`gui\estate_actions_lateralview.gui`；loc 键 `ESTATE_ACTIONS_LATERALVIEW_T = "与[阶级]互动"`、`ESTATE_ACTIONS_MENU_T = "阶层行动"`。这是玩家点开某个阶层后能**反复施力**的面板——EU5 里少数可以对同一对象反复交互的机制。

**机制底座**：Generic Actions 系统（`common\generic_actions\`，100+ 文件；官方首行："create an action entirely in script and place a GUI button for it entirely in script — **no need for coders!**"）

识别特征（决定某行动是否属于阶层互动体系）：

| 字段 | 值 | 含义 |
|---|---|---|
| `player_automated_category` | **`estates`** | 归入"阶层"自动化类别（玩家可开自动化交给 AI 代管） |
| `select_trigger` | `looking_for_a = estate` | **以阶层为选择目标** |
| `cooldown` | `{ type = <tag> years = 5 }` | 冷却 —— 构成"反复交互"的节奏 |
| `type` | 多为 `owncountry` | 行动类型枚举见 `generic_actions\readme.txt:5` |

### 核心两个（`generic_actions\estates.txt`）

| 行动 | 中文 | 机制 |
|---|---|---|
| `bribe_estate` | **贿赂阶层** | 花费 = 该阶层 `estate_tax_base × 12` 的金币 → `add_gold_to_estate` + 满意度温和加成；AI 前置：满意度 < 0.3 且 税基 > 0.01 |
| `perform_reduction` | **削减阶层力量** | 条件：`num_buildings_owned_by_estate > 0` + `power < 0.9` + **`satisfaction > 0.5`**（满意度够高才削得动） |

### 紧急行动 8 个（`estate_emergency_actions.txt`，每个 **5 年冷却**）

| 行动 | 中文 | 索取对象 |
|---|---|---|
| `ask_for_extra_levies` | 要求扩大征召 | 阶层 |
| `extraordinary_taxes` | 征收特别税 | 阶层 |
| `ask_clergy_for_legitimacy` | 请求神职者支持 | 教士 |
| `ask_nobility_for_diplomats` | 请求贵族使节 | 贵族 |
| `ask_burghers_for_loan` | 请求紧急拨款 | 市民 |
| `ask_commoners_for_stability` | 向平民求助 | 平民 |
| `ask_tribes_for_manpower` | 号召部落武装 | 部落 |
| `call_emergency_parliament` | 召开紧急议会 | —（召集议会） |

### 其他来源（横跨大量行动文件，凡带 `estate_type` / 阶层门控的都进这个体系）

- `court_and_country_actions.txt`：`rein_in_nobles_key` / `rein_in_clergy_key` / `rein_in_burghers_key` / `rein_in_peasants_key`（**压制各阶层**）
- `sell_work_of_art_to_estates.txt`（向阶层卖艺术品）、`hire_advisor.txt`（按阶层雇顾问）
- **灾难/局势专属的阶层索取**：`hussite_wars_actions.txt`（向贵族/教士/市民索捐款各一）、`succession_crisis_actions.txt`（`sc_ask_help_from_the_nobility` / `_clergy` / `_burghers`）、`black_death.txt`（18 个 `demand` 行动）、`great_pestilence.txt`（10 个）、`peasants_war_actions.txt`、`rise_of_the_szlachta_actions.txt`、`religious_turmoil_actions.txt`、`red_turban_rebellions.txt`、`fall_of_delhi.txt`、`the_revolution.txt`（提升/贬抑地方贵族）、`catholic.txt`（向教会索税/求援/求枢机）

**设计要点**：这套机制把「阶层关系」从静态数值（满意度/力量）变成**可反复操作的资源**——贿赂买满意度、削减压力量、紧急行动透支阶层换即时收益（5 年冷却）。它是 `player_automated_category = estates` 自动化的对象，也是 mod 里最容易扩展的阶层玩法接口。

## 五、议会（Parliament）

### 机制链条

1. **召集议会**（`call_parliament`）：除首都外，可在有 proximity 且为**城镇或省会**的地点召开，会址获少量奖励；**该地点被占领 → 议会自动失败**
2. **各阶层提出议程（agenda）**："在任何阶层宣布支持之前，我们要首先解决议程；这可能**代价高昂**，且**该阶层提供的支持程度取决于它们的阶层力量**"
3. 解决议程 → 换得**议会支持（parliament_support）**
4. 用支持度通过**议会诉求（issue）**："需长时间争论，交由议会召集各阶层表决；**如果未能通过，国家将因此遭受损失**"

### 原版实测：数量与字段出现率（花括号深度解析）

| 类目 | 规模 | 字段出现率 |
|---|---|---|
| `parliament_issues` 议案 | **160 项 / 9 文件**（按阶层分：`01_country_specific` / `02_crown_estate` / `03_nobles_estate` / `04_clergy_estate` / `05_burghers_estate` / `06_peasants_estate` / `07_expansion` / `10_hre` / `11_union`） | `on_debate_passed` · `on_debate_failed` · `chance` **各 100%**；`allow` 96%；`modifier_when_in_debate` 94%；**`estate` 94%**（阶层绑定是主流写法）；`potential` 9%；`wants_this_parliament_issue_bias` 7%；`type` 6% / `special_status` 6%（IO 版）；`selectable_for` 4% |
| `parliament_agendas` 议程 | **128 项 / 6 文件** | `on_accept` · `potential` · `chance` **各 100%**；**`estate` 97%**；`type` 16%（IO 版）；`importance` 14%；`can_bribe` / `on_bribe` 12%；`ai_will_do` 9%；`allow` 5% |
| `parliament_types` 议会类型 | **14 种 / 2 文件** | `type` · `modifier` **各 100%**；`potential` 79%；`allow` 43%；`locked` 36% |

**读法**：议案几乎总是**挂在某个阶层身上的**（94%），所以"议会"本质是**阶层系统的表决环节**，不是独立系统——这也解释了为什么 `parliament_issues` 的文件是按阶层切的。

### 议程字段（`parliament_agendas\readme.txt`）

`type`（country/IO）、**`estate`（可按阶层多项）**、`special_status`（IO）、`potential` / `allow`、**`on_accept`**（接受效果）、**`on_bribe` / `can_bribe`**（贿赂路径，用于 `bribe_estate`）、`chance`（出现几率）、**`importance`**（"越高对议会诉求的影响越大"，未定义时视为 1）。

### 诉求字段（`parliament_issues\readme.txt`）

`type`、**`estate`**（本体按阶层分文件：`02_crown` / `03_nobles` / `04_clergy` / `05_burghers` / `06_peasants` / `01_country_specific` / `07_expansion` / `10_hre` / `11_union`）、`modifier_when_in_debate`、`allow` / `potential` / `selectable_for`、`chance`、**`on_debate_start` / `on_debate_passed` / `on_debate_failed`**、`wants_this_parliament_issue_bias`。

### 议会行动（`generic_actions\parliament.txt`，10 个）

`call_parliament` 召集 · `request_more_taxes` 加税 · `prepare_for_war` 备战 · `ask_for_larger_levies` 扩征召 · **`ask_for_law_changes` 换政策（无损途径）** · `change_parliament_type` 换议会类型 · `force_parliament_issue` 强推诉求 · `challenge_bureaucratic_entrenchment` 挑战官僚固化 · `declare_holidays_request` 假日请求 · `iw_militarize_country`（意大利战争专属）

### 紧急行动（`generic_actions\estate_emergency_actions.txt`）——按阶层定向索取

`ask_for_extra_levies` 额外征召 · `extraordinary_taxes` 特别税 · `ask_clergy_for_legitimacy` 向教士要正统性 · `ask_nobility_for_diplomats` 向贵族要外交官 · `ask_burghers_for_loan` 向市民要贷款 · `ask_commoners_for_stability` 向平民要稳定度 · `ask_tribes_for_manpower` 向部落要人力 · `call_emergency_parliament` 紧急议会

其他阶层互动：`estates.txt` 的 `bribe_estate`（贿赂）/ `perform_reduction`（削减）、`sell_work_of_art_to_estates.txt`（向阶层卖艺术品）。

### 议会相关修正键

`parliament_base_support`（基础支持）、**`parliament_request_issue_support_needed`**（请求诉求所需支持度）、`parliament_duration_modifier` / `organization_parliament_duration_modifier`、`uses_parliament_for_law_votes`、`has_a_parliamentary_system`、`has_international_parliament`、`has_parliament_seat`、`permanent_parliament_location`、`can_call_rural_parliaments`、`change_parliament_type_cost_modifier` / `change_organization_parliament_type_cost_modifier`、`bribe_voter_for_policy_cost_modifier`、`policy_vote_cost_modifier`。
**各阶层议会参与权**：`crown/nobles/clergy/burghers/peasants/dhimmi_estate_can_participate_in_parliament`、`crown_estate_blocked_from_parliament`。

### 议会类型与 IO 议会

- `parliament_types\`：`type = country/international_organization`、`potential` / `allow` / `locked`（作用域随 type）、`modifier`（国家修正）；`01_international_organization.txt` 是 IO 版
- IO 议会（`io_parliament.txt` / `io_parliament_bribes.txt`）：由拥有 **special_status** 的成员提出自己的议程，完成后用 **special_status_power** 保障支持

## 六、叛乱诉求（`common\rebel_demands\`，13 条）

阶层不满的最终出口：叛乱爆发后叛军提出的"要求"，玩家可以选择**让步**（`concession_effect`）或硬扛（赢了走 `victory_effect`）。

| 项 | 内容 |
|---|---|
| 文件 | `999_default_rebel_demands.txt`（5 835 B）+ `900_country_specific_from_startup_or_events.txt`（301 B） |
| 顶层条目 | **13 条** = **12 条按叛乱类别的默认诉求** + 1 条国别（`the_goals_of_balliol`） |
| 12 条默认 | `default_slave_demand`、`default_nationalist_demand`、`default_religious_demand`、`default_pretender_demand`、**7 条阶层专属**（`default_{nobles|clergy|burghers|peasants|dhimmi|tribes|cossacks}_estate_demand`）、`default_rebel_demand`（兜底） |
| 字段 | `trigger` **100%**（多为 `rebel_category = <类别>`）、`concession_effect` **100%**（让步）、`victory_effect` 15%（2 条，叛乱成功时） |
| 加载顺序 | 文件头注释权威：**文件名 `000_`–`998_` 先加载**，所以专属诉求会排在默认诉求之前被评估 |

让步包里出现的字段族：`pacify_rebel_pops` 12、`change_societal_value` 13、`owner` 13、`rebel_estate_type` 7、`grant_benefits_to_estate` 7、`add_legitimacy` 3、`victory_effect` 2。

两个原版实例：

- **奴隶**：`pacify_rebel_pops = yes` + 把全国所有 `pop_type:slaves` 改成农民
- **民族主义**：接纳其文化（`add_accepted_culture`）+ **推动社会价值** `centralization_vs_decentralization` 朝分权一小步

**本地化：不需要键**——原版没有 `REBEL_DEMAND_*` 之类的键（`tools\loc-keys.md` 已记录这条），诉求文本走 `concession_effect` 的描述。

## 七、法律（Laws）与政策（Policies）

### 结构（`laws\readme.txt` 权威）

> **A law is a container for one or more policies** —— 法律是容器，内含一个或多个政策，**同一时间只能选一个政策**。

**法律字段**：`type`（country / international_organization）、`potential`、`allow`、`locked`、**`requires_vote`**（IO 中通过政策是否需投票）、`law_religion_group`、`law_gov_group`、`law_country_goup`（⚠️ 本体拼写如此）、`unique`、`custom_tags`、`show_tags_in_ui`；其余键都是可选的**政策标签**。

**政策字段**：
- `price`、`potential`、`allow`、`custom_tags`、`show_tags_in_ui`
- **`years/months/weeks/days`**（实施时长，**修正按完成度缩放**）
- **四个生命周期钩子**：`on_pay_price` / `on_activate` / `on_fully_activated` / `on_deactivate`
- 四类可缩放修正：`country_modifier` / `province_modifier` / `location_modifier` / `international_organization_modifier`（均为 scaled and triggered modifier：`scale` + `potential_trigger`）
- AI 意愿：`wants_this_policy_bias` / `wants_propose_policy` / `wants_keep_policy` / `reasons_to_join` / `diplomatic_capacity_cost`

### ⭐ IO 政策可"改写整个国际组织"

政策还支持一长串 IO 覆盖字段（readme 45–85 行）：`modifier`（替换成员修正）、`leader_modifier`、`non_leader_modifier`、`owned_location_modifier`、`can_join_trigger`、`can_leave_trigger`、`auto_leave_trigger`、`auto_disband_trigger`、参战规则六件套（`join_defensive/offensive_wars_always|auto_call|can_call`）、`can_declare_war`、`has_military_access`、`leader_title_key` / `title_is_suffix` / `leader_type`、`leader_change_trigger_type` / `leader_change_method` / `leadership_election_resolution` / `months_between_leader_changes`、`has_parliament`、`min_opinion` / `min_trust`、`antagonism_*`、`payments_implemented/repealed`、`special_statuses_implemented/repealed`、`gives_food_access_to_members`、`has_dynastic_power`…并注明 **"Newer policies will supersede older policies"**。

**含义**：HRE / 天朝 / 联盟这类 IO 的组织规则本身由法律政策定义——改一条 IO 政策 = 改组织宪法。

### 法律解锁 vs 政策变更（⚠️ 这是两个不同概念，勿混用）

| 对象 | 可做的操作 |
|---|---|
| **法律 `law`** | **只能解锁 / 锁定，本身不存在"被修改"**——解锁途径：政府类型（`law_gov_group`）、宗教（`law_religion_group`）、国家（`law_country_goup`）、**革新**（advance 的 `unlock_law`）、政府改革（`unlock_law_effect`）；锁定判定：`locked` / `is_locked_for` |
| **政策 `policy`** | **可切换**（`add_policy`）——玩家日常说的"改法律"实际改的都是政策：在同一条已解锁法律内换一个选项 |

### 政策变更的八条途径（议会只是其中最常用的一种）

| 途径 | 代价 | 本体出处 |
|---|---|---|
| **① 议会请求** `ask_for_law_changes` | **议会支持度**（靠完成议程攒）—— **无损** | `generic_actions\parliament.txt:592-699` |
| **② 议会议程** `pa_change_policy` | **稳定度惩罚** | `parliament_agendas\00_common.txt:57-97` |
| **③ 议会诉求** | 诉求自身条件 | `parliament_issues\01_country_specific_parliament_issues.txt:516`（南特敕令） |
| **④ 叛军让步** `grant_benefits_to_estate` | **给阶层特权 + 满意度 + 政策** | `scripted_effects\rebel_negotiate_effects.txt:12-47`（7 个叛军诉求调用） |
| **⑤ 平时直接切换**（UI 常规操作） | **100 稳定度 + 10 正义** | `prices\00_hardcoded.txt:90-97` |
| **⑥ 事件** | 事件选项代价 | DHE 大量调用：`flavor_TUR` 24 次、`flavor_ENG` 24 次、`flavor_FRA` 19 次… |
| **⑦ 通用行动**（虔诚改教法学派） | 行动价格 | `generic_actions\piety.txt:176-204`（9 次） |
| **⑧ IO 政策投票** | `requires_vote` + 走投票 | `laws\readme.txt` |

**议会的真实定位 = 最常用的「无损」切换方式**：不花稳定度、不花金币，代价是你得先替阶层办议程（时间 + 议程自身的代价）。其余七条途径本质都是"花钱 / 花稳定度 / 给阶层好处"来买同一件事。

#### 证据一：叛军让步也改政策（`rebel_negotiate_effects.txt:12-47`）

```
grant_benefits_to_estate = {
    # a new privilege
    random_possible_privilege = { root.owner = { grant_estate_privilege = prev } }
    # a law change                                    ← 注释原文
    random_possible_policy = {
        limit = { any_estate_type_preferring = { this = root.estate_type }
                  law = { NOT = { is_locked_for = root.owner } } }
        root.owner = { add_policy = prev }
    }
    add_estate_satisfaction = { type = root.estate_type value = estate_satisfaction_extreme_bonus }
}
```
同意叛军请求 = **给一项特权 + 改一项政策（挑该阶层偏好的未锁定政策）+ 阶层满意度极大提升**。

#### 证据二：议会议程直接改政策（`parliament_agendas\00_common.txt:57-97`）

```
#Agenda to change a policy preferred by estate
pa_change_policy = {
    estate = nobles_estate / clergy_estate / burghers_estate / peasants_estate
    on_accept = {
        random_possible_policy = { ... root = { add_policy = prev } }
        add_stability = stability_severe_penalty        # 代价：扣稳定度
    }
    ai_will_do = { subtract = { value = 1000 } }        # AI 拒绝（注释：宁愿失败扣 7 稳定度）
    chance = 10
}
```

#### 证据三：平时切换的常规代价（`prices\00_hardcoded.txt:90-97`）

```
set_policy    = { scaled_gold = 1.0   max_scale = 200 }    # 金币途径
change_policy = { stability = 100     righteousness = 10 } # 100 稳定度 + 10 正义
```

**成本修正键**：`change_policy_cost_modifier`、`set_policy_cost_modifier`、`policy_vote_cost_modifier`（IO 投票）。

官方词条补充：**不同政府类型与宗教解锁不同法律，随革新研发解锁更多**；**所选政策通常会影响社会价值轴**（政策是价值轴推手——这是 `guides\law-design.md` 强调"漂移是副产物不是身份"的机制背景）。

## 八、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 阶层数值 | `common\estates\00_default.txt`（`power_per_pop`／`tax_per_pop`／`satisfaction`／`high_power`／`low_power`／`opinion`） | 改 `power_per_pop` 会连锁影响税基分配与王室力量 |
| 阶层阈值/系数 | defines `NEstate` | 全局生效 |
| 阶层特权 | `common\estate_privileges\<阶层>.txt` | `estate` 字段决定归属；`can_revoke` 控制可否撤销 |
| 法律与政策 | `common\laws\<文件>.txt`（33 文件） | 设计方法见 `guides\law-design.md`；解锁链三处须同步 |
| 议会 | `parliament_types\`（14 种）、`parliament_issues\`（**160 项**，按阶层）、`parliament_agendas\`（**128 项**） | 议案 `estate` 字段决定归属阶层；议程 `importance` 影响诉求强度 |
| 叛乱诉求 | `common\rebel_demands\<000-998>_<名>.txt`（前缀数字决定优先级） | `trigger = { rebel_category = … }` + `concession_effect`；`victory_effect` 可选 |
| 议会/阶层行动 | `generic_actions\parliament.txt`、`estates.txt`、`estate_emergency_actions.txt` | — |
| **新增阶层互动** | `common\generic_actions\<新文件>.txt`：`type = owncountry` + `player_automated_category = estates` + `select_trigger = { looking_for_a = estate }` + `cooldown` | 纯脚本即可（官方：无需代码）；`show_in_gui_list = no` 可隐藏自动列表 |
| 阶层议会参与权 | 修正键 `*_estate_can_participate_in_parliament` 系列 | — |

**硬编码**：阶层力量的最终计算（POP 权重求和 + 特权加成）、税基在地点层面的分配、议会支持度的累积与结算、议程/诉求的抽取与表决流程、政策实施进度的缩放。

## 九、中文检索键

概念：`game_concept_estate`（阶层）、`estate_power`（阶层力量）、`estate_satisfaction`（阶层满意度）、`estate_opinions`、`estate_privilege`（特权）、`crown_power`（王室力量）、`law`（法律）、`policy`（政策）、**`parliament` 议会**（`game_concepts_l_simp_chinese.yml:1994`）、`parliament_seat` 议会会址（:1996）、`parliament_type` 议会类型（:1998）、**`parliament_agenda` 议会议程**（:2001，简称"议程"）、**`parliament_issue` 议会议案**（:2006，简称"议案"）、**`parliament_support` 议会支持度**（:2011）、`parliament_base_support` 议会基础支持度（:2014）、`parliament_requests` 议会要求（:2017）。
阶层互动行动的中文名（`actions_l_simp_chinese.yml`）：`bribe_estate` 贿赂阶层 · `perform_reduction` 削减阶层力量 · `ask_for_extra_levies` 要求扩大征召 · `extraordinary_taxes` 征收特别税 · `ask_clergy_for_legitimacy` 请求神职者支持 · `ask_nobility_for_diplomats` 请求贵族使节 · `ask_burghers_for_loan` 请求紧急拨款 · `ask_commoners_for_stability` 向平民求助 · `ask_tribes_for_manpower` 号召部落武装 · `call_emergency_parliament` 召开紧急议会 · `take_estate_loan` 向阶层贷款。
界面：`estate_actions_lateralview.gui`（**阶层互动面板**）、`government_lateralview.gui`、`parliament` 相关面板（`common\parliament_types\`）、阶层满意度/力量 tooltip（`relative_power_tooltip.gui`）。
