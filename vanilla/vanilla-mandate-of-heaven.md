# 原版解析：天命（vanilla mandate of heaven）

版本基准：EU5 1.3.x。**本篇只讲天命系统本身**（宣称天命 CB、夺取天命和约、天命丧失危机、入会条件、科举与专属内容）；**天朝 IO 的结构已拆到 `vanilla\vanilla-international-organizations.md`**（作为 IO 的案例深挖 §六），两篇合读。核心文件：

| 文件 | 内容 |
|---|---|
| `common\international_organizations\middle_kingdom.txt`（369 行） | 天朝 IO 定义 |
| `common\casus_belli\unify_china.txt`（65 行） | 战争理由：宣称天命 / 中华统一 |
| `common\peace_treaties\take_mandate_of_heaven.txt`（64 行） | 和平条约：夺取天命 |
| `common\laws\20_middle_kingdom.txt`（2252 行） | 天朝专属法律（消耗天朝威仪） |
| `common\generic_actions\middle_kingdom.txt` | 皇帝专属行动（科举/改革官僚制/强化部门/颁布诏令/免除朝贡） |
| `common\disasters\crisis_of_the_chinese_dynasty.txt` | 中华王朝危机灾难 |
| `common\international_organization_payments\middle_kingdom_tribute.txt` | 朝贡体系 |
| `common\international_organization_special_statuses\` | `celestial_governor`（天朝总督） |
| `common\cultures\east_asia.txt` | 相关文化组 |
| `common\societal_values\00_default.txt:389-426` | 汉化轴 |

## 一、术语对照（易混）

| 中文 | 内部 ID | 类型 |
|---|---|---|
| **宣称天命** | `cb_claim_mandate_of_heaven` | 战争理由（CB） |
| **夺取天命** | `peace_mandate_of_heaven` | 和平条约 |
| 天朝 | `middle_kingdom` | 国际组织（IO） |
| 中国皇帝 | `EMPEROR_CHINA` / `EMPEROR_CHINA_MALE` | 头衔 |
| 天朝威仪 | `celestial_authority`（0–100） | IO 变量 |
| 天朝总督 | `celestial_governor` | IO 特殊地位（最多 4） |
| 朝贡体系 | `middle_kingdom_tribute` | IO payments |

## 二、天朝 IO 的结构 → 见 IO 篇 §六

**天朝 IO（`middle_kingdom.txt`）已作为"IO 能力上限"的案例深挖写进 `vanilla\vanilla-international-organizations.md` §六**（皇帝作为角色领袖、`leader` 块在摄政期显示继承人、成员被推向"汉化"轴、非领袖禁止升帝国、领袖修正含**文化容量 +50**、与皇帝开战自动退会、`monthly_effect` 自动拉入新附属国、`auto_disband_trigger = always = no`…）。本篇只保留**天命系统自身的数值载体**：

### 天威变量（`celestial_authority`，天命系统的核心数值）

```
celestial_authority:        min 0 / max 100 / start 70
  monthly_change:
    - 基础衰减 −0.1
    - 内部战争：互战成员每个 −0.01/月
    - 朝贡国数量：每国 +0.005 × (opinion(皇帝)/200) × (country_economical_base/1000)
    - 皇帝 modifier:monthly_celestial_authority 全额计入
num_of_celestial_governors: min 0 / max 4 / start 1（由天朝总督地位驱动）
```

> 危机阈值（`celestial_authority <= 20`）、朝贡付款对天威的 ±0.2 影响、总督的 ±0.01 月增，都作用在这个变量上——详见 §六 与 IO 篇 §6.4–6.6。

## 三、加入天朝的三条路径

### 路径 1：外交邀请 / 主动申请
IO readme 权威字段：`invite_visible_trigger`（能否看到邀请行动）、`invite_enabled_trigger`、`join_visible_trigger`、`join_enabled_trigger`（后三者未定义 → **默认 yes**）。天朝把 `invite_visible_trigger` 设为 `always = yes`，故任何国家都可被邀请；最终都过 `can_join_trigger`。

### 路径 2：成为皇帝附庸 → 自动入会
`monthly_effect` 每月扫描 `leader_country.every_subject`，非成员者自动 `add_country_to_international_organization`。所以成为天朝的 **tributary（朝贡国）** 即等于入会 —— 渠道：外交 `OFFER_tributary` / `REQUEST_tributary`，或 CB `cb_make_tributary`（`common\casus_belli\make_tributary_cb.txt`）。

### 路径 3：事件与和平条约
- 和平条约"夺取天命"（战胜国先入会再成为领袖）
- 郑和宝船远征事件 `flavor_chi.2009`（`events\DHE\flavor_chi_treasure_expedition.txt:1318-1325`）：把 `nations_to_make_tributary` 列表国家一次性拉入 IO

### 硬条件：`can_join_trigger`（第 81–110 行）
1. `at_war = no` —— **必须和平**
2. `OR = { is_subject_of = scope:recipient.leader_country  is_subject = no }` —— **要么是皇帝的附庸，要么不是任何人的附庸**
3. `NOT = { country_rank = country_rank:rank_empire }` —— **不能是帝国等级**
4. `trigger_if = { 若是 japanese_shogunate 成员 → 必须是其领袖 }` —— 日本大名不能单独入贡，只有幕府将军可以
5. `OR = { 首都位于亚洲 ／ 文化组 chinese_group 或 confucian_group ／ 宗教组 folk_asian_group 或 buddhist }`

### AI 意愿
- `ai_desire_to_join`（274–329）：好感 ×0.2；好感 < −20 → −1000；皇帝外交声誉 ×2；超外交距离 → −1000；开战中 → −1000；距中国边界距离 ×−0.05
- `ai_desire_to_allow_new_member`（331–368）：+20 基础；皇帝破产 → −1000；当前天朝威仪 ×0.05；申请者经济规模 ×−1

## 四、战争理由"宣称天命"（`casus_belli\unify_china.txt`）

```txt
cb_chinese_unification = {          # 中华统一（另一条，条件：你是天朝领袖 + 目标占有天朝领土）
    speed = 8
    war_goal_type = superiority_chinese_unification
}

cb_claim_mandate_of_heaven = {
    speed = 10                       # 每月 +10% 创建进度 → 10 个月成型
    create_visible = { ... }         # 条件同下
    declare_enabled = { ... }        # 与 create_visible 完全相同
    war_goal_type = take_capital     # 战争目标：夺取首都
}
```

**条件**（`create_visible` = `declare_enabled`）：
1. `exists = international_organization:middle_kingdom`
2. `scope:target = { is_leader_of_international_organization = international_organization:middle_kingdom }` —— **只能打现任皇帝**
3. `OR = { 文化组 ∈ { chinese_group, confucian_group, jurchen_group, mongolian_group } ／ AND = { has_societal_value = sociatized_vs_unsinicized 且值 < 75 } }`

要点：
- **不要求是 IO 成员**（非成员也能宣称天命，和约会自动先拉入再扶为领袖）
- `speed = 10` 是官方 readme 的"每月创建进度百分比"（`100 = it's created`），即 CB 需要 **10 个月酝酿**
- 文化组实查（`common\cultures\east_asia.txt`）：
  - `chinese_group`：汉地各文化（yan/jilu/zhongyuan/liang/qin/wu/chu/shu/jin/minnan/yuehai…）+ hui_muslim
  - `confucian_group`：上述汉文化 + **korean + 日本（saigoku/tougoku/kyushu/touhoku）+ tamna + bai + hlai** ← 日、朝满足条件
  - `jurchen_group`：jurchen / haixi / hurga / udege
  - `mongolian_group`：mongolian / sarta / monguor / sibe / bonan / yugur / oirat / daur / buryat / khamag / kharchin / tumed
- 汉化轴（`societal_values\00_default.txt:389-426`）：左 = Sinicized（立法效率 +0.25、研究 +0.1、商人容量 +0.2、朝贡收入 +0.25、文化传统 −0.5）；右 = Unsinicized（稳定度成本效率 +0.66、威望衰减 −0.002、文化传统 +0.5、商人容量 −0.2）。轴解锁：文化组非 `chinese_group` 且（是 CHI 附庸 ／ 首都位于东亚或东南亚 ／ 是中原 IO 成员）

## 五、和平条约"夺取天命"（`peace_treaties\take_mandate_of_heaven.txt`）

| 字段 | 内容 |
|---|---|
| `cost` | `5 + (战败国总人口 ÷ 战胜国总人口) × 5` |
| `potential` | 战争 CB 为 `cb_claim_mandate_of_heaven`；且 `international_organization:middle_kingdom.leader_country = scope:loser` |
| `allow` | 空（无额外限制） |
| `effect` | 战胜国若不在 IO 中则先加入 → `set_leader_country = scope:winner`（登基） |
| `ai_desire` | 宿敌 +10；己方威望低于对方时按威望差加成 |

## 六、天命丧失：中华王朝危机（`disasters\crisis_of_the_chinese_dynasty.txt`）

```
can_start:
  current_age_or_later = { age = age_3_discovery }
  is_leader_of_international_organization = international_organization:middle_kingdom
  has_any_active_disaster = no
  NOT = { is_situation_active = situation:red_turban_rebellions }
  international_organization:middle_kingdom = { var:celestial_authority <= 20 }
  OR = { stability < 0  government_power < 50 }

modifier（灾难期间）:
  monthly_celestial_authority = -0.05
  monthly_war_exhaustion = 0.1
  monthly_rebel_growth = 0.01
  monthly_inflation = 0.001

on_end:
  清除 reformed_the_cabinet_variable / completed_tax_revision_variable /
       cmd_restored_stability_variable / cmd_strengthened_crown_variable
  若已不是天朝领袖 → 事件 crisis_of_the_chinese_dynasty.2（天命丢失）
  否则 → 事件 crisis_of_the_chinese_dynasty.3
```

相关事件链：`events\disaster\chinese_crisis_events.txt`（11 处引用）。

## 七、朝贡体系与总督（机制细节见 IO 篇）

**付款与地位的结构**（`middle_kingdom_tribute` 的方向、维护滑条对天威的 ±0.2、`celestial_governor` 的 `max_countries = 4` 与五种政府影响力 +0.1）**已写入 `vanilla\vanilla-international-organizations.md` §6.5–6.6**。本篇只记与天命直接相关的两个国家交互成本：

| 交互 | 效果 |
|---|---|
| `promote_member_to_celestial_governor` 提升为天朝总督 | **−5 天朝威仪** |
| `demote_celestial_governor_to_vassal` 降为附庸 | **−5 天朝威仪** |

（中文名见 `country_interactions_l_simp_chinese.yml:1048` 起。）

## 八、其他相关机制

| 机制 | 文件 |
|---|---|
| 红巾军起义（局势，阻挡危机灾难） | `common\situations\`（`situation:red_turban_rebellions`）、`common\casus_belli\red_turban_rebellions.txt` |
| 索要白银贡品（皇帝对成员） | `common\country_interactions\demand_silver_tribute.txt` |
| 帝国科举 | `common\generic_actions\middle_kingdom.txt:1`（`conduct_keju_examination`）、`events\imperial_examination_events.txt`（54 处） |
| 中国专属事件 | `events\DHE\flavor_chi.txt`（49 处）、`flavor_MCH.txt`（9 处） |
| 宝船巡行 | `events\DHE\flavor_chi_treasure_expedition.txt`（3506 行） |
| 忽必烈遗产（改革） | `common\government_reforms\country_specific.txt`（`legacy_of_kublai_khan` 等 12 处） |
| 天朝法律 | `common\laws\20_middle_kingdom.txt`（`CELESTIAL_LAWS` = "天朝律令"） |

## 九、Mod 改造建议

| 想改什么 | 动哪里 |
|---|---|
| 宣称天命条件 | `casus_belli\unify_china.txt` 的 `create_visible` / `declare_enabled`（**两处要同步改**） |
| CB 酝酿速度 | 同文件 `speed`（默认 10 = 10 个月） |
| 夺天命成本/效果 | `peace_treaties\take_mandate_of_heaven.txt` 的 `cost` / `effect` |
| 入会条件 | `international_organizations\middle_kingdom.txt` 的 `can_join_trigger` |
| 天命数值 | IO 变量 `celestial_authority`（min/max/start/monthly_change） |
| 危机阈值 | `disasters\crisis_of_the_chinese_dynasty.txt` 的 `can_start`（默认 ≤20） |
| 朝贡经济 | `international_organization_payments\middle_kingdom_tribute.txt` |
| 文化圈范围 | `common\cultures\*.txt` 的 `culture_groups = { ... }`（注意：**不是** `culture_groups\` 文件） |

**硬编码**：IO 领袖继承逻辑、威仪对法律通过的判定、CB 创建进度系统（`speed` 机制本身）。
