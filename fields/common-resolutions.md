# common/resolutions（国际组织决议）

> **一句话**：国际组织决议的字段全表：投票、价格、期限、AI 权重与各类效果，以及配套本地化与消息键。
> **什么时候看**：写国际组织决议或选举、要核对投票判定与最后一个 select_trigger 的语义时翻这篇。
> **体量**：189 行 · 约 9 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\resolutions\` 全量 **24 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `ai_tick` | 5 | 5 | monthly（4）、daily（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`proposal_price`**（2 种）：price:propose_curia_action（12）、price:policy_vote（1）
- **`international_organization_type`**（1 种）：catholic_church（12）
- **`months`**（1 种）：6（12）
- **`total_votes_needed`**（1 种）：1（2）
- **`ai_tick`**（2 种）：monthly（4）、daily（1）
- **`requires_explicit_votes`**（1 种）：no（4）
- **`loc`**（2 种）：election（2）、hre_election（1）
- **`days`**（2 种）：1（2）、365（1）
- **`show_message`**（1 种）：no（2）
- **`show_target_in_tooltip`**（1 种）：yes（1）

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `international_organization_type` | 12 | 21 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `action_name` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `actor` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `ai_interaction_source_list` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `ai_tick_frequency` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `allow_null` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `allow_self` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `any_active_resolution` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `bottom_widget` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `button` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `button_tooltip_override` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `cache_interaction_source_list` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `cache_order` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `cache_targets` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `click_mode` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `click_type` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `column` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `conditions` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `default` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `default_sort` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `description` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `effects` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `enabled` | 0 次（24 档） | **有**（出现在 12 个类目） |
| `format` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `interaction_source_list` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `looking_for_a` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `map_color` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `map_mode` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `max` | 0 次（24 档） | **有**（出现在 25 个类目） |
| `max_targets_for_ui` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `min` | 0 次（24 档） | **有**（出现在 19 个类目） |
| `name` | 0 次（24 档） | **有**（出现在 30 个类目） |
| `none_available_msg_key` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `NOTE` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `only_color_selectable` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `parameter` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `payee` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `payer` | 0 次（24 档） | **有**（出现在 1 个类目） |
| `pre_evaluation_number_to_evaluate_fully` | 0 次（24 档） | **有**（出现在 6 个类目） |
| `pre_evaluation_sort_value` | 0 次（24 档） | **有**（出现在 6 个类目） |
| `price_modifier` | 0 次（24 档） | **有**（出现在 4 个类目） |
| `proposal_payee` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `proposal_payer` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `proposal_price_modifier` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `proposer` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `resolution` | 0 次（24 档） | **有**（出现在 11 个类目） |
| `scripted_action_tooltip` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `secondary_map_color` | 0 次（24 档） | **有**（出现在 2 个类目） |
| `selected` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `set_vote` | 0 次（24 档） | **有**（出现在 5 个类目） |
| `show_if` | 0 次（24 档） | **有**（出现在 2 个类目） |
| `show_why_not_enabled` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `show_why_not_visible` | 0 次（24 档） | 全库也没有 → 疑似废弃字段 |
| `source` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `source_ai_override` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `source_flags` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `source_flags_ai_override` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `source_global_list` | 0 次（24 档） | **有**（出现在 3 个类目） |
| `step` | 0 次（24 档） | **有**（出现在 2 个类目） |
| `target_flag` | 0 次（24 档） | **有**（出现在 8 个类目） |
| `title` | 0 次（24 档） | **有**（出现在 1 个类目） |
| … | 另有 8 个 | |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_vote_weight` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `limit` | 275 | assert_if、random_policy_in_law、every_country_with_capital_in_geography、else_if |
| `value` | 191 | scope:potential_high_king、if、scope:vote、ai_will_do |
| `if` | 190 | scope:recipient、if、scope:vote.ruler_or_heir_if_regent、else_if |
| `add` | 142 | ai_will_do、else、scope:potential_high_king、else_if |
| `desc` | 131 | subtract、multiply、add、value |
| `scope:recipient` | 86 | if、visible、limit、selected |
| `NOT` | 86 | visible、can_vote、scope:vote、limit |
| `name` | 67 | remove_list_global_variable、save_temporary_value_as、add_to_variable_list、add_to_global_variable_list |
| `exists` | 67 | or、limit、AND、potential |
| `scope:actor` | 64 | allow、can_vote、potential、limit |
| `data` | 57 | column |
| `column` | 57 | select_trigger |
| `target_flag` | 48 | select_trigger |
| `looking_for_a` | 48 | select_trigger |
| `visible` | 48 | select_trigger |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `international_organization_type` | 12 | 21 |
