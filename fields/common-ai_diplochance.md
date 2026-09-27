# common/ai_diplochance（AI 外交接受度权重）

来源：**无有效 readme**。目录里只有两个文件：数据文件 `00_ai_diplochance.txt`（5 224 B）与被**放错位置**的 `ai_diplochance.info`（808 B）——后者其实是 `customizable_localization` 的格式文档（内容讲 scope 类型表、`text = { trigger/localization_key/fallback }`、`random_valid`、`parent`/`suffix` 继承），与本类目无关。字段语义由数据反推 + 对照 `country_interactions` 的接受度字段。

## 结构

```
<交互类型> = {
    <评估项> = <权重>      # 正数 = 更容易接受；负数 = 更难；用于总量修正与硬否决
}
```

原版 **16 张表**（每张表内的键是**已命名的评估项**，与 `country_interactions` 里的字段同名）：

| 表 | 用途 | 代表项 |
|---|---|---|
| `royal_marriage` | 王室联姻 | `different_religion −100`、`different_religion_group −200`、`age_female −10`、`marriage_desirability +1`、`culture_view +10`、`union_size −4` |
| `demand_become_subject_action` | 要求臣服 | `base −50`、`in_debt +1.0`、`negative_stability +0.2`、`war_exhaustion +0.5`、`has_truce −10` |
| `offermilaccess` / `buy_milaccess` | 军事通行 | `base 0 / +25`、`giving_defensive_support +5`、`receiving_defensive_support +10` |
| `offerloan` | 放贷 | `base −20`、`good_interest_rate +500`、`actor_creditworthiness +100`，以及 5 个 **−1000 硬否决**（贷款太小/到期太早/太晚/利率太高/已有太多） |
| `sell_location` / `buy_location` | 买卖地点 | 两表**符号相反**：卖看 `location_value +100`、`price −500`；买看 `capital −99999`、`in_debt +100` |
| `enforce_peace` | 强制和平 | `yesman +10000`、`relative_strength +25`、`war_exhaustion +5`、`competing_power −100` |
| `threaten_war` | 战争威胁 | `yesman +10000`、`relative_strength +40`、`capital −100`、`location_value −1000` |
| `requestpeace` | 请求和平 | `victory +1000`、`peaceoffer_seek_white_peace +50`、`war_enthusiam −100`、`desperation −20`、`warscore +1`、`months_at_war +0.5` |
| `ransom_subunits` | 赎回部队 | `warscore +0.2`、`price −20`、`strategic_interest +20` |
| `callaction_defensive` / `callaction_offensive` | 响应盟友召唤 | `enforced_demand +10000`、`betrayed_ally −1000`、`promised_land +20`（进攻侧）、`recipient_civil_war −50/−100` |
| `invite_to_international_organization` / `ask_to_join_international_organization` / `create_international_organization_targetting` | 国际组织 | 原版这三张几乎是**空表**（只有注释或空块） |

## 常见评估项（按出现次数）

`base` 9 / `trust_in_actor` 8 / `in_debt` 5 / `recipient_at_war` 4 / `war_exhaustion` 4 / `border_distance` 4 / `royal_ties` 4 / `recipient_occupied_beseiged_locations` 3 / `different_culture` 3 / `price` 3 / `rank_difference` 3 / `low_manpower` 3 / `negative_stability` 3 / `negative_opinion` 3 / `same_culture` 3 / `location_value` 3 / `opinion` 3 / `enforced_demand` 3 / `different_religion_group` 3 …（全文去重键 **107** 个 = 16 个表名 + **91 个评估项**；这 91 个键在 16 张表里共**使用 168 处**——`base` 9 张表都用、`trust_in_actor` 8 张，2026-09 体检核实）

值的量级很关键：**±1~50 是软倾向，±1000/10000 是"硬否决/硬必应"**（`yesman`、`loan_is_insignificant`、`betrayed_ally`）。

## 与 `country_interactions` 的关系（别搞混）

- `country_interactions\<交互>.txt` 里的 `ai_*` 字段：`ai_prerequisite`（AI 预筛，46 处）、`ai_will_do`（AI 想不想发起，105 处）、`ai_diplomacy_*` 等——**发起侧**。
- `common\ai_diplochance\`：同一批评估项的**权重表**，决定"对方接不接受"——**接受侧**。
- 两侧用的是同名评估项（`trust_in_actor`、`border_distance`、`royal_ties`…），改动时通常要一起看。

## 审查要点

- 表名要与交互类型对得上（原版 16 张对应 16 类交互）；写错的表名不会报错，只是永不生效。
- **不要用 `ai_diplochance.info` 当参考**：它是放错位置的 `customizable_localization` 文档（内容同 `common\customizable_localization\` 的 `.info`）。
- 加入自定义评估项前先确认引擎认这个键——原版 78 个键都是引擎提供的评估项名，不是任意脚本表达式。
- 未在 readme 中说明：本类目**没有可用 readme**；键的完整清单、权重如何合成、是否可叠加，均以本文件与本体数据为准。
