# 原版解析：国际组织 IO（vanilla international organizations）

> **一句话**：拆解 36 个 IO 的定义与字段，含 21 个决议的投票、特殊地位、定期付款、土地规则、IO 议会与常量，并以天朝 IO 作案例深挖。
> **什么时候看**：写新 IO、改成员加入退出与领袖变更、做决议投票或定期付款，或接 IO 的脚本钩子时翻这篇。
> **体量**：356 行 · 约 17 分钟通读

## 目录

- [术语对照（中文译名与内部名）](#术语对照中文译名与内部名)
- [一、总览：IO 是一台"可脚本化的多国容器"](#一总览io-是一台可脚本化的多国容器)
- [二、顶层字段全表（readme 24.4 KB 分类归纳）](#二顶层字段全表readme-244-kb-分类归纳)
  - [2.1 身份与目标](#21-身份与目标)
  - [2.2 领袖：**三种触发 × 五种方法**](#22-领袖三种触发--五种方法)
  - [2.3 成员进出](#23-成员进出)
  - [2.4 修正（5 类）与变量系统](#24-修正5-类与变量系统)
  - [2.5 议会与决议](#25-议会与决议)
  - [2.6 战争与军事](#26-战争与军事)
  - [2.7 外交倾向与吞并](#27-外交倾向与吞并)
  - [2.8 三作用域脚本清单（readme 138–201 行）](#28-三作用域脚本清单readme-138201-行)
- [三、36 个 IO 一览（实测字段对照）](#三36-个-io-一览实测字段对照)
- [四、IO 议会与决议引擎](#四io-议会与决议引擎)
  - [4.1 议会（`generic_actions\io_parliament.txt` + `io_parliament_bribes.txt`）](#41-议会generic_actionsio_parliamenttxt--io_parliament_bribestxt)
  - [4.2 决议字段（`resolutions\readme.txt` 权威）](#42-决议字段resolutionsreadmetxt-权威)
  - [4.3 实例：HRE 皇帝选举（`resolutions\hre_election.txt`，13 KB）](#43-实例hre-皇帝选举resolutionshre_electiontxt13-kb)
- [五、三个子类目](#五三个子类目)
  - [5.1 `international_organization_special_statuses\`（11 文件）](#51-international_organization_special_statuses11-文件)
  - [5.2 `international_organization_payments\`（7 文件）](#52-international_organization_payments7-文件)
  - [5.3 `international_organization_land_ownership_rules\`（9 文件）](#53-international_organization_land_ownership_rules9-文件)
- [六、案例深挖：天朝 IO（`middle_kingdom.txt`，7.7 KB）](#六案例深挖天朝-iomiddle_kingdomtxt77-kb)
  - [6.1 身份与领袖](#61-身份与领袖)
  - [6.2 修正：成员被"汉化"](#62-修正成员被汉化)
  - [6.3 加入 / 离开](#63-加入--离开)
  - [6.4 变量：天威与总督数](#64-变量天威与总督数)
  - [6.5 付款：**皇帝付钱给朝贡国**](#65-付款皇帝付钱给朝贡国)
  - [6.6 特殊地位：天朝总督（`celestial_governor`）](#66-特殊地位天朝总督celestial_governor)
  - [6.7 天朝与外部系统的接口](#67-天朝与外部系统的接口)
- [七、其余大 IO 速览](#七其余大-io-速览)
- [八、耦合表（跨系统接口）](#八耦合表跨系统接口)
- [九、Mod 改造建议（可改 vs 硬编码）](#九mod-改造建议可改-vs-硬编码)
- [十、中文检索键](#十中文检索键)

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| `common\international_organizations\` | **36 个 IO / 34 文件 / 282.7 KB**（`hre.txt` **29.9 KB** 最大） | **`readme.txt` 24.4 KB**（字段权威，含三作用域脚本清单） |
| `common\resolutions\` | **21 个决议定义 / 24 档 / 130.4 KB**（⚠️ `enact_policy` / `policy_vote` / `repeal_law` 用「名字一行、`{` 另起一行」写法，按 `key = {` 同行统计会误数成 18） | `readme.txt` 16 KB（投票引擎） |
| `common\international_organization_special_statuses\` | **11 文件 / 34.6 KB**（HRE 单独 17.4 KB） | `readme.txt` 1.6 KB |
| `common\international_organization_payments\` | **7 文件 / 21 KB** | `readme.txt` 2 KB |
| `common\international_organization_land_ownership_rules\` | **9 文件 / 8.9 KB** | `readme.txt` 1.6 KB |
| `common\generic_actions\` | `io_parliament.txt` 11.8 KB · `io_parliament_bribes.txt` 6.5 KB · `international_organizations.txt` 7.7 KB · `international_organizations_policy_issues.txt` 1.9 KB · `leave_international_organization.txt` 1.3 KB | `generic_actions\readme.txt` |
| `loading_screen\common\defines\00_defines.txt` | **`NInternationalOrganization`（2552）** + **`NImperialCircle`（2556–2562）** | 逐条注释 |
| 相关 | `common\laws\`（法律可增删 IO 的 payments/special statuses）、`common\situations\`（局势与 IO 同名配对）、`common\generic_actions\io_*.txt` | 见 `vanilla\vanilla-disaster-and-situation.md` |

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `international_organization` | **国际组织** | 「为某种原因联合在一起的国家群体，可选地有一个目标国家」 |
| `special_status` | **特殊地位** | 组织内的身份分层（选帝侯、自由市、朝贡总督…） |
| `special_status_power` | 特殊地位权力 | **IO 议会里按特殊地位分组的政治权力** |
| `resolution` | **决议** | 「完全用脚本创建决议并放置 GUI 按钮——不需要程序员」 |
| `io_parliament` | 国际组织议会 | HRE 的**帝国议会**（`hre_court_assembly`） |
| `payments` | 定期付款 | 成员之间（甚至国家↔阶层）的月度转移 |
| `land_ownership_rule` | 土地所有权规则 | IO 也能"拥有"地点（仍由国家持有，但附加效果） |
| `leader_change_method` | 领袖产生方式 | `rotation` / `vote` / `lottery` / `score` / `none` |
| `imperial_circle` | **帝国圈** | HRE 内的次级圈（`NImperialCircle` 有独立常量） |
| `has_dynastic_power` | 王朝权力 | 仅 HRE 开启 |

## 一、总览：IO 是一台"可脚本化的多国容器"

IO 之所以值得单独一章，是因为它把**六件本来独立的事**装进了一个容器：

```
成员集合 ─┬─ 领袖（三国作用域 / 五种产生法 / 可用决议选举）
          ├─ 特殊地位（分层 + special_status_power → 议会计票）
          ├─ 议会 + 决议（投票引擎：谁能投、几票、几票通过、何时结算）
          ├─ 定期付款（成员↔成员 / 成员↔阶层的月度转移 + 维护滑条）
          ├─ 土地所有权（IO "拥有"地点 + 只能靠和平条约取回）
          └─ 战争与外交（谁替谁参战、成员能否互打、能否吞并成员）
```

**关键认知**：IO 的"机制"几乎全在数据侧——原版 readme 首行就写着 IO 的脚本能力（创建、邀请、加入、驱逐、决议、付款、土地规则都是脚本字段），**引擎只提供容器与结算**。

## 二、顶层字段全表（readme 24.4 KB 分类归纳）

### 2.1 身份与目标

| 组 | 字段 |
|---|---|
| 唯一性 / 目标 | `unique`（全世界只有一个）、`has_target`、`target_color`、`show_strength_comparison_with_target`、`declare_war_on_target_casus_belli`、`potential_target_trigger`、`can_target_trigger`、`has_enemies`、`can_be_enemy_trigger` |
| 显示 | `should_show_ruler_history`、`background_texture`、`show_on_diplomatic_map`、`show_as_overlord_on_map_trigger`、`map_color_override` / `secondary_map_color_override`、`tooltip`、`leader_color` / `member_color` / `target_color`、`custom_name`（连 `customizable_localization`） |
| 其他 | `show_leave_message`、`disband_message_trigger`、`use_laws_as_join_reason`、`alert_view_tab`、`has_buildings` |

### 2.2 领袖：**三种触发 × 五种方法**

```
has_leader_country = yes/no            # 是否有一个"领袖国家"（默认 no）
leader_type = none / country / character
leader_change_trigger_type = none / rulerchange / timed
leader_change_method = rotation / vote / lottery / score / none
months_between_leader_changes = <int>  # 仅 timed 有效
leader = { <effect> }                  # root = IO；用 add_to_list = leaders 填
leader_title_key / title_is_suffix / override_ruler_title / use_regnal_number
can_lead_trigger / leader_score / leadership_election_resolution / disband_if_no_leader
```

- `leader` 是**效果块**，负责"谁显示在 UI 顶部"；`has_leader_country = no` 时可以不设
- `leader_change_method = vote` 时用 `leadership_election_resolution` 指一个**决议**当选举流程（HRE = `hre_election`、高王权 = `high_kingship_election`）
- `leader_score` 是 `score` 法的动态脚本值（root = 潜在领袖，recipient = 现有组织）

### 2.3 成员进出

| 组 | 字段 |
|---|---|
| 三组 visible + enabled | `create_visible_trigger` / `create_enabled_trigger`、`invite_visible_trigger` / `invite_enabled_trigger`、`join_visible_trigger` / `join_enabled_trigger`（**均默认 yes**） |
| 加入 / 离开 | `can_join_trigger`、`can_leave_trigger`、`auto_leave_trigger`、`auto_disband_trigger`、`disband_minimum_member_count`、`can_invite_countries`、`subject_limited`（**`no` = 连外交受限的附属国也能创建/加入**） |
| 驱逐 | `expel_members_who_are_targets_of_other_members`、`_who_target_the_leader`、`_who_are_attackers_at_war_with_other_members`、`_who_are_defenders_...` |
| 自动意见 | readme 开头即声明：**`io_opinion_<key>` / `io_trust_<key>` 自动挂同侪好感与信任** |

### 2.4 修正（5 类）与变量系统

- `modifier`（全体成员）、`leader_modifier`、`non_leader_modifier`、`owned_location_modifier`（IO 拥有的地点）、`target_modifier`、`international_organization_modifier`（IO 本身）
- 生命周期钩子：`on_creation` / `on_disband` / `on_joined` / `on_left` / **`monthly_effect`**
- 变量系统：

```
variables = {
    <var> = {
        format / change_format            # 显示格式键
        monthly_change = { <script value> } # 每月增减（可带 desc 逐项说明）
        monthly_change_hidden = yes
        start / min / max
    }
}
```

### 2.5 议会与决议

`has_parliament`、`parliament_type`、`resolution_widget`、`max_active_resolutions`、`can_initiate_policy_votes`、`ai_issue_voting_bias`；见 §四。

### 2.6 战争与军事

- 参战六件套：`join_defensive/offensive_wars_always` / `_auto_call` / `_can_call`
- 性能优化开关：`only_leader_country_joins_defensive_wars`（**只有领袖替成员挨打**，原版 HRE 与天朝都开）
- `joins_*_wars_as_co_belligerent`、`take_over_wars_when_called`、`can_declare_war`、`has_military_access`、`gives_military_access_to_all_when_at_war`、`fleet_basing_rights`
- 成员互助：`can_recruit_regiments_in_members`、`can_build_ships/roads/buildings/rgos_in_members`

### 2.7 外交倾向与吞并

- `opinion_bonus` / `opinion_trust`、`antagonism_towards_leader_modifier`、`antagonism_modifier_for_taking_land_from_fellow_member`（**对同侪动手的敌意**）、`antagonism_modifier_for_taking_land_from_fellow_member_as_outsider`、`no_cb_price_modifier_for_fellow_member`
- `min_opinion` / `min_trust`——**低于阈值组织破裂**（readme 附性能警告）
- 吞并：`allow_member_annexation`、`annexation_min_years_before`、`can_annex_members` / `can_annex_visible`、`annexation_speed`
- 解散：`annulled_by_peace_treaty` / `annullment_favours_required`、`<currency type> = yes/no`（IO 国库）、`diplomatic_capacity_cost`
- 常量：**`NInternationalOrganization` 只有一条**——`MONTHS_TO_DISBAND_FROM_BIAS = 3`（意见失衡累积 3 个月才解散）

### 2.8 三作用域脚本清单（readme 138–201 行）

- **IO 作用域**：触发器 `total_members` / `total_enemies` / `any_international_organization_member|_enemy|_owned_location`（支持 `count` / `percent`）、`is_international_organization_unique`、`law_/policy_visible_to_/enabled_to_international_organization`、`international_organization_can_own_land`、`has_elections`、`has_special_status_available`、`country_has_special_status`、`months_between_leader_changes`…；效果 `international_organization_member|_owned_location`（every/random/ordered）、`add_country_to_/remove_country_from_...`、`add_location_to_/remove_location_from_...`、`add_policy_to_/remove_policy_from_...`、`international_organization_chooses_new_leader`、`international_organization_add_special_status` / `_remove_special_status`、`add_international_organization_modifier`…
- **国家作用域**：`international_organizations_member_of|_target_of`、`is_member_of_international_organization(_of_type)`、`has_special_status_in_international_organization`、`can_lead_international_organization`、**`create_international_organization`**
- **地点作用域**：`is_owned_by_international_organization` 等

## 三、36 个 IO 一览（实测字段对照）

| IO | 文件大小 | 唯一 | 领袖类型 | 产生方式 | 议会 | 特殊地位 | 付款 | 土地规则 | 变量 |
|---|---|---|---|---|---|---|---|---|---|
| **`hre`** 神圣罗马帝国 | **29.9 KB** | yes | **角色** | rulerchange + **vote**（`hre_election`） | **yes**（`hre_court_assembly`） | **9 个** | yes | yes | `imperial_authority` |
| **`middle_kingdom`** 天朝 | 7.7 KB | yes | **角色**（皇帝） | — | — | `celestial_governor` | **yes** | yes | `celestial_authority`、`num_of_celestial_governors` |
| `catholic_church` 天主教会 | 7.0 KB | yes | 角色（教宗） | — | — | `curia` / `military_order` / `bishopric` | — | — | `papal_authority` |
| `union` 共主邦联 | 9.8 KB | — | 角色 | rulerchange | — | `senior_partner` / `junior_partner` | — | — | — |
| `marriage_union` 婚姻联盟 | 1.7 KB | — | 角色 | rulerchange | — | — | — | — | — |
| `sect` 教派 | 9.5 KB | no | **无** | — | — | — | — | yes | `religion`、`sect_favor` |
| `religious_leagues` 宗教联盟 | 12.1 KB | yes | 国家 | — | — | — | — | — | — |
| `crusade` 十字军 | 7.2 KB | yes | 国家 | — | — | — | — | — | `religious_zeal` |
| `jihad` 圣战 | 5.8 KB | — | 国家 | — | — | — | — | — | `religious_zeal` |
| `hindu_branch` / `shinto` / `sikhism` / `autocephalous_patriarchate` | 1.3–11.7 KB | yes | 角色/无 | — | — | — | — | — | `religion`（+`seat`） |
| `high_kingship` 爱尔兰高王权 | 3.6 KB | no | **角色** | rulerchange + **vote**（`high_kingship_election`） | — | `high_king` | — | yes | `high_kingship_region` |
| `japanese_shogunate` 日本幕府 | 4.8 KB | yes | **角色** | none | — | `japanese_emperor` / `shugo_daimyo` | — | yes | — |
| `lordship_of_ireland` 爱尔兰领主 | 4.3 KB | yes | **角色** | none | — | `lord_of_ireland` / `lieutenant` / `loyalist` / `absentee` | — | yes | — |
| `ilkhanate` 伊尔汗国 | 2.8 KB | yes | **角色** | none | — | `ilkhan_claimant` | — | — | — |
| `tatar_yoke` 鞑靼枷锁 | 1.5 KB | yes | **角色** | none | — | `tatar_overlord` / `tatar_tax_collector` | **yes** | — | — |
| `italian_league_1/2/3` 意大利联盟（三阶段） | 各 12.5 KB | yes | 国家 | — | — | `italian_league_sponsor` | **yes** | yes | — |
| `foreign_league_{hre,france,iberia,balkan}` 外部联盟 | 11.7–12.3 KB | yes | 国家 | — | — | — | — | yes | — |
| `swiss_confederation` 瑞士邦联 | 6.1 KB | yes | 无 | none（**48 个月**投票一次） | — | — | — | yes | — |
| `coalition` 包围网 | 4.5 KB | — | 国家 | — | — | — | — | — | — |
| `defensive_league` 防御联盟 | 7.9 KB | — | — | — | — | — | — | — | — |
| `independence_movement` 独立运动 | 8.2 KB | no | 国家 | none | — | — | — | — | — |
| `colonial_federation` 殖民地联邦 | 2.4 KB | no | 国家 | **score / 96 月** | — | — | — | — | — |
| `guelphs_and_ghibellines` 圭尔夫与吉伯林 | 7.1 KB | yes | 国家 | **timed + score / 1 月** | — | — | — | — | — |
| `red_turban_rebels` 红巾军 | 4.2 KB | yes | 国家 | timed + score / 1 月 | — | — | — | — | — |
| `tribal_confederation` / `jurchen_federation` 部落/女真联盟 | 2.1 / 4.0 KB | no | 国家 | — | — | — | — | — | — |

**读法**：原版 36 个 IO 里，**只有 HRE 开了议会、只有 9 个设了特殊地位、只有 5 个配了付款**——大部分 IO 只是"成员 + 参战 + 自动好感"的轻量容器。真正复杂的六个是 **HRE、天朝、天主教会、联统、爱尔兰高王权/领主、日本幕府**。

## 四、IO 议会与决议引擎

### 4.1 议会（`generic_actions\io_parliament.txt` + `io_parliament_bribes.txt`）

- **调用**：`call_organization_parliament`（`type = internationalorganization`），AI `monthly × 6`
- **条件**：IO 有 `has_parliament = yes` 且调用者带 `modifier:has_international_parliament = yes`；`can_have_parliament_called_by`；**当前没有活跃议会**；**距上次议会 > 59 个月**
- **会址**：必须是 **IO 拥有的地点**（`every_international_organization_owned_location`），原版 HRE 要求该地点属于拥有 `free_city`（自由市）或 `archbishop_elector`（大主教选帝侯）地位的成员；会址存在变量 `hre_diet_location` 里
- **行贿**：`io_parliament_bribes.txt` 6.5 KB——议会有独立的贿赂路径（对应 `special_status_power` 的计票）
- **政策议题**：`international_organizations_policy_issues.txt`（1.9 KB）——通过议会变更 IO 政策

### 4.2 决议字段（`resolutions\readme.txt` 权威）

| 组 | 字段 |
|---|---|
| 显示 / 门槛 | `loc`、`potential`（scope:actor）、`allow`（scope:proposer；scope:recipient = IO）、`can_vote`、**`is_live`**（是否活跃；选举用）、`ai_tick` |
| 价格 | 提案侧 `proposal_price` + `proposal_price_modifier` + `proposal_payer` / `proposal_payee`；执行侧 `price` + `price_modifier` + `payer` / `payee`——**默认付款人是提案者，默认收款人"无人"（钱消失）** |
| 投票 | `requires_vote`、`requires_explicit_votes`（是否逐国显式投票）、**`votes`**（每国票数脚本）、**`total_votes_needed`**（通过门槛）、**`should_finalize_vote`**（是否立即结算，**结算前先查截止期，截止期优先**） |

`should_finalize_vote` 内可用：`scope:total_votes_needed`、`scope:total_votes_available`、**`scope:highest_vote`**。

### 4.3 实例：HRE 皇帝选举（`resolutions\hre_election.txt`，13 KB）

```
hre_election = {
    can_vote = { scope:actor = { is_elector = yes  is_member_of_international_organization = international_organization:hre } }
    is_live  = { scope:recipient ?= { international_organization_has_leader = no } }   # 空位时才活跃
    votes    = { add = { desc = "[elector|e]"  value = 1 } }                           # 每位选帝侯 1 票
    total_votes_needed = { add = { value = scope:total_votes_available  multiply = 0.5 } }  # 过半
    requires_explicit_votes = no   # 注释原文："do not change this."
    should_finalize_vote = { OR = { 最高票 ≥ 门槛；或 HRE_ELECTORS_DEADLOCK（无人过半 + 空位 >2 年 + 0 选帝侯…） } }
}
```

配套：HRE 的 `leader_change_trigger_type = rulerchange`（皇帝一死就触发）、`leader_change_method = vote`、**`disband_if_no_leader = no`**（注释原文："can have no leader while there's a power battle to get enough votes"——**帝位可以长期空悬**）。

**帝国圈（`NImperialCircle`，defines 2556–2562）**：`FORMATION_PERIOD_MONTHS = 24`（成员 24 个月内入圈，之后锁定）、`CANDIDATE_AREA_HRE_LOCATION_PERCENT = 0.5`（一个地区至少 50% 地点属 HRE 才能成圈）、`MIN_VOTING_POWER_THRESHOLD = 10`（低于此的圈被相邻圈吞并）、**`MAX_VOTING_POWER_CAP = 0.35`**（单圈不得超过 HRE 总投票权 35%）、`CIRCLE_DORMANCY_THRESHOLD = 2`（少于 2 个活跃成员即休眠）；HRE 另有 `max_circles_at_formation = 10`。

## 五、三个子类目

### 5.1 `international_organization_special_statuses\`（11 文件）

```
<tag> = {
    priority = int              # 界面排序，越高越前，默认 1；0 = 等同于普通成员
    can_bestow_trigger          # root = country, scope:recipient = IO, scope:source = 该地位
    auto_bestowal_trigger / auto_dismissal_trigger
    max_countries = <script value>   # root = IO
    on_bestowed_effect / on_rescinded_effect
    modifier                    # 施加于持有国
    leader_modifier             # 施加于领袖，且**乘以持有该地位的国家数**
    map_color
    special_status_power = <script value>   # 该群体在 IO 议会里的政治权力
}
```

另有 `can_be_invited = no`（实测存在于 `celestial_governor`：**只能由皇帝授予，不能被邀请**）。

### 5.2 `international_organization_payments\`（7 文件）

「成员的定期月付款，可选地从某些成员到另一些成员」——**payer/payee 可以是国家或阶层**（readme 原文："anything that has an account"）：

| 字段 | 作用 |
|---|---|
| `get_payer_list` / `get_payee_list` | 效果块，用 `add_to_local_variable_list = { name = payers/payees  target = … }` 填列表（root = IO） |
| `price` / `price_multiplier` | 基础价格 × 脚本乘数 = 总转移额 |
| `uses_maintenance` / `maintenance_modifier` / `min_slider_value` | 是否带**维护滑条**；按维护程度施加修正；滑条下限（默认 0.5） |
| `proportion_for_payer` / `proportion_for_payee` | 谁付多少 / 谁收多少（脚本值） |
| `ai_maintenance_value` / `ai_maintenance_ignore_saving` | AI 的滑条值；储蓄模式下是否照付（默认不） |

原版 7 张：`hre`（帝国税）、`tithe`（什一税）、`union_contribution`、`tatar_yoke_payment`、`italian_league_sponsor_payments`、`middle_kingdom_tribute`（**反向**：皇帝付给朝贡国）、`readme`。

### 5.3 `international_organization_land_ownership_rules\`（9 文件）

「如果 IO 也能拥有土地，规则写在这里。**土地仍由国家持有**，只是附加效果」：

`modifier`（施加于 IO 拥有的地点）、`can_add_trigger` / `can_remove_trigger`（整体）、`can_add_location_trigger` / `can_remove_location_trigger`（逐地点，root = location，recipient = IO）、`on_added` / `on_removed`、`ai_desire_to_add`、`owned_location_color`（外交地图条纹）、**`removed_by_peace_treaty`**（只允许通过和平条约移除）、`remove_war_score_modifier`（移除的战争分修正）。

原版 8 张：`hre` / `high_kingship` / `japanese_shogunate` / `lordship_of_ireland` / `middle_kingdom` / `sect` / `swiss_conferation`（**注意原版拼写少了一个 d**）/ `italian_wars`。

## 六、案例深挖：天朝 IO（`middle_kingdom.txt`，7.7 KB）

> 这是"IO 能力上限"的最佳样例：一个 IO 同时用上了**角色领袖、变量、特殊地位、付款、土地规则、月度效果与自动拉人**。

### 6.1 身份与领袖

| 字段 | 值 |
|---|---|
| 唯一性 | `unique = yes`、`has_target = no`、`show_on_diplomatic_map = yes` |
| 领袖 | `has_leader_country = yes`；**文件里 `leader_type` 写了两次**（先 `country` 后 `character`，后者生效）→ **皇帝是角色** |
| 称号 | `leader_title_key = "EMPEROR_CHINA"`、`title_is_suffix = yes`、`override_ruler_title = yes`（**压过本国头衔**） |
| `leader` 块 | 若领袖国有摄政**且有继承人** → 显示**继承人**；否则显示统治者（幼帝/摄政期的特殊处理） |
| 解散 | `auto_disband_trigger = { always = no }`——**天朝永不自动解散** |

### 6.2 修正：成员被"汉化"

```
modifier = { monthly_towards_sinicized = societal_value_monthly_move }   # 全体成员持续被推向"汉化"轴
non_leader_modifier = { block_from_change_to_empire_rank = yes }          # 非领袖不得升为帝国等级
leader_modifier = { monthly_prestige = 0.1  cultures_capacity = 50        # ⭐ 文化容量 +50
                    diplomatic_capacity_modifier = 0.5  allow_tributary_subject = yes  government_size = 1 }
only_leader_country_joins_defensive_wars = yes    # 朝贡国被打 → 皇帝**有选项**介入（join_defensive_wars_auto_call）
can_declare_war = { always = yes }                # 成员之间（含皇帝）可以互打
```

### 6.3 加入 / 离开

- `can_join_trigger`：非战争；是皇帝附属**或**非附属；**不得是帝国等级**；日本幕府的领袖特例；且满足（首都亚洲 ∨ 中华/儒家文化组 ∨ 亚洲民间宗教组/佛教）
- `can_leave_trigger`：不是皇帝的附属、自己不是领袖
- **`auto_leave_trigger`：与皇帝开战即自动退出**
- `on_joined` / `on_left`：自动建立 / 移除 `exclusive_trade_rights_with_isolated` 关系
- **`monthly_effect`：把皇帝新获得的每一个附属国自动拉进天朝**（原文注释："auto-join anyone who becomes subject or tributary of China"）

### 6.4 变量：天威与总督数

| 变量 | 范围 | 起始 | 月度变化 |
|---|---|---|---|
| `celestial_authority` **天威** | 0–100 | **70** | **基础衰减 −0.1**；内部战争：每个交战成员 **−0.01**；朝贡国：每个 **+0.005 × (好感/200) × (经济基础/1000)**；另加领袖的 `monthly_celestial_authority` 修正 |
| `num_of_celestial_governors` 总督数 | 0–4 | 1 | `monthly_change_hidden = yes`（由特殊地位授予/撤销驱动） |

### 6.5 付款：**皇帝付钱给朝贡国**

`payments_implemented = { middle_kingdom_tribute }`——原版文件首行注释就点明："**leader pays tributes to members in exchange for getting more mandate**"：

- `get_payer_list` = 领袖国（皇帝）；`get_payee_list` = 所有非领袖成员
- `uses_maintenance = yes`、**`min_slider_value = 0`**（注释原文："otherwise Yuan quickly go broke when the RTR fires up"——红巾军一起，元朝立刻破产）
- 维护滑条效果按偏离 0.5 的距离 **×2** 缩放：<0.5 → `monthly_celestial_authority −0.2`；>0.5 → **+0.2**
- `price_multiplier` = 所有朝贡国 `monthly_income_trade_and_tax` 之和（含 `tribute_payment_received_modifier`）；`proportion_for_payee` 按各国收入分配

### 6.6 特殊地位：天朝总督（`celestial_governor`）

`priority = 9`、**`max_countries = 4`**、**`can_be_invited = no`**（只能皇帝授予）；`can_bestow_trigger` 要求**同大区已有另一位总督**；持有国修正给**五种政府影响力月增各 +0.1**（正统性/共和传统/奉献度/游牧团结/部落凝聚力）+ `monthly_towards_sinicized` 大步推进；`leader_modifier` 给皇帝 **+1 外交官**与 `monthly_celestial_authority +0.01`；授予/撤销时增减 `num_of_celestial_governors`。

### 6.7 天朝与外部系统的接口

| 系统 | 接口 |
|---|---|
| 附属国类型 | `allow_tributary_subject = yes`（领袖专属）——朝贡国是独立附属国类型 |
| 天命（`vanilla\vanilla-mandate-of-heaven.md`） | `celestial_authority` 是**天命系统的数值载体**；CB"宣称天命"、和平条约"夺取天命"、中华王朝危机灾难都围绕它 |
| 战争 | 成员互打可用 `cb_*`；皇帝对朝贡国有防御介入选项 |
| 社会价值 | `monthly_towards_sinicized` 直接推"汉化"轴 |
| 法律 | `block_from_change_to_emperor_rank` 类修正（非领袖禁止升帝国） |

## 七、其余大 IO 速览

| IO | 机制骨架 |
|---|---|
| **`hre`** | 9 个特殊地位（`emperor` / `elector` / `archbishop_elector` / `free_city` / `primas_germaniae` / `legatus_natus` / `imperial_prelate` / `imperial_prince` / `imperial_peasant_republic`）；变量 `imperial_authority`；**唯一开议会的 IO**（`hre_court_assembly`，60 个月一次，会址须在自由市/大主教选帝侯处）；选举决议 `hre_election`（选帝侯各 1 票、过半、僵局机制、**可以长期无皇帝**）；`has_dynastic_power = yes`；**帝国圈**（最多 10 个，单圈投票权上限 35%，24 个月成圈期）；`show_as_overlord_on_map_trigger = always yes`（在地图上像宗主一样显示）；自由市受帝国保护（`special_status:free_city` + `modifier:excluded_from_imperial_protection`） |
| **`catholic_church`** | 角色领袖（教宗）+ 特殊地位 `curia` / `military_order` / `bishopric`；变量 `papal_authority`；`max_active_resolutions = 1`；配套决议 `call_crusade` / `00_excommunicate` / `sublimis_deus` / `summis_desiderantes` / `libertas_ecclesiae` 与付款 `tithe`（什一税） |
| **`union` 共主邦联** | 角色领袖 + `rulerchange`；特殊地位 **`senior_partner` / `junior_partner`**；付款 `union_contribution`；`custom_name = union_name` |
| **`marriage_union` 婚姻联盟** | 轻量版（1.7 KB）：角色领袖 + `rulerchange`，无地位无付款 |
| **宗教 IO 模板族** | `sect` / `hindu_branch` / `shinto` / `sikhism` / `autocephalous_patriarchate` 共享变量 **`religion`**（（自家宗教），`sect` 另有 `sect_favor` + 土地规则；`autocephalous_patriarchate` 另有 `seat`（牧首座） |
| **圣战族** | `crusade` / `jihad` 共享变量 **`religious_zeal`**（宗教热忱）；`crusade` 有 `has_target` |
| **`italian_league_1/2/3` + `foreign_league_×4`** | 北意大利战争剧本：特殊地位 `italian_league_sponsor`、付款 `italian_league_sponsor_payments`、共享土地规则 `italian_wars_land_ownership` |
| **`high_kingship` 爱尔兰高王权** | `high_king` 地位 + 选举决议 `high_kingship_election` + 变量 `high_kingship_region` |
| **`japanese_shogunate`** | `japanese_emperor`（天皇）/ `shugo_daimyo`（守护大名）两个地位 + 专属土地规则；领袖 none 型 |
| **`lordship_of_ireland`** | 四个地位：`lord_of_ireland` / `lieutenant` / `loyalist` / `absentee` |
| **`tatar_yoke` 鞑靼枷锁** | `tatar_overlord` / `tatar_tax_collector` 两个地位 + 付款 `tatar_yoke_payment` |
| **`ilkhanate`** | 单一位 `ilkhan_claimant`（汗位觊觎者） |
| **`swiss_confederation`** | 领袖 none + **`months_between_leader_changes = 48`**（文件注释："even if no policy is active, the Swiss will always vote every 4 years"）+ 专属土地规则 |
| **`coalition` / `defensive_league` / `independence_movement` / `religious_leagues` / `colonial_federation` / `tribal_confederation` / `jurchen_federation` / `guelphs_and_ghibellines` / `red_turban_rebels`** | 无特殊地位、无付款：靠**成员集合 + 参战规则 + 自动好感**运转（`coalition` 与 `independence_movement` 有 `has_target`；`guelphs` 与 `red_turban_rebels` 每 **1 个月**按分数换领袖） |

## 八、耦合表（跨系统接口）

| 系统 | 接口 |
|---|---|
| **局势 · 灾难**（`vanilla\vanilla-disaster-and-situation.md`） | 同名配对（如 `colonial_revolution` ↔ `colonial_federation`）；灾难/局势用 `is_member_of_international_organization_of_type` 判定；IO 的 `create_visible_trigger` 常要求 `is_situation_active = situation:<名>` |
| **法律 · 阶层 · 议会** | `laws` / `policies` 可**增删 IO 的 payments 与 special statuses**（`payments_implemented` / `special_statuses_implemented` 是法律联动点）；IO 自己也能有 `has_parliament` 与议会类型 |
| **外交** | IO 成员间的自动 `io_opinion_<key>` / `io_trust_<key>`；`min_opinion` / `min_trust` 低到阈值即破裂；`diplomatic_capacity_cost` 计入外交容量 |
| **战争** | `join_*_wars_*` 六件套 + `only_leader_country_joins_*` 性能开关；`can_declare_war` 决定成员能否互打；`annulled_by_peace_treaty` 决定和平条约能否解散 |
| **附属国** | `subject_limited = no` 让外交受限的附属国也能建 IO；`allow_tributary_subject`（天朝）解锁朝贡国类型 |
| **社会价值** | IO 修正可直接推轴（天朝 `monthly_towards_sinicized`） |
| **地图 · 地理** | `land_ownership_rule` 让 IO "拥地"并在地图上以条纹显示（`owned_location_color`）；HRE 的帝国圈按 `area` 成圈 |
| **AI** | `ai_issue_voting_bias`、`ai_desire_to_join` / `ai_desire_to_allow_new_member`（可写 `desc` 逐项解释，见 AI 篇）、`ai_desire_to_add`（土地）、`ai_maintenance_value` |

## 九、Mod 改造建议（可改 vs 硬编码）

| 想改的东西 | 正规做法 |
|---|---|
| 新建一个 IO | `common\international_organizations\<名>.txt`，最小集 = `unique` + 三组 visible/enabled；要领袖就补 `leader_type` / `leader` / `*_change_*` |
| 做选举制 IO | `leader_change_trigger_type = rulerchange` + `leader_change_method = vote` + 写一个 `resolutions\<名>_election.txt` 并挂到 `leadership_election_resolution` |
| 给 IO 分层身份 | `international_organization_special_statuses\<名>.txt`；**要让它在议会里算票，必须写 `special_status_power`** |
| 让 IO 收税/发钱 | `international_organization_payments\<名>.txt` + IO 里 `payments_implemented`；付款对象可以是**阶层** |
| 让 IO 拥有土地 | `international_organization_land_ownership_rules\<名>.txt` + IO 里 `land_ownership_rule`；`removed_by_peace_treaty = yes` 可把取回绑定到和平条约 |
| 开议会 | `has_parliament = yes` + `parliament_type` + `resolution_widget`；调用走 `generic_actions\io_parliament.txt` 模式（60 个月间隔、会址须为 IO 拥有地点） |
| 让 IO 随时间变化 | `monthly_effect` + `variables`（带 `monthly_change` 与 `format` 键） |
| 改帝国圈参数 | defines `NImperialCircle`（成圈期 24 个月、单圈投票权上限 0.35、休眠阈值 2）+ HRE 的 `max_circles_at_formation` |

**硬编码边界**：IO 成员的存储与月度结算、议会流程的驱动、决议投票的计票与结算时机、付款的实际转账与维护滑条、帝国圈的成圈/合并算法、`custom_name` 的自定义本地化管线。**但注意 readme 的原话：连"创建决议 + GUI 按钮"都不需要程序员**——IO 层的脚本自由度是全游戏最高的之一。

## 十、中文检索键

**概念**（`game_concepts_l_simp_chinese.yml`）：`game_concept_international_organization` **国际组织**、`game_concept_special_status` **特殊地位**、`game_concept_resolution` **决议**、`game_concept_parliament`（IO 语境下为帝国议会）、`game_concept_hegemon` 霸权（见政体篇）。**具体 IO 的中文名取自各自的 loc**（如 `hre` 神圣罗马帝国、`middle_kingdom` 天朝、`catholic_church` 天主教会、`union` 共主邦联），**地位名与付款名走各自文件里的键**（`celestial_governor` 天朝总督、`tithe` 什一税…）。

**界面**（`in_game\gui\`）：`international_organization_lateralview.gui`、`panels\organization\`（各 IO 面板：`colonial_federation.gui`、`parliament.gui` 等）、`parliament` 相关 panel（HRE 帝国议会复用国家议会面板）。

**关联字段档**：`fields\common-international_organizations.md`（8.9 KB，字段权威）、`fields\common-resolutions.md`、`fields\common-laws.md`（法律增删 IO 付款/地位）、`fields\common-generic_actions.md`；机制耦合见 `vanilla\vanilla-disaster-and-situation.md`，具体案例见 `vanilla\vanilla-mandate-of-heaven.md`（天命与天朝）。
