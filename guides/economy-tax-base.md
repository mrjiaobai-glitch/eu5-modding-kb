# 经济基础 · 税基 · 财富 · 税基比例（economy chain）

> **一句话**：EU5 经济是一条四段链——**财富(wealth)** 是地点卖货的利润，**税基(tax_base) = 财富 × 控制(control)**，**税**从各阶层按 `tax_rate` 抽，而**经济基础(economical_base) 是"贡献力"刻度、不算控制**；**税基比例 = 税基 / 财富**，衡量你的财富有多少能被政府变现。
> **什么时候看**：改税制/阶层税率、算"为什么国家看着强却没钱"、调控制(control)相关修正、或查 `ECONOMICAL_BASE_FROM_TAX_BASE` 这类 defines 键名的真实语义时翻这篇。
> **体量**：166 行 · 约 8 分钟通读

来源：`main_menu\localization\*/game_concepts_*.yml` 概念原文（英/简中/葡三版一致）、`main_menu\localization\english\core_l_english.yml` 的 `ECO_BASE_*` 拆解行、`loading_screen\common\defines\00_defines.txt`（`NEconomy` 2063–2076、`NGUI` 105–107）、`in_game\common\estates\00_default.txt`（`tax_per_pop`）、`main_menu\common\modifier_type_definitions\00_modifier_types.txt`、`in_game\gui\shared\country_tooltips.gui:225`（`EconomicalBaseTooltip`）。**1.4.x 实查。**

## 一、四段链（谁影响谁）

```
地点财富 wealth  = RGO + 建筑 把商品卖到市场的利润（有 burghers 时 burgher_trade 可能再 +）
      │
      ├─×(wealth_share) 分给各阶层 ──→ 阶层财富（nobles 人均 >> burghers >> peasants）
      │                                   │
      │                                   └─→ 阶层结余 estate_enrichment 扣减
      │
      ├─×control ──→ 税基 tax_base（政府够得着的部分；无 control 的财富直接进阶层口袋）
      │                   │
      │                   ├─ 按 estate_power / tax_per_pop 分给阶层 → 各阶层按 tax_rate(相对 max_tax) 上缴
      │                   │        × tax_income_efficiency → 国库
      │                   │
      │                   └─ 同时是：人力/水手恢复、市场参考、战争价值、通胀成本的锚
      │
      └─ 全额计入 ──→ 经济基础 economical_base（国家实力刻度，不进国库）
                          + 人口 + 附庸 + 制度 + 利息/关税/外国建筑
```

**一句话记法**：**财富是"产出"，税基是"能收"，经济基础是"多强"，税基比例是"能收 ÷ 产出"。**

## 二、财富 wealth 与财富份额 wealth_share

- **定义**（概念原文）："location 的财富是指其 **RGO 和 buildings 通过向 market 出售 goods 所获得的综合利润**。若该地点有 burghers，`burgher_trade` 也可能增加地点财富。"
- **分配**：地点财富按各阶层在该地点的 **`wealth_share`** 分配；词条明写"**nobles 的人均份额远高于 burghers，burghers 又远高于 peasants**"。
- **数据落地**：每阶层在 `estates\00_default.txt` 有 **`tax_per_pop`**，阶梯与之吻合：

| 阶层 | `tax_per_pop` | 出处 |
| --- | --- | --- |
| 贵族 nobles | **100** | L46 |
| 市民 burghers | **40** | L578 |
| 教士 clergy | **25** | L318 |
| 平民 peasants | 1 | L780 |
| 齐米 dhimmi | 1 | — |
| 哥萨克 cossacks | 0.5 | — |
| 部落 tribes | 0.01 | — |
| 王冠 crown | 0 | L3 |

- **再分配**：`local_peasant_enfranchisment`（**location, percent**，注册表 L4137）把**平民份额的一部分转给贵族**。
- **阶层结余 estate_enrichment**（"阶层结余"）：从阶层税基里**先扣掉**的一块再算税；修正是 **country, percent**（注册表 L6234，`color=bad`）。
- **财富影响 wealth_impact**：阶层**月收入 ÷ 月支出**；>100% 才有余钱投资建筑/道路。

## 三、税基 tax_base 与税

- **定义**（概念原文）："税基是一个 location 可能受 taxes 影响的 wealth 总量，**直接取决于该地区的 control**。没有 control 的财富会直接流向 estates，无法被征税。"
- **公式**：`税基 = 地点财富 × control`。
- **阶层分账**（`estate_tax_base` 词条）："各 estate 根据其与其他 estates 的**相对 estate_power** 从每个 location 获得一定比例 tax_base 的 gold，然后再减去 estate_enrichment。"
- **税档三层**：
  1. `tax_rate`（税率）= 你向该阶层征的税**占其 `max_tax` 的百分比**；`min_tax` 是允许下限；
  2. `max_tax`/`min_tax` 用修正调：**8 个阶层各一对 + global**，全部 **country/percent**：`crown/nobles/clergy/burghers/peasants/dhimmi/cossacks/tribes_estate_max_tax|min_tax`、`global_estate_max_tax|min_tax`（如 `nobles_estate_max_tax` L2245、`peasants_estate_max_tax` L2494）；
  3. 收进国库前乘 **`tax_income_efficiency`**（**country, percent**）；高税**降阶层满意度**（`ESTATE_TAX_SATISFACTION_THRESHOLD = 0.01`，NCountry）。
- ⚠️ **注册表里 0 个 `wealth` 键**（`*wealth*` 命中 0）——**财富/税基没有直接修正可调**；能动的只有税档、结余、效率、控制、释权。想加财政，实际路径是**提控制**或**改税率档**。

## 四、经济基础 economical_base（公式已确认算"财富"）

### 4.1 概念原文
- 简中/英文/葡文三版一致："基于国家的**总财富**和**人口**，**与 control 无关**；附庸数量与已接纳制度也增加它，利息/关税/外国建筑收入同样。"
- `tax_base_ratio` 词条："**每地点把全部财富计入 economical_base**。但 gold 只来自你征收的 tax_base，所以低比例=看起来强大但没钱。"

### 4.2 引擎口径证据（关键：键名是遗留错误）
经济基础 tooltip 由 `[Country.GetEconomicalBaseInfo]` 生成（`country_tooltips.gui:225–246`），拆解行文字键（`core_l_english.yml`）**直接把那一项写成 wealth**：

| 拆解行键 | 英文原文 |
| --- | --- |
| `ECO_BASE_TAX` | "**$TAX\|2$ [wealth\|e]: $VAL**" ← **不是 tax base** |
| `ECO_BASE_SUBJECT` | "**$TAX\|2$ [wealth\|e] from [subjects\|e]**" |
| `ECO_BASE_POP` | "$POP$ population: $VAL" |
| `ECO_BASE_TRADE_VALUE` / `_TRADE_PROFIT` | trade volume / trade profit |
| `ECO_BASE_INTEREST` / `_FOREIGN_BUILDINGS` / `_TARIFF` | interest / foreign buildings / tariffs |
| `ECO_BASE_INSTITUTIONS` | "$NUM$ institutions embraced: $VAL**%**" |

**9 个显示行 ↔ 9 个 `NEconomy` 常量，一一对应**：

| 显示行 | 常量（`00_defines.txt:2065–2076`） | 系数 |
| --- | --- | --- |
| wealth | **`ECONOMICAL_BASE_FROM_TAX_BASE`（遗留名，实为 wealth）** | **0.5** |
| population | `ECONOMICAL_BASE_FROM_POP` | 0.015 |
| trade volume | `ECONOMICAL_BASE_FROM_TRADE_VALUE` | 0.2 |
| trade profit | `ECONOMICAL_BASE_FROM_TRADE_PROFIT` | 0.2 |
| interest | `ECONOMICAL_BASE_INTEREST` | 0.25 |
| foreign buildings | `ECONOMICAL_BASE_FROM_FOREIGN_BUILDINGS` | 1.0 |
| tariffs | `ECONOMICAL_BASE_FROM_TARIFF` | 0.2 |
| subjects 的 wealth | `ECONOMICAL_BASE_FROM_SUBJECT`（遗留名，实为附庸财富） | 0.05 |
| institutions | `ECONOMICAL_BASE_SCALE_FROM_EACH_INSTITUTION` | 每项 **+6.6%** |

**合成公式（据上述一一对应）**：
```
economical_base ≈ 0.5×Σ地点财富 + 0.015×人口 + 0.2×贸易量 + 0.2×贸易利润
                + 0.25×利息收入 + 1.0×外国建筑收入 + 0.2×关税收入 + 0.05×附庸财富
                然后 ×(1 + 0.066×已接纳制度数)
```
- `ECONOMICAL_BASE_ALL_HAS_TRADE = no`（注释："if no, only building based countries"）→ 贸易两项**只对建筑型国家**生效，非全国家。
- 增速回看窗口：`ECONOMICAL_BASE_GROWTH_SHORT_TERM_YEARS = 5`、`LONG_TERM_YEARS = 50`。
- 物价缩放：`PRICE_SCALE_FROM_ECO_BASE = 0.1`。

### 4.3 对 mod 的含义
- **改 `ECONOMICAL_BASE_FROM_TAX_BASE` 实际改的是"财富项权重"，不是税基项**——别被键名骗。
- 经济基础**不受 control 影响**，所以"征服高财富低控制地区"会**抬高经济基础但不增国库**——这正是低税基比例国家"看着强、打不起仗"的成因（但它会抬高基于经济基础的开支，如宫廷/外交/稳定度投入）。

## 五、税基比例 tax_base_ratio

- **公式**：`税基 ÷ 财富`（defines 注释原文："tax base / wealth ratio"）。
- **UI 阈值**（`NGUI` L105–107）：**≥0.75 绿 / ≥0.5 黄 / ≥0.25 橙 / 低于 0.25 红**。
- **读法**（词条原文）：低比例="看起来强大但资金很少，经济从不进入国库"；**差距就是 control**；"最大收益来自在控制低的地方提高**富地点**的控制——不需要新建筑或新 POP，只需攥紧你已产出的财富"。
- ⚠️ **简中未译全**：`game_concept_tax_base_ratio` 显示名仍是 **"Tax Ratio"**、desc 是英文。

## 六、控制 control（真正的杠杆）

| 常量 / 键 | 值 / 作用 |
| --- | --- |
| `MONTHLY_CONTROL_DECAY`（NLocation） | 0.01，每月衰减 |
| `LOW_CONTROL_THRESHOLD_FOR_BEST_TAX`（NCountry） | **0.95**——**≥95% 才算满税** |
| `MILITARY_CONTROL_FLOOR`（NCountry） | 0.10 |
| 修正键族（全部注册） | `local_max_control` / `global_max_control`、`local_monthly_control` / `global_monthly_control`、`local_max_rural_control` / `local_max_urban_control`、`local_monthly_control_decline` / `global_monthly_control_decline`、`control_importance_modifier` |

## 七、税基不只收钱：5 个"税基参考"用途

| 用途 | 常量 |
| --- | --- |
| 人力恢复 | `MONTHLY_MANPOWER_PER_BASE_TAX_REFERENCE = 0.04` |
| 水手恢复 | `MONTHLY_SAILORS_PER_BASE_TAX_REFERENCE = 0.0008` |
| 市场参考规模 | `MARKET_TAX_BASE_REFERENCE = 100.0` |
| 战争价值（和约估值） | `WAR_WORTH_TAX_BASE_SCALE = 0.13`、`WAR_WORTH_TAX_BASE_CAP = 6` |
| 通胀成本 | `MONTHLY_INFLATION_COST_PER_TAX_BASE = 0.33` |

## 八、Mod 可调点速查

| 想改什么 | 动哪里 | 注意 |
| --- | --- | --- |
| 某阶层税档 | `*_estate_max_tax` / `*_estate_min_tax`（country percent） | 8 阶层 + `global_*`；税率是相对 max_tax 的百分比 |
| 收税效率 | `tax_income_efficiency`（country percent） | — |
| 阶层结余扣减 | `estate_enrichment`（country percent，`color=bad`） | — |
| 平民财产转贵族 | `local_peasant_enfranchisment`（location percent） | — |
| 控制上限/月增长/衰减 | `local_max_control` / `local_monthly_control` / `local_monthly_control_decline`（及 global 版） | 控制直接乘进税基 |
| 经济基础各权重 | `NEconomy` 的 9 个常量 | ⚠️ **`_FROM_TAX_BASE` 实为财富项**；`_FROM_SUBJECT` 实为附庸财富 |
| 税基比例显示阈值 | `NGUI` 的 `TAX_BASE_RATIO_*_THRESHOLD` | 纯 UI 配色，不改玩法 |

## 九、引擎侧未证（别当已证）

- `tax_per_pop` 的**精确用途**：词条说阶层按 `estate_power` 分 tax_base，而数据给的是 `tax_per_pop`；二者关系未见任何文档（只知数值阶梯与"人均财富份额"描述吻合）。
- `estate_enrichment` 的**扣减公式**（只知它是 country percent 修正）。
- 经济基础财富项是"Σ地点财富"还是经某种折算（0.5 系数已由 tooltip 与 define 对齐，但"full wealth"的措辞与 0.5 的系数如何并置未证）。
- `tax_rate` / `max_tax` 的**生效顺序**与 `estate_power` 的具体介入点。
- `PRICE_SCALE_FROM_ECO_BASE`（物价随经济基础缩放）的精确套用方式。

## 中文检索键

| 中文 | 内部名 |
| --- | --- |
| 经济基础 | `economical_base`（`ECONOMICAL_BASE_*`、`ECO_BASE_*`） |
| 税基 / 税基比例 | `tax_base` / `tax_base_ratio`（**键名陷阱**：`ECONOMICAL_BASE_FROM_TAX_BASE` 实为 wealth） |
| 财富 / 财富份额 / 财富影响 | `wealth` / `wealth_share` / `wealth_impact` |
| 控制 | `control`（`local_max_control`、`MONTHLY_CONTROL_DECAY`） |
| 税率 / 最高税 / 最低税 | `tax_rate` / `max_tax` / `min_tax`（`*_estate_max_tax`） |
| 税收效率 / 阶层结余 | `tax_income_efficiency` / `estate_enrichment` |
| 平民释权 | `local_peasant_enfranchisment` |
| 阶层人均税权 | `tax_per_pop`（`estates\00_default.txt`） |
