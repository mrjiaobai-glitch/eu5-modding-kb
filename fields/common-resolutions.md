# common/resolutions（国际组织决议）

来源：`in_game\common\resolutions\readme.txt`

## 字段

```
<resolution key> = {
    loc = <string>                       # 可选；基础 loc 键（未指定则用决议键）
    potential = <trigger>                # 能否显示；scope:actor = 国家
    allow = <trigger>                    # 是否启用；scope:proposer = 提议国；scope:recipient = IO
    can_vote = <trigger>                 # 能否投票；scope:actor, scope:recipient = IO
    is_live = <trigger>                  # 决议是否活跃（主要用于选举）；活跃则测试能否定案
    proposal_price / price = <scripted price>   # 引用 \common\prices\（price:<price_id> 或脚本结果）
    proposal_price_modifier / price_modifier = <script value>
    proposal_payer / payer = <scripted country>  # 默认 proposer
    proposal_payee / payee = <scripted country>  # 默认无人
    requires_vote = <trigger>            # 是否必须投票（否则可单方面行动）
    requires_explicit_votes = <yes/no>   # 是否要求显式投票（选举用；否则取当前意见）
    votes = <scripted value>             # 每国票数；scope:actor/proposer/recipient/target...
    total_votes_needed = <scripted value>  # 可选；获胜阈值
    should_finalize_vote = <trigger>     # 是否立即定案；deadline 优先于该 trigger；额外参数 scope:total_votes_needed / scope:total_votes_available / highest_vote
    select_trigger = { ... }             # 可多个；格式同 generic_actions；**最后一个 select_trigger 是国家投票的对象**
    show_message = no                    # 可选
    ai_will_select = <scripted value>    # AI 想提议投票的程度；scope:actor/recipient/target...
    ai_will_do = <scripted value>        # AI 想投什么；scope:actor/proposer/recipient/target...
    ai_proposer_risk = <scripted value>  # AI 不想被拒的程度
    ai_tick_frequency = <scripted value> # scope:actor = country
    years / months / weeks / days = <int>  # 定案期限；到期后所有 AI 自动投票
    vote_ongoing_modifier = <modifier>   # 投票进行中对成员国的修正
    propose_effect = <effect>            # 提议时；scope:proposer/recipient/target...
    effect = <effect>                    # 通过时；外加 scope:price/scope:price_modifier/scope:payer/scope:payee
    reject_effect = <effect>             # 被拒时
    vote_effect = <effect>               # 国家投票时；scope:actor = 投票国, scope:proposer, scope:active_resolution, scope:vote, scope:recipient/target...
    abstain_effect = <effect>            # 国家撤票时；同上作用域
    cooldown = { type = <any tag> days/weeks/months/years = <integer> }
    show_target_in_tooltip = <yes/no>    # 默认 no
}
```

## 本地化

- 决议本身：名称 tag = `<action_tag>`、描述 tag = `<action_tag>_desc`（可用 `loc` 字段指定基础键）。
- 消息键同 generic_actions：`PERFORM_<Key>_ACTION` / `WE_PERFORM_<Key>_ACTION` / `OTHER_PERFORMS_<Key>_ACTION` / `ACTION_<Key>_PERFORMED_ON_US`；预设 `$ACTION$`/`$EFFECT$`/`$DESC$`。

## 可用 trigger/effect（readme 节选）

- `resolution:<key>`：进入决议作用域
- `any_active_resolution`：IO 作用域下所有正在投票的决议列表
- `set_vote = { resolution = <resolution> voter = <country> vote = <值> }`

## 审查要点

- 最后一个 select_trigger 即投票选项——顺序有意义。
- 未在 readme 中说明：无。
