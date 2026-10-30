# 参考资料：兵种属性速查（unit attributes）

> **用途**：查"某个兵种属性是什么、在哪个文件定义、受哪些常量影响、目前验证到什么程度"——把散在 `fields\common-unit*.md`、`vanilla\vanilla-combat.md`、`guides\defines.md` 里的内容合成一页。
> **来源**：本体 `common\unit_categories\` / `common\unit_types\` 的 `readme.txt` 与实查 + `loading_screen\common\defines\00_defines.txt`（NUnit/NCombat/NAI/NLocation），**EU5 1.4.5**，2026-10-30 整理。
> **体量**：约 6 分钟
> **验证状态**：✅ = 本体文件/注释可证；🟡 = 由字段名与常量注释**推断**；⚠️ = **未验证**（需实测，见 §六）

## 一、陆战属性总表

| 属性 | 作用（现状认知） | 定义位置 | 相关常量 | 状态 |
|---|---|---|---|---|
| `max_strength` | 单 Regiment 的**兵力上限**；`REGIMENT_SIZE = 1000` 是基准，实际按值缩放 | `unit_types`（**类目里全是 0，改类目无效**） | `NUnit.REGIMENT_SIZE = 1000` | ✅ |
| `combat_power` | 兵种的**战斗强度**，进伤害结算；时代模板按代翻倍（0.5→3 / 炮 2→7.5） | `unit_types` | 伤害通道 `NCombat.LAND_STRENGTH_DAMAGE_MODIFIER = 0.04` | ✅ 取值 / 🟡 进入公式的精确方式 |
| `initiative` | **主动性**：决定"接敌概率"；超过 12 的部分不再加接敌率 | `unit_categories` / `unit_types` | `INITIATIVE_BASE_CHANCE 0.60`、`CHANCE_EACH 0.025`、`CHANCE_PER_DAY 0.03`、`CHANCE_MAX 0.30` | ✅ |
| `frontage` | **正面宽度**：决定一个兵种占多少交战面（进而影响同时能接敌的单位数）；地形会削减，最低 `MIN_FRONTAGE_AFTER_TERRAIN = 2`；侧翼允许 **25% 超叠**（`MAX_FRONTAGE_OVERSTACKING = 1.25`） | `unit_categories` / `unit_types` | `NCombat.MAX_FRONTAGE_OVERSTACKING 1.25`、`NLocation.MIN_FRONTAGE_AFTER_TERRAIN 2` | ✅ 常量 / 🟡 交战位规则 |
| `flanking_ability` | **侧翼能力**：侧翼加成相关（原版类目 1.0~1.6，部分兵种 ±0.1~0.3） | `unit_categories` / `unit_types` | 无直接常量；AI 侧有 `AI_FLANKING_COMBAT_STRENGTH_FACTOR 0.3`（**NAI 段，仅供 AI 评估**） | ⚠️ 折算规则未验证 |
| `secure_flanks_defense` | 侧翼**防守**（类目 0 / 0.05；兵种 0.025） | `unit_categories` / `unit_types` | — | ⚠️ 规则未验证 |
| `combat_speed` | 战斗速度：影响攻击频率与撤退（`COMBAT_SPEED_SCALE = 0.1`，1.4.x 从 0.05 上调） | `unit_categories` / `unit_types` | `NCombat.COMBAT_SPEED_SCALE 0.1` | ✅ 常量 / 🟡 tick 换算 |
| `damage_taken` | 承受伤害倍率（轻/重步 1.0，炮/辅 1.25）——**readme 未声明但在用** | `unit_categories` | — | ✅ 取值 / 🟡 与强度的乘算 |
| `morale_damage_taken` / `strength_damage_taken` | 士气/强度**受伤**修正 | `unit_categories` | `LAND_MORALE_DAMAGE_MODIFIER 0.25`、`LAND_STRENGTH_DAMAGE_MODIFIER 0.04` | ✅ 字段 |
| `morale_damage_done` / `strength_damage_done` | 士气/强度**输出**修正 | `unit_categories` | 同上 | ✅ 字段 |
| `supply_weight` | 补给重量（影响补给消耗） | `unit_categories` | — | ✅ 字段 |
| `attrition_loss` | 损耗倍率（如炮兵 0.5） | `unit_categories` | `NUnit` 损耗系常量 | ✅ 字段 |
| `food_storage_per_strength` | 每点兵力需要的粮储（陆军类目 0.5/1.0/2.0） | `unit_categories` | `NUnit.DEPOSIT_FOOD_FRACTION 0.25` 等 | ✅ |
| `food_consumption_per_strength` | 每点兵力粮食消耗（炮兵 0.66） | `unit_categories` | — | ✅ |
| `movement_speed` | 兵种移速修正 | `unit_categories` | `NUnit.ARMY_MOVEMENT_SPEED 0.9` | ✅ |

## 二、海军属性

| 属性 | 作用 | 定义位置 | 状态 |
|---|---|---|---|
| `cannons` | 火炮数（重型船 30→120） | `unit_types` | ✅ |
| `hull_size` | 船体（重型 15→60） | `unit_types` | ✅ |
| `crew_size` / `transport_capacity` | 船员 / 运载量（运输船 0.25→1.5） | `unit_types` | ✅ |
| `maritime_presence` | 海上存在（`ship_*_maritime` 命名档） | `unit_types` | ✅ |
| `blockade_capacity` | 封锁能力（运输船 0.01） | `unit_categories` / `unit_types` | ✅ |
| `anti_piracy_warfare` | 反海盗（桨帆 0.2、轻型船 1.0、重型 0.1、运输 0.05） | `unit_categories` | ✅ |
| `repair_demand` | 补员/修理的货物需求（回退：单位→类目→`construction_demand`） | `unit_types` | ✅ |
| `transport` / `exclude_from_combined_arms` | 运输标记 / 不参与合成兵种 | `unit_categories` | ✅ |
| **`combat = {}`**（海域克制） | 海战按**海域**改战斗值：桨帆 `deep_ocean −1.0`/`narrows +0.5`；重型船 `inland_sea −0.25`/`narrows −0.40` | `unit_categories` | ✅（1.4.1 新增） |

## 三、数值在哪一层（改之前先看）

| 层 | 位置 | 说明 |
|---|---|---|
| **类目** | `common\unit_categories\*.txt` | 决定**站位/职能**属性（frontage/initiative/combat_speed/flanking、补给、海域克制）；**`max_strength` 在类目里全是 0**，改它无效 |
| **兵种** | `common\unit_types\*.txt` | 决定 `max_strength`、`combat_power` 等实际数值；`copy_from` 继承、`upgrades_to` 升级链 |
| **时代模板** | `unit_types\00_age_templates_land.txt` / `_navy.txt` | 数值随时代翻倍（陆 0.5→3.0；炮 2→7.5）；海军 1.4.1 重写（hull 15→60、cannons 30→120） |

> 口诀：**类目管"怎么打"，兵种管"打多狠"，模板管"什么时代多狠"**。

## 四、关键常量（1.4.5）

| 块 | 常量 | 值 | 备注 |
|---|---|---|---|
| NUnit | `REGIMENT_SIZE` | 1000 | 基准团规模 |
| NUnit | `LAND_MORALE` / `NAVAL_MORALE` | 3.0 / 3.0 | 士气满值 |
| NCombat | `INITIATIVE_BASE_CHANCE` | 0.60 | 接敌基础概率/日 |
| NCombat | `INITIATIVE_CHANCE_EACH` | 0.025 | 每点 initiative 的接敌加成 |
| NCombat | `INITIATIVE_CHANCE_PER_DAY` | 0.03 | 随战斗天数增长 |
| NCombat | `INITIATIVE_CHANCE_MAX` | 0.30 | initiative 部分封顶（等效 initiative 12 饱和） |
| NCombat | `MAX_FRONTAGE_OVERSTACKING` | 1.25 | 侧翼 25% 超叠 |
| NCombat | `COMBAT_SPEED_SCALE` | 0.1 | 战斗速度 → 攻击频率/撤退 |
| NCombat | `LAND_STRENGTH_DAMAGE_MODIFIER` / `LAND_MORALE_DAMAGE_MODIFIER` | 0.04 / 0.25 | 强度/士气伤害通道 |
| NCombat | `MORALE_SIZE_RATIO_MAX` | 4.0 | 兵力差对士气伤害的缩放上限 |
| NLocation | `MIN_FRONTAGE_AFTER_TERRAIN` | 2 | 地形削减后的最低正面宽度 |
| **NAI**（**AI 评估用，不是结算**） | `AVERAGE_FRONTAGE` | 12 | AI 假设的平均正面宽度 |
| **NAI** | `INITIATIVE_COMBAT_STRENGTH_FACTOR` | 0.025 | AI 把 initiative 折算成战斗力的系数 |
| **NAI** | `AI_FLANKING_COMBAT_STRENGTH_FACTOR` | 0.3 | AI 对侧翼优势的估值 |

## 五、已确认的公式片段
- **接敌概率** ≈ `0.60 + min(initiative×0.025, 0.30) + 0.03×已打天数`（封顶 1）——见 `vanilla\vanilla-combat.md` §3.2。
- **超限击溃**：首日 10:1（`INITIAL_OVERRUN_FACTOR`）、士气崩后 2:1（`MORALE_OVERRUN_FACTOR`）。
- **经验减伤**上限 50%（`LAND_EXPERIENCE_DAMAGE_REDUCTION 0.5`）。
- **未接敌**单位吃 `1.1`（强度）/`1.2`（士气）倍率（`NOT_ENGAGED_*`）。

## 六、⚠️ 未验证（需实测，别当结论）
1. **`frontage` 的精确规则**：常量的存在（25% 超叠、地形后最低 2）支持"决定交战位"，但**具体怎么算同时接敌数**没有文档/实证。
2. **`flanking_ability` 如何折算**：只有取值分布（1.0~1.6、±0.1~0.3），无公式。
3. **`secure_flanks_defense`**：同上。
4. **`combat_power` → 强度伤害**的乘算方式（通道常量已知，系数/随机项未知）。
5. **`combat_speed` → tick/攻击频率** 的精确映射（只知 `COMBAT_SPEED_SCALE=0.1`）。
6. **`max_strength` 与 `REGIMENT_SIZE`/strength 的换算**（团规模缩放细节）。
7. **NAI 常量是否只影响 AI**（按前缀推断为是；未实测）。

> 实测办法（与合并语义验证同套路）：做一个最小测试 mod，改单一属性（如某兵种 `frontage` +3），进游戏打一场、读 `logs\` 战斗日志/对比战损；或在控制台用 `test_event_trigger` 类手段构造条件。

### 实测步骤（把 ⚠️ 变 ✅ 的唯一办法）
1. 在测试 mod 里 **INJECT** 只改一个属性（例：给某 `unit_types` 兵种 `frontage` +3），其余不动；
2. 复制一份 playset 启用该 mod，开 **debug 模式**；
3. 构造**固定条件**战斗：同兵力、同地形、同兵种，只差分属性；
4. 看**战斗结算 tooltip / 战报**（日 tick 伤害、接敌单位数、侧翼提示）与 `logs\` 的变化；
5. 对照实验（改前/改后）各打 3 场，记录战损与持续时间差异；
6. 结论回填本档 §一/§二 的"作用"列，并把 ⚠️ 改为 ✅（附测试条件）。

> 也可看 `tools\review-checklist.md` 的"审查要点"里哪些能脚本判——能脚本判的别实跑。
## 七、相关档
- 字段权威：`fields\common-unit.md`（unit_categories 全字段 + 取值白名单）、`fields\common-unit_types.md`
- 机制与常量：`vanilla\vanilla-combat.md`（§二 单位体系、§三 战斗常数、§四 地形）、`vanilla\vanilla-army-movement.md`
- 常量全表：`guides\defines.md`、`reference\defines.md`
- 修正名：`reference\modifiers.md`（`*_army_*` / `*_navy_*` 类修正）
## 八、名称陷阱（别按字面读）
| 名字 | 字面误解 | 实际 |
|---|---|---|
| `max_strength`（类目里） | 类目最大兵力 | **类目里全是 0**；数值在 `unit_types`/模板 |
| `initiative` | 先攻/出手 | **接敌概率**（>12 不再加） |
| `combat_speed` | 移动速度 | **攻击频率/撤退**；移速是 `movement_speed` |
| `frontage` | 阵形宽度 | **交战面占位**（地形后最低 2、侧翼 25% 超叠） |
| `combat = {}` | 战斗技能 | **按海域/地形的战斗修正块** |
| `combat_power` | 绝对战力 | 🟡 乘数/指数，非可直接相加的兵力 |
| `NAI` 段常量 | 结算公式 | 🟡 **AI 评估用**（`AVERAGE_FRONTAGE` 等） |
| `flanking_ability` / `secure_flanks_defense` | 有明确折算 | ⚠️ 规则未验证 |

完整跨领域清单见 **`reference\name-traps.md`**（脚本/合并/经济/人口/scope 都有）。
