# 法律设计与落地（common/laws 制作层）

> **一句话**：法律与政策的制作方法：readme 未声明但实查可用的字段、解锁链三处同步、potential 周期复查与撤销、政策池大小与断代命名。
> **什么时候看**：设计一条新法律、或法律解锁后反复被自动撤销时，看本文的解锁链与 potential 两节。
> **体量**：711 行 · 约 33 分钟通读

## 目录

- [一、readme 未声明但确认可用的字段](#一readme-未声明但确认可用的字段)
- [二、本地化：名称即键，没有独立字段](#二本地化名称即键没有独立字段)
- [三、解锁链：三处必须同步，漏一处**静默失效**](#三解锁链三处必须同步漏一处静默失效)
  - [⚠ 解锁成功 ≠ 永久有效：`potential` 会被引擎周期复查（高危）](#-解锁成功--永久有效potential-会被引擎周期复查高危)
  - [那"能不能用 `potential` 判领地/属国，省掉事件"？——别换](#那能不能用-potential-判领地属国省掉事件别换)
  - [决议的"展示 / 可选"分离：`potential` 放宽、`allow` 收紧（2026-10-17 实做）](#决议的展示--可选分离potential-放宽allow-收紧2026-10-17-实做)
  - [更好的解法：**用决议当解锁器**（2026-10-17 定案，推荐优先考虑）](#更好的解法用决议当解锁器2026-10-17-定案推荐优先考虑)
  - [解锁器省性能：分层短路 + 降频（2026-10 实修）](#解锁器省性能分层短路--降频2026-10-实修)
- [四、政策的通行配方（三件套 + 时长）](#四政策的通行配方三件套--时长)
- [五、池子该开多大：本体通用池只有 5~9 条](#五池子该开多大本体通用池只有-59-条)
  - [类目级同样适用：**类目总条数 ≠ 玩家看到的条数**（2026-10-17 全目录实查，200 个法律块）](#类目级同样适用类目总条数--玩家看到的条数2026-10-17-全目录实查200-个法律块)
- [六、时代阶梯：本体自己就这么干](#六时代阶梯本体自己就这么干)
- [七、内容断代：三筛规则](#七内容断代三筛规则)
- [八、命名：三条禁令](#八命名三条禁令)
- [九、撞名检查（新增政策**必做**）](#九撞名检查新增政策必做)
- [十、审查要点（法律专属）](#十审查要点法律专属)
- [十一、区域化政策：只对特定地区生效（2026-10-17 实修）](#十一区域化政策只对特定地区生效2026-10-17-实修)
  - [想"只让自己的法律放宽/限制某栋建筑"：新建建筑 + 新注册 allow 键](#想只让自己的法律放宽限制某栋建筑新建建筑--新注册-allow-键)
  - [国家级 → 地点级：哪些键换得动，哪些换不动（2026-10-17 实做青藏法）](#国家级--地点级哪些键换得动哪些换不动2026-10-17-实做青藏法)
  - [⚠ 用 `area =`，不要想当然用 `topography = mountains`](#-用-area-不要想当然用-topography--mountains)
  - [判定"国内有某文化组人口"用现成触发器，别遍历 POP](#判定国内有某文化组人口用现成触发器别遍历-pop)
- [十二、政策该动"阶层与国家的关系"，不是国家的数字（2026-10-17 实修反例）](#十二政策该动阶层与国家的关系不是国家的数字2026-10-17-实修反例)
  - [地点级政策在"小地块国家"里是陷阱选项](#地点级政策在小地块国家里是陷阱选项)
- [十三、同族法律：`X 管理法` 这类"系列"怎么做才不重复（2026-10-17 实做 2 部后总结）](#十三同族法律x-管理法-这类系列怎么做才不重复2026-10-17-实做-2-部后总结)

来源：2026-09《尼泊尔王公自用平衡》实修（EU5 1.3.11）+ `in_game\common\laws\` 全目录逐块实查。

**字段权威见 `common-laws.md`**（`laws\readme.txt` 提炼）。本文只写 **readme 没写、但实查确认** 的东西，以及"怎么设计一条法律"的方法。

---

## 一、readme 未声明但确认可用的字段

| 字段 | 位置 | 说明 |
|---|---|---|
| `law_category` | law 块 | UI 分类显示。**实机确认生效**：`law_category = administrative` 显示为「行政法」。**不需要 `<法律键>_CATEGORY` loc 键**（本体部分老法律有该键做覆盖，做新法律不必） |
| `estate_preferences` | policy 块 | 政策靠山（列阶层名）。readme 完全未提，但本体大量使用（`administrative_system`、`cabinet_laws`、`02_distribution_of_power.txt`） |
| `monthly_towards_<轴>` | policy 的 `country_modifier` 内 | 社会价值轴漂移；值习惯用 script value `societal_value_monthly_move` |
| `potential = { current_age_or_later = { age = age_N } }` | policy 块 | 按时代解锁**该条政策**（不是整条法律） |

本体 30 个法律文件里只有 **3 个**用到 `current_age_or_later`（`01_common.txt`、`03_estate_laws.txt`、`23_lordship_of_ireland.txt`），**没有任何法律在法律级用 `age =`** —— 时代门控是"可用但非主流"的写法。

## 二、本地化：名称即键，没有独立字段

- **法律块名** → `<块名>` + `<块名>_desc`
- **政策块名** → `<政策名>` + `<政策名>_desc`

漏一个就显示 raw key。`common-laws.md` 里写的"未在 readme 中说明：本地化键格式"，就是这条。

## 三、解锁链：三处必须同步，漏一处**静默失效**

`unlock_law_effect = { type = X }` 内部是 `set_variable = { name = unlocked_law_$type$ }`，变量名由 `$type$` 派生。所以改法律键名时要同步**三处**：

1. 法律块名（`in_game\common\laws\<文件>.txt`）
2. 同文件 `potential = { has_unlocked_law_trigger = { type = X } }`
3. **调用侧**——通常在**另一个文件**里，如 `government_reforms\<文件>.txt` 的 `on_activate = { unlock_law_effect = { type = X } }` ← **最容易漏**

只改一边 → 名字对不上 → **法律永远解不开，而且不报错**。

**双调用有本体依据**：一条效果的 `on_activate` 里可以连调多次 `unlock_law_effect`。参照 `in_game\common\formable_countries\00_formable_countries.txt:2546-2551`——拜占庭内容解锁时连调两次 `unlock_government_reform_effect` + 四次 `unlock_policy_effect`。（`unlock_law_effect` 与 `unlock_policy_effect` 在 `scripted_effects\country_effects.txt` 里结构完全相同，只差 `law:`/`policy:` 与变量前缀。）

> 附带一条本体 readme 的自相矛盾：readme 警告「`custom_description` 的 `text` 键不可与 effect 键同名」，而 `unlock_law_effect` **正是这样定义的**。它是本体自身写法且被调用数百次，实际工作正常——该警告针对的是丢失 subject/object/value 拾取的场景。

### ⚠ 解锁成功 ≠ 永久有效：`potential` 会被引擎周期复查（高危）

引擎**周期性复查** `law` / `government_reform` 的 `potential`，**一旦失效，整条法律连同已选中的政策一起被自动撤销**（不是隐藏、不是保留，是移除）。

**实测事故**：某法律的 `potential` 里写了 `estate_power(estate_type:士兵阶层) > 0`。而 EU5 **军队即 soldiers POP**——军队全灭／整军遣散／组建军团转移全部部队／出租整军，都会让这个阶层力量归零，等价于"国家没有任何军队"。结果：法律消失 → 重建军队后回来 → 再选 → 再被撤，玩家观感是"**法律反复被自动撤销**"。

**铁律**：`law` / `reform` 的 `potential` 只放**几乎不变**的条件——政体、宗教、改革、tag。
**绝不放会随局势波动的动态值**（兵力、阶层力量、金币、战争状态）。

- 「想隐藏 UI」→ 用 `potential`
- 「想防止被撤销」→ 必须保证**已生效后 potential 恒真**


### 那"能不能用 `potential` 判领地/属国，省掉事件"？——别换

有人会想：既然 `potential` 本来就支持任意触发器，为什么不用 `any_subject` / `any_owned_location` 直接判，省掉脚本事件？

- **语法上确实可行**：`any_subject` 本体 153 处使用；`any_subject = { any_owned_location = … }` 嵌套见
  `expedition_types\circumnavigation_expedition.txt:91`。
- **但换不掉风险**：`potential` 会被周期复查（上一节），而领地与属国都是**易变**的 ⇒ 丢地/属国独立即撤法清政策。
- **性能收益是未证实的**：引擎多久复查一次 law `potential`，无法从文件确定；而脚本事件的频率是我们自己可控的。
- 结论：**用确定的好处（锁存）换不确定的收益（性能），不划算**。要省性能就省脚本事件那边（见下）。

### 决议的"展示 / 可选"分离：`potential` 放宽、`allow` 收紧（2026-10-17 实做）

想让玩家"看到这件事存在，但还做不到"时，别把条件全写进 `potential`——`potential` 只会让决议**凭空消失或凭空出现**。
正确分工：

| 字段 | 语义 | 放什么 | UI 表现 |
|---|---|---|---|
| `potential` | 决议**是否显示** | 宽松的地理/关系条件（如"与持有目标地区的国家接壤"） | 不满足 ⇒ 列表里根本没有这条 |
| `allow` | 决议**能否点** | 真正的门槛（拥有地区地点 / 文化组人口 / 属国满足 / `at_war = no`） | 不满足 ⇒ 决议灰着，**条件以灰字逐条列出** |

```txt
potential = {
    NOT = { has_unlocked_law_trigger = { type = X } }          # 已解锁就不再显示
    OR = {
        <实质条件>                                              # 已满足当然要显示
        any_neighbor_country = { any_owned_location = { region = region:tibet_region } }   # 接壤即显示
    }
}
allow = { at_war = no  OR = { <实质条件> } }
```

- `any_neighbor_country = { any_owned_location = { … } }` 本体先例：`decisions\flavor_decisions_rus.txt:150`；
  "接壤对象持有整块地区"用 `region = region:X`（location 作用域，`capital_buildings.txt:1059`）。
- 好处：条件对玩家**可见且可预期**——不用等"领土一接触就凭空多出一条法律"，也不用猜差什么。
- `potential` 里仍可放宽（它不撤销已发生的事）；但 `allow` 才是真正让人点不下去的那道闸。

### 更好的解法：**用决议当解锁器**（2026-10-17 定案，推荐优先考虑）

脉冲事件只是"没有别的办法时"的兜底。**决议（`common\decisions\`）天生更适合当一次性解锁器**：

| | 脉冲事件 | **决议** |
|---|---|---|
| 周期脚本成本 | 每国每次脉冲跑一遍 | **零**（不进任何 pulse） |
| 易变条件（领地/属国/文化占比） | 只能靠隐藏事件里锁存变量绕开"撤法"风险 | **天然无害**：`potential`/`allow` 只控制"显示与能否点"，不满足时决议消失/置灰，**不撤销任何已发生的事** |
| 幂等 | 自己写 `NOT has_unlocked_law_trigger` | **`only_once = yes`** 原生 |
| 玩家体验 | 悄无声息地多出一条法律 | 看见决议、点下去才拿到——风味内容更合适 |

**骨架（本体两处先例同构）**：

```txt
my_decision = {
    decision_category = government_decisions      # 类目 loc 本体已有（DECISION_CATEGORY_GOVERNMENT）
    only_once = yes
    potential = { …易变的门槛都可以直接写… }
    allow = { }                                   # 可选：一点仪式感门槛
    ai_will_do = { value = 100 }                  # ⚠️ 不写 AI 永不点
    option = {
        name = my_decision.a
        ai_chance = { value = 100 }
        effect = {
            show_as_tooltip = { unlock_law_effect = { type = my_law } }   # show_as_tooltip = 执行 + 显示自身 tooltip
        }
    }
}
```

- 决议解锁政府改革先例：`decisions\maghrebi_culture_decisons.txt:1-39`（`unlock_government_reform_effect`）
- 决议解锁法律先例：`decisions\flavor_decisions_fra.txt:204-221`（`show_as_tooltip = { unlock_law_effect = { type = fra_fouage_tax } }`）
- **本地化键是 `<id>.title` / `<id>.desc` / `<id>.a`**（不是 `<id>` 与 `<id>_desc`），依据
  `main_menu\localization\simp_chinese\events\culture\culture_maghrebi_events_l_simp_chinese.yml:119-126`
- 代价四条：玩家要自己点（名字要显眼）· AI 必须配 `ai_will_do`/`ai_chance` · 比隐藏事件多 3 个 loc 键 ·
  **`potential` 里的门槛玩家一个字都看不见**（potential 只决定"显示不显示"，`allow` 才会以灰字条件列表呈现）
  ⇒ 必须把"这个决议为什么会冒出来"写进 `.desc`，否则玩家只会觉得"界面上莫名其妙多了个可点的决议"。
  写法示例（把领地/文化组/属国三类条件都点名）："此决议出现于两种情形，一是朝廷治下有 X 地，二是境内或属国之中有 Y 文化组的人口"。

**什么时候仍该用脉冲事件**：要求"玩家完全不操作也必须发生"的解锁（例如给 AI 保底、或开局自动生效的机制）。

### 解锁器省性能：分层短路 + 降频（2026-10 实修）

若用隐藏事件做一次性解锁，两个开关都能拧：

1. **判定分层，按成本从便宜到贵排 `if / else_if` 阶梯**（不要写成 `OR = { 贵 便宜 }`——OR 求值顺序不由脚本控制）：

```txt
if = { limit = { NOT = { has_unlocked_law_trigger = { type = X } } }      # 幂等短路
    if      = { limit = { culture ?= { has_culture_group = culture_group:… } }                … } # O(1)
    else_if = { limit = { culture_group_percentage_in_country = { culture_group = …  value > 0 } } … } # 聚合值
    else_if = { limit = { any_subject = { OR = { culture ?= {…}  culture_group_percentage_in_country = {…} } } } … } # 属国数量级
    else_if = { limit = { any_owned_location = { OR = { area = area:… } } }                    … } # 领地数量级，最后
}
```

2. **脉冲频率按内容重要性选**：`monthly_country_pulse` / `biyearly_country_pulse`（`country_biyearly.txt:4`）/
   `four_yearly_country_pulse` / `yearly_country_pulse` 四条都在，跨文件追加 `events` 的语义一致
   （都是 `on_actions` / `random_events` / `events` 三种列表）。风味内容放到 biyearly 即可（成本 /6），
   核心机制再考虑 monthly。

最小改法（已生效的内容保留，门控只挡"从未选过 + 无阶层"的纯平民国家）：

```
potential = {
    OR = {
        "estate_power(estate_type:xxx)" > 0
        has_policy = <本法律的任一政策>
    }
}
```

也可以**直接删掉 law 级 `potential`**（原版 `army_doctrine_law` 就没有）。

> **对本 mod 的结论**：两条法律用 `has_unlocked_law_trigger = { type = X }` 作 potential，依赖政府改革 `on_activate` 设下的 `unlocked_law_<type>` 变量——**该变量不会被撤销，所以本写法天然安全**。但将来若在 potential 里加任何动态条件，就会踩这个坑。

> 完整机制、原版证例与修复手法见 `cases\laws-events-and-estates-2026-09.md` §1。（原版证例：`feudal_de_jure_law` 以 `government_type` 为 potential，政体一变法律即消失，证明"引擎自动撤销"是**标准行为而非 bug**。）

## 四、政策的通行配方（三件套 + 时长）

```
<policy> = {
    country_modifier = { <收益...> <硬代价...> <monthly_towards_* = societal_value_monthly_move> }
    estate_preferences = { <阶层> }
    months = 1        # 或 years = 2（本体大法律主流）
}
```

本体大法律（`administrative_system`、`cabinet_laws`）几乎条条如此。

**大法律不塌缩的机制不是削弱数值，而是让每个选项 = 一个具体动作 + 一批支持者。** 反过来说：如果一条政策的身份只是它的价值观漂移方向（去掉 `monthly_towards_*` 就只剩一堆可互换的百分比），那整条法律读起来就是"价值观加速器"——这是设计错误，不是平衡问题。

**漂移是副产物，不是身份。** 允许某些政策不带漂移。

## 五、池子该开多大：本体通用池只有 5~9 条

⚠ **别被大数字骗了。** 逐块数准（2026-09）：

| 法律 | 总政策 | **通用** | tag 专属 |
|---|---|---|---|
| `legal_code_law` | 42 | **5** | **37**（英国普通法、沙里亚、罗斯真理、巴伐利亚法典、大明律、经国大典、蒙古法、塞尔维亚、波兰、立陶宛…） |
| `administrative_system` | 24 | **9** | 15（诺夫哥罗德 pyatina、奥斯曼 eyalet、阿拉贡 merindad、纳瓦 calpixque、骑士团 tongues、印度 rajya/poligar/zabt、拜占庭 pronoia 系列…） |

**本体的"大法律"是靠 tag 门控堆出来的，通用池一律小而精。** 推论：

- 做**通用法律**（无条件解锁）→ 池子开 **5~9 条**，靠"每条更重"制造分化
- 想开大池子 → **必须**用 `potential` 做 tag / culture / 宗教 / 时代门控

**数政策时的两个坑**：
- 政策的 `unique = yes` 在**两个 tab**，法律级的才在一个 tab。用 `^\tunique = yes` 数会只命中法律级、把 tag 专属政策全漏掉。
- 有些政策块 opener 带行尾 `#` 注释（`01_legal_system.txt:432, 451, 496, 590, 619`），用 `\{$` 锚定会静默漏掉 → 用 `\s*\{`。
- 交叉验证用 `^\t\t(years|months|weeks|days) = `（政策级时长字段）；但**不是每条政策都有时长字段**（`traditional_law_policy` 就没有），以块计数为准。

### 类目级同样适用：**类目总条数 ≠ 玩家看到的条数**（2026-10-17 全目录实查，200 个法律块）

| 类目 | 法律块总数 | **无法律级 `potential`**（任何国家都会看到） |
|---|---|---|
| `socioeconomic` | 50 | **7** |
| `administrative` | 49 | **4** |
| `religious` | 40 | 2 |
| `military` | 22 | 3 |
| `estates` | 11 | **5** |
| `centralization` / `election` / `elector` / `common_laws` / `free_city` / `foreign_policy` / `foundation` / `leadership` | 8/5/4/2/2/1/1/1 | **全部 0** |

`socioeconomic` 那 7 条是：`economical_policies_law`、`cultural_traditions_law`、**`foreign_cultural_law`（异文化管理法）**、
`coin_laws`、`press_law`、`mining_law`、`immigration_law`。

其余 43 条都有法律级 `potential`：**6 条**用 `has_unlocked_law_trigger`（靠革新/事件解锁），
**37 条**用其它门槛（`has_tribal_government`、`"estate_power(estate_type:peasants_estate)" > 0`、文化/宗教/变量等）。
⚠️ **TAG 专属多写在「政策级」`potential`，不是法律级**——`foreign_cultural_law` 自己无门槛，但它的
`agrarian_constitution` 里是 `this = c:TEU`、两条中华政策用 `sub_continent` 判断。

⇒ 给新法律挑类目时**别按"这个类目有 50 条所以会很长"来判断**；要数就数"没有法律级 `potential` 的那几条"。

## 六、时代阶梯：本体自己就这么干

`in_game\common\laws\01_common.txt:589-972` 的 `administrative_system` 是范本——**1337 档全是去中心化选项**：

| 政策 | 解锁 | 效果关键词 |
|---|---|---|
| `autonomous_councils` | **1337** | 立法效率、**去中心化** |
| `central_councils` | **1337** | 内阁效率、集权、贵族化 |
| `local_courts_administration` | **1337** | **去中心化**、入叛门槛 −0.03、**农村控制 −0.05** |
| `feudal_administration` | **1337** | **去中心化**、粮食 +0.05 |
| `confederal_union` | 1437 | 去中心化、附庸忠诚 +10 |
| `federal_state` | 1537 | 全体满意度、去中心化 |
| `absolute_rulership` | **1637** | **王室权力 +0.1**、绝对主义 |
| `unitary_administration` | **1637** | **距离传播 +0.05**、集权 |

**时代起始年**：`age_1_traditions` 1337 ／ `age_2_renaissance` 1342 ／ `age_3_discovery` 1437 ／ `age_4_reformation` 1537 ／ `age_5_absolutism` 1637 ／ `age_6_revolutions` 1737。

**三条设计含义**：

1. **1337 年"行政体系"的选择本质是"你打算把多少权力让给地方"**——`local_courts_administration` 直接给**负的** `global_max_rural_control`。
2. 把绝对主义时代的举措（土地清册、常设税、国家驿传、王室权力增长）放到 1337 是**断代错误**。全本体只有 `absolute_rulership` 给 `global_crown_estate_power`，且锁在 1637。
3. **数值上的"弱一档"就是断代**，不一定需要加门控。同一个修正，1337 给 0.05、1637 给 0.1，本身就是时代差异。

## 七、内容断代：三筛规则

写**无条件解锁**的通用法律时，每条政策必须同时过三筛：

| 筛 | 要求 |
|---|---|
| **一、开局年做得到** | 不需要常设官僚、常备军、常设税 |
| **二、任何文明都有** | 不能是某个文明的专有制度 |
| **三、是一件具体的政府动作** | 不是价值观方向 |

二三两筛的交集只有一个东西：**普遍政府职能**。

> **关键区分：「专名」要禁，「职能词」不禁。**
> `常平仓`是汉代某具体机构的名字（专名，**禁**）；"粮仓"是全人类都有的职能（职能词，**不禁**）。
> `科举`是中国专有制度（**禁**）；"用人"是普遍职能（**不禁**）。
> 等级会议／封建封臣／修道院／巡回法庭是拉丁基督教世界的专有制度（**禁**）；"议事""司法""扶植宗教"是普遍职能（**不禁**）。

## 八、命名：三条禁令

通用法律的命名最容易连错三次，**每次都换一种错法**：

| 错法 | 例子 | 为什么错 |
|---|---|---|
| **汉典专名** | 清丈田亩、编户齐民、设立常平仓、开科取士、通商惠工 | 《汉书》典故、汉代具体机构名、中国专有制度 |
| **现代公文构词** | 户口**登记**、邮驿**体系**、**奖励**工商、攻势**外交** | `体系`／`登记`／`奖励`／`XX外交` 都是 19 世纪末才进中文的构词 |
| **文明专名** | 召集等级会议、委政封臣、修道院庇护、巡回审判 | 拉丁基督教世界的制度，元朝／德里苏丹国／马里帝国／阿兹特克一个都对不上 |

**本体自己划的线**：

- **通用法律一律用中性制度词**：本乡自治、巡回法庭、封建行政、集权管理、联邦国家、林业条例、航海法案、保护商路、选贤任能
- **专名一律门控**：六部、科举制度、`examination_system_policy_chinese` / `_korean` / `_vietnam`（中国/朝鲜/越南）；锁国与开国（日本）；八大臣内阁（马拉塔）

**规则**：政策名 = **一类制度或一项工程的通称**。不做典故、不做对偶、不用特定文明的官制／典籍术语。描述用现代短句直述，不用四六骈文。

## 九、撞名检查（新增政策**必做**）

**显示名与键名都要查**，对照本体 `main_menu\localization\simp_chinese\` 全目录（键名方面该目录的 `laws_and_policies_l_simp_chinese.yml` 收全部法律与政策显示名）。

实测撞到两个：

| 初拟 | 本体已有 | 后果 |
|---|---|---|
| 保护商路 | `protect_trade_routes: "保护商路"` | 两条不同法律里出现同名政策，UI 上是两个「保护商路」 |
| 举贤任能 | `meritocratic_recruitment_policy: "选贤任能"`（只差一字） | 同屏混淆 |

**只查显示名不查键名是不够的**——键名撞了会直接覆盖本体本地化。

## 十、审查要点（法律专属）

- 政策里改国级用 `country_modifier`；误用 `province_modifier` / `location_modifier` 会静默变成地点级。
- `unique = yes` 是法律级字段，含义与其他类目的 `unique` 一致（排他）。
- 引用 payment / special status / parliament_type 的 IO 政策须确认对应类目存在（见 `common-laws.md`）。
- 政策块 opener 若带行尾注释，**注释内容不参与解析**，但会干扰审查脚本的 `\{$` 锚定。
- **`law_category = foreign_policy` 在国家作用域没有本体先例**——本体唯一那条 `sc_relations_with_hre` 是国际组织作用域（`potential` 里是 `international_organization_type`）。类别显示大概率正常（`law_category` 字段生效已在行政法上实机确认），但属于**未实机验证**项，审查时标 `[存疑]`。

## 十一、区域化政策：只对特定地区生效（2026-10-17 实修）

**做法**：政策用 `location_modifier` + `potential_trigger` 圈范围，**条件外面套 `custom_tooltip`**。

```txt
himalayan_garrisons = {
    location_modifier = {
        potential_trigger = {
            custom_tooltip = {
                text = himalayan_law_region_tt          # ★ 给玩家的人话说明
                OR = {
                    area = area:nepal_area
                    area = area:monyul_area
                    area = area:upper_indus_area
                }
            }
        }
        local_manpower_modifier = 0.1
        local_defensive = 0.1
    }
    estate_preferences = { tribes_estate }
    years = 2
}
```

本体先例：`estate_privileges\tribes_estate.txt:244-282`（`desert_power` 用 `custom_tooltip` + `vegetation = desert`/`climate = arid`）、
`laws\01_common.txt:2350-2376`（`reliance_on_local_tribes` 用 `local_control < 0.25` + `any_pop = { pop_type = tribesmen }` 圈地点）。
法律里 `location_modifier` 共 15 处、`potential_trigger` 290 处，是**常规写法**，不是偏门。

### 想"只让自己的法律放宽/限制某栋建筑"：新建建筑 + 新注册 allow 键

直接改原版建筑（加 `location_potential` 地理限制、把 `town = no` 改 `yes`）会**连带影响所有能建它的国家**
（`clerical_archives` 的原版解锁键 `allow_clerical_archives` 就被教士特权 `clergy_literacy_rights` 用着）。正确做法是复制一份：

1. **新注册 boolean 修正键**（`main_menu\common\modifier_type_definitions\zzz_*.txt`，注册表支持多文件，本体已有 `00_`/`01_`/`02_`）：
   ```txt
   allow_tibetan_monastery = {
       boolean = yes
       game_data = { category = country }
   }
   ```
   照抄 `00_modifier_types.txt:17081-17086` 的 `allow_clerical_archives` 形状。
2. **新建建筑**（`in_game\common\building_types\zzz_*.txt`）：整块复制原版 + 只改你要的两处
   （本例 `town = yes` + `location_potential = { region = region:tibet_region }`），
   `country_potential = { modifier:allow_tibetan_monastery = yes }`。
3. 法律政策里给 `allow_tibetan_monastery = yes`，原版键一个都不碰。
4. 本地化要多 4 个键：`<building>`、`<building>_desc`、`MODIFIER_TYPE_NAME_<键>`、`MODIFIER_TYPE_DESC_<键>`。
   ⚠️ 追加到已有 loc 文件时**先删旧再写新**，否则同一个键出现两次（本项目刚踩：三处政策描述变成重复键，需去重）。

### 国家级 → 地点级：哪些键换得动，哪些换不动（2026-10-17 实做青藏法）

把一条法律从"全国生效"改成"只在某片地区生效"，不是把 `global_` 换成 `local_` 就行——**先查注册表有没有地点级变体**：

| 国家级键 | 地点级变体 | 本体取值锚点（地点级量级明显更大） |
|---|---|---|
| `global_clergy_max_literacy` | ✅ `local_clergy_max_literacy` | 10×17 众数（国家级是 5×20） |
| `global_tribesmen_pop_growth` | ✅ `local_tribesmen_pop_growth` | 本体仅 2 处：0.05 / **0.15**（`01_common.txt:2350-2376`） |
| `global_population_growth` | ✅ `local_population_growth` | 0.0005×3 众数（国家级是 0.0001×43，**量级差 5 倍**） |
| `global_monthly_development` | ✅ `local_monthly_development` | 0.005×3 众数（国家级 0.001×43） |
| `global_distance_from_capital_speed_propagation` | ✅ `local_distance_from_capital_speed_propagation` | 0.1×3 |
| `global_max_control` | ✅ `local_max_control` | 0.05×8 众数 |
| （无直接对应） | ✅ `local_monthly_control` | 0.01×4 众数 |
| `global_manpower_modifier` | ✅ `local_manpower_modifier` | 0.1×3 |
| `global_supply_limit_modifier` | ⚠️ `local_supply_limit_modifier` 存在但**正值仅 0.01**（`town_buildings.txt:335`，其余全负），撑不起一条路线 ⇒ 改用 `local_defensive` | `local_defensive` 0.1×11 |
| `global_integration_speed_modifier` | ⚠️ `local_integration_speed_modifier` 存在但**本体 4 处全负**（-0.25/-0.5/-1.00），唯一正值 0.25 档位不符 ⇒ 改用 `local_monthly_control` | — |
| `global_raw_material_output` | ✅ `local_raw_material_output` | 0.1×3 / 0.33×2 / **0.025×1**——想走温和档可以用 0.025 |
| `global_supply_limit_modifier` 的替代（见下） | ✅ `local_levy_size_modifier`（本地征召规模） | 0.1×3（`estate_buildings.txt:244` / `manpower_buildings.txt:417`） |
| —— | ✅ `monthly_prestige`（国家级） | 0.1×109 众数——"给名分"这类路线用每月威望比用满意度衰减直观 |
| `global_livestock_output_modifier` | ✅ `local_livestock_output_modifier` | 本体仅 1 处 = 0.2 |
| `tribes_estate_target_satisfaction` / `..._satisfaction_decay` | ❌ 无地点级（阶层数值） | 留国家级；可另加 `local_unrest = -0.05`（0.05×16）表达"本乡不闹事" |
| `tolerance_heretic` / `tolerance_heathen` | ❌ **无地点级定义** | 容忍度是国家层数值 |
| `monthly_karma` | ❌ **无地点级定义** | 业力是国家层数值 |
| `allow_clerical_archives`（及各类 `allow_*` 建筑解锁） | ❌ 只能国家级 | 建筑的 `country_potential` 就查它（`estate_buildings.txt:564`） |

两条实施要点：

1. **混合结构是合法的**：同一条政策可以同时有 `country_modifier` 与 `location_modifier` 两块
   （青藏法「寺院庄园」= 国家级解锁 `clerical_archives` + 地点级 `local_clergy_max_literacy`）。
2. **量级要跟着层级调整**：地点级是"一个地点"的量，本体自己给的就是国家级的好几倍；照抄国家级的数值会让地点级政策弱到看不见。
3. 地点门写法与 `area =` / `custom_tooltip` 的注意事项见下一节。

### ⚠ 用 `area =`，不要想当然用 `topography = mountains`

实测（`in_game\map_data\location_templates.txt`，逐地点一行 `topography = …`）：
**`kathmandu` 的 topography 是 `plateau`，`upardang` 是 `flatland`**，`nepal_area` 11 个地点里
是 **mountains×9 / plateau×1 / flatland×1**。⇒ 只写 `topography = mountains` 会**把首都排除在外**，
而且界面上完全看不出问题（地点就是不生效）。

`topography` 本身是好触发器（`common\*\*.txt` 90 处使用，`estate_buildings.txt:608-609` 就并列 hills + mountains），
但**"山地"必须自己定义成 `OR = { mountains  plateau  hills }`**，或者干脆按 `area` 圈。
`area = area:X` 在 location 作用域有本体先例（`estate_privileges\peasants_estate.txt:432`）。

### 判定"国内有某文化组人口"用现成触发器，别遍历 POP

```txt
culture_group_percentage_in_country = { culture_group = culture_group:nepali_group  value > 0 }
```

- 定义：`in_game\common\trigger_localization\country_triggers.txt:2847`（`CULTURE_GROUP_IN_COUNTRY_TRIGGER`）
- 本体先例：`parliament_issues\01_country_specific_parliament_issues.txt:314`（`british_group` > 0.1）、
  `town_rights\byzantine_greek_roman.txt:77`（`greek_group` > 0.25）
- 中文显示：「拥有 >0% 的 X 文化组人口」（`triggers_l_simp_chinese.yml:4921`）
- `value > 0` 等价于"国内存在该文化组人口"，成本远低于 `any_owned_location = { any_pop = { … } }`

## 十二、政策该动"阶层与国家的关系"，不是国家的数字（2026-10-17 实修反例）

**本体样板**：`laws\03_estate_laws.txt:2-40` 的 `nobles_rights_laws` 四条政策 =
**强化贵族权利 / 削减贵族 / 乡村贵族权威 / 贵族选举团**。每条都在回答
"**你打算把这个阶层摆在哪里**"：抬力量、压力量、给地方权威、给选举权。

**反例（本库实修）**：一条"某族群管理"的法律，第一版 5 条政策给的是
人力 / 要塞防御 / 整合速度 / 每月发展度 / RGO 规模 / 生产效率 —— 全是**通用发展修正**。
把它们换成贵族版就是"贵族耕地开垦效率 +5%""贵族要塞防御 +10%"，立刻能看出不对劲。

**审查问法**：把政策名里的族群/阶层替换成"贵族"，如果读起来荒谬，那这条政策就没有在"管理"那个群体。

### 地点级政策在"小地块国家"里是陷阱选项

法律的 `location_modifier` 只对**满足 `potential_trigger` 的己方地点**生效。若圈定范围只有几个 area，
对只有 6 个地点的小国，一条"`local_production_efficiency = 0.05`"实际等于 +5% 的极小盘；
而同屏并列的国家级政策（整合速度 +10%、文化影响力 +10%）在任何规模都实打实
⇒ **N 选 1 里地点级那条永远输**，法律等于没有选择。

两条解法：
1. **效果改国家级，地理限定交给"解锁条件"**（解锁只发给有这块地的国家）——本库采用；
2. 非要用地点级，就把数值放大到与国家级可比，并确保触发范围覆盖该国**大部分**地点。

**代价也要写清**：法律是"永久 + 同一时间只能选一条"，所以每条政策的强度应该**明显高于一项阶层特权**，
否则玩家感觉不到自己在"选路线"。

## 十三、同族法律：`X 管理法` 这类"系列"怎么做才不重复（2026-10-17 实做 2 部后总结）

要做的不是"每部法换个名字"，而是**把人群原型映射到杠杆层级**——这一步定死，重复就无从发生。

| 原型 | 判定依据（本体数据） | 该法的杠杆层级 | 已做实例 |
|---|---|---|---|
| **A 无钱阶层** | `tax_per_pop` 极低（部落民 **0.01**，`estates\00_default.txt:1090`） | 只能给**国家层面修正**（给"许可/建筑"是空牌，阶层买不起） | 喜马拉雅山民管理 |
| **B 有钱阶层** | `tax_per_pop` 高（教士 **25**、贵族 100） | 可给**阶层建筑解锁**等机制杠杆 | 青藏居民管理（`allow_clerical_archives`） |
| **C 编户对象** | 主体是农户（`tax_per_pop` 1，人口占比最大） | 税基/行政/官僚/科举类 | 待做（汉人管理法） |
| **D 军事阶层** | 有专属兵源或阶层建筑 | 征兵/军制/兵农分离类 | 待做（和人管理法） |

**三条硬规矩**：

1. **招牌机制化，别招牌数值化**。给"解锁一栋阶层建筑"这类**机制**，叠一百部法也不会变成 +400% 控制力；
   只有 A 型（阶层没钱）才被迫用国家百分比，那是**例外**，不该成为家族常态。
2. **同一修正族里，一部法最多占 1 个键；全家族每族最多 2 个（一国家、一地点）**。
   先查已有占位再动手。已占：控制/邻近（`global_max_control`、`global_max_rural_control`、`global_integration_speed_modifier`、
   `global_distance_from_capital_speed_propagation`）、军事（`global_manpower_modifier`、`global_supply_limit_modifier`）、
   经济产出（`global_raw_material_output`、`global_livestock_output_modifier`）、宗教（`tolerance_heretic`、`monthly_karma`）、
   人口（`global_tribesmen_pop_growth`、`global_population_growth`）、识字与建筑解锁（`global_clergy_max_literacy`、`allow_clerical_archives`）、
   发展（`global_monthly_development`）、阶层满意度（`tribes_estate_target_satisfaction`、`tribes_estate_satisfaction_decay`）。
3. **骨架该重复，招牌不该重复**。骨架固定为：`law_category` + `unique = yes` + `potential` 只查 `has_unlocked_law_trigger` +
   4 条政策（`years = 0` + `estate_preferences`）+ **决议解锁器**。四部法共用同一骨架，玩家一眼认得，也知道怎么用。

**写法律前先查"本体已经用这些键做过什么建筑/进步"（2026-10-17 汉人法返工）**：法律不是凭空加数值，
本体常有一栋建筑已经把同一件事做完了。做「汉人管理」时按作者追问逐个查，结果如下：

| 本体内容 | 类型 | 它占的键 |
|---|---|---|
| 官学 `confucian_school` | building（`culture_buildings.txt:106-157`） | `local_max_literacy = 5` · `local_pop_promotion_speed = 0.001` · `local_institution_growth_modifier = 0.25`；解锁靠 `age_1_traditions` 的 `confucian_school_advance`（`advances\1_building_unlocks.txt:1019-1040`），**儒家文化圈国家人人可得** |
| 书院 `confucian_academy` | building（`culture_buildings.txt:162-212`） | `local_burghers_max_literacy = 10` · `local_pop_promotion_speed = 0.001` · `local_institution_growth_modifier = 0.5`；`allow = { is_core_of = owner }`，**没有解锁键**（对所有人开放） |
| 县衙 `zhixian` | unique building（`unique_buildings.txt:5445-5497`） | `local_max_control = 0.10` · `local_nobles_estate_power = 0.25`；`country_potential` 硬绑 `international_organization:middle_kingdom` 领袖，`remove_if` 失位自动拆，**没有 allow 键** ⇒ 无法用修正键"解锁"，只能改原版（不可取） |
| 卫所 `weisuo_system` | **advance 不是建筑**（`advances\country_chi.txt:131-143`） | `global_manpower_modifier = 0.1` · `army_maintenance_efficiency = 0.1`；`potential = { has_or_had_tag = CHI  culture = { has_culture_group = culture_group:chinese_group } }` |
| `guild_hall` | building（burghers 阶层） | `local_burghers_estate_power 0.5` · `local_pop_promotion_speed_modifier −0.1` · `local_production_efficiency = small_production_efficiency_bonus`（=0.01，`script_values\default_values.txt:857`） |

结论与改法：
- **识字 / 升籍 / 思潮** 被官学+书院占满 ⇒ 法律再给 `local_max_literacy` 就是纯数值叠加，改管**人**：
  `local_migration_attraction`（0.2×4）+ `local_pop_promotion_speed_modifier`（百分比版，与 `guild_hall` 的 −0.1 反向互补）。
- **产出效率** 与 `guild_hall` 同键（0.01 vs 0.05）⇒ 换成 `local_max_rural_control`（"编户齐民"＝国家的手伸进乡村；
  注册表有定义但本体 0 处使用，锚薄需标注；与 `local_max_control` 是**两个不同的键**）。
- **卫所** 与我们的政策分层不冲突（advance 管国家人力/军费，法律管地点级出人与开垦），不必复用其键。
- 判定顺序建议：先搜本地化里该概念的中文名 → 找到 building / advance / law → 读它的 `modifier` 块 → 列出已被占用的键 → 再挑键。

**判定"某片人群"时别拿语言族去切地域组（2026-10-17 印度斯坦法返工）**：EU5 的文化可以有**两个**组，
常见的组合是"地域组 + 语系组"。直觉上写 `indian_group ∩ indo_aryan_group` 像是"北印人"，实测站不住：

`indian_group` 共 **79 个成员**，按第二个组分布：

| 第二个组 | 数量 | 例子 |
|---|---|---|
| `indo_aryan_group` | 45 | awadhi／bengali／magahi／gujarati |
| `dravidian_group` | 12 | tamil／telugu |
| `tibeto_burman_group` | 12 | bodo／garo／chakma／**newar**（尼泊尔） |
| `austroasiatic_group` | 8 | **santal／munda／kharia／ho／korku** |
| 无第二组 | 1 | `vedda`（斯里兰卡） |
| `jewish_group` | 1 | `kochini`（科钦犹太人） |

问题有两面：
- **误伤范围内的人**：santal／munda／ho 就住在贾坎德-奥里萨-孟加拉（正是"印度斯坦"地理集合内），却被语言族切掉；
  `newar`（尼泊尔主体）同样被切掉。
- **误收范围外的人**：`pashayi`／`nuristani`（阿富汗）属 `indo_aryan_group` 却不在 `indian_group`。

⇒ **规则：要圈"某片地方的人"，用游戏自己的地域组（`indian_group` / `chinese_group` / `tibetan_group` 这类）；
语系组（`indo_aryan_group` / `tibeto_burman_group`）留给"语言/文化细分"的场合**。
同一地域内不同语系的部族（南亚语系、藏缅语系）在法律里应与主体民族同样可及。
跨组交集只在**两个组的语义都是"人群身份"**时才用（汉人法 `chinese_group ∩ confucian_group` 就是这种：
一个是地域、一个是文化圈，交集才等于"汉人"）。

**政策强度基准：先跟本体同类法对一次标（2026-10-17 实做十九条法后作者反馈"buff 不够强"）**

自己一套法写完，很容易整体偏弱却不自知。判据不是"数值看起来大不大"，而是**跟本体同类目法律的同类政策比**：

| 维度 | 本体（`laws\00_monarchy.txt` / `01_common.txt` 抽样） | 常见失误 |
|---|---|---|
| 每条政策几键 | 1–3 键很常见，也有 6–7 键 | 以为"多键=强"，其实**单键本身不是问题** |
| 作用域 | 几乎全 `country_modifier` | 为"避免国家级太多"把一切都推到地点级，反而让数值被摊薄 |
| 量级 | `nobles_estate_levy_size 0.50`、`global_nobles_max_literacy 15`、`global_institution_growth_modifier 0.20` | 国级一律"减半"到 0.05 ⇒ 只有本体 1/10 |

**真正的三个病灶**（本项目 76 条政策普查 40 条单键后总结）：

1. **单键 + 冷门杠杆**：`local_sailors 0.01`、食物保存 5%、城镇特权 +1 这类，玩家在面板上看不出变化
   ⇒ 每条政策至少要有一个"立刻看得见"的杠杆（商人势力 / 识字 / 人力 / 征召规模 / 食物容量 / 贸易中心 / 人口增长）。
2. **国家级被自己的减半规则压死**：减半是为防爆表，但减到 0.05 就等于没效果。
   ⇒ 想既守配额又不弱，正确做法是**把主要收益放在地点级**（地点级可强的授权），国家级只保留 1–3 个"仪式性"键。
3. **把"阶层力量"当收益**：抬贵族/教士/齐米/农民**阶层力量**对玩家是**成本**（挤占王权与议会）。
   ⇒ 阶层力量只能作为"风味佐料"，必须配一个真实收益键，否则整条政策净负面。
   反面案例：某政策唯一效果是 `global_dhimmi_estate_power = 0.15` —— 花一条法律换一个负担。

**升档原则**：优先把冷门键抬到**本体出现过的最高档**（例 `local_sailors` 本体有 0.02 档、`local_food_capacity` 有 500 档），
而不是随手往上加数字；这样"强"仍在本体的数值体系里。

**符号方向必须查**：有的键本体**只当惩罚用过**（`local_food_production_mult` 在 1.4.1 只有 −0.1 ~ −0.3），
给正值属于"无先例且方向不明"，宁可换键（本项目改成 `local_food_capacity`）。

**工具**：mod 侧 `工具\audit_policy_strength.ps1` 输出每条政策的键数 / 作用域 / 本体正负用法比例 / 覆盖面积。
**键位去重的松绑：允许有理由的复用（2026-10-17 作者授权）**

前十五部法一直坚持"每法键位两两零重叠"，到第十六部（南岛诸族）时海洋类键已被南洋商埠法吃光
（`local_sailors` / `local_ship_build_speed` / `local_naval_supplies_output_modifier` /
`local_maritime_presence` / `local_trade_embark_disembark_efficiency` 全部占用）。
作者因此授权"允许键复用"。

**复用的三个条件**（缺一不可）：
1. **地理集合零重叠**——同一条修正在两处生效的区域不能相交；
2. **主题同源**——复用的键在两条法里讲的是同一件事（例：`local_sailors` 在两法里都是"水手"）；
3. **写进文档**——法文件头注、README、KB 三处都要说明复用了哪几个键、为什么。

**为什么要去重（原始理由，仍然有效）**：同键复用会让玩家在两部法里看到同一个修正、
也容易在数值上叠爆。所以**默认仍然去重，复用是例外而非常态**。
第十六部法只复用了 2 个键（`local_sailors` / `local_ship_build_speed`），其余 2 个（
`local_food_preservation_efficiency_modifier` / `local_population_capacity`）仍是首次使用。

**文化组当门槛的雷：先数成员，看它是否横跨已做过的区域（2026-10-17 实做高地法时踩到）**

想做"某语系一片"的法时，最容易犯的错是直接拿一个大文化组当门槛。实测：

| 文化组 | 成员数 | 危险在哪 |
|---|---|---|
| `tibeto_burman_group` | **55** | 同组既有彝/纳西/景颇/哈尼，也有**尼泊尔全族**（newar/tamang/gurung/magar/limbu/kirati/sherpa/sikkim）、**西藏全族**（khampa/amdowa/utsang/ngalop/monpa/balti/ladakh）、**阿萨姆米佐全族**（bodo/karbi/dimasa/garo/chakma/chutia/deori/koch/tiwa/rabha/tripuri/meitei/naga）⇒ 会把已完成的喜马拉雅/青藏/阿萨姆三部法全部污染 |
| `thai_group` | 16 | 除傣/壮/侗/布依/水/仡佬外，**含 `thai_culture`（泰人）与 `lao_culture`（老族）**⇒ 会拉进暹罗与澜沧主体 |
| `austroasiatic_group` | 23 | **含印度部族** ho / kharia / korku / munda / santal / sora / juang ⇒ 会让恒河法区域的国家也满足 |
| `austronesian_group` | 123 | 全印尼/菲律宾都算，等于没门槛 |
| `hmong_mien_group` | 2 | ✅ 干净，可以放心用 |

**正确做法**（高低地混合、跨三四个语系时）：
1. **生效靠 `scripted_geography`**，文化条件只作展示；
2. 文化组只挑成员数少且不跨区域的（如 `hmong_mien_group`），其余用**具名文化 OR**：
   `culture ?= culture:yi_culture`（本体先例 `advances\2_ship_unlocks.txt:294`、`culture_romagnol.txt:7`）；
3. 判断标准一句话：**这个组里有没有已经属于别的法的族群？有就不能用。**（印度支那法最后索性一个组都不用，全部具名文化 11 个）
4. **极少数大组可以用，条件苛刻**：波斯法用了 `iranian_group`（19 个成员全部是伊朗语族、与本法地理集合基本重合）。
   判据是"**组内族群与本法地理范围是否重合**"——重合就可以整组用，不重合（如 `tibeto_burman_group` 跨尼泊尔/西藏/阿萨姆/缅甸）
   就只能具名。截至第十三部法，只有 `iranian_group` 一个组通过了这条检验。

**国家级修正要配额：每法 ≤3 条，超出就下放地点级（2026-10-17 作者定，八法实测）**

`country_modifier` 是**整国生效**的，一部法里堆四五条就会变成全局膨胀——玩家感觉不到"这是某地的治理特色"，
只觉得数值莫名其妙地高。八部管理法做完后的实测配额（可作模板）：

| 法 | 国家级 | 地点级 |
|---|---|---|
| 阿萨姆 | 0 | 7 |
| 汉人 | 0 | 9 |
| 喜马拉雅 | 1 | 7 |
| 青藏 | 2 | 5 |
| 德干 | 3 | 5 |
| 西印海洋 | 3 | 3 |
| 拉其普特 | 3 | 3 |
| 印度斯坦 | 3 | 3 |

**配套原则：国家级压低的同时，地点级可以抬高**（2026-10-17 作者补充）。
理由是地点级只在**指定地理集合**内生效，天然自限；国家级一给就是全国。所以同样是"强"，地点级是"强在本地"，
国家级是"强在处处"。抬高的手法：**优先升到本体已有的更高档位**，而不是凭空加数字。本轮实例：

| 键 | 原 | 新 | 本体该档位存在 |
|---|---|---|---|
| `local_defensive` | 0.1 | **0.25** | ✅ 0.25×14（众数） |
| `local_garrison_size` | 1.0 | **2.0** | ✅ 2.0×1 |
| `local_unrest` | −0.05 | **−0.1** | ✅ −0.1×3 |
| `local_manpower_modifier` | 0.1 | **0.2** | ✅ 0.2×1 |
| `local_levy_size_modifier` | 0.1 | **0.25** | ✅ 0.25×1 |
| `local_merchant_power` | 0.1 | **0.5** | ✅ 0.5×4 |
| `local_trades_per_burgher` | 0.1 | **0.25** | ✅ 0.25×4 |
| `local_max_rgo_size_modifier` | 0.25 | **0.5** | ✅ 0.5×5（众数） |
| `local_institution_growth_modifier` | 0.1 | **0.25** | ✅ 0.25×3（众数） |
| `local_peasants_estate_power` | 0.25 | **0.5** | ✅ 0.5×2 |
| `local_pop_promotion_speed_modifier` | 0.2 | **0.25** | ✅ 0.25×2 |
| `local_monthly_literacy` | 0.1 | **0.15** | ✅ 0.15×1 |

少数没有现成更高档位的（`local_market_access` 0.01→0.05、`local_merchant_capacity_modifier` 0.25→0.5、
`local_max_rural_control` 0.05→0.1、`local_garrison_growth` 0.005→0.01）**要在文件头注释里标明"无该档先例"**，
实机验证后再定。

操作规则：
1. **能下放就下放**：把国家级键换成同概念的 `local_*` 键（例：`global_manpower_modifier` → `local_manpower`；
   `max_trade_subventions` → `local_merchant_capacity`；`global_garrison_size_modifier` → `local_garrison_growth`）。
2. **必须留国家级的**（宗教、外交、军制这类本来就是全国性的），**数值压到本体众数的一半或更低**：
   `land_morale_modifier` 本体 0.05×73 → 取 0.03；`siege_ability` 0.1×16 → 0.05；`max_tariff` 0.1×12 → 0.05；
   `global_dhimmi_estate_power` 0.25×3 → 0.15。
3. **同一条政策里别放两个"同方向"的国家级加成**（例：人力 + 征召规模 + 人力效率三条叠在一起），
   留一条最强的、其余砍掉或下放。
4. 地点级也不是无成本：它按覆盖面积放大（本项目汉地 1619 个地点），所以**地点级绝对值键**要按面积折算
   （见 pitfalls §二十三）。

**"某宗教专属政策"的写法（2026-10-17 实做印度斯坦法的齐米之制）**：给政策加**政策级 `potential`**，
只在国教属某宗教组时可选；不满足时该政策**整条不显示**（不是灰掉），玩家看到的路线数就少一条。

```txt
gangetic_dhimmi = {
    potential = {
        religion.group = religion_group:muslim      # 宗教作用域取 .group 属性
    }
    country_modifier = { … }
}
```

- 语法出处：`scripted_triggers\religion_triggers.txt:54` 就是 `religion.group = religion_group:muslim`；
  宗教组 id 见 `common\religion_groups\00_default.txt`（`muslim` 组含 sunni/shia/ibadi，行 15-27）。
  单宗教判定则直接 `religion = religion:bon`（本项目青藏法「苯佛并存」用法）。
- 配套：**该阶层本身是否专属**也要查——`dhimmi_estate`（`estates\00_default.txt:991`）是伊斯兰教专属阶层，
  所以 `estate_preferences = { dhimmi_estate }` 也只在伊斯兰国家有意义。
- ⚠️ 政策级 `potential` 会被**周期复查**：国教一旦改宗离开该宗教组，这条已生效的政策会失效
  （法律本身不受影响，因为法律的 `potential` 只查解锁变量）。设计上通常正是想要的，但要写进注释。
- 另一种更"软"的写法：用宗教**人口比例**而不是国教（`religion_group_percentage_in_country = { religion_group = religion_group:muslim  value > 0.05 }`），
  这样国土内有穆斯林人口就能走该路线，不要求国教——适合"多教共存"的国家，不适合"某某专属制度"。

**跨组文化才是"某民族"的正确定义（2026-10-17 实做汉人法）**：EU5 的文化可以**同时属于两个 culture_group**
（`cultures\` 里一个文化块写两行 `xxx_group`），所以"某民族"往往 = **两个组的交集**，不是单个组。

实例「汉人」= `culture_group:chinese_group` **且** `culture_group:confucian_group`。实查 `cultures\` 全量：

| 组合 | 文化 | 判定 |
|---|---|---|
| 两组兼具 | **62 个**（中原/江淮/吴/闽/粤/客家/晋/蜀…**以及回民 `hui_muslim_culture`**） | 汉人 |
| 只有 `chinese_group` | `qayfengi`（开封犹太人 `jewish_group, chinese_group`） | 不是 |
| 只有 `confucian_group` | `korean_culture`、4 个日本文化、`tamna_culture`、`bai_culture`、`vietnamese_culture`、`muong_culture` | 不是 |

写法（culture 作用域里两条 `has_culture_group` 即 AND；`has_culture_group` 对该文化的**任一**所属组返回真）：

```txt
culture ?= {
    has_culture_group = culture_group:chinese_group
    has_culture_group = culture_group:confucian_group
}
```

配合"国内有该组人口"时要注意：两个 `culture_group_percentage_in_country > 0` 分开写只能保证"两组都有一些"，
不是"有同时属两组的人"；要精确到人的交集需要 `any_pop` 类遍历，成本高，政策门槛上用前者即可。
另外 `儒家文化圈` 本身就是 **culture_group**（`confucian_group`），不是独立机制——查不到"文化圈"字段时先去看 `culture_groups\`。

**做"某大文化管理法"前先查本体有没有同类法律**：汉人法开工前实查发现本体已有
`imperial_examination_law`（`laws\02_country_specific.txt:970`，administrative 类目）占了
`government_size` / `country_cabinet_efficiency` / `global_pop_promotion_speed_modifier`。
⇒ 我们的法律改走**地点级**同类概念（`local_max_literacy` / `local_pop_promotion_speed_modifier`），
既不重复也不冲突。**先查再设计**，比写完发现撞车省事。

**"编旗 / 登记 / 划界"这类政策，用「容量」比用「增速」更像**：`local_max_rgo_size_modifier`（原产雇佣规模＝牧场能养多少牧人）、
`local_tribesmen_desired_pop`（岗位容量）这类键表达的是"这块地容得下多少人"，而人口增速键（`*_pop_growth`）
是"人多生得快"。前者天然只对相应产业区有意义（牧区吃得到、纯农业河谷吃不到），自带地理质感；
后者全球同质，且极易与特权/其他法律里的同族键撞车（本项目实例：青藏「牧地编旗」原用
`local_tribesmen_pop_growth = 0.15`，同时与尼泊尔特权 N1 的 `global_tribesmen_pop_growth = 0.1` 撞杠杆，
后改为 `local_max_rgo_size_modifier = 0.5`）。

**风味从机构名来，不从数值来**：基帕特／苏巴斯／穆基亚（尼泊尔）、寺院庄园／苯佛并存／牧场编旗／驿路羁縻（青藏）、
里甲／科举（汉）、检地／兵农分离（和）。机构名一换，玩家就知道是另一群人的另一套办法。

**门槛叠合是有意为之**：一个同时统治尼泊尔与西藏的国家会**同时持有**两条法（各选一条路线）。
这符合多民族帝国的史实，不要用 `has_law` 去做互斥——只要招牌键不重叠，叠加的是"广度"而不是"深度"。
**不要试图用引擎限制数量**：`unique = yes` 不是类目上限（本体 200 条法里 42 条带 `unique`，光 `socioeconomic` 就有 14 条），
政策也没有 `price` 字段先例（laws 目录 0 处 `price = price:`）。
