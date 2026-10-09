# common/estates（阶层数据）

> **一句话**：8 个阶层的**单文件数据表**：每 POP 税额、颜色、王朝与佣兵领袖规则、叛乱用语言，以及四个块型字段（`satisfaction` 满意加成 / `high_power`·`low_power` 力量档位 / `opinion` 阶层外交倾向计算 / `power` 王室专属）与 1.4 的**阶层私兵**参数。
> **什么时候看**：调阶层税额与满意度收益、改阶层力量高/低时的后果、写阶层对外国的观感权重，或给某个阶层开私兵时翻这篇。
> **体量**：90 行 · 约 5 分钟通读

来源：`in_game\common\estates\00_default.txt`（**单文件 32 KB，本体没有 readme**；2026-09 逐字段实查）

## 八个阶层

中文名直接取自本体本地化（`estate_l_simp_chinese.yml`），**注意 `peasants_estate` 游戏内叫「平民」不是「农民」**。

| 内部名 | 行号 | 中文名 | `tax_per_pop` | 特点 |
| --- | --- | --- | --- | --- |
| `crown_estate` | 1 | 王室 | **0** | 唯一带 `power` 块与 `ruler`/`priority_for_dynasty_head` 的阶层；`can_spawn_random_characters = no`、不参与 `bank` |
| `nobles_estate` | 44 | 贵族 | **100** | 税额最高；唯一 `characters_have_dynasty = always`；有**阶层私兵**参数 |
| `clergy_estate` | 316 | 教士 | 25 | 叛乱用 `liturgical_language`；`can_generate_mercenary_leaders = no` |
| `burghers_estate` | 576 | 市民 | 40 | `characters_have_dynasty = sometimes` |
| `peasants_estate` | 778 | **平民** | 1 | `disenfranchise_to = nobles_estate`；`use_diminutive = yes` |
| `dhimmi_estate` | 991 | 齐米 | 1 | 同上；`can_generate_mercenary_leaders = no` |
| `tribes_estate` | 1088 | 部落 | 0.01 | 不参与 `bank` |
| `cossacks_estate` | 1274 | 哥萨克 | 0.5 | 有**阶层私兵**参数，兵种偏轻骑 |

## 字段全表（标量字段 × 8 阶层）

`—` = 该阶层没写这个字段（继承默认）。数值为文件原文。

| 字段 | 王室 | 贵族 | 教士 | 市民 | 平民 | 齐米 | 部落 | 哥萨克 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `tax_per_pop` | 0 | 100 | 25 | 40 | 1 | 1 | 0.01 | 0.5 |
| `alliance` | 0 | 0.01 | 0.01 | 0.01 | 0.01 | 0.01 | 0.01 | 0.01 |
| `rival` | 0 | −0.01 | −0.01 | −0.01 | −0.01 | −0.01 | −0.01 | −0.01 |
| `color` | `estate_crown` | `pop_nobles` | `pop_clergy` | `pop_burghers` | `pop_peasants` | `estate_dhimmi` | `pop_tribes` | `map_cossack` |
| `bank` | — | yes | yes | yes | yes | yes | — | — |
| `can_generate_mercenary_leaders` | — | yes | **no** | yes | yes | **no** | yes | yes |
| `characters_have_dynasty` | — | **always** | sometimes | sometimes | never | never | never | never |
| `use_diminutive` | — | — | — | — | yes | yes | yes | yes |
| `disenfranchise_to` | — | — | — | — | `nobles_estate` | `nobles_estate` | — | — |
| `revolt_court_language` | `court_language` | `court_language` | **`liturgical_language`** | `common_language` | `common_language` | `common_language` | `common_language` | `common_language` |
| `can_spawn_random_characters` | **no** | — | — | — | — | — | — | — |
| `ruler` | **yes** | — | — | — | — | — | — | — |
| `priority_for_dynasty_head` | **yes** | — | — | — | — | — | — | — |
| `private_army_per_pop` | — | **0.1** | — | — | — | — | — | **0.1** |

## 六个块型字段（值本身是块）

| 字段 | 出现于 | 作用（实测） |
| --- | --- | --- |
| `satisfaction` | 8 个阶层 | **满意时给国家的加成**。例：贵族 `monthly_prestige = 0.2`、`monthly_legitimacy = 0.2`；其他阶层给 `global_raw_material_output`、`research_speed_modifier`、`levy_recovery_modifier`、`global_merchant_power`、`global_monthly_food_modifier`(×3 个阶层)、`tolerance_heathen` 等 |
| `high_power` | 7 个（王室除外） | **该阶层力量偏高时的后果**（约 30 个不同键）。贵族的写法最有代表性：`nobles_estate_max_tax = **−1.0**`（税额被砍）、`levy_combat_efficiency_modifier = 1.0`、`fort_maintenance_efficiency = 1.0`，外加 `monthly_legitimacy`/`monthly_republican_tradition`/`monthly_devotion`/`monthly_horde_unity`/`monthly_tribal_cohesion` = `high_power_estate_government_power_penalty`（**`main_menu\common\script_values\default_values.txt:995` = −0.2**） |
| `low_power` | 7 个 | **力量偏低时**：贵族 `nobles_estate_max_tax = 0.5`（税基放大）；另有 `pop_join_rebel_threshold`（2 个阶层）与 `*_estate_max_tax` 系列 |
| `opinion` | 7 个（王室除外） | **阶层对他国的观感**计算块（`desc = "ESTATE_OPINION_*"` 系列本地化键）。权重实测：同阶层文化 **+20**、同宗教 **+20**、忠诚附属 **+10**、宫廷语言相同/语言相通各 **+5**、双方皆君主制 **+5**、对方阶层力量 ×**5**、威望差 ×0.05、权力投射差 ×0.1；负面：被歧视文化 **−50**、军事弱于对方 **−10**、艺术水准低 **−10**、邻国 **−5**、不忠附属 **−10** |
| `power` | **仅 `crown_estate`** | 王室力量相关的国家效果：`global_estate_max_tax`、`revoke_privilege_cost_modifier`、`parliament_base_support`、`change_policy_cost_modifier`、`country_cabinet_efficiency`、`tariff_income`、`trade_income`、`power_projection`、`estate_building_destruction_satisfaction_impact`、`maintain/remove_bureaucracy_price_cost_modifier` |
| `private_army_unit_categories` | 贵族、哥萨克 | 见下节 |

## 阶层私兵（1.4 系统）

**数据侧只有两个阶层配齐了参数**（想给别的阶层开，两处都要补）：

| 阶层 | `private_army_per_pop` | `private_army_unit_categories`（权重） |
| --- | --- | --- |
| 贵族（L305–312） | 0.1 | 重骑 **2.0** / 轻骑 1.5 / 重步 1.0 / 轻步 0.5 |
| 哥萨克（L1417–1422） | 0.1 | 轻骑 **2.0** / 重骑 1.0 |

- **规模随该阶层 POP 数缩放**（`per_pop`），权重决定募什么兵、每团占多少额度 → 贵族私兵天然重骑为主、哥萨克轻骑为主。
- **开关不在本文件**：是国家修饰符 `*_estate_allowed_private_army`（**8 个阶层各一个**，`main_menu\common\modifier_type_definitions\00_modifier_types.txt:18596–18652`）。原版只给贵族与哥萨克配了特权（`estate_privileges\nobles_estate.txt:1827`、`cossacks_estate.txt:2`）——**"两代贵族私兵"的对照与"征用私兵"闭环见 `fields\common-estate_privileges.md`**。
- 三态计数：`estate_total_regiment_count` / `estate_raised_regiment_count`（服役中）/ `estate_unraised_regiment_count`（尚未征召）；`trigger_localization\estate_triggers.txt:57–72`。
- UI：`Estate.IsPrivateArmyAllowed` 控制阶层面板显隐（`in_game\gui\government_lateralview.gui:355`），提示走引擎的 `Estate.GetPrivateArmyInfo`；玩家标签「私人军队」「私人军团：$CURRENT$/$MAX$」「私人军队维护费」（`interfaces_l_simp_chinese.yml:1026/3912/3913`）。
- **未在数据文件中说明**：上限的确切取整公式、维护费金额、"服役中/未征召"的切换时机（引擎侧）。

## 审查要点

- 阶层内部名（`*_estate`）须与 `common\estate_privileges\`、`trigger_localization`、本地化键一致；**中文名以本体为准**（`peasants_estate` = 平民）。
- `color` 指向的是**调色板键**（`pop_*` / `estate_*` / `map_*` 三种前缀混用），不是十六进制色值。
- `satisfaction` / `high_power` / `low_power` 里的键都是**修正名**，必须能在本体修正注册表里找到（否则静默无效）——核对用 `fields\main_menu-modifier_type_definitions.md`。
- `opinion` 块是**脚本值语法**（`add` / `desc` / `multiply`），不是普通取值表；改它会影响所有阶层的对外观感与外交 AI。
- 给非贵族/哥萨克阶层开私兵时，**必须同时**补 `private_army_per_pop` 与 `private_army_unit_categories`，否则有开关但没有额度与兵种构成。
- 未在本体中说明：`estates` 与 `estate_types` 目录的分工（本文件即数据本体，`estate_types` 只是类型壳）。

## 中文检索键

| 中文 | 内部名 |
| --- | --- |
| 阶层 / 阶层满意度 / 阶层力量 / 阶层税基 | `estate` / `estate_satisfaction` / `estate_power` / `estate_tax_base` |
| **阶层外交倾向** | `estate_opinions`（`opinion` 块计算，官方概念译名即"阶层外交倾向"） |
| 贵族 / 教士 / 市民 / **平民** / 齐米 / 部落 / 哥萨克 / 王室 | `nobles_estate` / `clergy_estate` / `burghers_estate` / `peasants_estate` / `dhimmi_estate` / `tribes_estate` / `cossacks_estate` / `crown_estate` |
| 私人军队 / 私人军团 / 私兵 | `private_army_per_pop` / `private_army_unit_categories` / `*_estate_allowed_private_army` |
| 阶层税额上限 / 力量过高的政府惩罚 | `*_estate_max_tax` / `high_power_estate_government_power_penalty`（−0.2） |
| 剥夺权利（平民/齐米 → 贵族） | `disenfranchise_to` |
| 叛乱宫廷语言 / 礼仪语言 | `revolt_court_language` / `liturgical_language` |
