# 原版机制：雇佣兵（mercenaries）

> **什么时候看**：改雇佣兵池/合同/维护、写雇佣或"借兵"事件、调 `mercenaries_per_location` 时。
> **依据**：`defines\00_defines.txt` 的 **NMercenary** 全块、`common\unit_types\*.txt` 的 `mercenaries_per_location`、`prices\00_hardcoded.txt`、`script_docs`（触发器/效果）。**EU5 1.4.5**，2026-10-30。
> **体量**：约 5 分钟

## 一、雇佣兵是什么（与普通部队的区别）
- **独立实体**，有自己的 scope `mercenary`：可被雇佣、解雇、击溃、**出租到市场**、**借给别国**（lent unit）、甚至由**囚犯**转成。
- 触发器 38 条 / 效果 25 条（`reference\triggers.md` / `effects.md`），核心：
  | 类型 | 名字 |
  |---|---|
  | 存在/状态 | `has_mercenaries`、`is_mercenary`、`is_mercenary_of`、`is_mercenary_hired_by`、`is_mercenary_owned_by`、`is_mercenary_leader`、`has_customer`、`is_lent_unit`、`mercenary_has_owner` |
  | 成本 | `mercenary_hire_cost`、`mercenary_maintenance_cost`、`monthly_cost` |
  | 迭代 | `any_hired_mercenary`、`any_mercenary`、`any_mercenary_sub_unit`、`any_rented_out_mercenary`、`any_every_rented_out_mercenary` |
  | 效果 | `create_mercenary`、`hire_mercenary`、`become_hired_by`、`dismiss_mercenary`、`destroy_mercenary`、`make_unit_available_for_hire`、`lend_unit_to_country`、`recall_lent_unit`、`hire_prisoners_as_mercenaries` |

## 二、池子从哪来（关键：人口，不是金币）
- **`mercenaries_per_location`**（`unit_types` 的兵种块，location scope）：
  ```
  mercenaries_per_location = { pop_type = nobles  multiply = 0.2 }
  ```
  即：该地点 **nobles 人口的 20%** 可作为该兵种的雇佣兵来源。
- 全局上限（NMercenary）：
  | 常量 | 值 | 含义 |
  |---|---|---|
  | `MERCENARY_POOL_AVAILABILITY_SCALE_ARMY` | **0.1** | 地点人口中**最多 10%** 可成为陆战雇佣兵 |
  | `MERCENARY_POOL_AVAILABILITY_SCALE_NAVY` | **0.5** | 海军同项（船不是 1000 人团，所以单独给） |
  | `MERCENARY_MANPOWER_REPLENISH_TIME_DAYS` | **3600** | 人口"漂移"进雇佣兵池所需天数（约 10 年；注释：从 7200/240 月下调，原值太慢） |
  | `MERCENARY_ARMY_UNIT_MAX_SIZE` | 150 | 陆战雇佣兵单位规模上限 |
  | `MERCENARY_NAVY_UNIT_MAX_SIZE` | 25 | 海军同项 |

> 结论：**改招募成本/维护不会改变池子大小**；池子由 `pop_type × multiply` 与人口规模决定。

## 三、合同与维护
| 常量 | 值 | 含义 |
|---|---|---|
| `MERCENARY_DEFAULT_DURATION_IN_MONTHS` | 24 | 默认合同期 |
| `MERCENARY_MAX_DURATION_IN_MONTHS` | 120 | 最长合同期 |
| `MERCENARY_CONTRACT_ALERT_MONTHS` | 3 | 到期前 3 个月提醒 |
| `MERCENARY_MONTHLY_MAINTENANCE_MULTIPLIER` | **1.35** | 雇佣兵月维护倍率（比普通部队贵 35%） |
| `MERCENARY_PRISONER_MONTHLY_MAINTENANCE_MULTIPLIER` | 0.25 | 囚犯转雇佣兵的维护 |
| `MERCENARY_PRISONER_MAX_MORALE` | 0.6 | 囚犯兵士气上限 |
| `LENT_UNIT_VIRTUAL_CONTRACT_MONTHS` | 9999 | 借出单位的"虚拟合同"——**永不按时间到期**，靠战争/召回结束 |

## 四、时间与领导
| 常量 | 值 | 含义 |
|---|---|---|
| `MERCENARY_MOBILIZATION_TIME_BASE` | 2 | 动员时间基数 |
| `MERCENARY_GATHERING_TIME_BASE` | 7 | 集结时间基数 |
| `MERCENARY_RECRUITMENT_DISTANCE_FACTOR` | 0.025 | 距离对招募时间的影响 |
| `MERCENARY_SUBUNITS_AVAILABLE_PER_ADM_POINT` | 0.25 | **领袖行政（adm）越高，能带的雇佣兵越多** |
| `MERCENARY_LEADER_BASE_PRICE` | 50 | 雇佣兵领袖基础价 |
| `MERCENARY_LEADER_ABILITY_WEIGHT` | 50 | 能力权重（指数放大：高能力贵得多） |

价格对象：`price:hire_mercenary_leader`、`price:hire_mercenary_premium`（`prices\00_hardcoded.txt`）。

## 五、改 vs 硬编码 / 坑
| 想改 | 动这里 | 注意 |
|---|---|---|
| 池子大小/来源 | `unit_types` 的 `mercenaries_per_location` | 按 `pop_type` 与人口比例 |
| 上限比例/补员速度 | `NMercenary` 常量 | 3600 天有"从 7200 下调"的注释，改前想清楚 |
| 合同/维护 | `NMercenary` 常量 | 月维护是**倍率**不是绝对值 |
| 借出/召回 | 效果 `lend_unit_to_country` / `recall_lent_unit` | 虚拟合同 9999，别指望自动到期 |
| 雇佣成本 | `mercenary_hire_cost` 触发器 + `price:` 对象 | 只影响价格，不影响池子 |

**坑**：
1. 雇佣兵是**实体**，不是普通 regiment——普通部队的字段不适用。
2. **池子看人口**：想加雇佣兵来源要改 `mercenaries_per_location`，不是改金币。
3. 借出单位**不会按合同到期**（9999），只能战争结束或 `recall_lent_unit`。
4. 囚犯也能变雇佣兵（`can_hire_prisoners_as_mercenaries` / `hire_prisoners_as_mercenaries`），维护与士气另算。

## 六、相关档
- 单位体系/属性：`vanilla\vanilla-combat.md` §二、`reference\unit-attributes.md`
- 征兵与人力：`vanilla\vanilla-levies-and-manpower.md`
- 军队移动/补给：`vanilla\vanilla-army-movement.md`
- 触发器/效果：`reference\triggers.md`、`reference\effects.md`
