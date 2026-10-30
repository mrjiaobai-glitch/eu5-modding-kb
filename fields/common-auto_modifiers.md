# common/auto_modifiers（自动修正）

> **一句话**：自动修正的字段与顺序约束：类别与作用域类型须写在触发条件之前，另有缩放、限制与警报等应用设置。
> **什么时候看**：要写随条件自动施加的修正，或排查自动修正不生效时看。
> **体量**：114 行 · 约 6 分钟通读

来源：`in_game\common\auto_modifiers\readme.txt`

## 属性

```
<auto_modifier_id> = {
    category = <修正类别>       # 默认 country；若设置必须在任何 modifiers 之前
    type = <作用域类型>         # potential_trigger 的作用域类型；默认 country；若设置必须在 potential_trigger 之前
    icon = <icon 路径>
    requires_real = <yes/no>    # 是否要求真实国家
    potential_trigger = <trigger>   # 是否应用该自动修正
    scales_with = <script value>    # 评估一个值来乘自动修正效果
    limit = <trigger>           # 检查何时可以应用
    hide_effects = <yes/no>     # 是否隐藏修正
    alert = <yes/no>            # 激活时是否在警报中显示
    <modifiers>                 # 其余全是修正
}
```

## 审查要点

- `category`/`type` 必须放在 modifiers 与 potential_trigger **之前**（顺序约束，readme 明示）。
- 本地化键格式在 readme 中未说明；按实测经验为 `AUTO_MODIFIER_NAME_<名>`（SKILL.md 第 6 节有详细规则）。
- 未在 readme 中说明：`type` 的合法取值集合。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\auto_modifiers\` 全量 **8 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、取值白名单（本体出现过的值 + 次数）

- **`scales_with`**（11 种）：total_population（1）、num_of_religious_aspects（1）、byz_greek_population（1）、byz_romance_population（1）、byz_romance_diplo_count（1）、total_accepted_culture_population（1）、war_exhaustion（1）、power_projection（1）、byz_greek_diplo_count（1）、monthly_income_trade_and_tax（1）、num_of_non_rural（1）
- **`category`**（3 种）：country（28）、internationalorganization（1）、dynasty（1）
- **`monthly_towards_hellenization`**（4 种）：societal_value_min_scaling_monthly_move（18）、societal_value_tiny_monthly_move（4）、societal_value_minor_monthly_move（3）、societal_value_large_monthly_move（1）
- **`monthly_towards_latinization`**（4 种）：societal_value_min_scaling_monthly_move（16）、societal_value_tiny_monthly_move（5）、societal_value_minor_monthly_move（3）、societal_value_large_monthly_move（1）
- **`global_estate_target_satisfaction`**（12 种）：medium_permanent_target_satisfaction_penalty（6）、medium_permanent_target_satisfaction（3）、tiny_satisfaction_scaled_down（2）、-0.05（2）、tiny_satisfaction_scaled_down_penalty（1）、base_default_estate_target_satisfaction（1）、positive_stability_satisfaction_scale（1）、-0.01（1）、-0.1（1）、negative_stability_satisfaction_scale（1）、0.05（1）、-0.025（1）
- **`monthly_towards_belligerent`**（2 种）：societal_value_monthly_move（8）、societal_value_minor_monthly_move（4）
- **`monthly_prestige`**（4 种）：0.1（5）、0.2（3）、-0.1（1）、-0.2（1）
- **`monthly_legitimacy`**（8 种）：-0.5（3）、-0.02（1）、-0.03（1）、0.1（1）、-0.2（1）、-0.1（1）、-1.0（1）、0.2（1）
- **`clergy_estate_target_satisfaction`**（7 种）：medium_permanent_target_satisfaction_penalty（4）、-0.02（1）、-0.1（1）、clergy_default_satisfaction（1）、-0.2（1）、-0.05（1）、-0.03（1）
- **`pop_leave_rebels_threshold`**（3 种）：-0.05（6）、-0.1（1）、0.35（1）
- **`monthly_devotion`**（7 种）：-0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1）、-1.0（1）、-0.03（1）
- **`monthly_tribal_cohesion`**（7 种）：-0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1）、-1.0（1）、-0.03（1）
- **`monthly_horde_unity`**（7 种）：-0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1）、-1.0（1）、-0.03（1）
- **`monthly_republican_tradition`**（6 种）：-0.5（2）、-0.02（1）、0.1（1）、-0.1（1）、-1.0（1）、-0.03（1）
- **`diplomatic_reputation`**（7 种）：2（1）、diplomatic_reputation_weak_bonus（1）、diplomatic_reputation_severe_bonus（1）、-0.75（1）、1（1）、diplomatic_reputation_mild_bonus（1）、-0.5（1）
- **`nobles_estate_target_satisfaction`**（4 种）：medium_permanent_target_satisfaction_penalty（3）、-0.015（1）、-0.01（1）、nobles_default_satisfaction（1）
- **`monthly_religious_influence`**（5 种）：-0.05（2）、0.05（1）、-0.08（1）、-0.003（1）、0.1（1）
- **`monthly_doom`**（3 种）：-0.1（3）、0.25（2）、0.5（1）
- **`peasants_estate_target_satisfaction`**（5 种）：huge_permanent_target_satisfaction（2）、peasants_default_satisfaction（1）、-0.015（1）、-0.01（1）、yanatin_peasants_scale（1）
- **`burghers_estate_target_satisfaction`**（4 种）：medium_permanent_target_satisfaction_penalty（3）、-0.015（1）、burghers_default_satisfaction（1）、-0.01（1）
- …另有 236 个枚举字段，见完整普查报告

### 二、该用哪些修正（本体在这个类目里实际用过，前 20）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `monthly_towards_hellenization` | 26 | 1 | societal_value_min_scaling_monthly_move（18）、societal_value_tiny_monthly_move（4）、societal_value_minor_monthly_move（3）、societal_value_large_monthly_move（1） |
| `monthly_towards_latinization` | 25 | 1 | societal_value_min_scaling_monthly_move（16）、societal_value_tiny_monthly_move（5）、societal_value_minor_monthly_move（3）、societal_value_large_monthly_move（1） |
| `global_estate_target_satisfaction` | 21 | 2 | medium_permanent_target_satisfaction_penalty（6）、medium_permanent_target_satisfaction（3）、tiny_satisfaction_scaled_down（2）、-0.05（2）、tiny_satisfaction_scaled_down_penalty（1） |
| `monthly_towards_belligerent` | 12 | 1 | societal_value_monthly_move（8）、societal_value_minor_monthly_move（4） |
| `monthly_prestige` | 10 | 1 | 0.1（5）、0.2（3）、-0.1（1）、-0.2（1） |
| `monthly_legitimacy` | 10 | 2 | -0.5（3）、-0.02（1）、-0.03（1）、0.1（1）、-0.2（1） |
| `clergy_estate_target_satisfaction` | 10 | 3 | medium_permanent_target_satisfaction_penalty（4）、-0.02（1）、-0.1（1）、clergy_default_satisfaction（1）、-0.2（1） |
| `monthly_horde_unity` | 8 | 2 | -0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1） |
| `monthly_devotion` | 8 | 2 | -0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1） |
| `monthly_tribal_cohesion` | 8 | 2 | -0.5（2）、-0.02（1）、0.1（1）、-0.2（1）、-0.1（1） |
| `pop_leave_rebels_threshold` | 8 | 1 | -0.05（6）、-0.1（1）、0.35（1） |
| `monthly_republican_tradition` | 7 | 2 | -0.5（2）、-0.02（1）、0.1（1）、-0.1（1）、-1.0（1） |
| `diplomatic_reputation` | 7 | 2 | 2（1）、diplomatic_reputation_weak_bonus（1）、diplomatic_reputation_severe_bonus（1）、-0.75（1）、1（1） |
| `peasants_estate_target_satisfaction` | 6 | 2 | huge_permanent_target_satisfaction（2）、peasants_default_satisfaction（1）、-0.015（1）、-0.01（1）、yanatin_peasants_scale（1） |
| `monthly_religious_influence` | 6 | 2 | -0.05（2）、0.05（1）、-0.08（1）、-0.003（1）、0.1（1） |
| `monthly_doom` | 6 | 1 | -0.1（3）、0.25（2）、0.5（1） |
| `burghers_estate_target_satisfaction` | 6 | 2 | medium_permanent_target_satisfaction_penalty（3）、-0.015（1）、burghers_default_satisfaction（1）、-0.01（1） |
| `nobles_estate_target_satisfaction` | 6 | 2 | medium_permanent_target_satisfaction_penalty（3）、-0.015（1）、-0.01（1）、nobles_default_satisfaction（1） |
| `monthly_towards_naval` | 5 | 1 | societal_value_monthly_move（3）、societal_value_minor_monthly_move（2） |
| `monthly_towards_land` | 5 | 1 | societal_value_monthly_move（3）、societal_value_minor_monthly_move（2） |
| … | 另有 10 个修正名 | | |

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `hide_effects` | 0 次（8 档） | 全库也没有 → 疑似废弃字段 |
| `icon` | 0 次（8 档） | **有**（出现在 9 个类目） |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `value` | 73 | scales_with |
| `multiply` | 63 | scales_with |
| `has_societal_value` | 51 | potential_trigger |
| `religion` | 40 | potential_trigger |
| `add` | 29 | scales_with |
| `religion:catholic` | 27 | potential_trigger |
| `modifier:can_ignore_papal_bulls` | 27 | potential_trigger |
| `NOT` | 26 | any_current_war、any_merchant_in_market、potential_trigger、any_known_country |
| `max` | 22 | scales_with |
| `min` | 21 | scales_with |
| `has_ruler` | 18 | potential_trigger |
| `uses_government_power` | 14 | potential_trigger |
| `exists` | 13 | AND、potential_trigger |
| `subtract` | 12 | scales_with |
| `this` | 8 | potential_trigger |
