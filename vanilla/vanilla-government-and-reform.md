# 原版解析：政体 · 改革 · 官僚（vanilla government, reform & bureaucracy）

> **一句话**：梳理原版 5 个政体、328 项改革、25 个官僚部门与 17 条社会价值轴、50 项城镇特权、4 级国家等级的定义层与可改字段。
> **什么时候看**：加政体、改革、官僚部门或社会价值轴，改国家等级与城镇特权，或查政府影响力与正统性等译名时翻这篇。
> **体量**：385 行 · 约 18 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、总览：政体是一台"资源机"](#一总览政体是一台资源机)
- [二、五个政体（`government_types\00_default.txt`）](#二五个政体government_types00_defaulttxt)
- [三、政府改革（328 项）](#三政府改革328-项)
  - [3.1 分布：**64% 是国家专属**](#31-分布64-是国家专属)
  - [3.2 字段与出现率（花括号深度解析，328 项）](#32-字段与出现率花括号深度解析328-项)
  - [3.3 槽位来自革新（advance），是稀缺资源](#33-槽位来自革新advance是稀缺资源)
  - [3.4 改革与社会价值观：靠"焦点标签 + 衰减修正"](#34-改革与社会价值观靠焦点标签--衰减修正)
  - [3.5 拆除与迁移](#35-拆除与迁移)
- [四、改换政体（`generic_actions\government_conversions.txt`，8 个动作）](#四改换政体generic_actionsgovernment_conversionstxt8-个动作)
- [五、官僚部门（`bureaucracies\`，25 个）](#五官僚部门bureaucracies25-个)
  - [5.1 清单与三档价格](#51-清单与三档价格)
  - [5.2 三态修正：维护费滑条就是"要好处还是要坏处"](#52-三态修正维护费滑条就是要好处还是要坏处)
  - [5.3 根深蒂固（entrenchment）](#53-根深蒂固entrenchment)
  - [5.4 阶层好恶与槽位](#54-阶层好恶与槽位)
- [六、社会价值观（17 轴）](#六社会价值观17-轴)
  - [6.1 轴清单（`societal_values\00_default.txt`，499 行）](#61-轴清单societal_values00_defaulttxt499-行)
  - [6.2 结构：**两套修正包**](#62-结构两套修正包)
  - [6.3 焦点标签：34 个，**只有本地化键**](#63-焦点标签34-个只有本地化键)
  - [6.4 推动机制：30 个衰减型静态修正](#64-推动机制30-个衰减型静态修正)
- [七、城镇特权与城镇模板](#七城镇特权与城镇模板)
  - [7.1 `town_rights\`（50 项 / 9 文件）](#71-town_rights50-项--9-文件)
  - [7.2 `town_setups\00_default.txt`（117 个模板）——**不是特权**，是城镇建筑配置](#72-town_setups00_defaulttxt117-个模板不是特权是城镇建筑配置)
- [八、国家等级与霸权](#八国家等级与霸权)
  - [8.1 `country_ranks\`（4 级）](#81-country_ranks4-级)
  - [8.2 `hegemons\`（5 种）](#82-hegemons5-种)
- [九、耦合表（跨系统接口）](#九耦合表跨系统接口)
- [十、Mod 改造建议（可改 vs 硬编码）](#十mod-改造建议可改-vs-硬编码)
- [十一、中文检索键](#十一中文检索键)

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| `common\government_types\00_default.txt` | **5 个政体 / 140 行** | 无 readme，字段自明 |
| `common\government_reforms\` | **328 项改革 / 6 数据文件**（country_specific **210** · common **88** · monarchy 12 · republic 11 · theocracy 6 · steppe_horde **1**） | `readme.txt`（22 行） |
| `common\bureaucracies\` | **25 个官僚部门**（byz **11** · generic **10** · china 4） | `readme.txt`（22 行，含 `maintenance` / `entrenchment` 作用域） |
| `common\societal_values\00_default.txt` | **17 条社会价值轴 / 499 行** | 无 readme |
| `common\town_rights\` | **50 项城镇特权 / 9 文件** | readme（5 行，只有字段名） |
| `common\town_setups\00_default.txt` | **117 个城镇建筑模板 / 31 KB** | 无 readme |
| `common\country_ranks\00_default.txt` | **4 级国家等级 / 159 行** | `readme.txt` |
| `common\hegemons\` | **5 种霸权 / 5 文件** | 无 readme |
| `common\generic_actions\government_conversions.txt` | **8 个换政体动作 / 12 KB** | 无 readme |
| `common\prices\` | 政府影响力的价格表（36 处引用）+ `04_government.txt`（等级升级价） | `readme.txt` |
| `main_menu\common\static_modifiers\country.txt` | **30 个 `societal_value_push_*` 衰减修正** | — |
| `main_menu\common\modifier_type_definitions\` | 修正键注册表（2393 个修正类型） | — |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `government_type` | 政体 | 5 种：`monarchy` 君主国 / `republic` 共和国 / `theocracy` 神权国 / `steppe_horde` 草原游牧部落 / `tribe` 部落 |
| **`government_power`** | **政府影响力** | 概念词条原文：「这是衡量 [government] 运转情况的通用术语。**有数种类型**：正统性、共和传统、奉献度、游牧团结、部落凝聚力」——**同一个数字，五个政体各有各的名字** |
| `legitimacy` | 正统性 | 君主国的政府影响力 |
| `republican_tradition` | 共和传统 | 共和国的 |
| `devotion` | **奉献度** | 神权国的（不是"虔诚"） |
| `horde_unity` | 游牧团结 | 草原汗国的 |
| `tribal_cohesion` | 部落凝聚力 | 部落的 |
| `government_reform` | **政府改革** | 328 项；`major` 互斥 |
| `government_reform_slots` | 改革槽位 | **由革新（advance）授予** |
| `bureaucracy` | **官僚部门** | 25 个；「随时间发展、汲取资金、提供**动态强度的增益**」 |
| `bureaucracy_maintenance` | 官僚部门维护花费 | 0..1 滑条，决定拿多少好处 / 多少坏处 |
| `entrenchment` | 根深蒂固（固化） | 100 年一阶段 |
| `societal_value` | **社会价值观** | 17 条轴 |
| `*_focus` | （各轴两侧的焦点名） | 34 个焦点 + 1 杂项，**纯本地化键** |
| `town_rights` | **城镇特权** | 50 项，授予**地点** |
| `town_setup` | （城镇建筑模板） | 117 个，决定城镇建成时带哪些建筑 |
| `country_rank` | 国家等级 | 帝国 / 王国 / 公国 / 伯国 |
| `hegemon` | 霸权 | 经济 / 海军 / 军事 / 外交 / 文化 |

## 一、总览：政体是一台"资源机"

这套系统的骨架只有一句话：**政体决定你用哪种"政府影响力"，而几乎所有的政治操作都在花它。**

```
government_types（5）
   ├── government_power = legitimacy / republican_tradition / devotion / horde_unity / tribal_cohesion
   ├── heir_selection 白名单（3–23 种）
   ├── default_character_estate（角色默认阶层）
   └── modifier（政体自带修正）
          ↓ 花政府影响力
   government_reforms（328，槽位来自革新）· bureaucracies（25，另有维护费）
   · town_rights（50）· estate_privileges（授予 5）· change_heir_selection（50）
          ↓ 输出
   societal_values（17 轴）→ 30 个 societal_value_push_* 衰减修正
```

价格表实测（`common\prices\`，货币是**政府影响力**）：

| 操作 | 价格 |
|---|---|
| 改继承法 `change_heir_selection` | 政府影响力 **50** + 稳定 50 |
| 实施官僚部门 `implement_bureaucracy_price` | 政府影响力 **20** |
| 授予阶层特权 `grant_privilege` | 政府影响力 **5** |
| 授予城镇特权 `grant_town_rights` | 政府影响力 **5** |
| 撤销阶层特权 `revoke_privilege` | 稳定 **400** + 正义 10（**不用影响力，用稳定**） |
| 撤销城镇特权 `revoke_town_rights` | 稳定 **10** |
| 拆除政府改革 `remove_government_reform` | 稳定 **20** + 正义 10 |
| 重塑官僚部门 `reshape_bureaucracy` | 缩放金 0.7 + 稳定 50 |
| 换政体 `change_government_type_price` | 稳定 **50** + 正统性 25 |
| 国家等级升级 `rank_{duchy|kingdom|empire}_upgrade` | 金 100 / 250 / **1000** + 缩放金 0.5 / 1.0 / 2.0 |

## 二、五个政体（`government_types\00_default.txt`）

| 政体 | 政府影响力 | 角色默认阶层 | 可用继承法 | 特色修正（节选） |
|---|---|---|---|---|
| `monarchy` 君主国 | 正统性 | 贵族 | **23** 种 | `care_about_producing_heirs = yes`；`revolutionary_country_antagonism = 20`；`use_regnal_number = yes`（统治者加序号） |
| `republic` 共和国 | 共和传统 | 市民 | 16 种 | `court_language_is_market_language_importance_modifier = 3`（市场语言受偏爱） |
| `theocracy` 神权国 | 奉献度 | 教士 | 9 种 | `force_convert_created_subjects = yes`；礼仪语言重要性 **+10** |
| `steppe_horde` 草原汗国 | 游牧团结 | 贵族 | 3 种 | `horde_unity_hit_at_ruler_death = −50`（统治者一死就掉团结）、无 CB 宣战 −50%、劫掠 +33%、战争分效率 +25%、`generate_consorts = yes` |
| `tribe` 部落 | 部落凝聚力 | 贵族 | 5 种 | 贵族粮食消耗 −25%、外交维持效率 ×1.5、农村聚落降级成本 −50% |

三个要点：

1. **继承法白名单是政体的核心差异**：君主国 23 种继承法可选（含各种长子/选举/瓜分制），部落只有 3 种（`tribal_oldest_male` / `ritual_selection` / `matrilineal_non_exclusive`）。继承法本体见 `vanilla\vanilla-character-dynasty-cabinet.md`。
2. **`default_character_estate` 决定"每个角色天然属于哪个阶层"**——君主国角色默认贵族、共和国默认市民、神权国默认教士，直接决定阶层力量分布。
3. 政体自带修正是**无条件常驻**的（不是改革，不能拆）。

## 三、政府改革（328 项）

### 3.1 分布：**64% 是国家专属**

| 文件 | 项数 | 说明 |
|---|---|---|
| `country_specific.txt` | **210** | 各国专属（中华、拜占庭、奥斯曼……） |
| `common.txt` | 88 | 通用 |
| `monarchy.txt` / `republic.txt` / `theocracy.txt` | 12 / 11 / 6 | 政体专属 |
| `steppe_horde.txt` | 1 | 草原汗国专属 |

### 3.2 字段与出现率（花括号深度解析，328 项）

| 字段 | 出现 | 率 | 含义 |
|---|---|---|---|
| `country_modifier` | 328 | **100%** | 永久国家修正（可按实施进度缩放） |
| `years` | 300 | **91%** | 实施时长：**2 年 262 项**、1 年 29、0 年 4、3–4 年各 2、10 年 1 |
| `potential` | 239 | 73% | 是否出现（root = country） |
| `age` | 160 | 49% | 时代门控 |
| `unique` | 129 | 39% | 额外 UI 说明 |
| `allow` | 98 | 30% | 是否可以开始实施 |
| `government` | 82 | 25% | 政体门控 |
| `locked` | 75 | 23% | 锁死（不可交互） |
| **`major`** | **58** | 18% | **互斥改革：每国只能有一项** |
| `societal_values` | 51 | 16% | 需要的**社会价值焦点**（如 `humanist_focus`） |
| `content_priority` | 40 | 12% | 内容排序（原版值 300/400，DLC/国别内容用） |
| `on_activate` / `on_deactivate` | 13 / 8 | 4% / 2% | 实施开始 / 拆除时触发 |
| `location_modifier` | 6 | 2% | 作用到地点 |
| `months` | 3 | 1% | 更短的实施单位（readme 还支持 weeks/days） |
| `icon` | 3 | 1% | 自定义图标 |
| `block_for_rebel` | 3 | 1% | 叛军不可用 |
| `male_regnal_names` | 1 | 0% | 采用该改革后统治者的男名库（女名 readme 有但原版 0 用） |

⚠️ **改革本体在价格表里没有价格**——`prices\` 里只有 `remove_government_reform`。入手的门槛是**槽位 + 社会价值观 + 时代/政体 + major 互斥**，不是钱。

### 3.3 槽位来自革新（advance），是稀缺资源

`advances\` 里 **14 条**带 `government_reform_slots = 1`：**6 个时代各 1 条**（traditions / renaissance / reformation / absolutism / revolutions / discovery）+ **8 条国别/文化专属**（中国、伊尔 Khan、塞尔维亚、瑞典、托斯卡纳、希腊文化组、罗马尼亚文化组、那不勒斯文化）。也就是说改革槽位是**科技树上的战略选择**，不是白送的。

### 3.4 改革与社会价值观：靠"焦点标签 + 衰减修正"

- 改革用 `societal_values = { humanist_focus }` 声明它要的**焦点**（清单见 §六）
- 常量 `SOCIAL_VALUE_REQUIREMENT_FOR_REFORM = **50**`：你得先站到那一侧 50 以上
- 界面预测：`GOVERNMENT_REFORM_SOCIETAL_VALUE_REQUIREMENT_FORECAST_IN_MONTHS = 12` / `..._CUTOFF_IN_MONTHS = 24`（预告"还要多久够格"）
- 推动社会的方式是挂静态修正（见 §六）

### 3.5 拆除与迁移

`remove_government_reform`（稳定 20 + 正义 10）可拆；换继承法 50 影响力 + 50 稳定；换政体见 §四。

## 四、改换政体（`generic_actions\government_conversions.txt`，8 个动作）

```
steppe_horde_to_monarchy   monarchy_to_republic     monarchy_to_theocracy
theocracy_to_monarchy      theocracy_to_republic    republic_to_monarchy
gov_action_republic_to_theocracy                    tribal_to_monarchy
```

实测门槛（以 `steppe_horde_to_monarchy` 为例，其余同构）：

| 项 | 值 / 条件 |
|---|---|
| 价格 | `price:change_government_type_price` = **稳定 50 + 正统性 25** |
| 冷却 | `cooldown = { type = transition_government  years = 20 }`——**20 年一次** |
| 首都 | 必须是**城市**且**主流文化** |
| 战争 | `at_war = no` |
| 控制 | `average_control_in_home_region > 0.25` |
| 附属国 | **全部**附属国 `subject_loyalty > 50` |
| 其他 | 不在联合统治中、没有特定国家修正（如 `reformation_of_the_horde`） |
| AI | `ai_tick = monthly` × `ai_tick_frequency = 12`；`automation_tick = never`（**不参与自动化**） |

## 五、官僚部门（`bureaucracies\`，25 个）

### 5.1 清单与三档价格

| 文件 | 数量 | 面向 |
|---|---|---|
| `byz.txt` | 11 | 拜占庭专属（含 `implement_bureaucracy_price_cost_modifier` 之类专属修正） |
| `generic.txt` | 10 | 1600–1836 通用，注释自述「按主题分：ADM 4 / DIP 3 / MIL 3」 |
| `china.txt` | 4 | 中华（科举等） |

价格（`prices\05_byz.txt`）：

```
implement_bureaucracy_price = { government_power = 20 }   # 实施
maintain_bureaucracy_price  = { gold = 5 }                # 每月维护
remove_bureaucracy_price    = { stability = 50 }          # 移除
```

> ⚠️ **真坑**：这三个 id 被 `generic.txt` / `china.txt` / `byz.txt` 里**所有**官僚部门引用，但定义**只存在于 `prices\05_byz.txt`**——文件名叫 byz，实为这三个通用价格的定义处。覆盖/删掉它，全世界官僚部门的价格就失去定义。

### 5.2 三态修正：维护费滑条就是"要好处还是要坏处"

字段出现率 100% 的 8 个里，三个修正块的写法揭示了设计：

| 块 | 缩放 | 原版实例（税务委员会） |
|---|---|---|
| `neutral_modifier` | 恒定 | `government_size = 1` |
| `positive_modifier` | `scale = { value = scope:maintenance }` | `tax_income_efficiency = 0.1`（维护费给满才拿满） |
| `negative_modifier` | `scale = { value = 1  subtract = scope:maintenance }` | `estate_enrichment = 0.1`（省钱就拿这个坏处） |

另有 `maintenance_price_modifier`（如 `country_economical_base × 0.004`，**维护费随国家经济规模上涨**）、`potential`（多数要求 `has_embraced_institution = institution:confessionalism`）、`on_maintenance_changed`（改维护档次日触发，带 `scope:old_maintenance` / `scope:new_maintenance`）。

### 5.3 根深蒂固（entrenchment）

- 常量：`BUREAUCRACY_ENTRENCHMENT_YEARS_PER_PHASE = **100**`、`BUREAUCRACY_ENTRENCHMENT_QUOTE_PER_PHASE = **50**`
- readme 声明三个修正块都可用 `scope:entrenchment` 缩放；另有行动 `increase_bureaucratic_entrenchment`（+`global_bureaucracy_entrenchment_speed_modifier = 0.1`）、效果 `change_entrenchment`，以及议会行动 `challenge_bureaucratic_entrenchment`
- 相关触发器：`bureaucracy_maintenance`、`has_bureaucracy_of_type`、`bureaucracy_liked_by_estate(_type)`、`bureaucracy_disliked_by_estate(_type)`、`bureaucracy_type_liked_by_estate(_type)` 等 **10 个**

### 5.4 阶层好恶与槽位

- `estates_that_like` / `estates_that_dislike` 各 100% 出现（如税务委员会：市民喜欢、贵族讨厌），配常量 `ESTATE_SATISFACTION_BUREAUCRACY = 0.01`
- **槽位同样来自革新**：`global_max_bureaucracy_slots` 共 5 条（宗教改革 +1、革命 +1、拜占庭 **+2**、中华 +1、特拉比松 +1 → 合计 +6）
- 槽位触发器：`num_bureaucracies`、`num_open_bureaucracy_slots`、`max_bureaucracy_slots`、`allowed_bureaucracies`
- AI：`AI_PERFORMANCE_BUREAUCRACY_MONTHS_BETWEEN_UPDATES = 24`（24 个月才重估一次），授予/移除阈值均为 5

## 六、社会价值观（17 轴）

### 6.1 轴清单（`societal_values\00_default.txt`，499 行）

```
centralization_vs_decentralization      集权 / 分权
traditionalist_vs_innovative            传统 / 创新
spiritualist_vs_humanist                虔诚 / 人文
aristocracy_vs_plutocracy               贵族 / 财阀
serfdom_vs_free_subjects                农奴 / 自由民
mercantilism_vs_free_trade              重商 / 自由贸易
belligerent_vs_conciliatory             好战 / 和解
quality_vs_quantity                     质量 / 数量
offensive_vs_defensive                  进攻 / 防御
land_vs_naval                           陆权 / 海权
capital_economy_vs_traditional_economy  资本 / 传统经济
individualism_vs_communalism            个人 / 集体
outward_vs_inward                       外向 / 内向
sinicized_vs_unsinicized                汉化 / 未汉化
absolutism_vs_liberalism                专制 / 自由主义
mysticism_vs_jurisprudence              神秘主义 / 法理
latinization_vs_hellenization           拉丁化 / 希腊化
```

### 6.2 结构：**两套修正包**

- `left_modifier` / `right_modifier` **各 100% 出现**——每条轴的两侧各挂一包修正（例如集权侧：`global_crown_estate_power +0.5`、`subject_loyalty −20`、`annexation_speed_modifier +0.33`；分权侧：`subject_loyalty +30`、`global_estate_target_satisfaction` 提升、`control_importance_modifier −0.1`）
- 可选字段：`age` 18%、`allow` 18%、`opinion_importance_multiplier` 18%（影响 AI 对他国社会价值的观感权重）、`content_priority` 6%
- 无 readme；轴的数值范围与移动速率由 `script_values` 提供（原版移动量级如 `societal_value_minor_monthly_move` / `societal_value_huge_monthly_move`）

### 6.3 焦点标签：34 个，**只有本地化键**

`government_l_<lang>.yml` 里有 **35 个 `*_focus` 键**（34 = 17 轴 × 2 侧，+1 个杂项 `cab_naval_focus`）：`centralization_focus`、`decentralization_focus`、`spiritualist_focus`、`humanist_focus`、`plutocracy_focus`……

⚠️ **这些 focus 在 `common\` 与 `main_menu\common\` 里都没有定义块**——它们是**纯本地化标识**，被 `government_reforms`（51 处）、`laws`、`cabinet_actions`、`missions`、`advances` 当作"社会价值取向"引用。查"某个 focus 存不存在"必须搜 yml，搜 txt 会误判为不存在。

### 6.4 推动机制：30 个衰减型静态修正

`main_menu\common\static_modifiers\country.txt` 有 **30 个 `societal_value_push_<side>`**：

```
societal_value_push_humanist = {
    game_data = { category = country  decaying = yes }
    monthly_towards_humanist = societal_value_huge_monthly_move
}
```

`decaying = yes` + 每月朝某一侧推进——**改革、法律、事件推动社会价值观都是挂这个修正**，而不是直接改数值。相关本地化键 `STATIC_MODIFIER_NAME_societal_value_push_humanist`。

## 七、城镇特权与城镇模板

### 7.1 `town_rights\`（50 项 / 9 文件）

| 字段 | 出现 | 率 |
|---|---|---|
| `color` / `location_modifier` | 50 | 100% |
| `allow` | 37 | 74%（常用 `scope:target = { is_port = yes }`） |
| **`kept_at_conquest`** | 32 | 64%——**被征服后是否保留**（这是它最"地方性"的地方） |
| `potential` | 29 | 58% |
| `country_modifier` | 15 | 30% |

- 作用对象是**地点**：如 `staple_port`（`local_marketplace_building_levels +5`、`harbor_suitability +0.2`、`local_burghers_estate_power +0.5`）、`granary_town`（`local_food_capacity_modifier +0.5`）、`market_charter`
- 成本：授予 **5 政府影响力** / 撤销 **10 稳定**；行动文件 `generic_actions\revoke_town_rights.txt`
- 触发器 `has_town_rights` / `has_any_town_rights` / `has_max_town_rights` / `has_different_town_rights_than`；效果 `grant_town_rights` / `revoke_town_rights_of_type`

### 7.2 `town_setups\00_default.txt`（117 个模板）——**不是特权**，是城镇建筑配置

```
scandinavian_town = { brewery = 1  temple = 1  tools_guild = 1  weapon_guild = 1
                      mason = 1  pottery_guild = 1  tannery = 1
                      naval_supplies_guild = 1  marketplace = 1 }
```

- 字段就是**建筑名 = 等级**；原版出现率最高的是 `marketplace` 86%、`tools_guild` 68%、`mason` 67%、`weapon_guild` 66%、`pottery_guild` 66%、`temple` 65%
- **32% 的模板用 `copy_from` 继承另一个模板再做加法**（文件注释：「Unique city setups use copy_from + additive deltas」）——这是复用写法，mod 里照抄最省事

## 八、国家等级与霸权

### 8.1 `country_ranks\`（4 级）

| 等级 | level | 外交官 | 外交范围 | 宿敌位 | 艺术家 | 要塞上限 | `government_size` | 文化容量 | AI 征服欲望 | 语言力量 |
|---|---|---|---|---|---|---|---|---|---|---|
| `rank_empire` 帝国 | 4 | 2 | 1000 | 4 | 2 | 2 | **+1** | 2 | **×4** | 2 |
| `rank_kingdom` 王国 | 3 | 1 | 500 | 3 | 1 | 1 | — | 1 | ×2 | 1 |
| `rank_duchy` 公国 | 2 | — | 200 | 2 | — | — | — | 0.5 | — | 0.25 |
| `rank_county` 伯国 | 1 | — | — | 1 | — | — | — | — | — | 0.05 |

- 升级价：`gold` **1000 / 250 / 100** + `scaled_gold` **2.0 / 1.0 / 0.5**（`prices\04_government.txt`）；升级条件走触发器 `can_upgrade_country_rank`
- 其他字段：`allowed_alliance` / `allowed_guarantee` / `allowed_support_rebels`（伯爵只有同盟权）、`mercenary_range_modifier` 1/0.5/0.25/—、`num_local_governors`、`diplomatic_capacity` 3/2/1/—、`ai_level` = level、`character_ai_cooldown` / `diplomacy_ai_cooldown`（公国/伯国 2/3）、`victory_card = yes`、`language_power_scale`
- 神圣罗马帝国特例：帝国的 `allow` 里检查 `international_organization:hre` 与 `block_from_change_to_empire_rank_catholic` 修正——**在 HRE 里升帝国要过教宗那道门**

### 8.2 `hegemons\`（5 种）

`economic_hegemon` 经济霸权 / `naval_hegemon` / `military_hegemon` / `diplomatic_hegemon` / `cultural_hegemon`，每种 = **`gain` + `lose` 两段触发器**：

```
economic_hegemon = {
    gain = { is_great_power = yes  is_subject = no
             NOT = { any_other_great_power = { monthly_income_trade_and_tax > root.monthly_income_trade_and_tax } } }
    lose = { OR = { any_other_great_power = { monthly_income_trade_and_tax > { value = root.… multiply = 1.1 } … }  is_subject = yes } }
}
```

- **进入与退出用不同阈值**（退出要别人超过你 **10%** 才算）——滞回设计
- 常量：`HEGEMONY_LOST_MONTHS = **120**`（断档 10 年才失去）、`HEGEMONY_GRACE_PERIOD = 12`（被挑战后 1 年宽限）、`HEGEMONY_ACTIVE_UNTIL_REPLACED = yes`（要等对手接班才易主）、`HEGEMONY_MONTHLY_PROGRESS = 1`

## 九、耦合表（跨系统接口）

| 系统 | 接口 |
|---|---|
| 角色 · 王朝（角色篇） | `government_types` 的 `heir_selection` 白名单限制可选继承法；`default_character_estate` 决定角色阶层；`government_size` 决定内阁席位 |
| 法律 · 阶层 · 议会 | 阶层特权花政府影响力（5）；议会通过议案间接改法律（`ask_for_law_changes`）；官僚部门的阶层好恶改变阶层满意度；`challenge_bureaucratic_entrenchment` 是议会行动 |
| 生产 · 建筑 | 城镇模板 `town_setups` 决定城镇的建筑组合；城镇特权的 `location_modifier` 常直接改建筑等级上限 |
| 科技 · 时代 | **改革槽位与官僚槽位都来自革新**；`age` 门控改革 |
| 贸易 · 市场 | 共和国的市场语言偏好修正；`stake_port` / `market_charter` 等城镇特权改市场与贸易 |
| AI（AI 篇） | `ai_government_power_target_modifier`、`AI_PERFORMANCE_REFORMS_MONTHS_BETWEEN_UPDATES = 24`、`AI_GRANT_/REMOVE_BUREAUCRACY_THRESHOLD = 5`、`government_power` 的 AI 目标下限 50 |
| 灾难 · 局势 | 政体转换与国家修正挂钩（如 `reformation_of_the_horde` 阻止转政体） |

## 十、Mod 改造建议（可改 vs 硬编码）

| 想改的东西 | 正规做法 |
|---|---|
| 加一项改革 | `common\government_reforms\<你的文件>.txt`，给 `country_modifier` + `years`，注意 `age` / `government` / `major` 的排他性 |
| 让改革更早可用 | 改 `potential` / `allow` / `age`；或在 `advances` 里加 `government_reform_slots = 1` 增加槽位 |
| 让某国能改社会价值 | 写 30 个 `societal_value_push_*` 那样的**衰减修正**（`decaying = yes` + `monthly_towards_X`），不要直接改数值 |
| 加官僚部门 | 照 `generic.txt` 的三态写法；**价格 id 必须已有定义**（注意 `05_byz.txt` 那个坑） |
| 加城镇特权 | `location_modifier` + `allow`（`scope:target`）+ `kept_at_conquest`；成本走 `grant_town_rights` 价格 |
| 加城镇模板 | 用 `copy_from` 继承已有模板再加建筑差量 |
| 加/改政体 | `government_types\00_default.txt` 加条目，但**新政府影响力类型是引擎侧的**——只能复用现有五种之一 |
| 加霸权 | `hegemons\<n>_<领域>_hegemon.txt`，写 `gain` / `lose` 两段（建议保持滞回） |
| 改政体转换 | `generic_actions\government_conversions.txt` 加动作 + `prices` 里的价格 + `cooldown` |

**硬编码边界**：政府影响力五种类型的**具体数值管线**（月度增减、上限、UI 图标）、改革的界面流程与槽位判定、社会价值的移动速率合成、霸权的排名计算、政体转换后的连带处理（继承法重置、阶层重算）都在引擎里。数据侧能改的是**门槛、价格、修正与触发器**。

## 十一、中文检索键

**概念**（`main_menu\localization\simp_chinese\game_concepts_l_simp_chinese.yml`）：`game_concept_government_reform` **政府改革**（:878，desc :882「勾勒出了 [country] 的治理形态……**独一无二**……强大的权衡」）、**`game_concept_government_power` 政府影响力**（:933，desc 原文列出五种类型）、`game_concept_legitimacy` 正统性（:918）、`game_concept_republican_tradition` 共和传统（:921）、`game_concept_devotion` **奉献度**（:924）、`game_concept_horde_unity` 游牧团结（:927）、`game_concept_tribal_cohesion` 部落凝聚力（:930）、`game_concept_societal_value(s)` 社会价值观（:1518）、`game_concept_bureaucracy/bureaucracies` **官僚部门**（:2077）、`game_concept_bureaucracy_maintenance` **官僚部门维护花费**（:2080）、`game_concept_bureaucracy_type` 官僚部门类型（:2086）、`game_concept_town_rights` **城镇特权**（:2058）、`game_concept_country_rank(s)` 国家等级（:1179）、`game_concept_hegemon/hegemony` 霸权（:1171）。

**名称**（`government_names_l_simp_chinese.yml`）：`monarchy` 君主国（:980）、`tribe` 部落（:981）、`steppe_horde` 草原游牧部落（:982）、`republic` 共和国（:983）、`theocracy` 神权国（:984）；`rank_empire` 帝国（:47）、`rank_kingdom` 王国（:237）、`rank_duchy` 公国（:515）、`rank_county` 伯国（:705）。霸权名在 `diplomacy_l_simp_chinese.yml`（`economic_hegemon` 经济霸权 :91 起）。焦点键 35 个在 `government_l_<lang>.yml`。

**界面**（`in_game\gui\`）：**`government_lateralview.gui`（153 KB，主面板）**、**`shared\government_tooltips.gui`（72 KB）**、**`bureaucracies_lateralview.gui`（30 KB）**、**`societal_values_lateralview.gui`（28 KB）**、`select_interaction_cards\government.gui`（17 KB）、**`government_reform_per_age.gui`（15 KB，按时代分组）**、`government_reform.gui`（13 KB）、`town_rights.gui`（14 KB）、`select_societal_value.gui`、`attribute_columns\bureaucracy_type.gui`。

**关联字段档**：`fields\common-government_reforms.md`、`common-bureaucracies.md`、`common-societal_values.md`、`common-town_rights.md`、`common-country_ranks.md`；常量索引见 `guides\defines.md`，AI 侧见 `vanilla\vanilla-ai.md`。
