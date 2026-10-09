# common/scripted_relations（脚本化外交关系）

> **一句话**：脚本化外交关系的字段全表：类型、外交容量、战争与间谍中断条件、各类价格与 AI 评估字段。
> **什么时候看**：写自定义外交关系、要配置中断条件与 AI 意愿时翻这篇。
> **体量**：200 行 · 约 10 分钟通读

来源：`in_game\common\scripted_relations\readme.txt`

## 基础字段

```
<name> = {
    type = <diplomacy/subject/union>   # 对所有国家 / 仅附庸 / 仅联合伙伴
    relation_type = <oneway/mutual>    # 单向给予 / 双向对等
    uses_diplo_capacity = <none/mutual/giving/receiving>   # 是否占用外交关系槽
    diplomatic_capacity_cost = <script value>
    block_when_at_war = <yes/no>
    break_on_war = <yes/no>            # 任一方与另一方开战则结束
    break_on_becoming_subject = <yes/no>
    break_on_not_spying = <yes/no>     # 间谍网络停止（含被发现）则结束
    annulled_by_peace_treaty = <yes/no>
    annullment_favours_required = <integer>
    disallow_war = <yes/no>            # 禁止双方互相宣战
    embargo = <yes/no>
    military_access / fleet_basing_rights / food_access = <yes/no>  # mutual 双向；oneway 接收方获得
    is_exempt_from_sound_toll = <yes/no>
    is_exempt_from_isolation = <yes/no>
    block_building = <yes/no>          # 阻止在对方领土建外国建筑
    skip_diplomat_for_cancel = <yes/no>
    lifts_fog_of_war = <yes/no>
    called_in_defensively / called_in_offensively = <none/mutual/giving/receiving>
    lifts_trade_protection = <yes/no>
    trade_to_first / trade_to_second = <script value>
    gold_to_first / gold_to_second = <script value>        # 每月金币
    favors_to_first / favors_to_second = <script value>
    institution_spread_to_first / _second = <script value>
    diplomatic_cost = <price>          # 建立关系花费
    war_declaration_cost = <price>
    buy_price = <price>                # 未指定则不可购买
    monthly_ongoing_price_first_country = <price>  # mutual 双方都付
    monthly_ongoing_price_second_country = <price>  # oneway 第二国
    select_trigger = { ... }           # 格式同 generic_actions
    sound = "<sound gfx>"
    mutual_color / giving_color / receiving_color = <color definition>  # 外交地图模式
    visible = <trigger>
    offer_visible / request_visible / cancel_visible / break_visible = <trigger>
    offer_enabled / request_enabled / cancel_enabled / break_enabled = <triggers>
    will_expire_trigger = <triggers>
    should_ai_offer_trigger = <triggers>
    wants_to_give = <ai evaluation>          # mutual 与 oneway 都用；评估请求时
    wants_to_receive = <ai evaluation>       # 仅 oneway；评估 offer 时
    wants_to_give_diplo_chance / wants_to_receive_diplo_chance = <diplo evaluation>
    wants_to_keep = <ai evaluation>          # ≤0 时 AI 尝试取消/断开
    wants_to_keep_diplo_chance = <diplo evaluation>
    show_break_alert = <yes/no>
    giving_modifier_scale / receiving_modifier_scale / mutual_modifier_scale = <script math>  # scope:first, scope:second
    offer_effect / request_effect / cancel_effect / break_effect / offer_declined_effect / request_declined_effect / expire_effect = <effects>
    # --- ongoing 区域 ---
    is_ongoing = <yes/no>
    texture_file = <string>
    concept = <string>
    progress = <script value>          # 0–100；scope:first/second
}
```

## 自动修正/偏见（readme 声明）

- `<key>`：mutual 时修正；`giving_<key>`：给出一侧；`receiving_<key>`：接收一侧
- `opinion_<key>` / `opinion_giving_<key>` / `opinion_receiving_<key>` / `opinion_decline_<key>`
- `trust_<key>` / `trust_giving_<key>` / `trust_receiving_<key>` / `trust_decline_<key>`
- ongoing 附加 `<key>_ongoing_tooltip` 字符串

## Diplo chances 值清单（同 country_interactions，额外两项）

- 完整清单与 `common-country_interactions.md` 相同；本文件额外：`too_much_antagonism`（将超过国家愿承受敌意上限的国家数）、`common_rivals_and_enemies`、`common_rivals`

## 审查要点

- `type`/`relation_type`/`uses_diplo_capacity`/`called_in_*` 是枚举。
- 各 price 引用须在 common/prices 存在。
- 未在 readme 中说明：无。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\scripted_relations\` 全量 **36 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `relation_type_for_ai` | 23 | 23 | friendly（15）、hostile（8） |
| `diplomatic_cost_request` | 4 | 4 | fleet_basing_rights（1）、military_access（1）、anti_piracy_agreement_cost（1）、knowledge_sharing_cost（1） |
| `diplomatic_cost_offer` | 3 | 3 | agitate_for_liberty_cost（1）、corrupt_officials_cost（1）、sow_discontent_cost（1） |
| `concept` | 2 | 2 | spy_network（2） |
| `merchant_fraction_to_first` | 2 | 2 | 0.5（2） |
| `can_share_maps` | 1 | 1 | yes（1） |
| `is_exempt_from_import_tariff` | 1 | 1 | yes（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`type`**（3 种）：diplomacy（26）、subject（9）、union（1）
- **`relation_type`**（2 种）：oneway（34）、mutual（2）
- **`break_on_war`**（1 种）：yes（27）
- **`relation_type_for_ai`**（2 种）：friendly（15）、hostile（8）
- **`institution_spread_to_second`**（3 种）：monthly_institution_spread_weak（5）、monthly_institution_spread_mild（3）、monthly_institution_spread_severe（2）
- **`receiving_color`**（5 种）：rgb { 60 120 0 }（4）、define:NMapColors|DIPLOMACY_MIL_ACCESS_COLOR（2）、define:NMapColors|DIPLOMACY_FLEET_BASING_COLOR（1）、define:NMapColors|DIPLOMACY_FOOD_ACCESS_COLOR（1）、define:NMapColors|DIPLOMACY_GUARANTEE_COLOR（1）
- **`annulled_by_peace_treaty`**（1 种）：yes（8）
- **`block_when_at_war`**（2 种）：yes（6）、no（2）
- **`diplomatic_capacity_cost`**（7 种）：support_heir_upkeep_cost（1）、guarantee_upkeep_cost（1）、alliance_upkeep_cost（1）、rein_in_junior_diplomacy_upkeep_cost（1）、deny_market_access_upkeep_cost（1）、military_sponsorship_upkeep_cost（1）、inheritance_contract_upkeep_cost（1）
- **`uses_diplo_capacity`**（3 种）：giving（4）、receiving（2）、mutual（1）
- **`annullment_favours_required`**（3 种）：5（4）、10（2）、20（1）
- **`break_on_becoming_subject`**（1 种）：yes（5）
- **`use_with_enemies`**（1 种）：yes（5）
- **`giving_color`**（1 种）：rgb { 40 100 0 }（4）
- **`diplomatic_cost_request`**（4 种）：fleet_basing_rights（1）、military_access（1）、anti_piracy_agreement_cost（1）、knowledge_sharing_cost（1）
- **`diplomatic_cost_offer`**（3 种）：agitate_for_liberty_cost（1）、corrupt_officials_cost（1）、sow_discontent_cost（1）
- **`called_in_defensively`**（2 种）：mutual（2）、giving（1）
- **`break_on_not_spying`**（1 种）：yes（3）
- **`buy_price`**（3 种）：buy_military_access（1）、buy_fleet_basing_rights（1）、inheritance_contract_cost（1）
- **`skip_diplomat_for_cancel`**（1 种）：yes（3）
- …另有 27 个枚举字段，见完整普查报告

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_interaction_source_list` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `allow_null` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `allow_self` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `bottom_widget` | 0 次（36 档） | **有**（出现在 1 个类目） |
| `cache_interaction_source_list` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `cache_order` | 0 次（36 档） | **有**（出现在 1 个类目） |
| `cache_targets` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `column` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `default` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `default_sort` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `diplomatic_cost` | 0 次（36 档） | **有**（出现在 1 个类目） |
| `enabled` | 0 次（36 档） | **有**（出现在 12 个类目） |
| `format` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `giving_modifier_scale` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `gold_to_first` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `interaction_source_list` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `looking_for_a` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `map_color` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `map_mode` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `max` | 0 次（36 档） | **有**（出现在 25 个类目） |
| `max_targets_for_ui` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（36 档） | **有**（出现在 19 个类目） |
| `monthly_ongoing_price_second_country` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `mutual_modifier_scale` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `name` | 0 次（36 档） | **有**（出现在 30 个类目） |
| `none_available_msg_key` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `only_color_selectable` | 0 次（36 档） | **有**（出现在 1 个类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（36 档） | **有**（出现在 6 个类目） |
| `pre_evaluation_sort_value` | 0 次（36 档） | **有**（出现在 6 个类目） |
| `secondary_map_color` | 0 次（36 档） | **有**（出现在 2 个类目） |
| `selected` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `show_if` | 0 次（36 档） | **有**（出现在 2 个类目） |
| `show_why_not_enabled` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `show_why_not_visible` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |
| `sound` | 0 次（36 档） | **有**（出现在 2 个类目） |
| `source` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `source_ai_override` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `source_flags` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `source_flags_ai_override` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `source_global_list` | 0 次（36 档） | **有**（出现在 3 个类目） |
| `step` | 0 次（36 档） | **有**（出现在 2 个类目） |
| `target_flag` | 0 次（36 档） | **有**（出现在 8 个类目） |
| `top_widget` | 0 次（36 档） | **有**（出现在 4 个类目） |
| `war_declaration_cost` | 0 次（36 档） | 全库也没有 → 疑似废弃字段 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `category` | 36 | 36 |
| `buy_price_modifier` | 1 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 127 | scope:recipient、if、favors_to_first、multiply |
| `add` | 119 | receiving_modifier_scale、buy_price_modifier、if、wants_to_receive |
| `scope:actor` | 106 | AND、break_effect、offer_effect、if |
| `scope:recipient` | 106 | AND、break_effect、?、offer_effect |
| `not` | 103 | AND、scope:recipient、offer_enabled、limit |
| `desc` | 96 | subtract、wants_to_give、add |
| `limit` | 96 | every_country_with_coalition_grade_antagonism_against_us、every_port_in_country、trigger_if、else_if |
| `if` | 79 | break_effect、offer_effect、if、wants_to_receive |
| `target` | 40 | giving_scripted_relation、remove_trust_equilibrium、reverse_add_opinion、add_truce_with |
| `offer` | 36 | category |
| `cancellation` | 36 | category |
| `multiply` | 34 | scope:actor、add、scope:recipient、value |
| `break` | 31 | category |
| `OR` | 30 | scope:first、AND、scope:actor、limit |
| `request` | 30 | category |
