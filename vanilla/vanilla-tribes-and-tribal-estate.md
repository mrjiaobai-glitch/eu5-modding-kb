# 部落民与部落阶层（tribesmen & tribes_estate）

> **一句话**：EU5 的部落民是**唯一不吃粮、不纳税、永不被同化、且只能单向变成农民**的 POP 类型；它的阶层 `tribes_estate` **政治力量单价全阶层最高**（`power_per_pop` 0.25 = 农民的 10 倍），18 项原版部落特权的定价逻辑全是"**抬阶层力量换分散红利**"。本篇把 POP / 阶层 / 文本 / 机制四层定义连同**全部可用杠杆**一次列清。
> **什么时候看**：给部落加特权、改部落民人口或晋升、查某个 `tribes_estate_*` 键有没有先例、或要决定"谁算部落民"时翻这篇。
> **体量**：238 行 · 约 11 分钟通读
> **最后校验**：EU5 **1.4.1** / 2026-10-17（本篇全部行号按此版本）

来源：`in_game\common\pop_types\00_default.txt`（**无 readme**）、`estates\00_default.txt`、`estate_privileges\tribes_estate.txt`（283 行 / **18 项**）＋ 7 个兄弟档、`scripted_triggers\pop_triggers.txt`、`cabinet_actions\settle_tribesmen.txt`、`generic_actions\estate_emergency_actions.txt`、`unit_types\0_tribal.txt`、`laws\00_tribes.txt` + `01_common.txt`、`prices\00_hardcoded.txt` + `08_estate_emergency.txt`、`loading_screen\common\defines\00_defines.txt`、`main_menu\common\script_values\default_values.txt`、`main_menu\localization\simp_chinese\`。全部为 2026-10-17 实查。

---

## 一、POP 层：`pop_types\00_default.txt:209-236`

| 字段 | 值 | 推论 |
| --- | --- | --- |
| `power_per_pop` | **0.25** | 每 pop 折算的政治力量。 peasants **0.025**（10 倍）、soldiers 0.0625、burghers 4、clergy 10、nobles 25 |
| `promote_to` | **仅 `peasants`** | 唯一晋升出口是农民 ⇒ **永远无法自然上升为贵族/教士/市民** |
| `pop_food_consumption` | **0.0** | 完全不消费粮食（其余类型 1.0–20.0）⇒ 粮食类修正对他空转 |
| `assimilation_conversion_factor` | **0.0** | **不被同化、不改宗**（农民 1.0、劳工/士兵 0.5、上层 0.05） |
| `has_cap` | **缺失** | 无人口上限（nobles/clergy/burghers/laborers/soldiers 都有 `has_cap = yes`；peasants/slaves 同样无） |
| `promotion_factor` | **缺失** | 晋升不由它管，由 `tribal_rules` + `local_tribal_promotion` 管 |
| `tribal_rules` | `yes` | 原注释：`#can have special promotion factors` |
| `pop_percentage_impact` | `local_distance_from_capital_speed_propagation = -0.5`、`local_monthly_control = -0.0005` | **他们在哪，哪的控制力就低、离首都传播就慢**——"难管理"的量化 |
| `literacy_impact` | `local_tribal_promotion = 0.5` | **识字率直接加速部落民→农民** |
| `migration_factor` | 0.1 | 与农民同级（市民 0.5、士兵 0.05） |
| `city_graphics` | 0.01 | 城市视觉权重极低（农村属性） |

⚠️ `promote_to` 在**所有类型里只有 tribesmen 指向 peasants 且没有第二条出口**——这是"部落民是人口蓄水池"的机制根源。

## 二、阶层层：`estates\00_default.txt:1088-1272`（185 行）

| 字段 | 值 / 内容 |
| --- | --- |
| `tax_per_pop` | **0.01**（哥萨克 0.5、贵族 100）⇒ 基本不纳税 |
| `can_generate_mercenary_leaders` | `yes`（nobles/burghers/peasants/tribes/cossacks 为 yes；clergy/dhimmi 为 no） |
| `characters_have_dynasty` | **`never`** ⇒ 部落民出身人物无王朝 |
| `use_diminutive` / `revolt_court_language` | `yes` / `common_language` |
| `satisfaction` 块（× 满意度超出阈值部分） | `global_raw_material_output = 0.05` + `global_monthly_food_modifier = monthly_food_productivity_mild_bonus` |
| `high_power` 块（× 力量超出阈值部分） | `global_raw_material_output = 0.1`、`tribes_estate_max_tax = **-0.3**`、`monthly_towards_traditional_economy`，**并扣五种政府影响力**（`:1109-1113` legitimacy / republican_tradition / devotion / horde_unity / tribal_cohesion） |
| `low_power` 块 | `tribes_estate_max_tax = **+0.5**` |
| `opinion` 块（`:1122-1270`） | 同文化 +20 / 同宗教 +20 / 同语言 +5 / 目标为部落政体 +10；被歧视 **−50**、异教 −10、异端 −5、不同文化组 −5、宿敌 −20 |

**没有 `private_army_per_pop` / `private_army_unit_categories`**——全文件只有 `nobles_estate:305/307` 与 `cossacks_estate:1417/1419` 两处有。⇒ `tribes_estate_allowed_private_army = yes` 单独打开**没有额度**，必须同时改本档。

## 三、文本层（官方简中原文，设计意图的最直接证据）

- **POP 描述** `pops_l_simp_chinese.yml:15`：「居住于 [rural_locations] 的群体，作为**农民、牧羊人或渔猎者自给自足**生活，仍保持着自己**分化的社会结构**和组织，因此我们**很难有效管理**。」
- **阶层描述** `estate_l_simp_chinese.yml:15`：「以**血缘与宗族**组织为基础，主要分布在**农村地区**，常因**远离权力中心且相对独立而难以统治**。他们独特的习俗也常使他们被视为**令人畏惧、难以预测的战士**。」
- **阶层名按文化变体**（`:146-150`）：`tribes_estate_gaelic` 氏族 / `_barbaroi` 蛮族 / `_plemena` 部落 / `_qizilbash` / `_chibaloob`。

## 四、机制层：他们现在已经能做什么

| 机制 | 出处 | 关键数值 |
| --- | --- | --- |
| 征召兵种 `a_tribesmen` | `unit_types\0_tribal.txt:29-39` | `copy_from a_age_1_traditions_light_infantry`、`levy = yes`、走商品 `a_tribesmen_construction` / `a_tribesmen_maintenance` |
| 部落政体专属兵种 | 同档 `:1-27`（`a_moa_hunters`）、`:41-66`（`a_warriors`）、`:93-112`（`a_chieftains`） | `location_potential` 要求 `government_type = tribe` |
| 定居内阁行动 `settle_tribesmen` | `cabinet_actions\settle_tribesmen.txt` | 给 `local_tribal_promotion = **5.0**`；`is_finished` = 该省 tribesmen ≤ 0；`ability = mil` |
| 索取人力 `ask_tribes_for_manpower` | `generic_actions\estate_emergency_actions.txt:755-877` | 价 `political_influence 15` + `scaled_gold 6`（`prices\08_estate_emergency.txt:28`）；得**12 个月人力**；部落满意度 **−0.20**；冷却 **5 年** |
| 紧急征税 `estate_emergency_taxes_tribes` | `main_menu\common\static_modifiers\country.txt:1394` | `tribes_estate_max_tax = 0.10`，持续 12 个月 |
| 部落政体法律 6 条 | `laws\00_tribes.txt` | `tribal_legal_basis` / `_organization` / `_religious_values` / `_migration` / `_cultural_identity` / `_modernization`，全部 `potential = { has_tribal_government = yes }` ⇒ **非部落政体吃不到** |

## 五、阶层力量实算（为什么 2.5% 人口能有 15% 力量）

按 `power_per_pop` 直接换算（1337 开局 KTU 六省，`main_menu\setup\1337\06_pops.txt`）：

| 类型 | 人口 | 人口占比 | 力量 | 力量占比 |
| --- | --- | --- | --- | --- |
| peasants | 232.926 | 97.47% | 5.823 | 61.75% |
| **tribesmen** | **5.890** | **2.46%** | **1.473** | **15.61%** |
| clergy | 0.136 | 0.06% | 1.360 | 14.42% |
| nobles | 0.031 | 0.01% | 0.775 | 8.22% |

⇒ **部落民已是该国第三大政治力量**，折算 ≈ 58.9 个农民 pop。⚠️ 这是 `power_per_pop` 的裸换算（不含 `base_*_estate_power` 与内阁加成），只作量级参考。

## 六、18 项原版部落特权（`estate_privileges\tribes_estate.txt`，283 行）

| id | 简中名 | 给什么（节选） |
| --- | --- | --- |
| `tribes_land_rights` | 部落土地权 | `global_tribes_estate_power 0.25` + `monthly_towards_decentralization` |
| `tribes_allow_gatherings` | 允许集会 | 同上但走向 `communalism` |
| `tribes_pasture_access` | 授予牧场 | `expand_rgo_farming_cost_modifier −0.025` + power 0.2（**唯一不带满意度的一项**） |
| `tribes_tribal_levies` | 允许部落征召兵 | `tribes_estate_levy_size **0.3**` + power 0.33 |
| `tribes_tribal_registry` | 扩大户籍登记 | `global_tribesmen_pop_growth **0.1**` + power 0.5 |
| `tribes_protected_faith` | 保护部落信仰 | `global_tribal_promotion **−0.1**` + power 0.5（**压制变成农民**） |
| `tribes_respect_tribal_traditions` | 尊重部落传统 | `cultural_influence_modifier −0.1` + power 0.33 |
| `tribes_creach` / `tribes_fosterage` / `tribes_manrent` | 鼓励袭掠 / 寄养制 / 效忠契约制 | 苏格兰文化组限定；`global_livestock_output_modifier 0.05` / `research_speed_modifier 0.02` / `global_defensive 0.1` |
| `tribes_scotland_gaelic_heritage` ⇄ `autonomous_tribes_scotland_gaelic_heritage` | 共同的苏格兰传承（自治） | 互斥双态；前者 `global_monthly_control −0.0010` + `global_monthly_development 0.002`，后者 `global_tribes_estate_power **−0.10**` + `global_monthly_control 0.0005` + `monthly_army_tradition 0.2` |
| `jaysh_armies` | 加伊什军团 | `regiment_reinforcement_speed 0.05` + `monthly_towards_quantity` |
| `tribes_in_administration` | 内阁中的酋长 | **`tribes_estate_allowed_in_cabinet = yes`** + power 0.5 + 去中央集权 |
| `local_tribal_strongholds` | 部族据点 | `global_max_rural_control **0.05**` + power 0.5（马格里布限定，且不带满意度） |
| `mch_tribal_favor` | 首崇满洲 | 双 `country_modifier` 块 + `potential_trigger` |
| `tun_arab_bedouins` | 阿拉伯部落特权 | `pure_tooltip_entry = disables_tribal_cabinet_action` + power 0.33 |
| `desert_power` | 沙漠力量 | 双 country 块 + 双 `location_modifier`（沙漠/干旱，`potential_trigger` 用 `owner = { has_variable }`） |

**统计（实查）**：`country_modifier` 块 **20** 个（`mch_tribal_favor` / `desert_power` 各 2 块）· `location_modifier` 块 **2** 个（均在 `desert_power`）· `tribes_estate_target_satisfaction` **15/18 项**（缺的是 `tribes_pasture_access` / `autonomous_tribes_scotland_gaelic_heritage` / `local_tribal_strongholds`）· `pure_tooltip_entry` 3 处。
**`global_tribes_estate_power` 的取值档**：`0.2`×1 · `0.25`×5 · `0.33`×3 · `0.5`×8 · **`−0.10`**×1。

## 七、杠杆清单：键存在，但部落特权没用过

**先分清三种"没用过"**（三者含义完全不同，别混）：
- Ⓐ **脚本 0 处**（整个 `in_game\common` 只有注册表定义）⇒ 完全空白，无任何行为先例；
- Ⓑ **只在别处用**（laws / cabinet_actions / government_types / static_modifiers 用过，特权里没有）⇒ 有行为先例、无特权先例；
- Ⓒ **默认值已等于想给的值** ⇒ 这个键**给了也白给**（见 §七·补，最容易踩的坑）。

### Ⓐ 脚本 0 处

| 键 | 简体中文名 | 兄弟阶层的同名键有特权先例吗 |
| --- | --- | --- |
| `tribes_estate_allowed_to_build_rgo` | 允许扩建原产 | **有**：`nobles_estate.txt:23`、`peasants_estate.txt:26` |
| `tribes_estate_allowed_to_build_roads` | 允许修建道路 | **有**：`burghers_estate.txt:949` |
| `tribes_estate_allowed_private_army` | 允许私人军队 | **有**：`cossacks_estate.txt:9`、`nobles_estate.txt:1827` —— ⚠️ 但部落民阶层**无 `private_army_per_pop`**，单开无额度 |
| `tribes_estate_agenda_impact` | 议程影响 | **有**：`clergy_estate.txt:434`(=0.2)、`nobles_estate.txt:219/364/670`(0.25/0.2) |
| `tribes_estate_power_from_cabinet` | 内阁成员产生的阶层力量 | 无（仅 laws/advances 用兄弟键） |
| `tribes_estate_satisfaction_decay` | 满意度衰减 | 无 |
| `global_tribesmen_max_literacy` | 最大识字率 | **全库 0 处**；同族锚点 `5`（众数）/ `10`（常用上限），`already_percent=yes` |
| `global_tribesmen_desired_pop` / `_rural_` / `_city_` | 岗位容量 | **全库 0 处**（与建筑 `employment_size` 重复，实战已否决同类用法） |
| `global_tribesmen_conversion_blocked` | 禁止皈依 | 0 处（部落民本就 `assimilation_conversion_factor = 0`，冗余） |
| `ask_tribes_for_manpower_cost_modifier` | （**本地化未收录**） | 0 处；对应动作基础价 PI 15 + scaled_gold 6 |
| `local_tribes_estate_unrest` | 部落叛乱度 | 0 处 |

### Ⓑ 只在特权之外用过（可当风格先例）

`tribes_estate_allowed_noble_marriage`（`government_types`×2）· `tribes_estate_can_participate_in_parliament`（`laws\01_common.txt:2348`）· `tribes_estate_max_tax`（`estates`×2 + `static_modifiers`×2）· `tribes_estate_min_tax`（`static_modifiers`×1）· `tribes_estate_satisfaction_recovery`（`cabinet_actions\appease_estate.txt:140` = 0.001、`tyranny_of_the_capital_action.txt:182` = 0.0005）· `tribes_estate_allowed_leading_military`（`auto_modifiers\country.txt:188`，**是默认值不是用法**，见 §七·补）· `global_tribesmen_food_consumption`（`static_modifiers`×1）· `monthly_tribes_estate_rebel_growth`（`cabinet_actions`×1）。

### 已被特权用过的

`tribes_estate_allowed_in_cabinet`（特权内 1 处，`tribes_in_administration`）· `tribes_estate_levy_size`（1）· `global_tribesmen_pop_growth`（5）· `global_tribesmen_migration_allowed`（1，在 `cossacks_estate.txt:41`）· `global_tribesmen_assimilation_blocked`（1，在 `cossacks_estate.txt:160`——**由哥萨克特权保护部落民**）· `global_tribal_promotion`（1）· `block_tribal_promotion`（1，在 `cossacks_estate.txt:159`）。

## 七·补、先查默认值：`in_game\common\auto_modifiers\country.txt`（country base values）

这是**本篇最重要的一条方法论**：`*_estate_allowed_*` / `*_estate_can_participate_*` / `*_migration_allowed` 这些布尔键的**默认值**写在这里，**默认已为 `yes` 的键，特权里再写一遍 = 空操作**。

| base 值（实查行号） | 值 | 对部落的含义 |
| --- | --- | --- |
| `global_tribesmen_migration_allowed` `:168` | **no** | 部落民默认不迁移 ⇒ 给"允许迁移"**有效** |
| `nobles_estate_can_participate_in_parliament` `:175` | yes | **只有贵族**默认有议会席位；部落无此条 ⇒ 给"出席议会"**有效** |
| `nobles/clergy/burghers/crown_estate_allowed_in_cabinet` `:178-181` | yes | 部落**不在列表里**；`:182` 注释原文：*"they are by default not available until `tribes_estate_allowed_in_cabinet` is active"* ⇒ 给"入内阁"**有效** |
| `nobles_estate_allowed_noble_marriage` `:185` | yes | **只有贵族** ⇒ 给部落"允许通婚"**有效** |
| `tribes_estate_allowed_leading_military` **`:188`** | **yes** | 🔴 **部落默认就能指挥军队** ⇒ 给"允许指挥军队"**完全空操作**（兄弟阶层 clergy/dhimmi/cossacks 的特权写它是"从别的状态改回来"，部落不是） |
| `allowed_to_build_rgo` / `allowed_to_build_roads` / `allowed_private_army` / `agenda_impact` / `power_from_cabinet` | **base 里没有** | 默认按 `no` 处理（对照：贵族/农民靠特权才拿到开垦权）⇒ 给它们**有效** |

⇒ **结论：给部落民设计特权时，"解锁类"有效的是 开垦权 / 修路权 / 议会席位 / 内阁席位 / 通婚 / 迁移 / 私兵（需补 `private_army_per_pop`），而"指挥军队"是废牌。**

## 七·补2、哪些牌已经被"别的阶层特权"占了（2026-10-17 全量普查）

给部落设计特权时最容易犯的错：**挑了一张看起来很好的牌，其实别的阶层早就用它当招牌了**。`estate_privileges\` 8 个档全扫结果：

| 牌 | 特权内已有谁在用 | 给部落民是否重复 |
| --- | --- | --- |
| `*_migration_allowed` | 农民 ×4（`peasants_estate.txt:22/23/341/489`）、贵族 ×1（`:1447` 反向）、哥萨克 ×1——**且哥萨克那条直接给的就是部落民**（`cossacks_estate.txt:41 global_tribesmen_migration_allowed = yes`） | ❌ 重复（双重） |
| `global_tribesmen_pop_growth` | 哥萨克特权 **×4**（`cossacks_estate.txt:20/42/107/129`，0.05/0.1）、部落 ×1（`tribes_tribal_registry`） | ⚠️ 半重复 |
| `*_max_literacy` | 市民 ×2（5）、教士 ×4（5/10）、贵族 ×2（5） | ⚠️ 常规牌（补对称缺口可以，不算独创） |
| `research_speed_modifier` | 市民（0.1）、教士 ×2（0.05）、贵族 ×2（0.1/0.05）、部落（苏格兰 0.02） | ⚠️ 常规牌 |
| `cultural_influence_modifier` | 教士（0.15）、哥萨克（−0.15）、齐米（−0.15）、贵族 ×2（0.2/0.1）、农民（−0.15）、部落（−0.1） | ⚠️ 常规牌（6 个阶层都有） |
| `*_estate_agenda_impact` | 教士 ×2、贵族 ×3（0.2/0.25） | ⚠️ 常规牌 |
| `*_estate_satisfaction_decay` | 市民（0.05）、贵族（0.02） | ⚠️ 常规牌 |
| **`tribes_estate_satisfaction_recovery`** | **特权内 0 处**；全库仅两个内阁行动用过（`cabinet_actions\appease_estate.txt:140` = 0.001、`tyranny_of_the_capital_action.txt:182` = 0.0005） | ✅ **空白牌** |
| **`local_tribes_estate_unrest`** | **特权内 0 处；全库脚本 0 处**（只有注册表） | ✅ **空白牌**（⚠️ 但**没有任何取值先例**，用就得自推） |

⇒ 想让部落特权"不撞车"，可用的原创牌只有上表最后两行 + §七 的 Ⓐ 组；其余都是各阶层共用的常规牌。

## 八、四条硬约束（会让某些特权变成废牌）

1. **`assimilation_conversion_factor = 0.0`** ⇒ 部落民本来就不会被同化/皈依。原版 `tribes_protected_faith` 走的是 `global_tribal_promotion = −0.1`（**阻止他们变成农民**），不是"保护信仰"。任何"保护部落文化/信仰"的直给特权收益≈0。
2. **无 `has_cap` + 唯一出口是 peasants ⇒ 两条路线互斥**：给增长 = 加部落民；给 promotion = 把部落民变成农民。**同时给会自相抵消**，必须先选立场。
3. **`pop_food_consumption = 0.0`** ⇒ 粮食类修正对他空转，别浪费键位。4. **这个阶层买不起东西 ⇒ 所有"允许他自己动手"的牌都是死的。** 阶层花钱靠自己的税基，而税基由 `tax_per_pop` 决定：贵族 **100** / 市民 40 / 教士 25 / 农民·齐米 **1** / 哥萨克 0.5 / **部落 0.01**（`estates\00_default.txt:1090`）⇒ 部落民税基是农民的 **1%**。1337 年 KTU 六省实算：部落民 5.89 pop × 0.01 = **0.059**，农民 232.9 × 1 = **232.9** ⇒ 部落只占全国阶层税基 **0.03%**。所以 `tribes_estate_allowed_to_build_rgo` / `_allowed_to_build_roads` / `small_estate_building_cost_modifier` 这类"解锁/降价"牌对部落民基本无效。**更硬的限制：没有任何 modifier 能给阶层直接加钱**——`add_gold_to_estate` 是 effect（贿赂动作用），`tribes_estate_max_tax` 只是税率上下限（税基 0.059 上调它等于 0）。⇒ 部落特权只能落在"国家层面修正"或"规则解锁"上。

另：`high_power` 块按超出量扣**五种政府影响力**（`:1109-1113`）⇒ `global_tribes_estate_power` 抬得越高，正统性/共和传统/奉献度/游牧团结/部落凝聚力一起掉。

## 九、本体自己给的"部落治理四件事"（`laws\01_common.txt:2334-2363`）

法律 `amazigh_familial_connections`（`law_category = socioeconomic`，`potential` 要求首都位于 `region:maghreb_region` 且 `estate_power(tribes_estate) > 0`）的选项 `reliance_on_local_tribes`，把本体认为有效的四件事一次给全：

```txt
country_modifier = {
    global_tribes_estate_power = 1
    land_cost_on_distance_from_capital_speed_propagation = 0.1
    tribes_estate_can_participate_in_parliament = yes
}
location_modifier = {
    potential_trigger = { local_control < 0.25  any_pop = { pop_type = pop_type:tribesmen } }
    local_tribesmen_pop_growth = 0.15
}
estate_preferences = { tribes_estate }
years = 2
```

⇒ **本体承认的杠杆 = 阶层力量 / 首都距离成本 / 议会席位 / 低控制力(<0.25)地区的部落民增长**；对照选项 `cordial_relations`（:2364-2381）把同一个条件化增长写成 **0.05** ⇒ 这两个数是"温和 / 强化"两级锚点。

**`local_tribal_promotion` 的全部 5 处锚点**：建筑 `0.1`（`event_only_buildings.txt:142`、`religion_buildings.txt:189`）· 建筑 `0.5`（`foreign_buildings.txt:169`）· 内阁行动 `5.0`（`settle_tribesmen.txt:59`）· POP 识字产出 `0.5`（`pop_types:230`）。⇒ **特权级应取 0.1 量级，别抄内阁级的 5.0**。

## 十、`is_gaelic_clans`：部落阶层可由特权扩容（进阶）

`pop_types\00_default.txt` 在 nobles/clergy/burghers/laborers/soldiers/peasants 六处都写着 `tribes_estate = { is_gaelic_clans = yes }` —— 满足条件的**非部落民 POP 也归入部落阶层**。而它不是硬编码，是脚本触发器 `scripted_triggers\pop_triggers.txt:40-52`：

```txt
is_gaelic_clans = {
    culture ?= { language = language:gaelic_language  has_culture_group = culture_group:celtic_group }
    owner ?= { OR = { has_estate_privilege = estate_privilege:tribes_scotland_gaelic_heritage
                      has_estate_privilege = estate_privilege:autonomous_tribes_scotland_gaelic_heritage } }
    is_linked_to_foreign_building = no
}
```

⇒ "**谁算部落民**"是**可由特权开关的**（模板：文化组 + `owner ?= { has_estate_privilege = ... }`）。代价：`pop_types\00_default.txt` 是**单档**，要整份复制再改（注册语义风险等同 `static_modifiers`）。

## 十一、可改点与硬编码边界

| 想改什么 | 改哪里 |
| --- | --- |
| 加部落特权 | `in_game\common\estate_privileges\<你的文件>.txt`（字段见 `fields\common-estate_privileges.md`；**无 readme 之外的隐藏字段**） |
| 特权名与描述 | `main_menu\localization\<lang>\estate_l_<lang>.yml` 的 `<privilege_id>` + `<privilege_id>_desc` |
| 特权价格 | **不能只改一项**：`prices\00_hardcoded.txt:51` `grant_privilege = { political_influence = 20 }`、`:55` `revoke_privilege = { stability = 400 righteousness = 10 }`（全局，影响所有阶层） |
| 满意度档位 | `main_menu\common\script_values\default_values.txt:945-947`：`small/medium/large_privilege_target_satisfaction` = 0.025 / 0.05 / 0.10 |
| 部落民人口行为 | `pop_types\00_default.txt`（单档，注册语义） |
| 部落阶层数值 | `estates\00_default.txt`（单档；注意 `power_per_pop` 会连锁影响税基分配与王室力量） |
| 谁算部落民 | 自写 `scripted_triggers` + 改 `pop_types` 六处的 `tribes_estate = { ... }` |
| 定居速度 | `cabinet_actions\settle_tribesmen.txt`（5.0 是"主动定居一省"的标尺） |
| 索要人力 | `generic_actions\estate_emergency_actions.txt:755` + `prices\08_estate_emergency.txt:28` |

**边界（未在数据文件中说明，别猜）**：`estate_power` = `power_per_pop` × pop 数 的**确切归一化公式**（含 `base_*_estate_power` 与内阁贡献）在引擎侧；本篇 §五 的百分比只是裸换算量级。

## 十二、复现命令

```powershell
$g = 'F:\SteamLibrary\steamapps\common\Europa Universalis V\game'
# 1) 部落民 POP 定义
Select-String -Path "$g\in_game\common\pop_types\00_default.txt" -Pattern '^tribesmen\s*=\s*\{' -Context 0,28
# 2) 部落阶层块（1088..1272）
$t = Get-Content "$g\in_game\common\estates\00_default.txt" -Encoding UTF8
$t[1087..1271]
# 3) 18 项部落特权
Select-String -Path "$g\in_game\common\estate_privileges\tribes_estate.txt" -Pattern '^[a-z_0-9]+\s*=\s*\{'
# 4) 某键有没有先例（注意路径必须全部存在，否则 Select-String 整条失败、静默返回 0）
Select-String -Path "$g\in_game\common\*\*.txt","$g\main_menu\common\static_modifiers\*.txt" -Pattern '(^|[\s{])tribes_estate_allowed_to_build_rgo\s*='
```

> ⚠️ **本轮的教训**：`Select-String -Path` 只要有一个路径不存在就**整条命令报错并把结果当 0**（我因此一度误判"若干键全库 0 处"）。做"0 处"结论前，先 `Test-Path` 每个路径，或改用一个显式枚举出来的文件清单。