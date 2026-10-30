# 参考资料：名称陷阱（名字 ≠ 语义）

> **用途**：本库最容易"望文生义"的地方集中在这一页。**任何"看名字就下结论"的假设，先来这里对一眼**。
> **来源**：EU5 1.4.5 本体实查（defines / readme / exe 字符串）+ 本库已实跑验证的结论（标 ✅）；推断标 🟡；未验证标 ⚠️。
> **体量**：约 6 分钟
> **原则**：名字是**标签**，不是定义；真正语义只认 tooltip / 原注释 / 实跑日志。

## 一、脚本与合并
| 名字/写法 | 按字面会读成 | 实际 | 后果 | 详见 |
|---|---|---|---|---|
| `EDIT:` 前缀 | 一个可用的合并前缀 | **不存在**（1.4.5 exe 里 0 命中） | 写了也不生效/报错 | `guides\merging.md` §一 |
| 同名 `.txt` 覆盖 | "同块合并、只改没写的键" | **整文件覆盖**（同路径影子） | 漏写的键直接消失 | `guides\merging.md` §二 |
| 重复定义同名 ID/键 | "后加载的覆盖先加载的" | **先处理者胜，后到被丢弃**（日志 `Duplicated key … will not be created`） | 后写的静默失效 | `guides\merging.md` 🧪 |
| 本地化同键 | 同上（先到先得） | **相反：后加载的覆盖**（`EXIT` 实测被 mod 覆盖） | 与合并规则方向相反，别类推 | `guides\localization.md` |
| `INJECT` / `REPLACE` | "无条件插入/替换" | **目标必须已存在**；只有 `INJECT_OR_CREATE` / `REPLACE_OR_CREATE` 才新建 | 目标不存在 → 报错/不生效 | `guides\merging.md` §一 |
| `TRY_INJECT` / `TRY_REPLACE` | 与上面等价 | 🟡 推断：失败时**不报错**（"尝试"语义） | 失败静默，难排查 | 待测 |
| `HOURS_PER_PHASE` / `MINIMUM_COMBAT_DURATION` / `COMBAT_HOURLY_MORALE_TICK` / `BOMBARD_HOURS` | 仍是有效常量名 | **1.4.x 已改名为** `DAYS_PER_PHASE` / `MINIMUM_COMBAT_DAYS` / `COMBAT_DAILY_MORALE_TICK` / `BOMBARD_DAYS`；旧名在 1.4.5 defines **0 命中** | 老 mod 覆盖旧名**静默失效** | `vanilla\vanilla-combat.md` §三 |
| `metadata.json` 的 `supported_game_version` | "版本不符不能加载" | **只警告**（日志 `does not match game version`），照常加载 | 别拿它当版本闸门 | `guides\mod-skeleton.md` |

## 二、单位与战斗（详见 `reference\unit-attributes.md`）
| 名字 | 按字面会读成 | 实际 | 状态 |
|---|---|---|---|
| `max_strength`（unit_categories 里） | "该类单位的最大兵力" | **类目里全是 0**，真正数值在 `unit_types` / 时代模板；改类目无效 | ✅ 实查 |
| `damage_taken` | 通用伤害键（readme 应列） | `unit_categories\readme.txt` **没列**，但类目里实际在用（轻/重步 1.0、炮/辅 1.25） | ✅ 实查 |
| `initiative` | "先攻/出手顺序" | **接敌概率**；`0.60 + min(initiative×0.025, 0.30) + 0.03×天数`，**initiative > 12 不再加** | ✅ 常量+注释 |
| `combat_speed` | "移动速度" | **战斗速度**（攻击频率/撤退，`COMBAT_SPEED_SCALE=0.1`）；移速是另一个字段 `movement_speed` | ✅ |
| `frontage` | "阵形宽度" | **交战面占位**（决定同时能接敌的单位数）；地形削减后最低 `MIN_FRONTAGE_AFTER_TERRAIN=2`；侧翼允许 25% 超叠 | ✅ 常量 / 🟡 精确规则 |
| `flanking_ability` | "侧翼伤害倍率" | 只有取值（1.0~1.6、±0.1~0.3），**折算规则未知** | ⚠️ 未验证 |
| `secure_flanks_defense` | "侧翼防御值" | 同上 | ⚠️ 未验证 |
| `combat = { … }`（unit_categories） | "战斗技能/属性" | **按海域/地形的战斗修正块**（桨帆 `deep_ocean −1.0`、`narrows +0.5`） | ✅ 实查 |
| `combat_power` | "绝对战斗力" | 🟡 更像**乘数/指数**（0.25~7.5，随时代翻倍），不是可直接相加的兵力 | 🟡 |
| `AVERAGE_FRONTAGE` / `INITIATIVE_COMBAT_STRENGTH_FACTOR` / `AI_FLANKING_COMBAT_STRENGTH_FACTOR` | 结算公式常量 | 在 **NAI 段**，🟡 属 **AI 评估用**，不是战斗结算 | 🟡（未实测） |
| `LAND_LEVY_COMBAT_IMPACT` / `NAVAL_LEVY_IMPACT` | "征召兵影响描述" | **硬乘数**：0.75 / 0.05 | ✅ 常量 |
| `is_garrison` | "是否为驻军" | 全军**有且仅有一个** army 类目应为 yes | ✅ readme |
| `repair_demand` | 类目专属需求 | **三级回退**：单位级 → 类目级 → `construction_demand` | ✅ readme |

## 三、经济与阶层
| 名字 | 按字面会读成 | 实际 | 详见 |
|---|---|---|---|
| `ECONOMICAL_BASE_FROM_TAX_BASE` | "经济基础取自税基的权重" | **遗留错误命名**：tooltip 对应行是 `ECO_BASE_TAX`，文字写 **wealth** → 实际乘的是**财富** | `guides\economy-tax-base.md` L85、`guides\defines.md` L53 |
| `tax_base_ratio` | "税率" | **税基 / 财富** 的比值（defines 阈值注释：≥0.75 绿、≥0.5 黄、≥0.25 橙） | `reference\defines.md` `TAX_BASE_RATIO_*` |
| `estate_enrichment` | "阶层变富（好处）" | **阶层结余**：从阶层税基里**先扣**的一块再算税；修正注册表 **country / percent / color=bad** | `guides\economy-tax-base.md` L49 |
| `tax_base` vs `tax_base_ratio` | 同一个东西 | 前者是量，后者是比例；改修正时别混用 | 同上 |

## 四、人口
| 名字 | 按字面会读成 | 实际 | 详见 |
|---|---|---|---|
| `*_desired_pop` | "期望/目标人口" | **岗位容量**（number，如 `local_burghers_desired_pop = 4.0`）；`*_desired_pop_scaled` = 每 1,000 人口的容量（percent） | `reference\modifiers-location.md` |
| `*_pop_growth`（global/local 都有） | "global 是百分比、local 是加值" | **两者都是 percent**（注册表原文）；差别在作用域不在单位 | `reference\modifiers-country.md` / `modifiers-location.md` |
| `LOCAL_POP_GROWTH_UTIL` | "本地人口增长的百分比" | util 常量（除以人口数），**不是增长倍率** | `reference\defines.md` |
| `FOOD_SURPLUS_POP_GROWTH` / `FOOD_STORAGE_POP_GROWTH` | 人口增长倍率 | 每点盈余/库存粮的**增长点数**（0.0015 / 0.001） | 同上 |

## 五、scope 与文档命名
| 名字 | 说明 |
|---|---|
| `religion_group` / `government_type` / `goods_demand` / `cardinal` | wiki 把它们列成 scope，实际是**事件目标键 / 数据块键 / 字段值**；见 `reference\scope-aliases.md` |
| `bool` / `integer_flag` / `rebel` / `subunit` / `land_ownerhip_rule` | wiki 旧名/拼错；引擎名分别是 `boolean` / `flag` / `rebels` / `sub_unit` / `land_ownership_rule` |
| `none/any` | **文档记法**，不是脚本 token（脚本里 0 处） |
| wiki 页面版本标注（Trigger 1.1 / Scope pre-release / Effect 1.2） | 落后于 1.4.5；**以生成表为准** |

## 六、长期规则
1. 看到"名字像 X"的字段/常量/前缀：**先查本页**，再查对应档；都没有就按 🟡/⚠️ 标着用。
2. 新发现的名称陷阱 → **加一行到本页**（附证据：路径:行 / 日志 / 实测）。
3. 1.4.x 这类"常量改名"要特别小心：**旧名静默失效**，不报错。
4. 本页只收"名字会骗人"的；名字与语义一致的**不收**。
