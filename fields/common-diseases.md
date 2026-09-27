# common/diseases（疾病）

来源：`in_game\common\diseases\readme.txt`

## 字段

```
<disease_tag> = {
    potential = <trigger>                      # 不满足则不尝试每月生成（scope:disease = 疾病）
    monthly_spawn_chance = <script value>      # 每月生成概率（0..1）（scope:disease）
    spawn = <effect>                           # 决定生成时调用；须含 spawn_disease effect（scope:disease）
    r0 = <script value>                        # R0 值（每人每间隔传染人数）（root = location, scope:disease）
    environmental_infection = <script value>   # 环境传播加成（root = location, scope:disease/scope:disease_outbreak/scope:current_presence）
    calc_interval_days = <script value>        # 传播计算间隔天数；通常纯数字或随机区间；无作用域
    location_stagnation_chance = <script value>  # location 停滞概率（0..1）（root = location, scope:disease/disease_outbreak/current_presence）
    sub_unit_stagnation_chance = <script value>  # 子单位停滞概率（0..1）（root = sub unit, ...）
    percentage_to_meet_their_fate_on_calc = <script value>  # 每次计算间隔中命运被决定的比例（0..1）（scope:disease/disease_outbreak/current_presence）
    location_modifier = <modifier>             # 施加于有疾病的 location（×presence %）
    mortality_rate = <script value>            # 死亡率（0..1）（scope:disease）
    character_mortality_chance = <script value>  # 角色每计算间隔死亡概率（0..1）（root = location, ...）
    monthly_resistance_reduction = <script value>  # 每月抗性流失量；默认 0
    location_spread_threshold = <script value> # 开始向新 location 传播的最低百分比（0..1）（root = 角色所在 location, ...）
    on_spread_to_country = <event>             # 传播到国家时发的事件（root = country, scope:disease）
    map_color / secondary_map_color = <script color>  # root = location, scope:disease
    specific_pop_type_effect = { pop_type = <pop_type> multiplier = <float> }  # 按 pop 类型调整传播（默认均等）；也可用 culture/religion/religion_group/language/language_family 键；multiplier = 0 表示不受影响
}
```

## 配套 modifier 命名约定（readme 声明）

- `local_<disease_tag>_impact_modifier`：location 上用，减轻/加重影响（如医院带 `local_my_disease_impact_modifier = -0.9`）
- `local_<tag>_resistance_modifier`：location 抗性
- `national_<tag>_resistance_modifier`：国家抗性
- `local_<tag>_growth_modifier` / `national_<tag>_growth_modifier`：疾病增长

## 传播规则（readme 声明）

- 从 location 传播无门槛；以下情况**不**传播到目标 location：目标无人口；两地主国之间有禁运；目标已有 ≥50% presence；目标已停滞。
- 传播目标：邻居、市场中心、市场中心的贸易伙伴、location 所有者的首都。

## 审查要点

- `specific_pop_type_effect` 的 `pop_type` 引用须在 common/pop_types 存在。
- 各 script value 作用域差异大（r0 是 location；calc_interval_days 无作用域）——写错作用域读取不到。
- 未在 readme 中说明：本地化键格式。
