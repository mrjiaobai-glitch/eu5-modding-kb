# 原版机制：枢机与教廷（cardinals / papacy）

> **什么时候看**：改天主教玩法、做枢机/教宗选举相关事件、动 `seat_of_cardinal`、或用 `num_cardinals` / `cardinal` scope 时。
> **依据**：`international_organizations\catholic_church.txt`、`building_types\unique_buildings.txt`（`seat_of_cardinal`）、`generic_actions\catholic.txt`（`request_cardinal_seat`）、`regencies\00_papal_election.txt`、`heir_selections`、`situations\western_schism.txt`、`scripted_effects\on_action_effects.txt`（`set_up_cardinal`）、`static_modifiers\religion.txt`、`script_docs`。**EU5 1.4.5**，2026-10-30。
> **体量**：约 6 分钟

## 一、枢机是什么
- 一个**角色**，`religious_figure:cardinal`（中文 **"枢机"**），地位仅次于教宗；必须男性、成年、clergy 阶层、与座堂所在国同宗教。
- **`cardinal` 是 location→character 的 scope link**（`reference\scopes.md`）：`location = { cardinal = { … } }` 拿到该地枢机。
- 计数与迭代（`reference\triggers.md` / `effects.md`）：
  - 国家：`num_cardinals`（比较）；`any/every/ordered/random_cardinal_in_country`
  - 宗教：`has_cardinals`、`total_cardinals`；`any/every/ordered/random_cardinal_in_religion`、`*_country_with_cardinals`
- **没有** `cardinal_*` 修正键（`reference\modifiers.md` 0 条）——枢机不直接给国家修正，作用在**教廷行动**与**教宗权威**上。

## 二、怎么获得枢机（三条途径）
### 1. 枢机座堂 `seat_of_cardinal`（主途径）
```
seat_of_cardinal = {                  # unique_buildings.txt:996
	is_special = yes ; max_levels = 1 ; pop_type = clergy
	category = religious_category
	allow = { always = no }           # 只能被行动/事件建，不能手动造
	raw_modifier = { gives_cardinal = yes }   # ★ 关键：给枢机的 raw 标记
	modifier = { local_max_literacy = 2.5  local_clergy_food_consumption = 0.2 }
	is_indestructible = yes ; can_close = no
	remove_if = { NOT = { owner = { religion ?= religion:catholic } } }
	can_destroy = { situation:western_schism = { situation_is_active = no } }
	on_built = { location = { set_up_cardinal = yes } }
	on_destroyed = { location = { cardinal = { remove_cardinal_variables = yes } } }
}
```
- 中文名 **"枢机座堂"**；只能建在**有大教堂（cathedral）但还没有座堂**的地点。
- **拆除时**清掉该角色的枢机身份与任职地（`remove_cardinal_variables`）。
- **西方分裂期间不可拆**。

### 2. 通用行动 `request_cardinal_seat`（建造入口）
`generic_actions\catholic.txt:238`，`type = religious`：
| 条件 | 内容 |
|---|---|
| 基础 | `papacy_active = yes`、`country_exists = c:PAP` |
| 国家等级 | `rank_duchy/kingdom/empire` 或 `is_emperor` |
| 建筑 | 有 `cathedral`；且存在"有 cathedral、无 seat_of_cardinal"的自有地点 |
| 关系 | 自己是 PAP，或 **PAP 对你好感 ≥ 0** |
| 冷却/价格 | `recently_requested_cardinal` 1 年；`price = price:cardinal_price` |

效果里按 `num_cardinals` 与 `total_cardinals` 折算 **`cost_of_cardinal`** 变量（片段：`33 × 已有枢机 × 0.25` + `33 × 全教枢机 × 0.2` + …），再走建造。

### 3. 教宗政体改革 `papacy_reform`
`government_reforms\country_specific.txt`：`is_pope = yes` 的那项，`location_modifier` 在**首都**（`potential_trigger = { is_capital = yes }`）加 `gives_cardinal = yes` → 教宗首都自动有枢机。

> 另有引擎硬编码钩子（`on_action\_hardcoded.txt` 两处）与事件 `catholic_flavor.1000`（授予座堂）。

## 三、枢机怎么生成（`set_up_cardinal`，location scope）
`scripted_effects\on_action_effects.txt:668`：
1. 在 owner 国内找**合格人选**：存活、成年、男性、`clergy_estate`、同宗教、非艺术家、非修会成员、且**不是枢机**；
2. 找不到就 `create_character`（男性、clergy、40–65 岁）；
3. `set_character_location` 到座堂地点 + `set_character_religious_figure = religious_figure:cardinal`。
- **死亡自动补位**：`on_action\character_death_pulses.txt:328` 会再调 `set_up_cardinal`。
- 移除：`remove_cardinal_variables`（= 重置宗教身份 + 位置）。

## 四、教廷（catholic_church 国际组织）
- 教宗（PAP）是领袖；枢机用于 **`curia_actions`**（教廷行动，`common\generic_actions` 里的宗教行动）。
- **`papacy_active`**（scripted trigger）要求：是 `catholic_church` 成员、catholic 宗教没有 `curia_actions_blocked`、**教宗权威 ≥ 50**、当前无摄政。
- 教宗权威的枢机分量：`PA_CARDINALS` = Σ 成员国 `num_cardinals` × **0.002**（`catholic_church.txt`）。

## 五、教宗选举（`regencies\00_papal_election.txt`）
- 选框：`heir_selection:papal_election`（`use_election = no`、任期到死、只允许 clergy、`potential = { tag = PAP }`）。
- `papal_election_regency`：
  - `allow = { succession_law = heir_selection:papal_election }`
  - `start_effect`：随机取 `religious_figure_type = cardinal` 的成年男性枢机当 **regent**（无则取教士，再无则创建）；给 catholic 宗教加 **`papal_conclave_modifier`**（`curia_actions_blocked = yes`，`days = -1`）；`trigger_event_silently = papal_conclave_events.11`（1–650 天后出结果）。
  - `end_effect`：移除 `papal_conclave_modifier`。
- 即：**教宗空缺期 = 秘密会议（conclave）期，教廷行动被锁**。

## 六、西方分裂（`situations\western_schism.txt`，422 行）
- `can_start`：`catholic_church` IO 不存在 **或** PAP 不存在。
- 结局之一：`western_schism_conclave_deadlocked`（秘密会议僵局）。
- 用变量 `schism_cardinal` / `schism_pope_cardinal` 记录分裂枢机/分裂教宗；结束 `remove_religion_modifier = papal_conclave_modifier`。
- 分裂期间**枢机座堂不可拆**（见 §二）。

## 七、改 vs 硬编码 / 坑
| 想改 | 动这里 | 注意 |
|---|---|---|
| 枢机来源 | `seat_of_cardinal` 的 `gives_cardinal` / `request_cardinal_seat` 条件 | `allow = { always = no }` 是**故意的**，别改成可手动建 |
| 枢机人选规则 | `set_up_cardinal` 的 limit | 是脚本效果，可覆盖（同名文件=整文件覆盖） |
| 教宗选举时长 | `papal_conclave_events.11` 的 1–650 天 | 在事件里 |
| 秘密会议惩罚 | `static_modifiers\religion.txt` 的 `papal_conclave_modifier` | religion 类目修正 |
| 枢机权重 | `PA_CARDINALS` 的 0.002 | IO 权重，影响教宗权威 |

**坑**：
1. 枢机是**角色**，会老死/被替换——别把它当永久国家属性。
2. `gives_cardinal` 是 **raw_modifier** 键（建筑/改革 `location_modifier` 都能用），不是普通修正。
3. 中文名是 **"枢机 / 枢机座堂 / 教廷行动"**，搜英文 `cardinal` / `seat_of_cardinal` / `curia_actions` 才找得到。
4. `num_cardinals` 只数国家；全教用 `total_cardinals`（religion scope）。

## 八、相关档
- 宗教整体：`vanilla\vanilla-culture-and-religion.md`（修会/zeal 在 §三·bis）
- 国际组织：`vanilla\vanilla-international-organizations.md`
- 政体/改革：`vanilla\vanilla-government-and-reform.md`、`fields\common-government_reforms.md`
- 建筑：`fields\common-building_types.md`（`raw_modifier`）、`vanilla\vanilla-production-and-buildings.md`
- 触发器/效果/scope：`reference\triggers.md`、`reference\effects.md`、`reference\scopes.md`
- 脚本库：`reference\scripted-library.md`（`set_up_cardinal`、`remove_cardinal_variables`）
