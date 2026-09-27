# common/scripted_relations（脚本化外交关系）

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
